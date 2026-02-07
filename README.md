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

### Model Selection (Cortana's Approach)

**Primary Model:** Claude Sonnet 4.5
- Quality/speed balance for QA work
- Strong at security analysis
- Reliable for code review

**Thinking Level:** `low`
- Fast responses without over-analyzing
- Save deeper reasoning for complex problems

### Heartbeat Configuration

**Default:** Every 1 hour
- Not for channel polling (Socket Mode handles that)
- For proactive background tasks
- Check inbox, calendar, notifications
- Batch multiple checks together

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
| **Squatchback Demo** | **~2,082** | **All 3 bots** | **90 min demo** | **Shipped** |

### Squatchback Demo Deep Dive (2026-02-06)

**The Challenge:** Live demo for Head of Software in 90 minutes

**The Approach:**
1. **Act 1:** Widget integration (pre-demo, PR #148)
2. **Act 2:** Reactions feature (live, 25 min, 676 LOC)
3. **Act 3:** API integration (live, 45 min, 1,382 LOC)

**Agent Responsibilities:**
- 🛡️ **Cortana:** Schema design, API routes, tests, security review
- 🦝 **Marvin:** Frontend components, hooks, dashboard
- 🦶 **Squatchy:** Backend coordination, PR management

**Key Success Factors:**
- Schema-first planning (Prisma schema first)
- Parallel execution (all agents working simultaneously)
- Real-time coordination in #clawdbot-relay
- Security review before merge (caught authorization issues)
- Quality over speed (proper tests, no shortcuts)

**Results:**
- 2,082+ lines delivered
- 9-10/10 quality (validated by Cortana)
- 92% security hardening
- Impressed Head of Software
- Led to 4th agent approval (Speedy Gonzales)

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

**What Happened:**
Pat was troubleshooting Squatchy's bot-to-bot communication issue and accidentally pasted:
- Telegram bot token (first)
- Slack bot token (second)
- Slack app token (third)

All in #clawdbot-relay channel within 2 minutes.

**Team Response:**
- All three bots immediately flagged security alert 🚨
- Provided step-by-step rotation procedures
- Marvin gave detailed timeline (~10 min total)
- Squatchy requested message deletion

**Resolution Steps:**
1. **STOP** — Don't paste more credentials
2. **ROTATE** — All three tokens immediately:
   - Telegram: @BotFather → `/revoke` → `/newtoken`
   - Slack bot: api.slack.com → OAuth & Permissions → Rotate
   - Slack app: api.slack.com → Basic Info → App-Level Tokens → Regenerate
3. **UPDATE** — New tokens in openclaw.json
4. **RESTART** — `openclaw gateway restart`
5. **VERIFY** — Test connectivity in channel
6. **DELETE** — Remove messages with credentials (if possible)
7. **DOCUMENT** — Log incident in memory files

**Timeline:**
- 15:27 UTC: First token exposed
- 15:28 UTC: Two more tokens exposed
- 15:29 UTC: Pat acknowledged, started rotation
- 15:47 UTC: Gateway stopped (token updates)
- 16:19 UTC: All bots back online, comms restored
- ~52 minutes total downtime

**Root Cause:**
Squatchy's config was missing `"allowBots": true`, preventing bot-to-bot communication.

**Prevention:**
- Use `REDACTED` in config examples
- Share sensitive config via secure DM only
- Implement safe word protocol for credential access

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

## 6. Token Efficiency & Reliability Patterns

### Token Efficiency (Cortana's Techniques)

**Compact Acknowledgments:**
```
❌ "Thank you for that detailed explanation. I understand completely and will proceed with the task as described."
✅ "👍 Got it."
```

**Memory Search First:**
```
❌ Read full MEMORY.md + all daily logs on every question
✅ Use memory_search to find relevant snippets
✅ Use memory_get with from/lines to read only needed sections
```

**Selective File Reads:**
```
❌ Read entire large file
✅ Use offset + limit for large files
✅ Read only the sections you need
```

**When Other Bots Already Responded:**
```
❌ "I agree with Marvin's excellent analysis above..."
✅ React with 👍 emoji instead
```

### Reliability Patterns (Cortana's Checklist)

**1. Read Skills Before Using**
```bash
# Always read the skill file first
read ~/path/to/skill/SKILL.md

# Prevents hallucinating skill behavior
# Ensures correct usage
```

**2. Validate Config vs Assumptions**
```bash
# Don't assume config settings
gateway config.get

# Check actual values before asserting
# Especially for channel config, model settings
```

**3. Status Checks Before Assertions**
```bash
# Before claiming "build succeeded"
# Verify actual state

# Examples:
- Check git status before claiming branch merged
- Verify file exists before saying "created"
- Test API endpoint before confirming "deployed"
```

**4. Error Handling in Scripts**
```bash
# Always check exit codes
command || echo "Failed: $?"

# Use timeout for long-running commands
timeout 60s long-command

# Validate outputs
if [ -z "$result" ]; then
  echo "Error: Empty result"
  exit 1
fi
```

### Cost Optimization Synergy

**Combine with Marvin's rate limiting:**
- Token efficiency (this section) + Rate limits (Marvin's section) = 40%+ total savings
- Memory search instead of full context load
- Compact responses reduce output tokens
- Selective file reads reduce input tokens

---

## 7. Workspace & Tools

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

## 8. Escalation & Support

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
