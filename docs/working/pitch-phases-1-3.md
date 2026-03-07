# Pitch Phases 1-3: The DevOps Decision Kit

## Phase 1: Landing Page Copy

### 1.1 Headline

**Stop Googling "Terraform vs Pulumi." Start Deciding.**

### Subheadline

A decision framework with deployable templates that gives you a confident infrastructure answer in 15 minutes -- based on your team size, your budget, and your stack. Not another comparison blog post.

---

### 1.2 Above-the-Fold Hook

Your boss asked you to design the infrastructure. You have been reading comparison blog posts for three weeks. You still do not know if you should use ECS or EKS, Terraform or Pulumi, and every article ends with "it depends" without telling you what it depends ON.

The DevOps Decision Kit gives you the decision, not more comparisons. Answer four questions about your situation. Get a specific recommendation with the reasoning to defend it. Deploy from the companion templates. Move on.

---

### 1.3 Problem Section

**You know this feeling.**

You got promoted. Or you took a new role. Or your company's "DevOps person" left and now it is you. Either way, someone is asking you to make infrastructure decisions you have never made before -- and you are expected to be confident about it.

**The comparison rabbit hole.**
You open a browser tab. Then another. Then thirty more. "Terraform vs Pulumi 2026." "ECS vs EKS for small teams." "Best CI/CD pipeline for startups." Three hours later, you have read fifteen blog posts that all say "it depends" and you still cannot decide. As one engineer put it: *"The amount of time I've spent fiddling with Terraform, Ansible... absurd, probably more than actual application code."*

**The copy-paste gamble.**
So you do what everyone does. You copy a Terraform config from a Medium article written for a company 100x your size. You duplicate folder structures across environments. You end up maintaining infrastructure you did not build and do not understand. *"87% of Terraform code is copy-paste."* You know this is true because you are looking at your own repo right now.

**The 30-tool paralysis.**
The DevOps roadmap shows 30+ tools you are supposed to know. Terraform, Pulumi, CDK, CloudFormation. ECS, EKS, plain Docker, Lambda. GitHub Actions, GitLab CI, Jenkins, CircleCI. Prometheus, Grafana, Datadog, CloudWatch. *"You need to learn 5 to 10 technologies right at the beginning, and you can't just learn them in isolation, but you have to know how to combine them and integrate them together."* 89% of junior DevOps engineers felt overwhelmed and considered quitting in their first two months. The number does not get much better at the mid-level -- it just gets quieter because admitting it feels worse.

**The AI crutch.**
You asked Copilot to write your Terraform. It worked. You deployed it. But you do not actually understand what it created, and you are terrified of what happens when it breaks at 2am. 70% of Terraform at Flipkart is Copilot-generated. *"AI-generated code is helping teams ship to production faster, but not always more safely."* You are shipping faster. You are not sleeping better.

**The silence.**
Maybe you are the only DevOps person at your company. *"You don't work with more experienced team members, so you cannot brainstorm any solutions for problems you are facing."* Every tool choice is made alone. Every architecture decision is a guess you cannot validate. There is no one to tell you whether your gut feeling is right or dangerously wrong.

The worst part is not the uncertainty. The worst part is that everyone else seems to know what they are doing. They do not. *"As soon as I feel comfortable with something, there's a new thing that I feel lost with."* But knowing that does not make the next decision any easier.

---

### 1.4 Solution Section

**Here is what changes.**

**Before:** You stare at a Terraform plan that touches 47 resources and you have no idea which ones are safe to apply. Your boss needs an answer on the infrastructure architecture by Friday and you have spent the week reading comparison posts that end with "it depends."

**After:** You open the Decision Kit, answer four questions about your team (size, budget, cloud provider, existing language skills), and get a specific tool recommendation with a two-paragraph justification you can paste into a Slack message to your manager. You clone the companion template and have a working proof-of-concept by lunch.

**How it works:**

1. **Start with the Quick Start decision tree.** Four questions. Fifteen minutes. You walk away with your first infrastructure decision made and justified.
2. **Go deep on each layer.** Five detailed decision trees cover IaC tools, container orchestration, CI/CD pipelines, monitoring stacks, and AI-generated infrastructure review. Each tree branches on YOUR situation -- not a generic comparison.
3. **Deploy from the companion repo.** Every recommendation path has a matching deployable template with annotated READMEs explaining WHY every line exists. Clone, configure, deploy.

**Speed:** Your first confident decision happens in 15 minutes. A deployable proof-of-concept within an hour. The entire kit is designed to be used at your desk while you are facing a real decision -- not as a course you watch over six weekends.

**Effort:** The Decision Kit does the thinking for you. Instead of synthesizing 47 blog posts into a decision, you follow a decision tree. The framework decides based on your context. You execute. The companion repo means you are not starting from a blank `main.tf` -- you are starting from annotated, deployable infrastructure code.

---

### 1.5 What's Inside

**Section 1: Quick Start -- Your First Decision in 15 Minutes**
A single-page master decision tree. Answer four questions (team size, monthly cloud budget, primary cloud provider, existing codebase language) and get pointed to the right section and recommendation path. This is the thing you open first when your boss asks "what should our infrastructure look like?" and you need an answer before the end of the day.

**Section 2: IaC Tool Decision -- Terraform vs. Pulumi vs. CDK vs. CloudFormation**
A decision tree with three branches based on team size, existing language skills, and multi-cloud requirements. Each branch terminates in a specific recommendation with a two-paragraph justification and a link to the companion repo template. Includes the "when to switch" analysis -- the reversibility criteria that no comparison blog post ever provides. You will know not just what to pick, but when to reconsider.

