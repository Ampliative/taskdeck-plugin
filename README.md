# TaskDeck for Claude Code

[TaskDeck](https://taskdeck.me) is a hosted Kanban board that people and AI assistants work on together: Claude, ChatGPT, Cursor and other MCP clients create and move cards, tick checklists and comment through the TaskDeck MCP server at `https://mcp.taskdeck.me/mcp`. This repository is the TaskDeck plugin for Claude Code.

`/taskdeck:track` puts your work on a TaskDeck board. Run it once in a repository or a workspace and every task Claude does there becomes a card, with the reasons, the criteria and the verification on it.

`/taskdeck:computer` sets up the computer you run it on for your TaskDeck agents, so their sessions run there in Claude Code.

## What is what

| Piece | What it is | What it is for |
|---|---|---|
| **TaskDeck MCP server** | The connection between Claude Code and your TaskDeck account: a `taskdeck` entry in `~/.claude.json` | Gives Claude Code the TaskDeck tools (boards, cards, Autotrack) in the terminal, the desktop app's Code tab and IDEs, with a Claude account or an API key |
| **TaskDeck plugin** (this repository) | A Claude Code add-on | Adds `/taskdeck:track` and `/taskdeck:computer`, the setup commands. Each adds the TaskDeck server first if it is missing |
| **`/taskdeck:track`** | The setup, run once per folder | Picks or creates the board, turns Autotrack on and writes a short block into the folder's CLAUDE.md |
| **`/taskdeck:computer`** | The setup, run once per computer | Installs and pairs the TaskDeck runner, adds your repositories, starts it at log-in and puts the computer on your agents' Runs on |

## Quick start

1. Install the plugin:

   ```bash
   claude plugin marketplace add Ampliative/taskdeck-plugin
   claude plugin install taskdeck@taskdeck
   ```

2. In the folder you want tracked, run:

   ```
   /taskdeck:track
   ```

   If TaskDeck is not connected yet, it adds the TaskDeck server to `~/.claude.json` and asks you to open a new session, sign in with `/mcp` and run `/taskdeck:track` again. The setup then asks at most a few questions: which board, how it should look, when finished cards leave it, and whose name Claude's work carries.

`/taskdeck:track Web` tracks the folder on an existing board called Web without asking which.

No terminal, only the desktop app? Do it in the app:

1. **Connect TaskDeck** as a connector: Customize → Connectors → Add custom connector, name it TaskDeck, paste `https://mcp.taskdeck.me/mcp`, then click Connect and approve. Code tab sessions get the TaskDeck tools from it too.
2. **Install the plugin** from the Chat or Cowork section, not the Code tab: Settings → Plugins, add the marketplace `Ampliative/taskdeck-plugin`, then add TaskDeck. The Code tab's Plugins panel adds the marketplace but never lists its plugins.
3. **Run `/taskdeck:track`** in a new Code tab session in the folder.

## Manual install

Do the same steps by hand to see what each one does:

1. **Connect TaskDeck.** Add this under `"mcpServers"` in `~/.claude.json`:

   ```json
   "taskdeck": { "type": "http", "url": "https://mcp.taskdeck.me/mcp" }
   ```

   In the terminal, `claude mcp add --transport http --scope user taskdeck https://mcp.taskdeck.me/mcp` does the same. Open a new session and run `/mcp` to sign in to TaskDeck.
2. **Install the plugin** with the two commands above. `/taskdeck:track` appears in Claude Code.
3. **Run `/taskdeck:track`** in the folder.

In Claude's Chat tab, on the web and on mobile, TaskDeck is a connector instead: Customize → Connectors → Add custom connector, with the same URL.

## Run your agents on this computer

In a repository on the computer, run:

```
/taskdeck:computer
```

Claude installs the TaskDeck runner, pairs it, adds the repositories you choose, starts it at log-in and puts the computer on your agents' Runs on. You approve the pairing code in TaskDeck, and accept Claude Code's trust prompt once in each folder with `claude agents`. `/taskdeck:computer Planner` puts the computer on the agent called Planner without asking which.

## After setup

Ask Claude for work the way you always do. It calls `start-work` before changing anything, comments on the card as it goes, and calls `finish-work` when the work is verified.
