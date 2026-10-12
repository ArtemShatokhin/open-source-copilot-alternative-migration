# What is Microsoft Copilot, and why Kortix is the open source alternative

Microsoft Copilot is Microsoft's AI assistant for work, and it is not open source. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector live in one git repo you own, and the agents work on real cloud computers. The definition draws on Microsoft's own product pages and Microsoft Learn, so the decisions in the migration runbook rest on facts you can check.

## The short definition

Microsoft Copilot is Microsoft's AI assistant for work and personal use, built into Word, Excel, PowerPoint, Outlook and Teams. Microsoft's own product page calls it "AI built for work" and says it connects with your work content and the apps you already use, brings together the latest AI models, and helps you create finished work rather than just answers. It runs across desktop, mobile and web, and Microsoft delivers it as a hosted service on Microsoft 365 plans. Microsoft ships more than one product under the name, so the first job is telling them apart.

## What Microsoft ships under the Copilot name

| Product | What it is | Where it runs | Open source |
| --- | --- | --- | --- |
| Microsoft 365 Copilot | Assistant inside Word, Excel, PowerPoint, Outlook and Teams | Microsoft's cloud | No |
| Copilot Studio | Studio for building and managing custom agents | Microsoft's cloud | No |
| GitHub Copilot | AI coding agent for developers | GitHub's cloud | No |

Microsoft's home page lists the three lanes directly: GitHub Copilot for code, Copilot Studio for building agents, and Microsoft 365 Copilot for work inside its apps. The work layer is the target of this migration, and Copilot Studio is treated as the agent-building option inside the same Microsoft estate.

## The three Microsoft 365 Copilot license tiers

Microsoft Learn splits the work product into three licenses, and the split matters because grounding differs between them.

| License | Grounding | Copilot in Office apps | Administration |
| --- | --- | --- | --- |
| Copilot Chat (Basic) | Web data, plus content you provide | No add-on; chat in Outlook and Teams | Basic reporting |
| Microsoft 365 Copilot (Basic) | Web data | Standard access in Word, Excel, PowerPoint, OneNote | Basic reporting |
| Microsoft 365 Copilot (Premium) | Web and work data via Graph and Work IQ | Priority access to the full experience | Advanced analytics |

Copilot Chat (Basic) is the standalone baseline included with eligible Microsoft 365 licenses. It primarily uses web data, and it can use organizational content only when you provide it with the prompt, work with open content in Teams or Outlook, or use a pay-as-you-go agent. Microsoft 365 Copilot (Premium) adds automatic grounding in organizational content, pulls it without manual uploads, and includes access to Microsoft Copilot Cowork through usage-based billing. Microsoft Learn also notes that declarative agents grounded in instructions and public websites come with Copilot Chat, while access to custom or other agents is pay-as-you-go.

## How grounding works

All three licenses are powered by the same three things: large language models for understanding and generation, grounding in web or organizational data, and access scoped by user permissions. The organizational grounding comes from Microsoft Graph and Work IQ. Work IQ is Microsoft's intelligence layer that connects context, workflows and relationships, drawn from Microsoft 365 content and metadata, line-of-business data ingested through Copilot connectors, and a semantic index, per Microsoft's business pricing page.

Microsoft protects prompts and responses with Enterprise Data Protection when people sign in with Microsoft Entra accounts, and Microsoft Learn lists the same protection and the European Union Data Boundary across all three licenses. Access is always scoped to each person's permissions, which is Microsoft's answer to the governance question.

## Building agents

Microsoft offers two ways to build agents. Agent Builder turns expertise into work-grounded agents in natural language, directly inside Copilot. Copilot Studio is the way to extend and manage those agents at scale, per Microsoft's Copilot page. Copilot Studio is a graphical, low-code platform for creating and managing custom agents, workflows and apps. It reaches external systems through 1,500+ prebuilt data connectors and MCP servers, and it adds application lifecycle management with version control, release pipelines, approval gates, environment promotion and audit logs that flow into Microsoft Purview. It also supports multi-agent orchestration and role-based access controls.

Copilot Studio is billed by consumption. Microsoft sells tenant-wide Copilot Credit packs of 25,000 credits at $200.00 per pack per month, or a pay-as-you-go meter with no upfront commitment. Copilot Studio was formerly known as Power Virtual Agents.

## Is Microsoft Copilot open source?

No. Microsoft publishes no Copilot source code, so there is nothing to read, fork or audit, and Microsoft's pages describe a hosted service rather than a distributable product. The product is not self-hostable: Microsoft's enterprise pricing page states that Copilot is available in the commercial cloud and in select sovereign cloud environments, which is where it runs. And the stack is Microsoft's: Microsoft selects the models, as its Copilot page says when it describes Auto weighing accuracy, speed and cost per request, and its pages document no way to bring your own provider or your own keys.

## What it costs

Microsoft prices Copilot per seat, usually on top of a qualifying Microsoft 365 plan, and the entry points differ by audience. Microsoft's individual pricing page includes Copilot with Microsoft 365 Personal at $9.99 per month, with a limited free version subject to capacity and limits. Microsoft's business pricing page lists Microsoft 365 Business Standard with Copilot at $23.50 per user per month paid yearly, and the Microsoft 365 Copilot Business add-on at $21.00, discounted to $18.00 for a promotional period, for up to 300 users. The enterprise pricing page lists the Microsoft 365 Copilot add-on at $30.00 per user per month paid yearly, on an annual commitment, added to a qualifying base plan. Copilot Chat is included at no additional cost with eligible Microsoft 365 plans. Prices checked October 2026.

## GitHub Copilot is a different product

GitHub Copilot shares the name and a different job. It is GitHub's AI coding agent for developers, built into editors, the command line and GitHub itself, and GitHub prices it separately for individuals and organizations. It does not ground answers in Microsoft 365 work content, and it is not the product this migration guide replaces. If your team runs GitHub Copilot, leave it in place; the switch concerns the work layer.

## What an open-source alternative changes

An open-source alternative reverses all four of those facts. The code is published, so a security review reads the implementation rather than the documentation. The system is self-hostable, so it runs where you decide. The models are your choice, with your own keys. And the configuration is files in a git repo you own, so it is versioned, diffable and portable.

Kortix is the open-source AI Operating System that meets all four. Agents are `agents/<name>.md` plus a `kortix.yaml` block. Memory is files that accumulate. Connectors reach 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP, with credentials brokered server-side. One isolated sandbox per session, on its own branch. Session work reaches `main` through a change request a person approves. Run it on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host for free.

## Where Kortix fits

If Copilot's job is assistance inside Microsoft 365, Copilot is the right tool and no migration is needed. If the job is an agent layer the company owns, with the configuration in git and a review gate before anything lands, Kortix is built for it. The [migration runbook](migrate-from-microsoft-copilot-to-kortix.md) maps each Copilot concept to its Kortix construct and gives the order to move in, the [comparison](microsoft-copilot-vs-kortix.md) works through the head-to-head, and the [FAQ](https://opensourcecopilotalternative.com/faq) answers the short questions.
