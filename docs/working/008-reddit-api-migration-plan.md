# Reddit API Migration Plan: Playwright to API-Based Harvesting

**Date**: 2026-02-12
**Status**: Research complete, ready for implementation
**Goal**: Replace Playwright browser scraping with Reddit API calls so reddit-harvest can run headlessly via a Telegram-triggered agent

---

## A. API vs Playwright Comparison

### What the API CAN do (that Playwright currently does)

| Capability | Playwright (current) | Reddit API | Notes |
|---|---|---|---|
| Search posts within a subreddit | Yes (navigate to search URL) | Yes (`GET /r/{sub}/search?q=...&sort=top&t=year`) | API returns structured JSON, no DOM parsing needed |
| Get post metadata (title, author, score, timestamp) | Yes (DOM extraction) | Yes (all fields in submission object) | API returns `score`, `author`, `created_utc`, `num_comments`, `permalink`, `selftext` |
| Get comments on a post | Yes (scroll to load, then extract) | Yes (`GET /comments/{article_id}`) | API returns full comment tree with `body`, `score`, `author`, `created_utc`, `depth`, `replies` |
| Get upvote counts | Yes (parse DOM `faceplate-number`) | Yes (`score` field on both posts and comments) | Note: Reddit fuzzes exact vote counts but the API `score` is the same as what the UI shows |
| Get comment counts | Yes (parse DOM) | Yes (`num_comments` on submission) | Exact match |
| Get timestamps | Yes (parse `<time>` elements) | Yes (`created_utc` as Unix timestamp) | API is more reliable — no date-string parsing |
| Get comment depth/nesting | Yes (parse `depth` attribute on `shreddit-comment`) | Yes (`depth` field in comment data) | Direct field access |
| Get comment replies | Yes (count child `shreddit-comment` elements) | Yes (`replies` field contains nested comment listing) | API gives the full tree structure |
| Get author usernames | Yes (parse `a[href*="/user/"]`) | Yes (`author` field) | Direct field access |
| Get permalink to comment | Yes (construct from `thingid`) | Yes (`permalink` field) | Direct field, no construction needed |
| Detect "same here" agreement | Yes (regex on reply text) | Yes (iterate `replies` in comment tree, same regex) | Same logic, cleaner data |
| Subreddit member count (for niche adjustment) | No (not extracted currently) | Yes (`GET /r/{sub}/about` returns `subscribers`) | API advantage: enables automatic niche subreddit scoring adjustment |

### What the API CANNOT do (or does worse)

| Limitation | Impact | Mitigation |
|---|---|---|
| No date-range filtering on search | Can't search "posts from last 6 months" directly | Use `time_filter` param (`year`, `month`, `week`) which covers our use case. For finer control, filter client-side by `created_utc` |
| Rate limited to ~100 req/min (free tier) | A 30-post harvest with comment expansion could hit 60-90 requests | PRAW handles rate limiting automatically; batch wisely |
| `replace_more()` costs 1 request per expansion | Deep comment threads (500+ comments) may need many requests | Set `limit=10` on `replace_more()` to cap API calls per thread; we only need top-level pain quotes, not every nested reply |
| Score fuzzing | Reddit fuzzes exact upvote/downvote counts | Same fuzzing applies to the browser view, so no difference |
| No access to "hidden" scores (new comments <2hrs) | Some fresh comments show `[score hidden]` | Same in browser. Not a real limitation for our use case (we want established, upvoted pain quotes) |
| Login-gated content (quarantined/NSFW subs) | API script app uses your account, so same access as logged-in browser | Actually equivalent or better since the API handles auth tokens automatically |

### Verdict

The Reddit API provides **everything** the Playwright scraping extracts, with additional structured data (subscriber counts, full comment trees, precise Unix timestamps) and without the fragility of DOM selectors that break when Reddit ships frontend updates.

---

## B. Authentication Setup

### Step-by-Step: Register a Reddit "Script" App

