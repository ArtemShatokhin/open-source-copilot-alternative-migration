# Migrate from Microsoft Copilot to Kortix: the open-source migration runbook

Kortix is open source, and this runbook moves a working Microsoft Copilot deployment onto it. The task is re-homing the three things a Copilot deployment actually holds: the agents that do the work, the grounding that feeds them, and the permissions that decide who and what may act. In Kortix those three become files in one git repo you own, and every change to them lands through a change request a person approves.

Work through the phases in order. Each one ends with a state you can check before moving on. If you only need the definition of the product you are leaving, read [What is Microsoft Copilot](what-is-microsoft-copilot.md) first.

## What moves, and what does not

A Copilot deployment is configuration inside Microsoft's products. Agents are built and managed in Microsoft's interfaces, grounding is a managed service, and access is scoped by the signed-in user's Microsoft Entra permissions. Microsoft documents no self-host option, no way to bring your own model provider, and no published source. The stack is Microsoft's, and the configuration follows it.

Kortix inverts the last sentence. A Kortix project is a git repo, and the repo is the company: configuration and accumulated state in one place, all text, readable by a person and editable by an agent. An agent is `agents/<name>.md` (behavior) plus an `agents.<name>` block in `kortix.yaml` (governance). Company memory is files that accumulate. Connectors, triggers and secret names live in the same repo. You can `grep` the whole company, diff any change, and roll any part of it back.

So each Copilot construct has to be rewritten as the file that holds it in Kortix. The phase order follows the dependency: repo first, then agents, then grounding, then permissions, then the way work reaches the default branch.

## The concept map

| In Microsoft Copilot | How it works there | In Kortix | How it works here |
| --- | --- | --- | --- |
| Agent Builder agents | Built in natural language, work-grounded | `agents/<name>.md` plus a `kortix.yaml` block | Behavior in markdown, governance in the manifest |
| Microsoft Graph and Work IQ | Automatic grounding in company content | Memory files plus connectors | Context in files, reach into the same systems |
| Copilot Studio | Studio to build and manage agents at scale | Skills, agents, connectors, per-call policy | Each agent and skill is a file you own |
| Microsoft 365 admin and Entra | Controls who may edit and run agents | Roles, groups, per-resource permissions | People and agents are both principals |
| Copilot connectors | Reaches external data sources | 3,000+ apps plus MCP, OpenAPI, Postman, GraphQL, raw HTTP | One scoped token per connector, brokered server-side |
| Auto model selection | Microsoft weighs accuracy, speed and cost | Any model provider with your own keys | You pick the provider and hold the key |
| Replies in the Microsoft 365 apps | Output appears in Word, Excel, Outlook | Change requests toward `main` | Session work lands through a review gate |
| Microsoft cloud only | Commercial and select sovereign clouds | Kortix Cloud, your VPC, or on-prem | Self-host is free |

Each row is a task, not a slogan. An Agent Builder agent becomes a markdown file. A Graph source becomes a connector plus the memory files that keep what it learned. An Entra role becomes a grant in the manifest and a per-resource permission.

## Phase 1: Pick the boundary

Decide which jobs move and which stay. Microsoft's in-app assistance inside Word, Excel, PowerPoint, Outlook and Teams is genuinely strong, and Microsoft documents it as the point of the product. Leave that where it is if your team lives in Office documents.

Move the work that is a process rather than a keystroke: ticket triage, reply drafting, CRM updates, research, reports, scheduled digests, anything an agent does end to end. Those are the jobs where the ownership difference pays off, because the agent needs your tools, your company memory, and a place for its output to land.

Write the boundary down. A table of "job, moves or stays, why" is enough, and it doubles as the rollout plan.

## Phase 2: Stand up one project

