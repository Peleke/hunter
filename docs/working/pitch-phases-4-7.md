# Pitch Phases 4-7: The DevOps Decision Kit

---

## Phase 4: Email Sequence

### Email 1: Your DevOps Decision Kit is ready -- open the Quick Start first (Day 0)

**Subject**: Your DevOps Decision Kit is ready -- open the Quick Start first

---

Hey,

Your Decision Kit just landed. Here is your download link:

**[Download The DevOps Decision Kit]({download_link})**

Here is how to get value from this in the next 10 minutes:

1. Open the **Quick Start: Your First Decision in 15 Minutes** (Section 1).
2. Answer the four questions: team size, monthly cloud budget, primary cloud provider, existing codebase language.
3. Follow the arrow to your recommendation.

By the time you finish your coffee, you will have a specific, defensible answer to "what should we use?" for at least one infrastructure layer. That is the whole point -- you stop Googling and start deciding.

The kit has 5 decision trees covering IaC tools, container orchestration, CI/CD pipelines, monitoring stacks, and an AI-generated infrastructure review checklist. You do not need to read them all at once. Start with whatever decision is staring you down this week.

Over the next 7 days, I am going to send you a few emails:

- Day 2: A specific technique from the kit you can use immediately
- Day 4: A production war story that explains why one of the sections exists
- Day 6: An honest answer to the biggest doubt you might have about this approach
- Day 7: What I am building next for engineers like you

Each email has one idea. No sales pitches, no filler.

Go open that Quick Start.

-- Peleke

P.S. The companion GitHub repo is here: **[{repo_link}]**. It has deployable Terraform, Docker, and CI/CD templates that match each decision path in the kit. Clone it alongside the guide.

---

### Email 2: Try this the next time someone asks "should we use Kubernetes?" (Day 2)

**Subject**: Try this the next time someone asks "should we use Kubernetes?"

---

Hey,

Here is something you can do in the next 5 minutes that will save you a week of analysis paralysis.

Open Section 3 of the Decision Kit -- the Container Orchestration Decision. Go to the first question in the tree:

**"Do you actually need Kubernetes?"**

Here is the condensed version:

1. **Check your workload type.** If it is a single web application with fewer than 10 services, you almost certainly do not need Kubernetes. ECS Fargate or plain Docker Compose on a single host will do.

2. **Check your team's Kubernetes experience.** If nobody on your team has operated a Kubernetes cluster in production before, EKS will cost you months of learning curve plus $2,400/year just for the control plane -- before you run a single pod.

3. **Check your budget.** If your monthly cloud spend is under $5,000, Kubernetes overhead (both operational and financial) will eat a disproportionate share of it.

4. **Apply the decision rule.** If you answered "single app," "no experience," or "under $5K/month" to any of those -- use ECS Fargate or Lambda. The kit explains exactly when each one fits.

5. **Use the template.** The companion repo has a deployable ECS Fargate configuration with an annotated README explaining every resource. Clone it, fill in your values, and run `terraform plan`.

After doing this, you will have a specific container orchestration recommendation for your team that you can explain to your boss in plain language. Not "I read that Kubernetes is industry standard" but "Given our team size, budget, and workload, ECS Fargate is the right call because [reasons]."

That is the difference between information and a decision.

Try it this week and hit reply to tell me how it went. I read every response.

-- Peleke

---

### Email 3: The deploy that mass-deleted production records at Qortex (Day 4)

**Subject**: The deploy that mass-deleted production records at Qortex

---

Hey,

I want to tell you about a production decision I got wrong and what it taught me about infrastructure choices.

At Qortex, we were building a complex backend system with multiple services, real-time processing, and a data layer that had to handle significant throughput. Early on, I made the classic mistake: I cargo-culted an architecture pattern from a company 50x our size.

I looked at how large-scale systems handled similar problems, read the blog posts, watched the conference talks, and implemented a version of their infrastructure. It worked -- in staging. In production, the complexity we imported created debugging nightmares. When something went wrong, nobody on the team (including me) could trace the issue through all the layers we had introduced. We had infrastructure we could not reason about.

The lesson was painful and specific: **the right infrastructure decision is not "what does Netflix use?" It is "what can my team operate, debug, and maintain at 2am when something breaks?"**

That experience is why Section 2 (the IaC Tool Decision) and Section 3 (Container Orchestration) both start with team size and operational capacity -- not with feature comparisons. The first question is never "which tool is better?" The first question is "which tool can your team actually run?"

It is also why every decision tree in the kit includes a "When to Switch" section. The goal is to make a reversible decision fast, not a perfect decision never. We eventually right-sized our architecture at Qortex, but only after burning weeks we did not have.

This is the stuff that comparison blog posts never tell you. They compare features. They do not tell you that choosing the "better" tool can be the wrong decision if your team cannot operate it.

That thinking is baked into every section of the Decision Kit.

-- Peleke

---

### Email 4: "Can't I just ask ChatGPT which tool to use?" (Day 6)

**Subject**: "Can't I just ask ChatGPT which tool to use?"

---

Hey,

If you have been sitting on the Decision Kit without using it, there is a good chance this thought crossed your mind:

