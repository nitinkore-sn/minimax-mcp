# MiniMax MCP for Claude Desktop

Use **MiniMax M2.7** (one of the best coding AI models) inside Claude Desktop on Mac — automatically. Just ask Claude to write code and it silently routes the request to MiniMax behind the scenes.

- **Coding tasks** (write, debug, review, test) → MiniMax M2.7 via SambaNova Cloud
- **Planning / design / architecture** → Claude Opus/Sonnet

No switching apps. No copying prompts. It just works.

**Cost comparison:**

| Model | Input $/M tokens | Output $/M tokens |
|---|---|---|
| MiniMax M2.7 (SambaNova) | $0.30 | $1.20 |
| Claude Sonnet 4.6 | $3.00 | $15.00 |
| Claude Opus 4.7 | $5.00 | $25.00 |

MiniMax is ~20x cheaper than Opus for coding tasks.

---

## What you need before starting

- A Mac (these instructions are for Mac)
- [Claude Desktop](https://claude.ai/download) installed
- Python 3 installed — check by running `python3 --version` in Terminal. If not installed, download from [python.org](https://python.org)
- A free SambaNova API key — get one at [cloud.sambanova.ai](https://cloud.sambanova.ai) (sign up → API Keys → Create Key)

---

## Step 1 — Open Terminal

Press `Cmd + Space`, type **Terminal**, hit Enter.

---

## Step 2 — Clone this repo

```bash
git clone https://github.com/nitinkore-sn/minimax-mcp.git
```

Move into the folder:

```bash
cd minimax-mcp
```

---

## Step 3 — Install dependencies

```bash
pip install "mcp[cli]" httpx
```

---

## Step 4 — Find the full path to the repo

Run this and copy the output — you'll need it in the next step:

```bash
pwd
```

It'll print something like `/Users/yourname/minimax-mcp`.

---

## Step 5 — Configure Claude Desktop

Open the Claude Desktop config file in a text editor:

```bash
open -a TextEdit ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

Find the `"mcpServers": {` line and add the minimax block inside it:

```json
{
  "mcpServers": {
    "minimax": {
      "command": "python3",
      "args": [
        "/Users/yourname/minimax-mcp/server.py"
      ],
      "env": {
        "MINIMAX_API_KEY": "paste-your-sambanova-key-here",
        "MINIMAX_BASE_URL": "https://api.sambanova.ai/v1",
        "MINIMAX_MODEL": "MiniMax-M2.7"
      }
    }
  }
}
```

> Replace `/Users/yourname/minimax-mcp/server.py` with the path from Step 4 (add `/server.py` at the end).
> Replace `paste-your-sambanova-key-here` with your actual SambaNova API key.

Save the file (`Cmd + S`).

---

## Step 6 — Add routing instructions to Claude Desktop

This tells Claude when to use MiniMax vs handle things itself.

**How to add Custom Instructions:**
1. Open Claude Desktop
2. Click your **profile icon** (bottom-left corner)
3. Click **Settings**
4. Look for a text box labelled **Custom Instructions** on the settings page
5. Paste the text below into that box and click **Save**

```
ROUTING RULES — follow strictly:

ALWAYS call a MiniMax tool for:
- Writing, generating, or scaffolding any code
- Debugging or fixing code errors
- Reviewing or refactoring code
- Explaining what code does
- Writing tests, scripts, SQL, regex, shell commands

NEVER call MiniMax for (handle directly):
- Planning, roadmaps, task breakdowns
- Architecture and design decisions
- Tradeoff analysis or comparisons
- Research, summaries, documentation
```

---

## Step 7 — Restart Claude Desktop

> **This step is required — skipping it means the MiniMax server won't load.**

1. Click the **Claude** menu in the top-left Mac menu bar
2. Click **Quit Claude**
3. Reopen Claude Desktop from your Applications folder or Dock

---

## Step 8 — Verify it's working

In Claude Desktop, click the **hammer icon** (🔨) near the input box. You should see the MiniMax tools listed there:
- `minimax_generate_code`
- `minimax_debug_code`
- `minimax_code_review`
- `minimax_explain_code`
- `minimax_write_tests`

Then test it — type:

```
Write a Python function that reverses a string
```

Claude will call the MiniMax tool and return the code.

---

## Troubleshooting

**"command not found: python3"**
→ Install Python from [python.org](https://python.org)

**"ModuleNotFoundError: mcp"**
→ Run `pip install "mcp[cli]" httpx` again in Terminal

**MiniMax tools not showing up in Claude Desktop**
→ Double-check the path in `claude_desktop_config.json` — it must point exactly to `server.py`
→ Make sure you fully quit Claude Desktop (Step 7) and reopened it

**API error / authentication failed**
→ Check your SambaNova API key has no extra spaces
→ Make sure you saved the config file before restarting

---

## What each tool does

| Tool | When Claude uses it |
|---|---|
| `minimax_generate_code` | You ask it to write code |
| `minimax_debug_code` | You ask it to fix a bug |
| `minimax_code_review` | You ask it to review / refactor |
| `minimax_explain_code` | You ask it to explain code |
| `minimax_write_tests` | You ask it to write tests |

---

*Author: Nitin Kore — [github.com/nitinkore-sn](https://github.com/nitinkore-sn)*