1. **Log in** to Reddit with the account you want to use for API access
2. **Navigate** to https://www.reddit.com/prefs/apps
3. **Click** "create another app..." at the bottom
4. **Fill out the form**:
   - **Name**: `reddit-harvest-bot` (or similar)
   - **App type**: Select **"script"** (this is for personal-use automation running on hardware you control)
   - **Description**: "Pain quote harvester for content research" (optional)
   - **About URL**: Leave blank
   - **Redirect URI**: `http://localhost:8080` (required field but unused for script apps)
5. **Click** "create app"
6. **Record your credentials**:
   - **client_id**: The string under the app name (looks like `abc123XYZ`)
   - **client_secret**: The string labeled "secret"
   - **username**: Your Reddit username
   - **password**: Your Reddit password
   - **user_agent**: `reddit-harvest:v1.0.0 (by /u/YOUR_USERNAME)`

### Store Credentials

Create a `.env` file (NOT committed to git) or use a secrets manager:

```env
REDDIT_CLIENT_ID=abc123XYZ
REDDIT_CLIENT_SECRET=your_secret_here
REDDIT_USERNAME=your_username
REDDIT_PASSWORD=your_password
REDDIT_USER_AGENT=reddit-harvest:v1.0.0 (by /u/your_username)
```

### Authentication Flow (Script App)

The "script" app type uses the **OAuth2 Password Grant** flow:
1. Your app sends `client_id`, `client_secret`, `username`, `password` to `https://www.reddit.com/api/v1/access_token`
2. Reddit returns a bearer token valid for 1 hour
3. All subsequent API calls use `https://oauth.reddit.com/` with the bearer token
4. PRAW handles this entire flow automatically — you just pass credentials to the constructor

### Important: Free Tier Qualification

Our use case qualifies for the **free tier** because:
- Non-commercial (personal content research)
- Read-only (no posting, voting, or moderation)
- Low volume (<100 req/min, well under 10K/month for typical harvests)
- Not monetizing Reddit data directly

If the product this feeds (Skool community, decision kits) grows to commercial scale, Reddit's terms may require reassessment. For now, free tier is appropriate.

---

## C. Implementation Approach

### Three Options Evaluated

#### Option 1: Python script invoked via Bash
- Write a standalone Python script using PRAW
- SKILL.md calls it via `Bash` tool: `python3 /path/to/harvest.py --topic "k8s pain" --subreddits "devops,kubernetes"`
- Script outputs JSON to stdout or a file, skill consumes it

**Pros**: Simple, self-contained, easy to test independently
**Cons**: Requires Python + PRAW installed in the agent's environment; two-step flow (run script, then process output)

#### Option 2: Reddit MCP server
- Install an existing Reddit MCP server (e.g., `GeLi2001/reddit-mcp` or `Pranay-A17/reddit-mcp-server`)
- SKILL.md uses MCP tools like `search_reddit_posts`, `get_post_comments` instead of Playwright tools
- Swap tool calls 1:1 in the skill workflow

**Pros**: Native MCP integration; tools available to any agent with the MCP configured; matches existing skill architecture (swap `browser_navigate` for `search_reddit_posts`); can be used by other skills too
**Cons**: Dependency on third-party MCP server maintenance; may need customization for our specific search patterns; another server process to manage

#### Option 3: Modify SKILL.md to call Reddit API directly via Bash/curl
- Use `Bash` tool with `curl` commands to call Reddit API endpoints
- Parse JSON responses in the skill logic
- No Python dependency, no MCP server

**Pros**: Zero dependencies beyond curl and jq
**Cons**: Extremely verbose; manual OAuth token management; painful JSON parsing in bash; no automatic rate-limit handling; error-prone

### Recommended: Option 2 (Reddit MCP Server) with Option 1 as Fallback

**Primary recommendation: Install a Reddit MCP server.**