*"Why did I pay for this? I can ask ChatGPT."*

Fair question. Here is the honest answer.

Ask ChatGPT: "Should I use Terraform or Pulumi?" You will get something like: "Terraform is great for multi-cloud declarative infrastructure. Pulumi is great if your team prefers general-purpose languages. It depends on your use case."

That is a comparison. It is not a decision.

Now open the IaC Decision Tree in the kit (Section 2). It asks you three things: your team size (solo, small, enterprise), your team's existing programming language skills (Python? TypeScript? Neither?), and whether you need multi-cloud.

If you are a solo engineer with a Python background deploying to AWS only, the tree tells you: **use Pulumi with Python. Here is why. Here is the starter template. Here is when to reconsider.**

ChatGPT gave you a menu. The Decision Kit gave you an order.

The difference matters because the thing that keeps engineers stuck is not lack of information -- you have been reading comparison posts for weeks. The thing that keeps you stuck is that nobody will tell you what to do given YOUR specific situation. AI gives you options. A decision framework that accounts for team size, budget, existing skills, and cloud provider gives you an answer.

That said, AI is a phenomenal tool when you know what you are building. Section 6 of the kit (the "Should I Use AI for This?" Checklist) is specifically designed for that: once you have DECIDED what to build, here is how to use Copilot to build it safely.

The framework decides. AI executes. That is the workflow.

Go open the section that matches whatever decision you are facing this week. Answer the questions. Follow the tree. You will have your answer in 15 minutes.

-- Peleke

P.S. If you genuinely tried the framework and it did not help you make a decision, reply and tell me where it broke down. I will either point you to the right section or improve the kit based on your feedback. The 30-day guarantee is real.

---

### Email 5: Building something for engineers who make infrastructure decisions alone (Day 7)

**Subject**: Building something for engineers who make infrastructure decisions alone

---

Hey,

One thing I have heard from Decision Kit buyers (and from years working in this space) keeps coming up:

*"I am the only DevOps/infrastructure person on my team. I have nobody to sanity-check my decisions against."*

If that is you, you know the feeling. You run through the decision tree, you pick an answer, and then the doubt creeps in: "But what if I'm wrong? There's nobody here to tell me."

Research backs this up. Solo DevOps engineers report that the hardest part is not the technical work -- it is having no peers to brainstorm with. One engineer put it this way: "Multiple areas of responsibility meant that no matter how good you became at one thing, there were other areas that you always felt embarrassingly ignorant about."

I am building a community specifically for engineers in this situation. Not a course platform. Not a certification mill. A place where mid-level DevOps and infrastructure engineers can post a decision, get feedback from practitioners who have been there, and stop making every call alone.

What it will look like:
- Weekly infrastructure decision teardowns (real anonymized decisions from members, discussed as a group)
- A "decision review" channel where you post your tree output and get peer feedback
- Live office hours where you can bring your specific situation

It does not exist yet. I am looking for founding members -- people who would actually show up and participate, not just lurk.

If this sounds like something you would use, reply to this email with one sentence: **what infrastructure decision are you facing right now that you wish you could get a second opinion on?**

That is it. One sentence. I will use the responses to design the community around real problems, not hypothetical ones.

-- Peleke

P.S. Whether or not the community happens, the Decision Kit is yours forever. This is not a pitch to buy something else. It is me asking if the thing I want to build next is something you actually need.

---

## Phase 5: Launch Checklist

### Pre-Launch Setup (Days -7 to -1)

