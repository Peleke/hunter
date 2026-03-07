---
type: pitch
date: 2026-02-08
status: draft
tags:
  - hunter/pitch
  - hunter/domain/devops-education
  - hunter/opportunity/post-tutorial-gap
offer_ref: "devops-decision-kit-2026-02-08"
persona_ref: "devops-post-tutorial-gap-2026-02-08"
swot_ref: "devops-decision-kit-swot-2026-02-08"
decision_ref: "domain-selection-2026-02-08"
signal_scan_ref: "initial-4-domain-survey-2026-02-08"
---

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

---

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

---

## References

- **Offer Spec**: [[Admin/Product-Discovery/Offers/devops-decision-kit-2026-02-08]]
- **Persona**: [[Admin/Product-Discovery/Personas/devops-post-tutorial-gap-2026-02-08]]
- **SWOT Analysis**: [[Admin/Product-Discovery/SWOT/devops-decision-kit-swot-2026-02-08]]
- **Decision Log**: [[Admin/Product-Discovery/Decisions/domain-selection-2026-02-08]]
- **Signal Scan**: [[Admin/Product-Discovery/Signal-Scans/initial-4-domain-survey-2026-02-08]]
- **Launchpad Branch**: [devops-decision-kit](https://github.com/Peleke/launchpad/tree/devops-decision-kit)