Justification:
1. **Architecture alignment**: The current skill already works through MCP tools (Playwright). Swapping Playwright tools for Reddit API tools is a clean 1:1 replacement that preserves the skill's structure.
2. **Reusability**: Other skills (wild-scan, content-planner) could also benefit from Reddit API access through the same MCP server.
3. **Agent compatibility**: A Telegram-triggered agent just needs the MCP server in its config. No Python environment to manage, no script paths to resolve.
4. **Rate limiting**: Existing MCP servers like `GeLi2001/reddit-mcp` and `Pranay-A17/reddit-mcp-server` handle rate limiting internally.
5. **Maintainability**: If the MCP server breaks, swap it for another one (there are 5+ implementations). If we wrote our own script, we own all the maintenance.

**Fallback**: If no existing MCP server meets our needs (missing search-within-subreddit, missing comment expansion), write a minimal Python PRAW script (Option 1) and wrap it as a custom MCP server using `fastmcp`. This gives us full control while keeping the MCP interface.

### Which MCP Server?

| Server | Search in Subreddit | Get Comments | Comment Depth | Auth | Language |
|--------|--------------------|--------------|----|------|----------|
| `GeLi2001/reddit-mcp` | Yes (`search_reddit_posts`) | Yes (post details) | Unknown | OAuth (client_id/secret) | Python (FastMCP) |
| `Pranay-A17/reddit-mcp-server` | Yes | Yes | Unknown | OAuth | Python |
| `Hawstein/mcp-server-reddit` | No (only hot/top/new/rising) | Yes (`get_post_comments`) | Unknown | Uses `redditwarp` (public API) | Python |

**Best candidate: `GeLi2001/reddit-mcp`** because:
- Uses Reddit's official OAuth API (not public JSON hack)
- Read-only by design (matches our use case)
- Built with FastMCP (modern, easy to extend)
- Has both subreddit search AND post detail retrieval

If it lacks specific features (e.g., `replace_more` for deep comment expansion, `time_filter` on search), we can fork and add them — it's FastMCP, which means adding a tool is ~20 lines of Python.

---

## D. Data Mapping: Playwright DOM Fields to Reddit API Fields

### Search Results (Phase 1)

| Field (current extraction) | Playwright Source | API Equivalent (PRAW) | API Endpoint |
|---|---|---|---|
| `title` | `a[id*="post-title"]` text | `submission.title` | `GET /r/{sub}/search?q=...` |
| `author` | `a[href*="/user/"]` text | `submission.author.name` | (same) |
| `url` | `a[href*="/comments/"]` href | `f"https://reddit.com{submission.permalink}"` | (same) |
| `upvotes` | `faceplate-number[pretty]` attr | `submission.score` | (same) |
| `comments` | Comment count from DOM | `submission.num_comments` | (same) |
| `timestamp` | `<time>` datetime attr | `submission.created_utc` (Unix timestamp) | (same) |
| `subreddit` | `a[href*="/r/"]` text | `submission.subreddit.display_name` | (same) |
| `postId` | Regex from URL `/comments/([a-z0-9]+)/` | `submission.id` | (same) |

### Post Metadata (Phase 3)

| Field (current extraction) | Playwright Source | API Equivalent (PRAW) | API Endpoint |
|---|---|---|---|
| `title` | `h1` or `[slot="title"]` text | `submission.title` | `GET /comments/{id}` |
| `author` | Post author link | `submission.author.name` | (same) |
| `body` | `[id*="post-rtjson-content"]` text | `submission.selftext` | (same) |
| `upvotes` | `faceplate-number[pretty]` attr | `submission.score` | (same) |
| `commentCount` | Comment count element | `submission.num_comments` | (same) |
| `timestamp` | `<time>` datetime attr | `submission.created_utc` | (same) |
| `subreddit` | Subreddit link text | `submission.subreddit.display_name` | (same) |
| `url` | `window.location.href` | `f"https://reddit.com{submission.permalink}"` | (same) |

### Comments (Phase 3)