Kortix installs with three commands. `kortix init` scaffolds a project and writes `kortix.yaml`; `kortix ship` pushes the repo and brings it live. If you would rather run it yourself, `kortix self-host start` brings up one Docker Compose stack on a Linux box you control. Self-host is free, and the [self-hosting guide](https://opensourcecopilotalternative.com/self-hosting) walks through the first run.

A minimal project starts like this:

```yaml
kortix_version: 2
default_agent: kortix

project:
  name: northwind-ops
  description: Company operations, moved from Microsoft 365 Copilot.

agents:
  support-triage:
    file: agents/support-triage.md
    connectors: all

triggers:
  - slug: daily-digest
    type: cron
    cron: '0 0 9 * * 1-5'
    prompt: Summarize yesterday's support tickets and open a change request with the digest.
```

`kortix.yaml` declares the machine image, the connectors and the triggers, and the secret names and grants. Secret values never appear in the repo. Channels are not declared here: a connected channel is its own connection, configured where you connect it.

Checkpoint: the repo clones, `kortix.yaml` parses, and a first session starts.

## Phase 3: Rebuild each Copilot agent as a file

For every Agent Builder agent you are moving, write two things. First, `agents/<name>.md`, the behavior: what the agent does and how it does it, in plain markdown. Second, an `agents.<name>` block in `kortix.yaml`, the governance: which connectors it may reach.

```markdown
# Support triage

You triage the incoming support queue for Northwind.

Read new tickets, group them by product area, and draft a first reply for each.
Flag anything that mentions a data loss or a security issue for a person.
Open a change request with the drafts. Do not send anything yourself.
```

The scoping field in the governance block is `permission`, not `tools`, and behavior keys do not belong in `kortix.yaml`. An agent is a stock OpenCode agent file plus its governance block, and both are versioned like any other code.

Checkpoint: the agent answers in a session, reaches only the connectors the block allows, and shows up in the repo as two files a person can review.

## Phase 4: Rebuild grounding as memory and connectors

Work IQ and Microsoft Graph ground Copilot in company content automatically, scoped to each person's permissions. Reproduce that with two mechanisms in Kortix.

Connectors replace the reach. A connector is one-click access into an app, an API or an MCP server. Kortix reaches 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP, and connector credentials are brokered server-side and never enter the machine. Point a connector at each system Graph read from.

Memory files replace the accumulation. What the old system inferred about your style, habits and recurring context becomes plain files in the repo that agents read and update. Microsoft's own migration note for individuals says chat history and memory do not transfer automatically; the same is true here, and it is why memory is rebuilt as files rather than imported as a blob.

Checkpoint: an agent answers a question that needs two connected systems plus a memory file, and you can point at the file that supplied the answer.

## Phase 5: Move permissions into the repo

Copilot scopes access by the signed-in user's Microsoft Entra permissions. Kortix governs people and agents as principals in their own right.

- Per-resource permissions for people and agents, with roles, groups, and an audit trail.
- Each tool call is set to Allow, Ask or Block, down to the arguments of each call. An Ask holds the call until a person approves it, then the agent resumes.
- Approval gates you set. They are off until you set them.
- SAML 2.0 single sign-on and SCIM 2.0 for enterprise identity, and secrets encrypted at rest with a key per project.

The grant that decides whether work can land by itself is `project.gitops.merge`. A session can never merge a change request it opened itself, and an admin grants the capability only where it belongs. Map each Entra role that mattered to the closest Kortix role, then name the tool calls that should ask.

Checkpoint: a member of each role sees the resources their role allows and no more, and a dangerous call pauses for approval.

## Phase 6: Connect the channels

Microsoft Teams is live in Kortix. So is Slack. Email is experimental, per project. Add the bot to the channel your team already works in, invite it, and mention it with a task. The message starts a session; the agent works on its own cloud computer with the connected tools and replies in the same thread.

This is the step that decides adoption. The first useful session should arrive in the tool a person already has open, not in a new tab.

Checkpoint: a task typed in Teams starts a session and returns the result in the thread.

## Phase 7: Add triggers for scheduled work

Anything that ran on a schedule in the old setup becomes a trigger. Kortix has two types: `cron` and `webhook`. A cron trigger fires on the clock; a signed webhook fires on an event. A trigger starts a session with no person present, and the agent's work still lands through the same review gate.

Checkpoint: a trigger fires, opens a session, and files a change request by morning.

## Phase 8: Turn on the review gate and run the first parallel workload

Session work reaches `main` through a change request. Merge is default-deny for agents. A person reads the diff and decides. An agent can edit its own configuration on its session branch and propose the change, so the system improves one reviewed change at a time.

Run one real job at volume. Each session gets its own isolated sandbox and its own branch, so the same config can run many sessions at once without them touching each other. Watch the diffs, not the logs: the change requests are the result.

## How to verify the migration

- **Ownership.** Clone the repo on a clean machine and read every agent, skill, memory file and trigger. Nothing important lives outside it.
- **Isolation.** Start two sessions and confirm each has its own sandbox and branch.
- **Governance.** Confirm a tool call you set to Ask pauses, and that a session cannot merge its own change request.
- **Grounding.** Ask for a fact that lives in a memory file and one that lives behind a connector, and point at the source of each.
- **Review.** Confirm every change to `main` arrived as a change request a person approved.
- **Model and hosting.** Confirm the project runs on the model provider and the infrastructure you chose.

## Where to get help

The [FAQ](https://opensourcecopilotalternative.com/faq) answers the short questions about self-hosting, models, approval gates and cost. The [open-source Copilot alternatives guide](https://opensourcecopilotalternative.com/best-open-source-copilot-alternatives) places Kortix in the wider field, and the [Kortix vs Copilot comparison](https://opensourcecopilotalternative.com/kortix-vs-copilot) works through the head-to-head.
