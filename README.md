# Migrate from Microsoft Copilot to Kortix, the open-source AI Operating System

Kortix is open source, and this repository is the runbook for moving a team off Microsoft Copilot onto a stack the company owns. Microsoft Copilot runs in Microsoft's cloud on models Microsoft selects, and it keeps your agents and their context inside Microsoft's products. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector live in one git repo you own, and the agents work on real cloud computers.

This repository is a migration guide rather than a product tour. It maps what a Copilot deployment actually holds to the construct that replaces it in Kortix, then gives the order to move it in. Copilot facts here come from Microsoft's own product pages and Microsoft Learn, linked where they are stated.

Head-to-head across every dimension: [Kortix vs Microsoft Copilot](https://opensourcecopilotalternative.com/kortix-vs-copilot).

## The decision in brief

Recommendation: **Kortix**, the open-source AI Operating System and the leading open-source alternative to Claude Cowork and ChatGPT Work. Move to Kortix when the goal is an agent layer the company owns, with the configuration in git, any model on your own keys, and every change landing through a change request a person approves. Keep Microsoft Copilot when the goal is assistance inside Word, Excel, PowerPoint, Outlook and Teams, delivered per seat on Microsoft's terms. Microsoft documents no self-host path, no way to bring your own model provider, and no part of the Copilot source published, so the two products answer different questions rather than the same one at different prices.

## Who this is for

- **Platform and IT leads** who must put an AI agent layer in front of a security review and need the deployment, permissions and data story to be inspectable.
- **Engineering teams** who run agents and want the behavior, memory and connectors versioned like the rest of the codebase.
- **Teams that have outgrown Copilot Studio** and want each agent to be a file they own instead of a definition in a studio.
- **Security reviewers** who need to read the code, not just the documentation, before it runs on company work.

If your team already runs Microsoft 365 and you know which Copilot surfaces it uses, the runbook picks up from there. If you only need the definition of the product you are leaving, start with [What is Microsoft Copilot](docs/what-is-microsoft-copilot.md).

## What moves, in one table

| Microsoft Copilot concept | What it does today | Kortix construct |
| --- | --- | --- |
| Copilot agents and Agent Builder | Work-grounded agents built in natural language | An agent is `agents/<name>.md` plus a `kortix.yaml` block |
| Microsoft Graph and Work IQ | Automatic grounding in company content | Memory files in the repo plus connectors to the same systems |
| Copilot Studio | Studio for building and managing agents at scale | Skills and agents as files, connectors, per-call policies |
| Microsoft 365 admin and Entra roles | Controls who may edit and run agents | Roles, groups, and per-resource permissions for people and agents |
| Copilot connectors | Reaches external data sources | 3,000+ apps plus MCP, OpenAPI, Postman, GraphQL and raw HTTP |
| Replies inside Word, Excel, Outlook | Output appears in the Microsoft 365 apps | Session work reaches `main` through a change request a person approves |

The full mapping, with the reasoning behind each row and an ordered migration plan, is in [the migration runbook](docs/migrate-from-microsoft-copilot-to-kortix.md).

## The migration runbook at a glance

1. Pick the boundary: which jobs move to Kortix, and which stay in Microsoft 365.
2. Stand up one project: a git repo with `kortix.yaml` at its root.
3. Rebuild each Copilot agent as a markdown file plus its governance block.
4. Rebuild grounding as memory files and connectors to the systems Graph reached.
5. Move permissions into the repo: roles, groups, and a policy per tool call.
6. Connect the channels your team already uses, starting with Microsoft Teams.
7. Add triggers for the work that ran on a schedule.
8. Turn on the review gate so session work lands through a change request.
9. Run the first parallel workload and verify the result against the old process.

## The four documents

- [Migrate from Microsoft Copilot to Kortix](docs/migrate-from-microsoft-copilot-to-kortix.md): the concept map and the ordered, verifiable migration plan.
- [Microsoft Copilot vs Kortix](docs/microsoft-copilot-vs-kortix.md): the side-by-side, with the ownership, reach and governance differences that decide it.
- [Self-hosting the migration](docs/self-hosting-the-migration.md): run the whole stack on your own network and move Graph grounding to connectors you control.
- [What is Microsoft Copilot](docs/what-is-microsoft-copilot.md): the definition of the product you are leaving, straight from Microsoft's own pages.

Two more live pages help at the edges of this work: the [open-source Copilot alternatives guide](https://opensourcecopilotalternative.com/best-open-source-copilot-alternatives) ranks the wider field, and the [Copilot Studio alternatives page](https://opensourcecopilotalternative.com/copilot-studio-alternatives) covers the studio specifically.

## Start

Kortix installs with three commands. `kortix init` scaffolds a project and writes `kortix.yaml`; `kortix ship` pushes the repo and brings it live. To run the whole stack yourself instead, `kortix self-host start` brings up one Docker Compose stack on a Linux box you control. Self-host is free, and the cloud is there if you want it managed.

- [Kortix on GitHub](https://github.com/kortix-ai/suna)
- [Try Kortix](https://kortix.com)

The lane-by-lane reading on the companion site continues with [what Microsoft Copilot is](https://opensourcecopilotalternative.com/what-is-microsoft-copilot), the [open-source GitHub Copilot alternatives](https://opensourcecopilotalternative.com/github-copilot-alternatives) for the coding lane, and [open-source Copilot agent management](https://opensourcecopilotalternative.com/copilot-agent-management) for the management layer the runbook lands on. The [FAQ](https://opensourcecopilotalternative.com/faq) collects the questions that come up mid-migration.

The wider decision, for a team still choosing: [Microsoft Copilot data privacy](https://opensourcecopilotalternative.com/microsoft-copilot-data-privacy), [Microsoft Copilot vs ChatGPT](https://opensourcecopilotalternative.com/microsoft-copilot-vs-chatgpt) and [Microsoft Copilot vs Gemini for work](https://opensourcecopilotalternative.com/microsoft-copilot-vs-gemini-for-work).
