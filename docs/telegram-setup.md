# Telegram Bot Setup

Step-by-step guide to set up Telegram bots for your agent team.

## Prerequisites

- Telegram account
- Python 3.10+
- tmux installed (`brew install tmux` on macOS)
- Claude Code installed

## Step 1: Create Bots via BotFather

Open Telegram, search for `@BotFather`, and create one bot per agent:

```
/newbot
> Name: Maya (Your Org)
> Username: maya_yourname_bot

/newbot
> Name: Viktor (Your Org)
> Username: viktor_yourname_bot

... repeat for each agent
```

Save each token BotFather gives you.

## Step 2: Get Your Telegram User ID

Send any message to `@userinfobot`. It will reply with your user ID (a number like `123456789`).

## Step 3: Configure Environment

Create the env file:

```bash
mkdir -p ~/.config/telegram
cat > ~/.config/telegram/.env << 'EOF'
ALEX_TELEGRAM_ID=123456789

MAYA_BOT_TOKEN=7123456789:AAF...
VIKTOR_BOT_TOKEN=7234567890:AAG...
LUNA_BOT_TOKEN=7345678901:AAH...
MARCO_BOT_TOKEN=7456789012:AAI...
SAGE_BOT_TOKEN=7567890123:AAJ...
KAI_BOT_TOKEN=7678901234:AAK...
EOF
```

Replace with your actual values. Adjust agent names and env var names to match your setup.

## Step 4: Install Python Dependencies

```bash
python3 -m venv ~/.config/telegram/venv
source ~/.config/telegram/venv/bin/activate
pip install python-telegram-bot python-dotenv
```

## Step 5: Start the Bots

```bash
# Start all bots
.bin/telegram-bots.sh

# Or start a single bot for testing
.bin/telegram-bots.sh --agent maya
```

## Step 6: Test

Send a message to your bot in Telegram. You should see:
1. Eyes reaction (👀) — bot received the message
2. Agent reaction (🫡 or 👌) — agent is processing
3. Reply message — agent's response

## Commands

Each bot supports:
- `/start` — greeting from the agent
- `/reset` — clear conversation context (restarts the Claude session)

## Management

```bash
# Check status
.bin/telegram-bots.sh --status

# Stop all bots
.bin/telegram-bots.sh --kill

# Send a proactive message
.bin/telegram-send.py --agent maya "Your daily review is ready"

# View bot logs
tmux attach -t agent-bots
```

## Troubleshooting

**Bot doesn't respond:**
- Check tokens in `~/.config/telegram/.env`
- Verify your Telegram user ID matches `ALEX_TELEGRAM_ID`
- Check tmux session: `tmux attach -t agent-bots`

**"Claude not ready" in logs:**
- Agent takes 60-120s to initialize (loading CLAUDE.md, MCP servers)
- Wait and try again

**Bot responds to wrong person:**
- The `ALEX_TELEGRAM_ID` check ensures only you can use the bots
- Others get "This bot is private."

**Context gets stale:**
- Sessions auto-restart every 4 hours
- Send `/reset` to manually clear context