| Field (current extraction) | Playwright Source | API Equivalent (PRAW) | API Endpoint |
|---|---|---|---|
| `text` | `[id*="comment-content"]` text | `comment.body` | `submission.comments` |
| `author` | `el.getAttribute('author')` | `comment.author.name` | (same) |
| `depth` | `el.getAttribute('depth')` | `comment.depth` | (same) |
| `upvotes` | `faceplate-number[pretty]` attr | `comment.score` | (same) |
| `timestamp` | `<time>` datetime attr | `comment.created_utc` (Unix timestamp) | (same) |
| `permalink` | Constructed from `thingid` | `f"https://reddit.com{comment.permalink}"` | (same) |
| `replyCount` | Count of `:scope > shreddit-comment` | `len(comment.replies)` (after `replace_more`) | (same) |

### Additional Fields Available via API (Not Currently Extracted)

| New Field | Source | Use Case |
|---|---|---|
| `submission.subreddit.subscribers` | `GET /r/{sub}/about` | Automatic niche subreddit detection for engagement score adjustment |
| `comment.is_submitter` | Comment object | Flag if commenter is also OP (may indicate different pain context) |
| `submission.upvote_ratio` | Submission object | Controversial posts (low ratio, high score) may indicate divisive pain |
| `submission.link_flair_text` | Submission object | Filter by flair (e.g., "Rant", "Help", "Question") for better signal |
| `comment.controversiality` | Comment object | Controversial comments may indicate disputed pain points |

---

## E. Rate Limits and Constraints

### Free Tier Limits

| Metric | Limit | Source |
|---|---|---|
| Requests per minute (OAuth) | 100 | Reddit Data API Wiki |
| Monthly total (free tier) | ~10,000 (reported) | Various; not officially documented hard cap |
| Token lifetime | 1 hour | Reddit OAuth2 docs |
| Rate limit headers | `X-Ratelimit-Remaining`, `X-Ratelimit-Reset` | Response headers |

### Request Budget for a Typical Harvest

A typical reddit-harvest session (3 subreddits, 10 posts each):

| Operation | Requests | Calculation |
|---|---|---|
| Subreddit search (4 query categories x 3 subs) | 12 | 1 request per search |
| Subreddit info (for subscriber count) | 3 | 1 per subreddit |
| Post detail + comments (30 posts) | 30 | 1 request per post |
| Comment expansion (`replace_more`, limit=10 per post) | up to 300 | 10 expansions x 30 posts (worst case) |
| **Total (worst case)** | **~345** | |
| **Total (typical, limit=3 per post)** | **~135** | |

At 100 requests/minute, a typical harvest completes in **~2 minutes** of API time (vs. 15-25 minutes of browser time with Playwright). Even the worst case finishes in under 4 minutes.

### PRAW's Built-in Rate Limiting

PRAW automatically:
- Respects `X-Ratelimit-Remaining` headers
- Sleeps when approaching the limit
- Refreshes OAuth tokens before expiry
- Retries on 5xx errors with exponential backoff

This means we do NOT need to implement any rate-limiting logic ourselves.

### Monthly Budget Considerations

If the monthly cap is truly ~10,000 requests:
- One harvest session: ~135-345 requests
- That allows **29-74 harvest sessions per month** on the free tier
- More than enough for "one harvest per topic per day" usage pattern

### Constraints to Watch