| Day | Task | Tool | Est. Time |
|-----|------|------|-----------|
| -7 | Set up payment processing: create product page for "The DevOps Decision Kit" at $29. Upload deliverable (PDF/zip). Configure instant delivery email. Set up a coupon code for launch pricing if testing $19 variant. | Gumroad (or LemonSqueezy) | 1 hour |
| -7 | Set up email tool: create account, configure sender domain/DKIM, create "Decision Kit Buyers" segment. | ConvertKit (or Buttondown) | 1 hour |
| -6 | Import email sequence: create 5-email automation triggered by purchase tag. Paste Email 1-5 from Phase 4. Set Day 0/2/4/6/7 delays. | ConvertKit | 1.5 hours |
| -6 | Set up GitHub companion repo: create public repo `devops-decision-kit`. Build directory structure from Phase 3a. Push free-tier content (2-3 decision trees as README content). Add deployable templates for free sections. Include "Get the Full Kit" CTA link in README. | GitHub + VS Code | 2 hours |
| -5 | Create landing page: paste Phase 1 copy (headline, subheadline, problem, solution, what's inside, pricing, FAQ, CTA). Add Gumroad buy button embed. Add email capture form for lead magnet ("Get the IaC Decision Tree free -- full kit is $29"). | Carrd ($19/yr plan) | 2 hours |
| -4 | Create lead magnet landing page: separate page for free IaC decision tree PDF. Email gate via ConvertKit form. Tag subscribers as "lead-magnet" for future segmentation. | Carrd + ConvertKit | 1 hour |
| -4 | Prepare all launch posts: finalize Reddit post (r/devops), LinkedIn post, Twitter/X thread from Phase 2. Save as drafts. Proofread each one. Check all links. | Google Docs or text editor | 1.5 hours |
| -3 | Configure UTM parameters: create tagged links for each channel (utm_source=reddit, utm_source=linkedin, utm_source=twitter, utm_source=github, utm_source=devto). Add to each launch post and README. | Manual + URL builder | 30 min |
| -2 | Full funnel test: visit landing page as new user. Click buy. Complete test purchase on Gumroad. Verify product delivery email arrives. Verify ConvertKit automation triggers. Verify Email 1 sends. Check all links in Email 1. | All tools | 1 hour |
| -2 | Test lead magnet funnel: visit lead magnet page. Submit email. Verify PDF delivery. Verify ConvertKit tag. Verify no overlap with buyer sequence. | Carrd + ConvertKit | 30 min |
| -1 | Final content review: re-read all 5 emails, all 3 launch posts, landing page copy, and README. Fix typos. Verify all links. | Manual | 1 hour |
| -1 | Pre-load community engagement: spend 30 minutes in r/devops, r/terraform, r/kubernetes genuinely answering questions (no links, no self-promo). Build recent post history so launch post does not come from a dormant account. | Reddit | 30 min |

**Total pre-launch time**: ~13.5 hours across 7 days

### Launch Day (Day 0)

| Time (EST) | Action | Platform | Notes |
|------------|--------|----------|-------|
| 8:00 AM | **Final checks.** Visit landing page -- all links work? Click buy button -- Gumroad loads? Check ConvertKit automation -- active? Check GitHub repo -- all files present, README renders correctly? Verify UTM links resolve. | All | Do not skip this. A broken buy link on launch day kills momentum. |
| 9:00 AM | **Publish primary launch post on r/devops.** Post the value-first piece: "I built decision trees for the 5 infrastructure choices that paralyze most DevOps engineers..." Give 80% of the IaC decision tree free. Link to GitHub repo. Mention $29 kit at the end. | Reddit r/devops | Follow r/devops self-promo rules strictly. No more than 10% self-promotional posts in your history. The post must provide genuine standalone value. |
| 10:00 AM | **Publish LinkedIn post.** Shorter format, story arc: problem (tool paralysis) -> insight (decision trees > comparisons) -> framework preview -> CTA. | LinkedIn | Use 3-5 hashtags: #DevOps #InfrastructureAsCode #Terraform #PlatformEngineering #SRE |
| 11:00 AM | **Publish Twitter/X thread.** 5-8 tweet thread. Hook tweet, framework tweets, credibility tweet, CTA tweet. | Twitter/X | Pin the thread. Like and RT your own first tweet from any secondary account. |
| 12:00 PM | **First engagement pass.** Check Reddit for comments -- respond to EVERY comment within 1 hour. Check LinkedIn for reactions and comments. Check Twitter for replies and quote tweets. Engage genuinely with everyone. | All | The first 2-3 hours of Reddit comments determine whether the post rises or dies. Be present. |
| 2:00 PM | **Share in DevOps communities.** Post in DevOps Fans Discord (if appropriate channel exists). Share in SweetOps Slack. Only share if you are an existing, active member. Do NOT join and immediately drop a link. | Discord / Slack | If you are not already a member of these communities, skip this step. Cold-dropping links will backfire. |
| 4:00 PM | **Second engagement pass.** Respond to all new comments. Note feedback themes: what resonates? What objections come up? What questions do people ask? | All | Start a running doc of feedback themes. This feeds Week 1 iteration. |
| 6:00 PM | **Cross-post to r/terraform.** If the r/devops post gained traction (10+ upvotes), post a focused version of the IaC decision tree in r/terraform. Different angle, same value-first approach. | Reddit r/terraform | This is a separate, tailored post -- not a cross-post link. Write it for the r/terraform audience specifically. |
| 8:00 PM | **End-of-day engagement pass.** Respond to everything. Check Gumroad dashboard: how many views? How many sales? Check GitHub repo: any stars or forks? Check ConvertKit: any email signups? | All | Record Day 0 numbers: landing page views, sales, repo stars, email signups. This is your baseline. |

### Week 1 Follow-Up (Days 1-7)

| Day | Actions |
|-----|---------|
| **Day 1** | (1) Respond to all overnight comments on Reddit, LinkedIn, Twitter. (2) Check Gumroad: sales count, traffic sources. Check GitHub: stars, forks, issues. (3) Send a personal thank-you DM/email to anyone who shared or publicly endorsed the post. (4) Note which platform got the most engagement -- this is your primary channel going forward. |
| **Day 2** | (1) Post a follow-up comment on the r/devops thread: "A few people asked about [specific question]. Here is the answer..." This re-surfaces the post. Do NOT make a new post. (2) If LinkedIn post got traction (50+ reactions), write a short follow-up post expanding on the most-commented point. (3) Publish the Decision Kit GitHub repo link on DEV.to as a short "resources" post if you have a DEV.to account. |
| **Day 3** | (1) Cross-post to a secondary subreddit if primary channel worked. Options: r/kubernetes (container orchestration angle), r/sysadmin (for the "Overwhelmed Olga" persona), r/ExperiencedDevs (for the "Promoted Pat" persona). (2) Engage in 3-5 existing threads on r/devops with genuine, helpful answers (no links). Build community presence. |
| **Day 4** | (1) Write and publish a DEV.to article: a deep dive into ONE decision from the kit. Example: "How to decide between ECS and EKS in 2026 (a decision tree for your specific situation)." Standalone value. Links to GitHub repo and kit at the end. (2) Share the DEV.to article on LinkedIn and Twitter. |
| **Day 5** | (1) Review sales data. How many units? From which channels (check UTM params in Gumroad analytics)? Which post drove the most traffic? (2) Review GitHub repo analytics: unique visitors, clones, referral sources. (3) Review ConvertKit: how many email signups from the lead magnet page? (4) Compile Week 1 metrics into a simple spreadsheet. |
| **Day 6** | (1) Double down on what is working. If Reddit drove sales, plan next week's r/devops value post. If GitHub is getting stars, improve the README and add another free decision tree. If DEV.to article got views, plan the next article. (2) Pause what is not working. If Twitter got zero engagement, stop investing time there for now. |
| **Day 7** | **Week 1 retrospective.** Compare results against kill criteria (Phase 7). Key questions: (1) Did we hit 3+ sales? (2) Did the launch post get meaningful engagement (20+ upvotes/comments on Reddit, or equivalent)? (3) Did the GitHub repo get any organic stars? (4) What feedback themes emerged? **Decision point**: continue as-is, iterate on positioning/channel, or trigger kill criteria review. Write a 1-paragraph summary and file it. |

### Ongoing Cadence (Weeks 2-4)

| Frequency | Action | Platform |
|-----------|--------|----------|
| 2x/week | Publish a value-first post about an infrastructure decision topic. Alternate between "quick tip" format (short, actionable) and "deep dive" format (full article). Every post links back to the GitHub repo. Mention the paid kit naturally when relevant. | r/devops + DEV.to |
| 3x/week | Short-form posts about infrastructure decision-making. LinkedIn-native format: short paragraphs, a specific insight, a question to drive comments. Build the "DevOps decision-making" brand positioning. | LinkedIn |
| 1x/week | Tweet thread or single tweet with a specific decision insight. Reference a real scenario. | Twitter/X |
| Daily (15 min) | Engage genuinely in r/devops, r/terraform, r/kubernetes threads. Answer questions. Be helpful. Do NOT link to the kit unless it is directly and obviously relevant to the question. | Reddit |
| Daily (10 min) | Participate in DevOps Discord/Slack communities. Same rule: be helpful first, visible second. | Discord / Slack |
| 1x/week | Update GitHub repo based on feedback. Add a new template, improve a README, fix an issue. Each update is a reason to mention the repo in a post. | GitHub |
| 1x/week | Send a value email to the subscriber list (if 20+ subscribers). One insight, one technique, one link to a new post or repo update. Short. | ConvertKit |
| 1x/week (Friday) | Metrics check. Update the tracking spreadsheet: weekly sales, cumulative sales, landing page visitors by source, email list size, GitHub stars. 15 minutes. | Gumroad + ConvertKit + GitHub analytics |

### Month 2-3 Growth Actions

**If Reddit is converting (most sales come from r/devops posts):**
- Increase posting cadence to 3x/week on r/devops, alternating between value posts and discussion-starting questions.
- Identify and post in adjacent subreddits: r/terraform, r/kubernetes, r/aws, r/sysadmin.
- Create a "best of" compilation post every 2 weeks linking back to your previous high-performing posts.
- Track which specific topics drive the most engagement and double down.

**If GitHub is driving traffic (repo stars growing, referral traffic to landing page):**
- Invest 2-3 hours/week improving the repo: add more templates, improve documentation, create GitHub Issues for community contributions.
- Target 500 stars by Day 90. If at 200+ stars by Day 60, you are on track.
- Submit the repo to DevOps curated lists and awesome-lists on GitHub.
- Write a "behind the scenes" DEV.to article about building the repo (meta-content that DevOps engineers love).

**If DEV.to articles are driving traffic:**
- Increase article cadence to 2x/week.
- Experiment with article formats: decision teardowns, "I was wrong about X," tool migration stories.
- Cross-post to Hashnode and personal blog for additional reach.
- Engage heavily in DEV.to comments on your own articles and others' articles.

**If email is converting (subscribers are opening, clicking, replying):**
- Create a second lead magnet (e.g., the AI Infrastructure Checklist as a standalone freebie).
- Add a second email capture point in the GitHub README.
- Consider a "mini-course" email sequence (5-day email course on one decision topic) as an additional funnel.

**If nothing is converting after 60 days:**
- Revisit kill criteria (Phase 7). Check: is traffic reaching the landing page (distribution problem) or arriving but not converting (positioning/pricing problem)?
- If distribution problem: try 2-3 guest posts on established DevOps blogs, reach out to DevOps newsletter curators (DevOps Weekly, TLDR DevOps), or create a YouTube video walkthrough of one decision tree.
- If positioning problem: test a new headline (Phase 6 variants), test the $19 price point, or reframe the value proposition based on feedback themes.
- If neither works by Day 90: trigger kill criteria.

---

## Phase 6: A/B Test Spec

### 6.1 Headline Variants

**Current headline (Control):** "Stop Googling 'Terraform vs Pulumi.' Here's What to Actually Use for Your Team, Your Budget, and Your Stack."

**Variant A (Pain-focused):** "You've Been Reading Comparison Blog Posts for 3 Weeks. You Still Haven't Decided. Here's Why."
- *Rationale*: Leans directly into the persona's #1 pain story -- the analysis paralysis loop of reading blog posts that compare tools without ever telling you what to choose. Uses the specific "3 weeks" timeframe from persona data ("Reads 47 comparison blog posts and still can't decide after weeks"). Targets the emotional state of frustration and self-recognition.

**Variant B (Outcome-focused):** "Make a Confident Infrastructure Decision in 15 Minutes -- Not 3 Weeks"
- *Rationale*: Leads with the transformation, not the pain. Emphasizes speed (15 minutes, from the Quick Start section) versus the current stuck state (3 weeks). Appeals to engineers who respond better to "here is what you get" than "here is what is wrong." Shorter, punchier, more landing-page-native.

**Variant C (Curiosity-focused):** "The 4-Question Framework That Replaces 47 Blog Posts When Choosing DevOps Tools"
- *Rationale*: Uses specific numbers (4 questions, 47 blog posts) to create curiosity. "4-question framework" implies simplicity and system. "Replaces 47 blog posts" quantifies the value and references the persona's exact behavior pattern. Engineers are drawn to systems and frameworks -- this headline signals "there is a system."

### 6.2 Price Variants

**Variant A: $19**
- *Rationale*: Below the psychological $20 threshold. Matches the bottom of "Copypaste Carlos" willingness-to-pay range ($19-29 for AI infrastructure review checklist). Maximizes unit volume and email list growth at the expense of revenue per sale. At $19, a casual buyer takes the risk. Good for the cold-start phase where email list growth matters more than revenue. Risk: signals "cheap" rather than "valuable" to the primary personas (Promoted Pat, Overwhelmed Olga) who earn $90-130K and expect to pay more for professional tools.
- *When to test*: If conversion rate at $29 is below 3% after 200+ landing page visitors.

**Variant B: $29 (Current)**
- *Rationale*: Floor of both primary persona WTP ranges (Promoted Pat: $49-79, Overwhelmed Olga: $29-49). Minimizes purchase friction at zero audience while maintaining "this is a professional tool" positioning. Matches KodeKloud's monthly AI plan pricing ($29/month), but as a one-time purchase -- making it feel like a deal. SPEND data confirms the $29-79 one-time purchase tier is unoccupied.
- *When to test*: This is the default launch price.

**Variant C: $49**
- *Rationale*: Mid-range of both persona WTP ranges. Signals higher value and positions the kit as a professional reference tool, not a casual purchase. At $49, fewer units are needed to hit revenue targets (6 sales = $294 vs 10 sales at $29 = $290). Matches the "DevOps Decision Guide" price recommended in the persona's Section 5 offer mapping. Risk: higher friction for cold traffic with no social proof. The operator has zero reviews, zero testimonials, and zero brand recognition. $49 from an unknown creator is a harder sell than $29.
- *When to test*: After accumulating 5+ positive testimonials/reviews from $29 buyers, OR after the GitHub repo reaches 500+ stars (social proof from a different channel).

### 6.3 Channel Priority

| Rank | Channel | Rationale | Test Budget (time, not money) |
|------|---------|-----------|-------------------------------|
| 1 | **Reddit (r/devops, r/terraform, r/kubernetes)** | 386K members on r/devops alone. The primary persona (Promoted Pat, Overwhelmed Olga) explicitly hangs out here. The value-first post format aligns with Reddit norms. Highest expected conversion rate for warm community traffic (3-5%). Reddit posts have a 24-48 hour engagement window but can drive sustained traffic if upvoted. | 3 hours/week: 1 value post (1.5 hrs writing + 1.5 hrs engaging in comments) |
| 2 | **GitHub repo** | Organic discovery engine. DevOps engineers live in GitHub. Stars and forks compound over time. The repo serves as a permanent, always-on lead generation channel. Lower immediate conversion but higher long-term value. No direct "posting" -- the investment is in repo quality. | 2 hours/week: improve README, add templates, respond to issues |
| 3 | **DEV.to** | Strong DevOps readership. Articles have a longer shelf life than social posts. Cross-posting to Hashnode and personal blog multiplies reach. Good for SEO (DEV.to articles rank well for technical queries). Lower conversion rate than Reddit but wider reach per article. | 2 hours/week: 1 article (1.5 hrs writing + 30 min promotion) |
| 4 | **LinkedIn** | Builds professional brand positioning. Lower direct conversion to sales but higher value for credibility and community building. The "DevOps decision-making" positioning differentiates from every other DevOps influencer talking about tools. | 1.5 hours/week: 3 short posts (30 min each) |
| 5 | **Twitter/X** | Lowest expected ROI for DevOps content. The DevOps community is more active on Reddit and LinkedIn. Twitter threads can go viral but the base rate is low. Worth maintaining as a secondary channel but not worth primary investment. | 45 min/week: 1 thread or 3 single tweets |

**Total weekly time investment**: ~9.25 hours/week across all channels

### 6.4 Metrics

| Metric | Target (Week 1) | Target (Month 1) | Measurement Method |
|--------|-----------------|-------------------|--------------------|
| Landing page visitors (total) | 200+ | 800+ | Gumroad analytics (if using Gumroad page) or Carrd analytics + UTM parameters |
| Landing page visitors by source | Track per-channel via UTM | Identify top 2 channels | UTM parameters in Gumroad/Carrd analytics |
| Landing page conversion rate (visitor -> purchase) | 3-5% on warm traffic, 1-2% on cold | 2-4% blended | Gumroad: sales / unique visitors |
| Email signup rate (visitor -> lead magnet email) | 5-10% | 5-10% | ConvertKit: signups / lead magnet page visitors |
| Reddit post engagement (r/devops launch post) | 30+ upvotes, 15+ comments | N/A (launch post only) | Reddit natively |
| DEV.to article views | N/A (no article Week 1) | 500+ per article | DEV.to dashboard |
| GitHub repo stars | 10-30 | 100+ | GitHub repo insights |
| Revenue (total) | $87-290 (3-10 units) | $435-870 (15-30 units) | Gumroad dashboard |
| Email list size (buyers + leads) | 10-30 | 50-100 | ConvertKit subscriber count |
| Email open rate (sequence emails) | 50%+ | 40%+ | ConvertKit email analytics |

### 6.5 Decision Thresholds

**Headline test:**
- *Method*: Sequential testing. Run the Control headline for 2 weeks (collect 150+ landing page visitors). Switch to Variant A for 2 weeks. Compare conversion rates.
- *Threshold*: If Variant A conversion rate is 1.5x or higher than Control over 150+ visitors each, switch permanently.
- *Action*: Adopt the winner. Test the next variant against the new winner after another 2 weeks.

**Price test:**
- *Method*: Sequential testing. Run $29 for the first 30 days. If conversion rate is below 3% after 200+ visitors, switch to $19 for 2 weeks. If conversion rate at $29 is above 5%, test $49.
- *Threshold*: Optimize for total revenue, not conversion rate alone. $19 at 6% conversion = $1.14/visitor. $29 at 4% conversion = $1.16/visitor. $49 at 2.5% conversion = $1.225/visitor. The price that maximizes revenue-per-visitor wins.
- *Action*: Lock in the winning price. Do not change again for at least 60 days.

**Channel test:**
- *Method*: Track UTM-tagged revenue per channel per week. Allocate time proportionally to revenue generated.
- *Threshold*: After 4 weeks, any channel generating less than 5% of total revenue with more than 15% of total time investment gets deprioritized.
- *Action*: Reallocate time from underperforming channels to top 2 performers. Do not kill any channel entirely until Month 2 -- some channels (GitHub, DEV.to) have longer payoff curves.

**Email sequence test:**
- *Method*: Monitor open rates and click rates per email. If any email has <25% open rate, rewrite the subject line. If any email has <5% click rate, rewrite the CTA.
- *Threshold*: A subject line rewrite that improves open rate by 10+ percentage points is a winner.
- *Action*: Update the automation in ConvertKit. Keep the old subject line in a "tested variants" doc for reference.

---

## Phase 7: Kill Criteria

### 7.1 Week 1 Threshold

| Metric | Minimum Acceptable | Source |
|--------|--------------------|--------|
| Units sold | 3 | Gumroad dashboard |
| Launch post engagement (r/devops) | 20 upvotes + 10 comments | Reddit |
| GitHub repo stars | 5 | GitHub insights |
| Landing page unique visitors | 100 | Gumroad/Carrd analytics |
| Email list signups (buyers + leads) | 10 | ConvertKit |

**If below threshold on units sold (fewer than 3 sales):**
1. **Diagnose first, do not kill.** Check: did 100+ people visit the landing page?
   - **Yes (100+ visitors, <3 sales)**: This is a positioning or pricing problem. The distribution worked but the page did not convert. Actions: (a) Test a new headline (Variant A or B from Phase 6). (b) Test the $19 price point. (c) Ask 2-3 people from the target audience to review the landing page and give brutally honest feedback.
   - **No (<100 visitors)**: This is a distribution problem. The content did not reach enough people. Actions: (a) Post in 2 additional subreddits (r/terraform, r/sysadmin). (b) Publish a DEV.to article. (c) Share in a Discord/Slack community where you are an active member.
2. **Do NOT kill after Week 1 unless there is zero engagement on free content.** If the Reddit post got 0 upvotes and 0 comments, the DEV.to article got 0 views, and the GitHub repo got 0 stars -- that is a "the topic does not resonate" signal. Even then, try one more post with a different angle before killing.

**If below threshold on engagement (post got <20 upvotes):**
- This is likely a content quality or timing issue, not a product-market issue. Rewrite the post with a different angle and repost in 5-7 days (Reddit allows this if it is genuinely different content). If the second post also fails to gain traction, the distribution thesis for Reddit needs revisiting.

### 7.2 Month 1 Threshold

| Metric | Minimum Acceptable | Source |
|--------|--------------------|--------|
| Cumulative units sold | 10 | Gumroad dashboard |
| Cumulative revenue | $290 | Gumroad dashboard |
| Email list size (buyers + leads) | 40 | ConvertKit |
| GitHub repo stars | 50 | GitHub insights |
| Total time invested (post-build) | <40 hours | Manual tracking |

**If below threshold on cumulative sales (fewer than 10 units in 30 days):**
1. **Check the funnel.** Where are people dropping off?
   - If traffic is reaching the landing page but not converting: pricing or positioning problem. Test $19 price. Test a different headline. Rewrite the problem section of the landing page using different persona pain stories.
   - If traffic is NOT reaching the landing page: distribution problem. The content strategy is not generating enough qualified visitors. Pivot to: (a) guest posting on established DevOps blogs, (b) reaching out to DevOps newsletter curators for a feature, (c) creating a free tool or calculator that drives traffic (e.g., "DevOps Stack Cost Calculator").
2. **Consider format pivot.** If people engage with the free content but do not buy the kit, consider making the entire kit free and monetizing via: (a) consulting calls booked from the free kit, (b) a premium "annotated" version with video walkthroughs, (c) sponsorships from DevOps tool companies.

**If below threshold on GitHub stars (fewer than 50 in 30 days):**
- The organic distribution thesis is underperforming. The repo either lacks visibility or lacks quality.
- Action: (a) Submit the repo to 3-5 "awesome-devops" curated lists on GitHub. (b) Improve the README significantly -- add more free content, better formatting, a GIF or diagram. (c) Create a GitHub Discussion thread asking for community input on what decisions to add. (d) If stars are still flat after these actions, GitHub is not the distribution engine for this product. Shift to content marketing (DEV.to, Reddit, LinkedIn) as the primary acquisition channel.

**If total time invested exceeds 40 hours before reaching 10 sales:**
- The unit economics do not justify continued investment at this pace. The operator has spent 40+ hours (build time + marketing) for <$290 in revenue. That is <$7.25/hour.
- Action: Either (a) dramatically reduce time per week (cap at 3 hours/week, passive mode) and let organic traffic trickle in over 90 days, or (b) kill this product and apply learnings to the next offer in the pipeline.

### 7.3 Month 3 Threshold

| Metric | Minimum Acceptable | Source |
|--------|--------------------|--------|
| Cumulative units sold | 40 | Gumroad dashboard |
| Cumulative revenue | $1,160 | Gumroad dashboard |
| Month-over-month unit growth | Positive (Month 3 > Month 2) | Gumroad dashboard |
| Email list size | 150 | ConvertKit |
| GitHub repo stars | 200 | GitHub insights |
| Weekly organic landing page visitors (no active promotion) | 30+ | Gumroad/Carrd analytics |

**If cumulative sales are below 40 units by Day 90:**
- The product has not achieved sufficient product-market fit to sustain ongoing investment. At $29 x 40 = $1,160 over 3 months, this is a side-project-level outcome, not a business.
- Action: **Strategic decision point.** Three options:
  1. **Iterate (keep the product, change the approach):** If engagement is high but conversion is low, test a bundle (Decision Kit + Incident Survival Kit for $39), test a higher price ($49) for higher perceived value, or add a video walkthrough component.
  2. **Pivot (change the product, keep the audience):** If the audience is engaged but the Decision Kit format is not what they want to pay for, pivot to: a cohort-based live workshop ($99-199), a consulting offer ($200/hour for infrastructure architecture review), or a different info product format (video course, interactive web app).
  3. **Kill (stop investing in this opportunity):** If engagement AND conversion are both low -- the audience is not responding to the topic regardless of format. Archive the repo, keep the email list, and move to the next opportunity in the pipeline.

**If month-over-month growth is negative (Month 3 < Month 2):**
- The initial launch spike has decayed and organic discovery is not replacing it. This means the content marketing flywheel is not spinning.
- Action: Before killing, try one high-effort distribution push: a guest post on a high-traffic DevOps blog, a partnership with a DevOps newsletter, or a free webinar/live stream on a specific decision topic. If this does not inflect the curve, the organic growth thesis has failed.

**If community launch is viable (100+ email list, consistent buyer replies):**
- MRR target from community: $500/month (10 members at $49/month) by end of Month 4.
- If community MRR is below $200/month after 60 days of operation: the community value proposition needs rework or the audience prefers one-time purchases over subscriptions.

### 7.4 Qualitative Signals

**"Iterate" signals (mechanism needs tuning, thesis is right):**
- People engage with free content (upvotes, comments, stars) but do not visit the landing page. *Diagnosis*: The CTA or bridge from free to paid is weak. Fix the CTA copy, add more CTAs in the content, or make the paid product more clearly differentiated from the free content.
- People visit the landing page but do not buy. *Diagnosis*: Pricing, positioning, or landing page copy is off. Test price variants, rewrite the headline, or add more social proof (even 1-2 testimonials from early buyers).
- Buyers open Email 1 but do not engage with subsequent emails. *Diagnosis*: The email sequence is not delivering enough value, or the subject lines after Email 1 are weak. Rewrite subject lines, improve content.
- People reply to emails with questions or feedback. *Diagnosis*: This is a GOOD sign. The product is generating engagement. Use the feedback to improve the kit and create new content.

**"Pivot" signals (approach needs changing, audience is real):**
- People say "I love the concept but I want this as a video course / interactive tool / live workshop." *Diagnosis*: Format mismatch. The audience wants the transformation but in a different format. Pivot the delivery mechanism while keeping the content.
- People say "This is great but I need help with [different decision category not in the kit]." *Diagnosis*: The scope is wrong. Expand the kit to cover the decisions people actually need, or create a new product for the most-requested category.
- People buy but request refunds because "I already knew most of this." *Diagnosis*: The kit is not differentiated enough from free content. Add more advanced, nuanced decision logic. Add case studies. Add the "When to Switch" analysis that free content never provides.
- Strong engagement from a different persona than expected (e.g., team leads buying for their teams, or CTOs asking about bulk pricing). *Diagnosis*: Pivot positioning to serve the actual buyer, not the assumed buyer.

**"Kill" signals (the hypothesis is wrong):**
- Zero engagement on free content across ALL channels after 30 days of consistent posting (no upvotes, no comments, no stars, no views). *Diagnosis*: The topic does not resonate, the positioning is wrong, or the audience is not where we think they are. This is a fundamental thesis failure, not a tuning problem.
- Multiple buyers request refunds with feedback like "ChatGPT gives me the same thing for free" or "This is just common sense." *Diagnosis*: The product does not deliver enough value above the free-content floor. The AI commoditization threat (SWOT T2) has materialized.
- After 90 days, the email list has fewer than 50 subscribers and none of them reply to emails. *Diagnosis*: There is no engaged audience for this product. The "post-tutorial gap" pain is real, but nobody is willing to pay this operator to solve it.

### 7.5 Pivot Triggers

| Trigger | Pivot Direction | Kill Condition |
|---------|-----------------|----------------|
| High free engagement, low paid conversion (50+ stars, <5% landing page conversion after 200 visitors) | Make the full kit free. Monetize via: (a) paid consulting/advisory calls linked from the kit, (b) a premium "enterprise team" version with team-specific decision templates and live Q&A, (c) sponsorships from DevOps tool vendors. | If the free version also gets no engagement (download but no usage signals), the content itself is not valuable enough. Kill. |
| Buyers want a different format (3+ refund requests or feedback emails requesting video/interactive/live) | Rebuild the top-performing section as a 1-hour live workshop ($49-99). Test demand with a pre-sale or waitlist. Keep the kit as a companion resource. | If the workshop pre-sale gets fewer than 10 signups with 200+ landing page visitors, the format pivot did not solve the problem. Kill the product, keep the email list, and test a fundamentally different offer. |
| Unexpected buyer persona emerges (team leads, managers, or CTOs buying, not individual engineers) | Reposition as a team resource. Create a "Team License" at $99-199. Add a "Team Decision Workshop" facilitation guide. Change the landing page headline to address the buyer (the manager), not the user (the engineer). | If the team-license pivot does not generate 5+ sales in 30 days, the B2B angle is not strong enough. Return to B2C positioning or kill. |
| Strong interest in community, weak interest in static product (many replies to Email 5, few kit purchases from new visitors) | Skip the kit as the entry product. Launch the community directly with a lower price ($19-29/month). Use the kit sections as weekly community content. The community IS the product. | If the community fails to retain 10+ paying members after 60 days (18% churn math means you need 12+ members to stay above 10 with no new acquisition), the community thesis is wrong for this audience at this time. Kill the community, keep the kit as a passive income product. |
| The "AI replaced my product" scenario (ChatGPT or a DevOps AI tool launches context-aware infrastructure recommendations) | Pivot the kit from "decision framework" to "AI audit framework" -- teach engineers how to evaluate and validate AI-generated infrastructure recommendations. The kit becomes "the human review layer on top of AI." This leans into SWOT O3 (AI elevates decision frameworks). | If the AI tool also handles auditing/validation (i.e., it is self-correcting), the entire product category is commoditized. Kill. Pivot to community or consulting where human judgment is irreplaceable. |

---

## Appendix: Key Numbers at a Glance

| Milestone | Date | Metric | Number |
|-----------|------|--------|--------|
| Launch | Day 0 | Product live, all funnels tested | -- |
| Week 1 check | Day 7 | Minimum units sold | 3 |
| Week 1 check | Day 7 | Minimum landing page visitors | 100 |
| Month 1 check | Day 30 | Minimum units sold | 10 |
| Month 1 check | Day 30 | Minimum revenue | $290 |
| Month 1 check | Day 30 | Minimum email list | 40 |
| Month 1 check | Day 30 | Minimum GitHub stars | 50 |
| Month 1 check | Day 30 | Maximum time invested (post-build) | 40 hours |
| Month 3 check | Day 90 | Minimum units sold | 40 |
| Month 3 check | Day 90 | Minimum revenue | $1,160 |
| Month 3 check | Day 90 | Minimum email list | 150 |
| Month 3 check | Day 90 | Minimum GitHub stars | 200 |
| Month 3 check | Day 90 | MoM growth | Positive |
