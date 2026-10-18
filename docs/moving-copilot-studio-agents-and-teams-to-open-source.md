# Moving a team off Copilot to the open-source alternative: surface first, agents second

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. For a team that runs on Microsoft Teams, the migration order decides whether it sticks. Port every agent first and it stalls. Get the agent answering in the chats and channels where the team already works, then move the two or three agents people actually mention.

## Why agent-first rollouts stall

The default plan rebuilds every Copilot agent before anyone touches the new system, which takes weeks. The launch hands the team a catalog of agents in a tool nobody has open, while Copilot keeps answering in the pane people already use. Adoption never arrives, and the migration becomes a second system to maintain.

The cause is placement. Work in a Teams-first company happens in chats, channels and meeting follow-ups. A replacement that answers elsewhere asks every person to change where they work first. Most people decline.

## What Copilot does in the thread

Copilot in Microsoft Teams is Microsoft's assistant for chats, channels and meetings. Microsoft's guide describes it as a way to get up to speed on conversations, reviewing the main points, action items and decisions. Copilot opens in a side pane in a chat or channel and answers over the thread it sits in, with a 30-day history by default. A channel thread needs at least 1,000 characters before Copilot generates a summary, and Microsoft lists what it cannot summarize: images, loop components, or shared files ([Microsoft's guide](https://support.microsoft.com/en-us/teams/copilot/how-to-use-microsoft-365-copilot-in-teams-chats-and-channels), checked October 2026).

Any Microsoft Copilot for Teams alternative starts behind: Copilot already sits in the pane. The assistant reads the thread and hands back shorter reading. The follow-up work an agent could do, the ticket or the reply, still sits in other systems.

## Surface first: the agent in the channel

Kortix's README documents the doorway: the team uses it in the web app, Slack and Microsoft Teams, or from the CLI. Add the app to Slack, invite the bot to a channel, and mention it with a task. The message starts a session: the agent works on its own cloud computer with the project's connected tools, replies in the same thread, and follow-ups stay in that session. Microsoft Teams works the same way ([Kortix on GitHub](https://github.com/kortix-ai/suna)). Slack and Microsoft Teams are live. Email is experimental, per project.

Where the session runs stays your call. Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free. The [self-hosting guide](https://opensourcecopilotalternative.com/self-hosting) walks the first run. The rollout cares about one thing: the answer lands where the team reads.

Governance belongs in the setup. Approval gates you set. Off until you set them. When a connector call needs a decision, a card with the choices posts in the chat or thread where the request was made (the [Microsoft Copilot for Teams page](https://opensourcecopilotalternative.com/microsoft-copilot-for-teams) documents the flow).

## Agents second: port the agents with real usage

Once the surface works, count the agents by what people actually mention. A few weeks of thread history gives an honest list: two or three carry the real load, and the rest were experiments. Port those, one at a time, and leave the rest. An agent nobody talks to is not worth porting.

Porting is also where ownership gets decided, and [Kortix's blog](https://kortix.com/blog/the-test-of-sovereignty) puts the reason plainly: "If your workflows are embedded in a proprietary vendor's toolchain, you do not own them." An agent rebuilt as a markdown file plus a governance block in `kortix.yaml` is something the team can read, diff and pass on. Session work reaches `main` through a change request. Merge is default-deny for agents. The lane-by-lane mapping is in the [open-source migration guide](https://opensourcecopilotalternative.com/migrate-from-microsoft-copilot), and Copilot Studio, where Microsoft's agents get built and managed, is compared with Kortix on the [Copilot Studio vs Kortix page](https://opensourcecopilotalternative.com/copilot-studio-vs-kortix).

The rollout should also decide what the team shares. [Kortix's comparison with Claude Cowork](https://kortix.com/blog/kortix-vs-claude-cowork) states the per-person limit directly: "In Cowork, each person's agents, skills, and context stay on their own desktop." The question applies to any replacement: what one person teaches the agents should reach the whole team. In Kortix that teaching lands as files in the shared repo, so the next agent starts with what the last one learned.

## The test to run first

Give the order a deadline. Add the app to one channel the team already uses, link each person's account, and hand it real tasks for two weeks. Judge the rollout by what happens in that channel: whether people keep asking there, and whether answers arrive where the questions were asked.

When the surface has earned its keep, port the agents with real usage and run each one against the old output for a cycle, retiring a Copilot lane only when the new side holds it. The runbook in this repo walks each mapping step; the order keeps them from becoming the whole project.

A Copilot migration succeeds when the replacement answers where the team works and the agents follow the usage. Add the app to one channel this week and see whether the team keeps asking there. The rest of the switch is mapped at [opensourcecopilotalternative.com](https://opensourcecopilotalternative.com).
