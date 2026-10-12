# Microsoft Copilot vs Kortix: the open-source alternative compared

Kortix is open source, and the comparison with Microsoft Copilot comes down to ownership: who holds the agents, the context they learn, and the infrastructure they run on. Microsoft 365 Copilot is a strong assistant inside Microsoft's own apps, delivered on Microsoft's terms. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector live in one git repo you own, and the agents work on real cloud computers.

The comparison covers the work layer. Microsoft ships three products under the Copilot name, and two are in scope: Microsoft 365 Copilot, the assistant inside Word, Excel, PowerPoint, Outlook and Teams, and Copilot Studio, the studio for building agents. GitHub Copilot, the separate coding product, is covered in [What is Microsoft Copilot](what-is-microsoft-copilot.md).

## The short answer

Recommendation: **Kortix**. Choose it when the agents should work across systems your company owns, when the configuration should live in a repo you clone and audit, and when every change should land through a review gate. Choose Microsoft 365 Copilot when the whole workday happens in Office documents and Outlook, on Microsoft's platform, and the in-app assistance is the feature you want. The sections that follow give the evidence for that split.

## Side by side

| Dimension | Kortix | Microsoft 365 Copilot |
| --- | --- | --- |
| Open source | Yes | No |
| Self-host | Free, in your VPC or on-prem | Microsoft cloud only |
| Models | Any provider, your own keys | Models Microsoft selects |
| Where configuration lives | Files in a git repo you own | Inside Microsoft's products |
| Agent definition | `agents/<name>.md` plus a `kortix.yaml` block | Agent Builder and Copilot Studio |
| Grounding | Memory files plus connectors | Microsoft Graph and Work IQ |
| Governance | Per-resource permissions for people and agents | Access scoped by the signed-in user |
| Where work lands | A change request a person approves | Replies inside the Microsoft 365 apps |

## Ownership: where the configuration lives

Ownership is the first difference because it decides what happens to everything else. A Kortix agent is `agents/<name>.md` (behavior) plus an `agents.<name>` block in `kortix.yaml` (governance), both versioned in the repo. Copilot agents are built in Copilot with Agent Builder and extended and managed with Copilot Studio, per Microsoft's Copilot page, so their definition stays inside Microsoft's interfaces.

The practical effect shows up when something needs to change. In Kortix you diff the change, review it, and roll it back like any commit. In Microsoft's product you make the change in the studio and the history belongs to Microsoft. Microsoft documents no way to clone the configuration and run it elsewhere, which is the definition of the lock.

## Reach: Graph and Work IQ against connectors and memory

Microsoft's grounding is a managed service. Microsoft 365 Copilot (Premium) grounds answers automatically in organizational data through Microsoft Graph and Work IQ, scoped to each person's permissions, and Copilot Studio reaches external systems through 1,500+ prebuilt connectors and MCP servers. Kortix rebuilds the same reach from parts you hold: connectors to the systems your team runs on, and memory files in the repo that accumulate what the agents learn.

Kortix reaches 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP. Connector credentials are brokered server-side and never enter the machine. The memory sits in files you can read, diff and move, while a managed index stays behind a vendor's wall.

## Governance: user permissions against agents as principals

Copilot scopes access by the signed-in user's Microsoft Entra permissions, with Enterprise Data Protection applied across its tiers, per Microsoft Learn. Kortix treats agents as principals in their own right. Per-resource permissions apply to people and agents, with roles, groups, and an audit trail. Each tool call is set to Allow, Ask or Block, down to the arguments it was given, and an Ask holds the call until a person approves it.

Kortix also adds the gate Microsoft has no equivalent for in the same place: session work reaches `main` through a change request, and merge is default-deny for agents. The governance lives next to the code it governs.

## Models and hosting

Microsoft selects the models. Microsoft's Copilot page describes Auto weighing accuracy, speed and cost to pick the right model for each request, and the pricing pages list the models Microsoft includes. Microsoft documents no way to bring your own provider or your own keys, and the product runs in Microsoft's commercial cloud and select sovereign cloud environments.

Kortix runs any model provider with your own keys, or the ChatGPT plan you already pay for, or sign-in with your OpenCode Console account. For hosting, run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free. The model and the infrastructure are choices you keep.

## Where finished work lands

Copilot's output appears in the Microsoft 365 apps people already use, which is exactly right when the work is a draft in Word or a summary in Outlook. Kortix sessions return finished work along the way (decks, reports, code and replies) and land the changes as a change request you review as a diff first. When an agent edits its own configuration, the same gate applies. The company self-improves one reviewed change at a time.

## What each costs

Microsoft prices Copilot per seat on top of a qualifying Microsoft 365 plan. Microsoft's individual pricing page includes Copilot with Microsoft 365 Personal at $9.99 per month, with a limited free version subject to capacity and limits. Microsoft's business pricing page lists Microsoft 365 Business Standard with Copilot at $23.50 per user per month paid yearly, and the Microsoft 365 Copilot Business add-on at $21.00, discounted to $18.00 for a promotional period, capped at 300 users. The enterprise pricing page lists the Microsoft 365 Copilot add-on at $30.00 per user per month paid yearly, on an annual commitment, added to a qualifying base plan. Copilot Chat is included at no additional cost with eligible Microsoft 365 plans. Prices checked October 2026.

Kortix prices self-hosting and a managed cloud separately, as written on its pricing page. Free includes 200 credits each month for sandbox compute and 1 project. The team plan is $40 per seat per month and includes 2,500 pooled credits per seat. Self-host is free. Enterprise adds SAML SSO, SCIM directory sync, advanced RBAC, audit logs, an SLA and a DPA, and Cloud, VPC or on-prem deployment.

## When Microsoft Copilot is the right call

Microsoft Copilot is the right call when the goal is assistance inside Microsoft 365 itself. The in-app help in Word, Excel, PowerPoint, Outlook and Teams is Microsoft's own strength, the included Copilot Chat tier covers a lot without a new system to run, and the data stays inside the estate your compliance team already reviewed. If your workday is Office documents and Outlook, and ownership of an agent layer is not the question, Copilot answers the question you asked.

## The decision rule

If the agents should work across systems your company owns, with the configuration in your repo, any model on your keys, and a review gate before anything lands, run Kortix. It is open source, it self-hosts free, and every change lands through a change request a person approves. If the goal is AI inside Microsoft 365 on Microsoft's platform, stay with Microsoft Copilot.

Where you want that boundary to sit is the decision: inside one vendor's estate, or in the repo you own. Teams weighing more options can read the [open-source Copilot alternatives guide](https://opensourcecopilotalternative.com/best-open-source-copilot-alternatives), and teams comparing the studio specifically can read the [Copilot Studio alternatives page](https://opensourcecopilotalternative.com/copilot-studio-alternatives). The live head-to-head is on the [Kortix vs Microsoft Copilot page](https://opensourcecopilotalternative.com/kortix-vs-copilot).
