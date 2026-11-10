---
name: computer
description: Set up the computer you are working on to run the person's TaskDeck agents: install and pair the TaskDeck runner, add their repositories, start it at log-in and put the computer on their agents' Runs on. Use it whenever the person asks to run their TaskDeck agents on this computer, to set up or pair a computer, to install the TaskDeck runner, or to add this machine to an agent's Runs on, even if they do not say "runner".
argument-hint: [agent]
---

Set up the computer this session runs on for the person's TaskDeck agents. Run every command yourself. Ask the person only for the two things you cannot do: approving the pairing code in TaskDeck, and accepting Claude Code's trust prompt in each folder. Ask questions as multiple-choice questions with the default pre-selected where this client has them, plain text otherwise.

0. Make sure TaskDeck is connected. If the TaskDeck tools (`who-am-i`, `list-computers`, `update-agent`) are available in this session, go to step 1. If they are not:
   - Add TaskDeck to Claude Code's MCP servers yourself by editing the file, not with the `claude` command: Claude Desktop has none. If `~/.claude.json` has no `taskdeck` server under `"mcpServers"`, add one with `"type": "http"` and `"url": "https://mcp.taskdeck.me/mcp"`, and change nothing else in that file.
   - A session does not load a server added while it is open. Tell the person in one sentence: open a new session, run /mcp there to sign in to TaskDeck, then run /taskdeck:computer again. Stop here.
1. Call the TaskDeck tool `who-am-i`. If the connection acts as an AI agent, stop and tell the person: setting up a computer needs a connection that acts as them.
2. Check the computer: `claude --version` and `claude auth status`, `git --version`, and `docker --version`, which matters only for repositories with a Docker Compose file. If Claude Code is missing or signed out, stop and say what to fix.
3. If `taskdeck-runner version` fails, install the runner. On macOS or Linux run `curl -fsSL https://taskdeck.me/downloads/runner/latest/install.sh | sh`. On Windows, ask the person to download it from TaskDeck: Profile → Computers → Add computer. When the installer says its folder is not on PATH, call the runner by its full path from then on.
4. Run `taskdeck-runner status`. Unless it says the computer is approved, run `taskdeck-runner pair` in the background so you can read its output while it waits. It shows a code and opens TaskDeck's page for approving it. Tell the person the code, and wait until it says the computer is paired.
5. Ask which repositories the agents may work in, offering the folder this session is in when it is a git repository with a remote, or each repository in it when it holds several. Run `taskdeck-runner add` with each folder chosen.
6. Run `taskdeck-runner status`. For each folder not listed as trusted, ask the person to run `claude agents` in that folder, choose "Yes, I trust this folder" and press Ctrl+C. Check the status again once they say it is done.
7. Run `taskdeck-runner install-service`, then `taskdeck-runner status`, and check that the runner is running.
8. Call `list-computers` to find this computer among the person's own, and `list-agents` for their agents. If the person gave an argument, put the computer on the Runs on of the agent named "$ARGUMENTS"; otherwise ask which agents should run on it. For each agent, call `read-agent`, then `update-agent` with `runs_on` set to its current computers with this one added at the end.
9. Tell the person in a few sentences: the computer's name, each folder and whether it is trusted, that the runner starts at log-in, and the agents that now run on it. The computer's page in TaskDeck, under Profile → Computers, shows any step still left.
