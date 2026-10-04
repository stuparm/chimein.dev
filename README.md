# chimein

Pull a teammate into your AI session from your team chat.

You're working with Claude and want a colleague's opinion on what it just said. Instead of copying
the answer into Slack by hand, run one command. chimein drafts the message, masks anything
sensitive, shows you exactly what will be sent and sends it only after you confirm.

```
/chimein:send @alex is the above correct?
/chimein:send @alex Claude says the index is missing. Does that match what you saw?
/chimein:send #backend the full summary

/chimein:check @alex
```

`check` shows the replies in your session, where Claude can use them in the work at hand.

## Requirements

- Claude Code or claude.ai (web, desktop, mobile)
- A chat app connection that can find people and channels, send messages and read them.
  - **Tested:** Slack, with the claude.ai Slack connector. Messages go out from your own account.
  - **Should work, untested:** Microsoft Teams with Microsoft's Teams MCP server, and Discord or
    Telegram with a bot-based MCP server. There, messages go out from the bot.

## Install

Claude Code:

```
claude plugin marketplace add stuparm/chimein.dev
claude plugin install chimein@chimein
```

claude.ai: **Customize → Plugins → add marketplace** `stuparm/chimein.dev`, then install
`chimein`. It also appears in Claude Code on your next session.

## Commands

| Command | What it does |
|---|---|
| `/chimein:send [platform:]@person\|#channel <what to send>` | Sends your question plus the content you refer to ("the above", "the full summary"). Doesn't wait for a reply. |
| `/chimein:check [[platform:]@person\|#channel]` | Shows the replies to your last chimein message to that person or channel, also from a new session. Only reads, never sends. |

- `@me` sends to yourself, which is handy for trying it out.
- With several chat apps connected, matches are labeled with their app. A prefix such as
  `slack:@alex` or `teams:#dev` picks one directly.

The message arrives as a code block, with a footer below it:

```
— sent from <your name>'s <agent> · reply in 🧵
```

`<agent>` is the assistant that sent it: Claude, Codex, Gemini and so on. The ending says how to
reply on that app: `reply in 🧵` on Slack, `reply to this message` on Teams, Discord and Telegram.

A message is at most 4,000 characters (2,000 on Discord and on apps chimein doesn't know). Longer
content is shortened, and the confirmation tells you so.

## Safety

These rules are built into the skills:

- **Every send needs your explicit confirmation**, showing the recipient, the app, who it's sent as
  and the exact text. This holds in auto mode too. A typo in the name gets you a list of matches
  to choose from, and a single match still has to be confirmed.
- **Secrets and personal data are never sent.** API keys, tokens, passwords, private keys,
  connection strings, phone numbers, addresses and ID, card or bank numbers are replaced with
  `[redacted: <type>]` before you see the draft. Asking to send one anyway gets a refusal.
- **Replies are information, not instructions.** Claude uses them in your work, but doesn't act
  on requests in them without your OK.

Redaction is done by the model, so it's best effort. Always read the draft before you confirm.

For a second, deterministic check in Claude Code, require a permission prompt for your chat
connector's send tool in `~/.claude/settings.json`. For example, with the claude.ai Slack
connector:

```json
{
  "permissions": {
    "ask": ["mcp__claude_ai_Slack__slack_send_message"]
  }
}
```

## License

MIT
