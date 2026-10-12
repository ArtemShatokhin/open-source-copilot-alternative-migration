# Self-hosting the Copilot migration: the open-source Kortix stack

Kortix is open source, and self-hosting it is free. The Microsoft Copilots run in Microsoft's cloud, reached through Microsoft's apps and cloud endpoints. Kortix runs as one Docker Compose stack on a Linux box you control, or on Kortix Cloud, in your VPC, or on your own on-prem network. Self-hosting fits the team whose security review requires the agent layer on its own network.

Self-hosting does not mean rebuilding the migration. The repo you wrote in the [migration runbook](migrate-from-microsoft-copilot-to-kortix.md) is the same repo. You point the platform at it and run it on infrastructure you own.

## Why self-host the migration

A security review asks four questions about any AI tool: where the code runs, where the data goes, who can read the implementation, and what happens when the vendor changes terms. Microsoft documents no self-host path for Copilot, no way to bring your own model provider, and no published source, so those questions are answered by Microsoft's commitments rather than by your own infrastructure. Self-hosting Kortix answers them directly: the stack is in your network, the model key is yours, the code is open source, and the configuration is a repo you keep.

## What the stack is

Kortix runs as one Docker Compose stack. It holds the frontend, the API, the LLM gateway and the Supabase distribution. Agent sessions do not run on this stack. A session runs on a separate sandbox provider, which the platform reaches over the network. One isolated sandbox per session, and each session has its own isolated machine and branch.

Keeping the control plane and the sandboxes apart is what lets you scale the agents without scaling the stack you patched and reviewed.

## Three commands

The Kortix self-hosting docs give the full sequence: point DNS at the box, open ports 80 and 443, then start the stack. While it comes up, `kortix self-host status` reports the state, `kortix self-host logs` shows the output, and `kortix self-host doctor` names any missing prerequisite.

Bookmark `kortix self-host doctor`. It is the fastest way to see which prerequisite is missing on a new box.

## What the first run asks

The first run asks six things and no model key. GitHub connects later in the dashboard, under Settings, then Git. Models are connected in the app, and you bring your own key. This keeps a fresh install free of secrets until you are ready to point it at a provider.

## Models stay yours

Kortix runs any model provider with your own keys. You can also use the ChatGPT plan you already pay for, or sign in with your OpenCode Console account for OpenCode Zen and Go. A self-hosted stack never needs a Kortix-managed model to work; the LLM gateway in the stack handles provider authentication for you.

This is the mirror image of Copilot, where the models are Microsoft's call. On a self-hosted Kortix, the model is a line in your configuration and the key is one you can rotate.

## The sandbox provider is separate

Agent sessions run on a separate sandbox provider, not on the Docker Compose stack. That provider is where each session's isolated machine and branch come from. Plan for outbound connectivity from the stack to the provider, and from the provider back to your instance so the sandbox can call back.

## What it is not

Self-hosted Kortix needs the internet. The install pulls images from docker.io, and the instance must be reachable so the sandbox can call back. It is not air-gapped, and no copy should claim it is. If your requirement is a fully disconnected network, that calls for a scoping conversation.

## Moving Graph grounding to a self-hosted stack

Microsoft Graph grounding is a managed service, and it does not come with you. On self-hosted Kortix you rebuild it from connectors and memory files.

- For each system Graph read from, add a connector. Kortix reaches 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP. Connector credentials are brokered server-side and never enter the machine.
- For the context Copilot accumulated, write memory files into the repo. They are plain files, versioned with everything else.
- For each tool that should be watched, set the call to Allow, Ask or Block, down to the arguments of the call. Approval gates are off until you set them.

Because the grounding is now files and connections you own, a security review can read it rather than take a vendor's word for it.

## A staged rollout

1. Stand the stack up on a test box and confirm `kortix self-host doctor` is clean.
2. Connect the repo and start a read-only session against one connector.
3. Add the memory files and a second connector, and check an answer that needs both.
4. Move one real job from Microsoft 365 Copilot and watch the change requests it files.
5. Turn on the review gate for that job, then repeat for the next job.

Keep the Microsoft 365 Copilot license until the parallel run on the new stack beats it on the jobs you moved. The two systems coexist during the transition.

## Backups and evaluation mode

Run the self-hosted stack in an evaluation mode first, so a failed test does not touch production data, and back up the database and the repo before you turn real work loose. The Kortix self-hosting docs cover the evaluation mode and the backup procedure. The repo is the part you can always re-clone; the database is the part to schedule.

## Start

Point one box at a repo you own, run the three commands, and connect a single job. The [self-hosting page](https://opensourcecopilotalternative.com/self-hosting) has the short version, and the [FAQ](https://opensourcecopilotalternative.com/faq) answers the common questions about self-hosting, models and approval gates.