**Section 3: Container Orchestration Decision -- ECS vs. EKS vs. Plain Docker vs. Lambda**
The first question is "Do you actually need Kubernetes?" (answer: usually no). Decision tree branches on workload type, team Kubernetes experience, and budget. Includes the "EKS costs $2,400/year just for the control plane" reality check that the Kubernetes evangelists never mention. Companion repo has a deployable ECS Fargate template and a minimal EKS template.

**Section 4: CI/CD Pipeline Decision -- GitHub Actions vs. GitLab CI vs. Jenkins vs. CircleCI**
Decision tree based on where your code lives, team size, and self-hosted runner requirements. Companion repo has starter workflow files for GitHub Actions and GitLab CI with annotated comments explaining every step. Includes the honest "Jenkins is legacy -- here is when you should migrate and when you should not" analysis.

**Section 5: Monitoring Stack Decision -- Prometheus+Grafana vs. Datadog vs. CloudWatch vs. New Relic**
A scoring framework (not a binary tree, because monitoring involves more tradeoffs). Covers cost at scale (Datadog's per-host pricing surprise), open-source operational burden (Prometheus requires maintenance), and cloud-native lock-in (CloudWatch is easy but vendor-locked). Companion repo has a deployable Prometheus+Grafana stack for the reader who chooses open-source.

**Section 6: The "Should I Use AI for This?" Checklist**
15 checks to run on Copilot-generated Terraform before you `terraform apply`. Covers security (exposed ports, public S3 buckets, missing encryption), cost (oversized instances, missing auto-scaling), and architectural red flags (hardcoded values, no state management, missing outputs). Each item has a one-sentence explanation of why it matters and what can go wrong. This is the section for everyone who knows their infrastructure was half-written by AI and half-understood by them.

**Companion GitHub Repo**
Real, deployable infrastructure code for every recommendation path. Terraform modules, Docker configs, CI/CD workflow files, Prometheus+Grafana stack. Every file has an annotated README explaining the decisions behind the code -- not just what it does, but why it exists and when you would choose differently.

---

### 1.6 Social Proof Strategy

**Built by a practitioner, not a course creator.**

This kit was built by an engineer who designed production infrastructure for Qortex (complex backend systems with real traffic), built MindMirror (enterprise federated GraphQL + Expo mobile), and created and ran a 24-week cybersecurity curriculum at a company doing $60MM+ ARR. Every decision tree in this kit traces back to a real production decision -- not a theoretical exercise.

**Why that matters to you:** DevOps engineers can smell "I watched a YouTube video and made a course" from a mile away. The Decision Kit does not teach you what Terraform is. It tells you when to use Terraform and when not to, based on real tradeoffs encountered in real production systems. The author was the 13th most popular writer on Scotch.io when it was the go-to technical education site. That combination -- practitioner who has shipped production infrastructure AND proven ability to teach complex technical concepts -- is what makes this different from another comparison blog post.

**How this was built:** The decision trees are not opinions. They are structured analysis of real infrastructure decisions across multiple production systems, combined with research into how hundreds of DevOps engineers describe their tool selection pain (r/devops, Hacker News, DEV.to, industry surveys). The persona research alone draws from 30+ primary sources. If a recommendation appears in this kit, there is a real production deployment behind it.

---

### 1.7 Pricing Section

**What this would cost you otherwise:**

- A DevOps consulting engagement to make these five decisions: **$2,000-5,000** (at $200-500/hr for 10 hours of architecture review)
- The salary cost of three weeks of analysis paralysis (your time spent reading comparison posts instead of building): **$2,500+** at a $130K salary
- KodeKloud subscription to learn each tool individually and still not know which to pick: **$180-348/year** (and it does not make the decision for you)

**What you get:**

| Component | Value |
|-----------|-------|
| 5 Decision Trees (IaC, Containers, CI/CD, Monitoring, Quick Start) | $150 |
| Companion GitHub Repo with Deployable Templates | $100 |
| The "Should I Use AI for This?" 15-Point Checklist | $50 |
| "When to Switch" Reversibility Analysis for Every Decision | $75 |
| Annotated READMEs Explaining Every Infrastructure Choice | $75 |
| **Total Value** | **$450** |

**Your price: $29**

One payment. Yours forever. No subscription that disappears when you stop paying. No upsell wall hiding the good parts.

**The guarantee:** If the Decision Kit does not help you make a confident infrastructure decision within 30 days, email me and I will refund every penny. I would rather have you know me as someone who stands behind their work than keep $29 from someone it did not help. No questions asked.

**Launch pricing.** The price goes to $49 after the first 100 copies. If you are reading this and the price still says $29, you are early.

---

### 1.8 FAQ Section

**"I can just ask ChatGPT which tool to use."**
You can, and you will get a generic answer that does not account for your team size, budget, existing skills, or specific constraints. ChatGPT cannot tell you "you have 2 engineers, $500/month cloud budget, and a Python codebase -- use Pulumi over Terraform because your team will ship faster in a language they already know." The Decision Kit does exactly that. It is the context-aware reasoning that AI cannot provide, because AI does not know YOUR situation until you spend 30 minutes prompt-engineering it -- and then you still do not know if the answer is right. The Decision Kit gives you the decision tree and the reasoning to evaluate it yourself.

**"There is so much free content about DevOps tools already."**
There is. And you have been reading it for three weeks and still have not decided. Free content gives you comparisons: "Terraform is good for X and Pulumi is good for Y." The Decision Kit gives you a DECISION: "Given YOUR situation, use Terraform. Here is the template. Here is when you should reconsider." The difference between information and a decision is the difference between reading about swimming and jumping in the pool. You have enough information. You need a decision.

**"I should just learn everything deeply and then I will know what to choose."**
You do not have six months. Your boss needs an answer next week. The Decision Kit gives you a defensible answer TODAY and shows you the 20% of each tool you need to understand to make a confident choice. Depth comes later, from building the thing you chose. The decision tree gives you the starting point so you can stop researching and start building.

**"$29 seems expensive for a guide."**
KodeKloud costs $15-29/month -- that is $180-348/year, and it teaches you tools without telling you which to pick. A single wasted week of analysis paralysis costs your company $2,500+ in your salary alone. The Decision Kit pays for itself the first time it saves you from a three-week comparison rabbit hole. And it is yours forever -- not a subscription that disappears when you stop paying.

**"What if my situation is not covered by the decision trees?"**
The kit covers the five infrastructure decisions that paralyze the most engineers: IaC tooling, container orchestration, CI/CD pipelines, monitoring stacks, and AI-generated infrastructure review. These are the decisions that appear in 90%+ of DevOps architecture discussions on r/devops and Hacker News. If your situation falls outside these five, email me -- I read every message and will either point you to the right branch or add your scenario to the next version.

**"How is this different from the DevOps Roadmap or 90DaysOfDevOps?"**
Those are learning paths -- they tell you what to study. The Decision Kit is a decision tool -- it tells you what to USE. The DevOps Roadmap shows you 30+ tools and says "learn these." The Decision Kit asks you four questions about your situation and says "use this one. Here is why. Here is the template." They are complementary, not competing. Use the Roadmap to learn. Use the Decision Kit to decide.

**"Is this for beginners?"**
No. This is for mid-level engineers (2-10 years in) who have finished tutorials, know the tools exist, and need to make a production decision. If you have never heard of Terraform, start with KodeKloud or TechWorld with Nana. If you have heard of Terraform, Pulumi, AND CDK and cannot decide which to use for your team, this is for you.

---

### 1.9 CTA Copy

**Primary CTA:**

> **Get the DevOps Decision Kit -- $29**
>
> Five decision trees. Deployable templates. Annotated READMEs. Your next infrastructure decision, made and justified in 15 minutes.
>
> [Get the Decision Kit]
>
> 30-day money-back guarantee. One payment. Yours forever.

**Secondary CTA:**

> **Not ready to buy? Start with the free IaC Decision Tree.**
>
> The full Terraform vs. Pulumi vs. CDK vs. CloudFormation decision tree is published for free in the companion GitHub repo. Star the repo, use the tree, and come back when you are ready for the full kit.
>
> [View the GitHub Repo]

---
---

## Phase 2: Launch Posts

### Reddit r/devops Post

**Title:** I built decision trees for the 5 infrastructure choices that paralyze most DevOps engineers -- here is the full IaC tree (free)

**Body:**

I have been a DevOps/infrastructure engineer for several years, and every time I join a new team or start a greenfield project, I watch the same pattern play out:

1. Someone asks "what should we use for IaC?"
2. The team reads 15 blog posts comparing Terraform vs. Pulumi vs. CDK
3. Every post ends with "it depends"
4. Three weeks later, someone picks Terraform because it has the most Stack Overflow answers
5. Six months later, they wish they had picked something else

The comparison posts are not wrong -- it genuinely does depend. But they never tell you what it depends ON in a way that maps to YOUR specific situation.

So I built decision trees for the five infrastructure choices I see paralyze teams most often. Posting the full IaC decision tree here because I think it is genuinely useful and I want feedback from people who have made these decisions in production.

---

**The IaC Decision Tree**

Start here. Answer the questions in order.

**Question 1: Team size?**

- **Solo or 2-3 engineers** -> Go to Q2
- **4-10 engineers** -> Go to Q3
- **10+ engineers or multiple teams** -> Terraform (HCL). At this scale, you need the ecosystem: modules registry, state management patterns, and the largest hiring pool. The learning curve is worth the long-term operational stability. Skip to Q4.

**Question 2 (Solo/Small): What languages does your team write daily?**

- **Python** -> Pulumi (Python SDK). Your team already thinks in Python. Pulumi lets them define infrastructure in a language they know, with real loops, conditionals, and type checking. The ramp-up time is days, not weeks.
- **TypeScript/JavaScript** -> CDK (if AWS-only) or Pulumi (TypeScript SDK, if multi-cloud). CDK gives you the tightest AWS integration. Pulumi gives you flexibility if you ever need GCP or Azure.
- **Go** -> Pulumi (Go SDK). Same reasoning as Python -- use the language your team already knows.
- **None of the above / team has no strong language preference** -> Terraform (HCL). HCL is purpose-built for infrastructure. It is not a general-purpose language, which means it constrains you in useful ways. For small teams without existing language preferences, the constraint is a feature.

**Question 3 (Mid-size): Multi-cloud requirement?**

- **Single cloud (AWS, GCP, or Azure)** -> If AWS, evaluate CDK vs. Terraform. CDK if your team is TypeScript-heavy and you want tight CloudFormation integration. Terraform if you want provider-agnostic patterns even within a single cloud.
- **Multi-cloud or hybrid** -> Terraform or Pulumi. Terraform has broader provider coverage. Pulumi has better programming language support. Decide based on Q2 (language preference).

**Question 4 (All sizes): Existing infrastructure?**

- **Greenfield (nothing exists yet)** -> Use whatever you picked above. Start clean.
- **Existing CloudFormation** -> Consider CDK as a migration path (CDK compiles to CloudFormation). Alternatively, use Terraform import to bring existing resources under management.
- **Existing Terraform** -> Stay on Terraform unless you have a strong reason to migrate. "The grass is greener" is not a strong reason. Migration costs are real.
- **Existing manual/ClickOps infrastructure** -> Terraform import or Pulumi import to bring existing resources under code management. Terraform has more mature import tooling as of 2026.

**The reversibility check (the part nobody talks about):**

Every IaC tool has migration paths to and from every other IaC tool. None of these decisions are permanent. The real question is not "which is the best tool?" -- it is "which tool lets my team ship infrastructure changes this month?" Pick. Build. Revisit in 6 months with real production data. A wrong decision made today beats a perfect decision made in three months.

---

**Where to find the other four trees:**

I also built decision trees for:
- Container orchestration (ECS vs. EKS vs. plain Docker vs. Lambda)
- CI/CD pipelines (GitHub Actions vs. GitLab CI vs. Jenkins vs. CircleCI)
- Monitoring stacks (Prometheus+Grafana vs. Datadog vs. CloudWatch vs. New Relic)
- An AI-generated infrastructure review checklist (15 checks before `terraform apply` on Copilot code)

The IaC tree and a preview of the container orchestration tree are free in the companion GitHub repo: [link]

The full kit with all five decision trees, deployable templates, and annotated READMEs is $29: [link]

Happy to discuss the reasoning behind any branch. If your situation does not fit the tree, tell me your context and I will help you think through it.

---

### LinkedIn Post

I watched a mid-level engineer spend 3 weeks deciding between Terraform and Pulumi.

They read every comparison blog post. Every one ended with "it depends."

They never shipped.

Here is what those blog posts do not tell you:

The decision depends on exactly 3 things.
Your team size. Your team's existing language skills. Whether you need multi-cloud.

That is it.

Solo engineer who writes Python daily? Use Pulumi's Python SDK. You will be productive in days, not weeks.

10+ person team? Terraform. The ecosystem (modules registry, state management patterns, hiring pool) matters more than language elegance at that scale.

TypeScript team on AWS only? CDK gives you the tightest integration. Multi-cloud? Pulumi TypeScript SDK.

Team with no strong language preference? Terraform's HCL. The constraints of a purpose-built language are a feature for infrastructure, not a bug.

The kicker: none of these decisions are permanent. Every IaC tool has migration paths. A wrong decision made today beats a perfect decision made in 3 months.

I built decision trees like this for the 5 infrastructure choices that paralyze most DevOps engineers (IaC, containers, CI/CD, monitoring, AI code review).

The IaC tree is free in the companion repo (link in comments).

The full kit with all 5 trees + deployable templates is $29.

Built from real production decisions, not theoretical exercises.

#DevOps #InfrastructureAsCode #Terraform #Pulumi #PlatformEngineering

---

### Twitter/X Thread

**Tweet 1:**
Every DevOps comparison blog post ends with "it depends."

I built decision trees that tell you what it depends ON.

Here is the IaC decision tree -- the one that answers "Terraform vs. Pulumi vs. CDK" in 4 questions:

**Tweet 2:**
Q1: Team size?

- Solo/2-3 people -> your daily language matters most
- 4-10 people -> multi-cloud requirement matters most
- 10+ people -> Terraform. Full stop. Ecosystem + hiring pool wins at scale.

**Tweet 3:**
Q2: What language does your team write every day?

- Python -> Pulumi (Python SDK)
- TypeScript -> CDK (AWS only) or Pulumi TS (multi-cloud)
- Go -> Pulumi (Go SDK)
- No preference -> Terraform HCL

Use the language you already know. IaC is not the place to learn a new one.

**Tweet 4:**
The part no comparison post covers: reversibility.

Every IaC tool has migration paths to every other IaC tool.

None of these decisions are permanent.

A wrong decision made today beats a perfect decision made in 3 months.

Pick. Build. Revisit with real production data in 6 months.

**Tweet 5:**
Existing infrastructure changes the math:

- Existing CloudFormation -> CDK (compiles to CFN)
- Existing Terraform -> stay unless you have a STRONG reason
- Existing ClickOps -> Terraform import (most mature tooling in 2026)

"The grass is greener" is not a migration justification.

**Tweet 6:**
I built this because I watched the same pattern at every company:

1. Team needs IaC tool
2. 3 weeks of comparison blog posts
3. Pick whatever has the most YouTube tutorials
4. Regret it 6 months later

Decision trees fix this. 4 questions. 1 recommendation. Reasoning included.

**Tweet 7:**
Background: I have designed production infrastructure for complex backend systems, enterprise federated GraphQL platforms, and built a 24-week cybersecurity curriculum at a $60MM+ ARR company.

These trees come from real decisions under real production pressure. Not hypotheticals.

**Tweet 8:**
I built decision trees for 5 infra choices:

1. IaC (Terraform/Pulumi/CDK/CFN)
2. Containers (ECS/EKS/Docker/Lambda)
3. CI/CD (GH Actions/GitLab/Jenkins/CircleCI)
4. Monitoring (Prom+Grafana/Datadog/CloudWatch)
5. AI code review checklist

IaC tree is free: [repo link]
Full kit (all 5 + templates): $29 [link]

---
---

## Phase 3: GitHub Product README

### 3a: Product Structure Sketch

#### Directory Layout

```
devops-decision-kit/
├── README.md
├── LICENSE (MIT)
├── CONTRIBUTING.md
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── decision-feedback.md
│   │   ├── scenario-request.md
│   │   └── bug-report.md
│   └── FUNDING.yml
├── docs/
│   ├── getting-started.md
│   ├── how-to-use-decision-trees.md
│   ├── methodology.md
│   └── faq.md
├── decision-trees/
│   ├── 00-quick-start/
│   │   ├── README.md
│   │   └── master-decision-tree.md
│   ├── 01-iac-tool/
│   │   ├── README.md
│   │   ├── decision-tree.md
│   │   ├── when-to-switch.md
│   │   └── comparison-table.md
│   ├── 02-container-orchestration/
│   │   ├── README.md
│   │   ├── decision-tree.md
│   │   ├── do-you-need-kubernetes.md
│   │   ├── cost-analysis.md
│   │   └── when-to-switch.md
│   ├── 03-cicd-pipeline/
│   │   ├── README.md
│   │   ├── decision-tree.md
│   │   ├── jenkins-migration-guide.md
│   │   └── when-to-switch.md
│   ├── 04-monitoring-stack/
│   │   ├── README.md
│   │   ├── scoring-framework.md
│   │   ├── cost-at-scale.md
│   │   └── when-to-switch.md
│   └── 05-ai-infrastructure-review/
│       ├── README.md
│       ├── checklist.md
│       └── common-ai-failures.md
├── templates/
│   ├── terraform/
│   │   ├── aws-ecs-fargate/
│   │   │   ├── README.md
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   ├── outputs.tf
│   │   │   └── terraform.tfvars.example
│   │   ├── aws-eks-minimal/
│   │   │   ├── README.md
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   ├── outputs.tf
│   │   │   └── terraform.tfvars.example
│   │   └── aws-lambda-api/
│   │       ├── README.md
│   │       ├── main.tf
│   │       ├── variables.tf
│   │       ├── outputs.tf
│   │       └── terraform.tfvars.example
│   ├── pulumi/
│   │   ├── python-ecs/
│   │   │   ├── README.md
│   │   │   ├── __main__.py
│   │   │   ├── Pulumi.yaml
│   │   │   └── requirements.txt
│   │   └── typescript-eks/
│   │       ├── README.md
│   │       ├── index.ts
│   │       ├── Pulumi.yaml
│   │       └── package.json
│   ├── cicd/
│   │   ├── github-actions/
│   │   │   ├── README.md
│   │   │   ├── ci.yml
│   │   │   ├── cd-ecs.yml
│   │   │   └── cd-eks.yml
│   │   └── gitlab-ci/
│   │       ├── README.md
│   │       └── .gitlab-ci.yml
│   └── monitoring/
│       ├── prometheus-grafana/
│       │   ├── README.md
│       │   ├── docker-compose.yml
│       │   ├── prometheus.yml
│       │   └── grafana/
│       │       └── dashboards/
│       │           └── devops-kit-dashboard.json
│       └── cloudwatch-alarms/
│           ├── README.md
│           └── alarms.tf
└── examples/
    ├── small-team-aws/
    │   ├── README.md
    │   └── architecture-diagram.md
    ├── mid-team-multi-cloud/
    │   ├── README.md
    │   └── architecture-diagram.md
    └── solo-engineer-startup/
        ├── README.md
        └── architecture-diagram.md
```

#### File Manifest

| File | Description | Tier |
|------|-------------|------|
| `README.md` | Product README with full IaC decision tree, repo overview, and CTAs | Free |
| `LICENSE` | MIT license | Free |
| `CONTRIBUTING.md` | How to contribute scenarios, feedback, and case studies | Free |
| `docs/getting-started.md` | How to use the decision trees and templates | Free |
| `docs/how-to-use-decision-trees.md` | Guide for navigating the trees with your specific context | Free |
| `docs/methodology.md` | How the decision trees were built (research process, production grounding) | Free |
| `docs/faq.md` | Common questions about the kit and decision approach | Free |
| `decision-trees/00-quick-start/README.md` | Overview of the Quick Start master tree | Free |
| `decision-trees/00-quick-start/master-decision-tree.md` | 4-question master tree pointing to each section | Free |
| `decision-trees/01-iac-tool/README.md` | Overview of the IaC decision section | Free |
| `decision-trees/01-iac-tool/decision-tree.md` | Full IaC decision tree (Terraform/Pulumi/CDK/CFN) | Free |
| `decision-trees/01-iac-tool/when-to-switch.md` | Reversibility analysis for IaC tools | Free |
| `decision-trees/01-iac-tool/comparison-table.md` | Feature comparison matrix for quick reference | Free |
| `decision-trees/02-container-orchestration/README.md` | Overview of the container orchestration section | Free |
| `decision-trees/02-container-orchestration/decision-tree.md` | Full container orchestration decision tree | **Premium** |
| `decision-trees/02-container-orchestration/do-you-need-kubernetes.md` | The "Do you actually need K8s?" analysis | Free |
| `decision-trees/02-container-orchestration/cost-analysis.md` | EKS/ECS/Lambda cost comparison by workload type | **Premium** |
| `decision-trees/02-container-orchestration/when-to-switch.md` | Reversibility analysis for container platforms | **Premium** |
| `decision-trees/03-cicd-pipeline/README.md` | Overview of the CI/CD decision section | Free |
| `decision-trees/03-cicd-pipeline/decision-tree.md` | Full CI/CD decision tree | **Premium** |
| `decision-trees/03-cicd-pipeline/jenkins-migration-guide.md` | When to migrate off Jenkins (and when not to) | Free |
| `decision-trees/03-cicd-pipeline/when-to-switch.md` | Reversibility analysis for CI/CD platforms | **Premium** |
| `decision-trees/04-monitoring-stack/README.md` | Overview of the monitoring decision section | Free |
| `decision-trees/04-monitoring-stack/scoring-framework.md` | Monitoring stack scoring framework | **Premium** |
| `decision-trees/04-monitoring-stack/cost-at-scale.md` | Monitoring cost projections at 10/50/200 hosts | **Premium** |
| `decision-trees/04-monitoring-stack/when-to-switch.md` | Reversibility analysis for monitoring stacks | **Premium** |
| `decision-trees/05-ai-infrastructure-review/README.md` | Overview of the AI review checklist | Free |
| `decision-trees/05-ai-infrastructure-review/checklist.md` | Full 15-point AI infrastructure review checklist | **Premium** |
| `decision-trees/05-ai-infrastructure-review/common-ai-failures.md` | Real examples of AI-generated infra failures | **Premium** |
| `templates/terraform/aws-ecs-fargate/*` | Deployable ECS Fargate template with annotated README | Free |
| `templates/terraform/aws-eks-minimal/*` | Minimal EKS template with annotated README | **Premium** |
| `templates/terraform/aws-lambda-api/*` | Lambda API template with annotated README | Free |
| `templates/pulumi/python-ecs/*` | Pulumi Python ECS template | **Premium** |
| `templates/pulumi/typescript-eks/*` | Pulumi TypeScript EKS template | **Premium** |
| `templates/cicd/github-actions/*` | GitHub Actions workflow templates (CI + CD) | Free |
| `templates/cicd/gitlab-ci/*` | GitLab CI template | **Premium** |
| `templates/monitoring/prometheus-grafana/*` | Deployable Prom+Grafana stack with dashboards | **Premium** |
| `templates/monitoring/cloudwatch-alarms/*` | CloudWatch alarm Terraform config | Free |
| `examples/small-team-aws/*` | End-to-end example: 3-person team, AWS, $500/mo budget | Free |
| `examples/mid-team-multi-cloud/*` | End-to-end example: 8-person team, AWS+GCP | **Premium** |
| `examples/solo-engineer-startup/*` | End-to-end example: solo engineer, startup, minimal budget | Free |

**Free tier (approx 60-70% of content):** Full IaC decision tree, Quick Start master tree, "Do you need Kubernetes?" analysis, Jenkins migration guide, AI review overview, ECS Fargate template, Lambda template, GitHub Actions workflows, CloudWatch alarms, two end-to-end examples, all READMEs and documentation.

**Premium tier (approx 30-40% of content):** Container orchestration full tree + cost analysis, CI/CD full tree, monitoring scoring framework + cost projections, full AI checklist + failure examples, EKS template, Pulumi templates, GitLab CI template, Prometheus+Grafana stack, multi-cloud example.

#### Learning Path

1. **Start here:** Read the `README.md` for orientation and the IaC decision tree preview.
2. **Quick Start:** Open `decision-trees/00-quick-start/master-decision-tree.md`. Answer 4 questions. Get pointed to the right section.
3. **Your first decision:** Follow the recommended decision tree. Read the full tree, find your branch, get your recommendation.
4. **Deploy:** Navigate to the matching template in `templates/`. Read the annotated README. Clone, configure, deploy.
5. **Go deeper:** Read the "when to switch" analysis for your chosen tool. Understand the reversibility criteria.
6. **Review AI output:** If you use Copilot/ChatGPT for infrastructure, run through the AI review checklist before applying.
7. **End-to-end context:** Browse `examples/` for a complete architecture that matches your team profile.
8. **Get the full kit:** If you are using the free tier and want the remaining decision trees, deployable templates, and cost analyses, get the full Decision Kit for $29.

#### Prerequisites

- **Experience level:** Mid-level (2-10 years in DevOps, SRE, Platform Engineering, or Infrastructure Engineering). You should know what Terraform, Docker, and CI/CD are. You do not need to be an expert in any of them.
- **Technical prerequisites:** A terminal, git, and an AWS account (free tier is sufficient for most templates). Some templates require Terraform >= 1.5, Docker, or Pulumi.
- **Concepts you should know:** What infrastructure-as-code is (conceptually). What containers are (conceptually). What CI/CD pipelines do (conceptually). You do NOT need to know which specific tool to use -- that is what this kit decides for you.
- **Time commitment:** 15 minutes for your first decision (Quick Start). 1-2 hours to go deep on a single decision tree and deploy a template. 4-6 hours to work through the entire kit.

---

### 3b: README.md Content

```markdown
# The DevOps Decision Kit

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

**Stop reading comparison blog posts. Start making infrastructure decisions.**

You know what Terraform, Pulumi, ECS, EKS, GitHub Actions, and Datadog are. You do not know which one to use for YOUR team, YOUR budget, and YOUR stack. Every comparison post ends with "it depends" without telling you what it depends on.

This repo contains decision trees for the 5 infrastructure choices that paralyze most DevOps engineers. Answer a few questions about your situation. Get a specific recommendation with reasoning. Deploy from the companion templates.

**Before:** Three weeks of comparison blog posts. Still cannot decide. Copies Terraform from a Medium article written for a company 100x your size.

**After:** 15 minutes. One decision. Justified reasoning you can explain to your team. A deployable template to start building today.

---

## What's In This Repo

```
devops-decision-kit/
├── decision-trees/          # Decision frameworks for 5 infrastructure choices
│   ├── 00-quick-start/      # 4-question master tree -> your first decision in 15 min
│   ├── 01-iac-tool/         # Terraform vs. Pulumi vs. CDK vs. CloudFormation
│   ├── 02-container-orchestration/  # ECS vs. EKS vs. Docker vs. Lambda
│   ├── 03-cicd-pipeline/    # GitHub Actions vs. GitLab CI vs. Jenkins vs. CircleCI
│   ├── 04-monitoring-stack/ # Prometheus+Grafana vs. Datadog vs. CloudWatch vs. New Relic
│   └── 05-ai-infrastructure-review/ # 15 checks before terraform apply on AI code
├── templates/               # Deployable infrastructure code for each recommendation
│   ├── terraform/           # ECS Fargate, EKS, Lambda templates
│   ├── pulumi/              # Python ECS, TypeScript EKS templates
│   ├── cicd/                # GitHub Actions and GitLab CI workflows
│   └── monitoring/          # Prometheus+Grafana stack, CloudWatch alarms
├── examples/                # End-to-end architecture examples by team profile
│   ├── small-team-aws/      # 3-person team, AWS, $500/mo budget
│   ├── mid-team-multi-cloud/# 8-person team, AWS+GCP
│   └── solo-engineer-startup/# Solo engineer, startup, minimal budget
└── docs/                    # Getting started, methodology, FAQ
```

---

## Quick Start: Your First Decision in 15 Minutes

Answer these four questions:

### 1. How many engineers manage your infrastructure?

| Answer | Go to |
|--------|-------|
| Just me (solo) | Continue to Q2 |
| 2-3 engineers | Continue to Q2 |
| 4-10 engineers | Continue to Q3 |
| 10+ engineers or multiple teams | Your IaC tool is **Terraform**. [Read why](decision-trees/01-iac-tool/decision-tree.md#large-teams). Continue to Q4. |

### 2. What programming language does your team use daily?

| Answer | IaC Recommendation |
|--------|-------------------|
| Python | **Pulumi** (Python SDK) -- define infra in a language you already know |
| TypeScript / JavaScript | **CDK** (if AWS-only) or **Pulumi** (TypeScript SDK, if multi-cloud) |
| Go | **Pulumi** (Go SDK) |
| Java / C# / Other | **Terraform** (HCL) -- purpose-built for infra, no language bias needed |
| No strong preference | **Terraform** (HCL) -- the constraints of a DSL are a feature for infra |

### 3. Do you need multi-cloud support?

| Answer | IaC Recommendation |
|--------|-------------------|
| Single cloud (AWS/GCP/Azure) | **CDK** (if AWS + TypeScript team) or **Terraform** (otherwise) |
| Multi-cloud or hybrid | **Terraform** (broadest provider coverage) or **Pulumi** (better language support) -- decide based on Q2 |

### 4. What existing infrastructure do you have?

| Answer | Recommendation |
|--------|---------------|
| Greenfield (nothing exists) | Use whatever Q1-Q3 recommended. Start clean. |
| Existing CloudFormation | Consider **CDK** (compiles to CFN) as a migration path |
| Existing Terraform | **Stay on Terraform** unless you have a strong reason. Migration costs are real. |
| Existing ClickOps (manual) | **Terraform import** (most mature tooling as of 2026) |

---

## IaC Decision Tree (Full)

This is the complete Infrastructure-as-Code decision tree. It is free. Use it.

### The Decision Matrix

```
                              Team Size?
                    /            |            \
                Solo/Small    Mid (4-10)    Large (10+)
                   |              |              |
            Daily Language?   Multi-cloud?   -> Terraform
              /  |  |  \       /      \         (ecosystem +
            Py  TS  Go  None  Yes     No        hiring pool)
            |   |   |    |    |       |
         Pulumi |  Pulumi TF  TF or  CDK(AWS+TS)
         (Py)   |  (Go)      Pulumi  or TF
                |
          CDK(AWS-only)
          or Pulumi(TS)
```

### Decision Details

#### Solo / Small Team (1-3 engineers)

**If your team writes Python daily -> Pulumi (Python SDK)**

Why: Pulumi lets you define infrastructure using real Python -- with loops, conditionals, type checking, and your existing IDE setup. For a small team, the ramp-up speed of using a familiar language outweighs Terraform's larger ecosystem. You will be writing `for` loops over subnets, not copy-pasting HCL blocks.

When to reconsider: If you hire and your new engineers do not know Python, or if you find yourself fighting Pulumi's state management on complex multi-stack deployments. Terraform's state management is more battle-tested at scale.

Template: [`templates/pulumi/python-ecs/`](templates/pulumi/python-ecs/)

**If your team writes TypeScript daily -> CDK (AWS-only) or Pulumi TypeScript SDK (multi-cloud)**

Why: CDK compiles to CloudFormation, giving you the tightest AWS integration and the ability to use every CloudFormation feature on day one. If you are AWS-only, CDK is the path of least resistance. If you need GCP or Azure, Pulumi's TypeScript SDK gives you the same language benefits without the AWS lock-in.

When to reconsider: CDK's CloudFormation dependency means you are subject to CFN deployment limits and debugging CFN stack failures (which are famously opaque). If you hit CFN pain, Pulumi TypeScript is the natural migration.

Template: [`templates/pulumi/typescript-eks/`](templates/pulumi/typescript-eks/)

**If your team writes Go daily -> Pulumi (Go SDK)**

Why: Same reasoning as Python. Use the language your team knows. Pulumi's Go SDK is well-maintained and Go's type system catches infrastructure misconfigurations at compile time.

When to reconsider: Pulumi's Go SDK has a smaller community than the Python/TypeScript SDKs. If you are blocked on a provider issue, check community activity before committing.

**If no strong language preference -> Terraform (HCL)**

Why: HCL is a domain-specific language built for infrastructure. It is declarative by design, which prevents the "I accidentally wrote an infinite loop in my infrastructure code" class of bugs. For a team without existing language preferences, the constraint of a DSL is a feature -- it forces infrastructure-appropriate patterns. Terraform also has the largest ecosystem: the module registry, provider coverage, and community support are unmatched.

When to reconsider: If your team grows and wants to share logic between application code and infrastructure code, or if HCL's limited programming constructs (no real functions, limited loops before 1.x) frustrate your team.

Template: [`templates/terraform/aws-ecs-fargate/`](templates/terraform/aws-ecs-fargate/)

#### Mid-Size Team (4-10 engineers)

The team-size dynamics shift at 4+ engineers. Code review, module sharing, and onboarding new engineers matter more than individual productivity.

**Single cloud -> Evaluate CDK vs. Terraform**

- CDK if: your team is TypeScript-heavy, you are AWS-only, and you want the tightest cloud integration.
- Terraform if: you want provider-agnostic patterns (useful even within a single cloud for portability), or your team is not TypeScript-heavy.

**Multi-cloud -> Terraform or Pulumi**

- Terraform if: you need the broadest provider coverage and want the largest hiring pool.
- Pulumi if: your team has a strong language preference (Python/TS/Go) and values using general-purpose languages over HCL.

#### Large Team (10+ engineers)

**Default: Terraform.**

At this scale, ecosystem matters more than language elegance. Terraform gives you: the largest hiring pool (most DevOps job postings list Terraform), the most mature module registry, battle-tested state management patterns (remote state, workspaces, state locking), and the broadest provider coverage. The HCL learning curve is a one-time cost that pays dividends in team scalability.

### The Reversibility Check

**This is the part no comparison post covers.**

Every IaC tool has migration paths to every other IaC tool:
- Terraform -> Pulumi: `pulumi import` can read Terraform state files
- CDK -> Terraform: Export CloudFormation, import into Terraform
- Pulumi -> Terraform: Export state, import resources
- Any tool -> Any tool: At worst, you `import` existing cloud resources into the new tool

**None of these decisions are permanent.** The real cost of switching is not technical -- it is the team knowledge and module library you have built. That cost grows over time, which is why making a good-enough decision now matters more than making a perfect decision in three months.

**Rule of thumb:** If you are spending more than one week deciding, you are overthinking it. Pick the option that matches your team's existing skills. Build for 6 months. Revisit with real production data. The data will make the next decision obvious.

---

## Container Orchestration Preview: "Do You Actually Need Kubernetes?"

Before you decide between ECS and EKS, answer this question honestly:

### The Kubernetes Necessity Check

| Question | If Yes | If No |
|----------|--------|-------|
| Do you have 3+ microservices that need independent scaling? | K8s might be justified | Skip K8s |
| Does your team have at least one engineer with production K8s experience? | K8s might be justified | Skip K8s |
| Is your monthly cloud budget above $5,000? | K8s is financially viable | ECS Fargate or Lambda |
| Do you need multi-cloud workload portability? | K8s is the only real option | Cloud-native services are fine |
| Are you running ML/AI workloads that need GPU scheduling? | K8s (EKS) for GPU node pools | Probably do not need K8s |

**If you answered "No" to 3+ of these questions: you do not need Kubernetes.**

Use ECS Fargate (for container workloads) or Lambda (for event-driven workloads). You will save money, operational complexity, and late-night debugging sessions.

**The number most people do not know:** EKS costs $2,400/year just for the control plane ($0.10/hr x 24hrs x 365 days) -- before you run a single container. ECS has no control plane cost. For a small team with a tight budget, that $2,400 buys a lot of ECS tasks.

> The full container orchestration decision tree (including ECS vs. EKS vs. Lambda vs. plain Docker branching by workload type, team experience, and budget) is in the complete Decision Kit.

---

## What the Full Kit Includes

The free content in this repo covers the complete IaC decision tree, the "Do you need Kubernetes?" analysis, the Jenkins migration guide, deployable ECS Fargate and Lambda templates, GitHub Actions workflows, and two end-to-end architecture examples.

**The full DevOps Decision Kit ($29) adds:**

- Complete decision trees for container orchestration, CI/CD pipelines, and monitoring stacks
- The 15-point AI infrastructure review checklist (for Copilot/ChatGPT-generated Terraform)
- Cost-at-scale analysis for monitoring stacks (the Datadog pricing surprise)
- Deployable EKS, Pulumi, GitLab CI, and Prometheus+Grafana templates
- "When to switch" reversibility analysis for every decision
- Multi-cloud end-to-end architecture example

One payment. Yours forever. 30-day money-back guarantee.

**[Get the Full DevOps Decision Kit -- $29](link)**

---

## Stay Updated

The infrastructure landscape changes. Decision trees should too.

Sign up for the DevOps Decision Kit newsletter and get:
- Updates when decision trees are revised (new tools, changed pricing, shifted ecosystems)
- One real infrastructure decision breakdown per week -- what was chosen, why, and what happened
- Early access to new decision frameworks before they hit the repo

**[Subscribe for decision tree updates](link)**

No spam. No "10x your DevOps" marketing. Just infrastructure decisions, analyzed.

---

## About the Author

Built by an engineer who designed production infrastructure for:
- **Qortex** -- complex backend systems handling real production traffic
- **MindMirror** -- enterprise federated GraphQL platform + Expo mobile application
- **24-week cybersecurity curriculum** -- built and ran at a company doing $60MM+ ARR

Previously the 13th most popular author on Scotch.io (the go-to frontend education platform before its acquisition). That background means this is not just a practitioner writing docs -- it is a proven technical educator who builds production systems.

Every decision tree in this repo traces back to a real infrastructure decision made under real production pressure. Not hypothetical. Not theoretical. Real teams, real budgets, real consequences.

---

## Contributing

This repo gets better when practitioners share their decisions.

**How to contribute:**

- **Report a scenario the trees do not cover:** Open an issue using the [scenario request template](.github/ISSUE_TEMPLATE/scenario-request.md). Describe your team size, budget, stack, and what you need to decide. I will add a branch or clarify an existing one.
- **Share your decision outcome:** Used a decision tree and deployed? Tell me what happened. Open an issue using the [decision feedback template](.github/ISSUE_TEMPLATE/decision-feedback.md). Real-world feedback makes the trees more accurate.
- **Submit a case study:** If you used the Decision Kit to make an infrastructure choice and have 6+ months of production data, I would love to include your case study (anonymized if needed). This is the highest-value contribution.
- **Fix errors:** Found a factual mistake, outdated pricing, or broken template? PRs welcome.

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

MIT License. See [LICENSE](LICENSE) for details.

The decision trees, templates, and documentation in this repo are free to use, modify, and share. The [full DevOps Decision Kit](link) is a paid product that includes additional decision trees, templates, and analysis not available in this repo.
```