1. **Reddit may tighten free tier further** (they've been doing this since 2023). Monitor r/redditdev for announcements.
2. **Commercial use requires pre-approval** if the harvested data feeds a paid product. Our current use (content research) is non-commercial, but if Skool revenue grows, reassess.
3. **Sensitive data restrictions**: User interaction histories and moderation data require explicit Reddit permission. We don't need either — we only read public posts and comments.

---

## F. Recommended Approach

### Phase 1: Install and Configure `GeLi2001/reddit-mcp` (Day 1)

1. Register a Reddit "script" app at reddit.com/prefs/apps
2. Store credentials in `.env` or agent secrets
3. Clone and install `GeLi2001/reddit-mcp`:
   ```bash
   pip install reddit-mcp  # or clone and install from source
   ```
4. Add to Claude Code MCP config (`~/.claude/agent.json` or equivalent):
   ```json
   {
     "mcpServers": {
       "reddit": {
         "command": "python",
         "args": ["-m", "reddit_mcp"],
         "env": {
           "REDDIT_CLIENT_ID": "...",
           "REDDIT_CLIENT_SECRET": "...",
           "REDDIT_USER_AGENT": "reddit-harvest:v1.0.0 (by /u/...)"
         }
       }
     }
   }
   ```
5. Verify: call `search_reddit_posts` with a test query and confirm structured data returns

### Phase 2: Evaluate and Extend MCP Server (Day 1-2)

Test whether the MCP server provides:
- [ ] Subreddit search with `sort` and `time_filter` parameters
- [ ] Full comment tree retrieval with score and depth
- [ ] Comment expansion (equivalent to `replace_more`)
- [ ] Subreddit info (subscriber count for niche adjustment)

If any are missing, fork the server and add them using FastMCP. Each missing tool is ~20-30 lines of Python wrapping a PRAW call.

### Phase 3: Update SKILL.md (Day 2-3)

Create a new version of SKILL.md that:
1. Removes all Playwright MCP tool references
2. Replaces with Reddit MCP tool calls
3. Removes "Browser Session Requirement" / login verification
4. Removes scroll-and-load logic (API returns full data)
5. Removes selectors.md dependency (no DOM parsing)
6. Updates rate limiting section (API limits, not browser pacing)
7. Keeps scoring, export, and quality checklist unchanged (these are data-independent)

Key SKILL.md changes:

| Current (Playwright) | New (Reddit API) |
|---|---|
| `browser_navigate` to search URL | `search_reddit_posts(subreddit, query, sort, time_filter)` |
| `browser_evaluate` with search extraction JS | Parse structured JSON response directly |
| `browser_navigate` to thread URL | `get_post_details(post_id)` |
| `browser_run_code` scroll script | Not needed (API returns full comment tree) |
| `browser_evaluate` post metadata extraction | Parse submission fields from API response |
| `browser_evaluate` comment extraction | Parse comment tree from API response |
| `browser_evaluate` same-here detection | Same regex logic applied to `comment.body` in response data |
| `browser_evaluate` pain-category matcher | Same keyword matching on API text data |
| `browser_evaluate` login check | Not needed (OAuth handles auth) |

### Phase 4: Update selectors.md (Day 3)

The current `selectors.md` contains Playwright DOM extraction JavaScript. For the API version:
- **Keep** the pain-category keyword matcher (logic is platform-independent, just rewrite as instructions instead of JS)
- **Keep** the same-here agreement patterns (same regex patterns, applied to API text)
- **Remove** all DOM selectors and `querySelectorAll` scripts
- **Add** API field reference table (this document's Section D)

Alternatively, rename to `api-field-mapping.md` and replace the content entirely.

### Phase 5: Test End-to-End (Day 3-4)

1. Run a full harvest for "Kubernetes deployment pain" across r/devops, r/kubernetes, r/terraform
2. Verify output matches wild-scan format (JSON schema validation)
3. Compare quote yield and quality to a previous Playwright harvest
4. Confirm Markdown file renders correctly in Obsidian
5. Test from a headless agent context (no browser, no human interaction)

### Phase 6: Telegram Agent Integration (Day 4+)

Once the API-based harvest works headlessly:
1. Configure the Telegram agent to include the Reddit MCP server
2. Add trigger: `/reddit-harvest [topic]` in Telegram
3. Agent runs the skill, writes output to vault, reports summary back via Telegram

---

## Appendix: Library/Tool Reference

### PRAW (Python Reddit API Wrapper)
- **Version**: 7.7.1 stable / 7.8.2.dev0 latest
- **PyPI**: https://pypi.org/project/praw/
- **Docs**: https://praw.readthedocs.io/
- **Python**: 3.9+
- **Status**: Actively maintained

### Async PRAW
- **Version**: 7.8.1
- **PyPI**: https://pypi.org/project/asyncpraw/
- **Docs**: https://asyncpraw.readthedocs.io/
- **Python**: 3.9+
- **Status**: Actively maintained, last release Aug 2025

### Reddit MCP Servers (Evaluated)

| Repository | Stars | Key Tools | Auth | Notes |
|---|---|---|---|---|
| [GeLi2001/reddit-mcp](https://github.com/GeLi2001/reddit-mcp) | - | search_reddit_posts, search_reddit_all, subreddit info, post details | OAuth (official API) | **Recommended** - FastMCP, read-only, official API |
| [Pranay-A17/reddit-mcp-server](https://github.com/Pranay-A17/reddit-mcp-server) | - | Search posts, comments, subreddits, user profiles | OAuth | Part of Klavis AI collection |
| [Hawstein/mcp-server-reddit](https://github.com/Hawstein/mcp-server-reddit) | - | frontpage, hot/new/top/rising posts, post content, comments | redditwarp (public API) | No subreddit search tool |
| [adhikasp/mcp-reddit](https://github.com/adhikasp/mcp-reddit) | - | Fetch and analyze Reddit content | Unknown | Less documented |

### Reddit API Endpoints Used

| Endpoint | Method | Purpose |
|---|---|---|
| `POST /api/v1/access_token` | POST | OAuth2 token exchange |
| `GET /r/{subreddit}/search` | GET | Search posts within a subreddit (params: `q`, `sort`, `t`, `limit`) |
| `GET /r/{subreddit}/about` | GET | Subreddit metadata (subscriber count) |
| `GET /comments/{article_id}` | GET | Post details + full comment tree |
| `GET /r/{subreddit}/top` | GET | Top posts (params: `t`, `limit`) |

---

## Sources

- [Reddit Data API Wiki](https://support.reddithelp.com/hc/en-us/articles/16160319875092-Reddit-Data-API-Wiki)
- [Reddit API Rate Limits 2026 Guide](https://painonsocial.com/blog/reddit-api-rate-limits-guide)
- [Reddit API Pricing 2026](https://autogpt.net/how-reddit-api-pricing-works/)
- [Reddit's 2025 API Crackdown](https://replydaddy.com/blog/reddit-api-pre-approval-2025-personal-projects-crackdown)
- [Reddit API Cost 2025](https://rankvise.com/blog/reddit-api-cost-guide/)
- [How to Get Reddit API Key](https://data365.co/blog/how-to-get-reddit-api-key)
- [PRAW Documentation](https://praw.readthedocs.io/)
- [PRAW Quick Start](https://praw.readthedocs.io/en/latest/getting_started/quick_start.html)
- [PRAW Comment Extraction Tutorial](https://praw.readthedocs.io/en/stable/tutorials/comments.html)
- [PRAW Submission Model](https://praw.readthedocs.io/en/latest/code_overview/models/submission.html)
- [PRAW Subreddit Model](https://praw.readthedocs.io/en/stable/code_overview/models/subreddit.html)
- [Async PRAW on PyPI](https://pypi.org/project/asyncpraw/)
- [GeLi2001/reddit-mcp](https://github.com/GeLi2001/reddit-mcp)
- [Hawstein/mcp-server-reddit](https://github.com/Hawstein/mcp-server-reddit)
- [Pranay-A17/reddit-mcp-server](https://github.com/Pranay-A17/reddit-mcp-server)
- [Reddit OAuth2 Wiki](https://github.com/reddit-archive/reddit/wiki/oauth2)
- [Reddit JSON API Structure](https://github.com/reddit-archive/reddit/wiki/json)
- [Reddit API Endpoints List](https://painonsocial.com/blog/reddit-api-endpoints-list)
- [Reddit API Limits - Data365](https://data365.co/blog/reddit-api-limits)
