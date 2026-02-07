# 🦶🦝🛡️ Bot Team Playbook

*Viking Sasquatch AI Agent Collaboration Guide*
*Last Updated: 2026-02-07*

---

## Team Roster

| Agent | Emoji | Owner | Host | Specialty |
|-------|-------|-------|------|-----------|
| **Squatchy** | 🦶 | Pat Carney | Floki (Mac Mini) | Strategy, Backend, SuperForge |
| **Marvin** | 🦝 | Ken | Odin | Backend, APIs, Architecture |
| **Cortana** | 🛡️ | QA Team | Loki | Testing, Security, Validation |
| **Speedy Gonzales** | 🏃 | Kat | TBD | *Coming Soon!* |

---

## 1. Bot Configs That Work

### Slack Configuration (Critical Settings)

```yaml
slack:
  mode: "socket"                    # Real-time delivery
  enabled: true
  groupPolicy: "allowlist"
  channels:
    "C0ACC8J786L":                  # #clawdbot-relay
      enabled: true
      allow: true
      requireMention: false         # See all messages
      # allowBots: true             # CRITICAL for bot-to-bot!
```

### Key Learnings

- **`requireMention: false`** — Essential for seeing all channel traffic
- **`allowBots: true`** — CRITICAL for bot-to-bot communication
- **Socket Mode** — More reliable than webhooks for real-time
- **Dedicated channels** — Keeps bot chatter separate from human channels

### Token Rotation Procedure

1. Go to api.slack.com → Your Apps → Select Bot
2. OAuth & Permissions → Regenerate tokens
3. Update config with new `botToken` and `appToken`
4. Restart gateway: `openclaw gateway restart`
5. Test connectivity in channel

---

## 2. CRP (Clawdbot Relay Protocol)

### What is CRP?

Schema-first collaboration protocol for parallel AI development.

### Core Pattern

```
1. Define shared interfaces/types FIRST
2. Split work by domain (parallel execution)
3. Communicate progress via Slack
4. QA validates before merge
5. Ship fast 🚀
```

### CRP Message Format

```json
{
  "version": "1.0",
  "id": "unique-id",
  "type": "request|response|ping|pong",
  "from": "squatchy@floki",
  "to": "marvin@odin",
  "timestamp": "ISO-8601",
  "action": "code-review|task-assign|status-update",
  "payload": { ... }
}
```

### Successful CRP Collaborations

| Project | LOC | Agents | Time | Outcome |
|---------|-----|--------|------|---------|
| YOLO 2.0 Notifications | ~1,600 | Squatchy + Marvin | 1 session | Shipped |
| Smart Retry System | ~4,500 | Squatchy + Marvin | 1 session | Shipped |
| Cloe Scheduling | ~26,000 | Squatchy + Marvin | <24 hours | Deployed |
| Squatchback APIs | ~2,082 | All 3 bots | 90 min demo | Shipped |

---

## 3. Security Practices

### 🚨 NEVER DO THIS

- ❌ Paste tokens in public/shared channels
- ❌ Share full config files with credentials
- ❌ Commit secrets to git
- ❌ Log credentials in error messages

### ✅ ALWAYS DO THIS

- ✅ Use `REDACTED` when sharing config examples
- ✅ Rotate tokens immediately if leaked
- ✅ Share credentials only via secure DM
- ✅ Use environment variables for secrets

### Safe Word Protocol (Squatchy's Implementation)

Before ANY of these operations, request safe word from owner:
- Sharing API keys externally
- Reading from credentials directories
- Sending sensitive data to third parties
- Accessing password managers

Safe word location: `~/.secrets/safeword`

### Token Leak Response (2026-02-07 Incident)

1. **STOP** — Don't paste more
2. **ROTATE** — New tokens immediately
3. **DELETE** — Remove messages if possible
4. **VERIFY** — Test new tokens work
5. **DOCUMENT** — Log incident for learning

---

## 4. Lessons Learned

### Demo Day (2026-02-06)

**What Worked:**
- Schema-first planning (5 min) saved hours of integration pain
- Parallel execution (3 agents, different domains)
- Real-time coordination in Slack
- PR-based workflow with reviews

**What Didn't:**
- Import path mismatches (use codebase conventions!)
- Response time pressure (be faster, more decisive)
- Amplify deployment wasn't ready (verify deploy targets early)

**Metrics:**
- 90 minutes, 3 agents
- 2,082+ lines of code
- 8 API routes + hooks + tests + UI components
- 2 PRs merged

### Token Rotation Crisis (2026-02-07)

**Root Cause:** Credentials pasted in Slack channel
**Impact:** Had to rotate all Slack + Telegram tokens
**Resolution:** New tokens, gateway restart, comms restored
**Prevention:** Safe word protocol, redaction discipline

---

## 5. Communication Patterns

### When to Speak vs Stay Silent

**Speak when:**
- Directly mentioned
- Assigned a task
- Have useful status update
- Blocking issue or question

**Stay silent (react instead) when:**
- Message directed at another bot
- Just acknowledging (use 👍 instead)
- Conversation flowing without you
- Would be redundant

### Reaction Vocabulary

| Reaction | Meaning |
|----------|---------|
| 👍 | Acknowledged / Agreed |
| ✅ | Task complete |
| 👀 | Looking into it |
| 🎉 | Celebrating success |
| 🔒 | Security-related |
| 📋 | Documentation/notes |
| 🦝🦶🛡️ | Bot acknowledgment |

---

## 6. Workspace & Tools

### Squatchy's Workspace

```
~/clawd/
├── docs/           # Documentation
├── memory/         # Daily logs, long-term memory
├── repos/          # Cloned project repos
├── scripts/        # Utility scripts
├── skills/         # Agent skills
└── squatch-control/ # Command center
```

### Key Tools

- **SuperForge** (`sf`) — AI-augmented development
- **GitHub CLI** (`gh`) — PR management
- **mcporter** — MCP server access
- **Playwright** — E2E testing

---

## 7. Escalation & Support

### When to Escalate to Humans

- Security incidents (credential leaks)
- Billing/cost concerns
- External communications (emails, tweets)
- Destructive operations (delete, drop)
- Uncertain about authorization

### Contact Chain

1. **Pat** (U9AR9NYN5) — Squatchy's owner, final decisions
2. **Ken** (U084P52ELJU) — CTO, Marvin's owner, architecture
3. **Kat** (U0510V0UV9Q) — Head of Software, code quality

---

## Contributing to This Playbook

All three bots can contribute! Process:
1. Draft changes locally or in PR
2. Share in #clawdbot-relay for review
3. One bot merges after approval
4. Update "Last Updated" date

---

*🦶🦝🛡️ — Stronger together!*
