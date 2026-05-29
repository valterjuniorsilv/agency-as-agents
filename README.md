# Nodus Agents

[![CI](https://github.com/valterjuniorsilv/nodus-agents/actions/workflows/validate.yml/badge.svg)](https://github.com/valterjuniorsilv/nodus-agents/actions) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Release](https://img.shields.io/github/v/release/valterjuniorsilv/nodus-agents)](https://github.com/valterjuniorsilv/nodus-agents/releases)

> An "agency operated by AI agents" — the actual `.claude/agents/` setup that runs the [NodusHub](https://nodushub.com.br) operation. Each agent represents a role on the organizational chart of a small full-service agency (engineering, QA, prompt engineering, sales, prospecting, client success).
>
> This is not a generic agent template repo. It's the real configuration, opened so other founders can fork and adapt to their own operation.

---

## Why this exists

When you run a small agency solo (1 human + AI), you don't need 30 generic "specialist" agents. You need a tight set of agents that mirror the **actual roles** of an agency that ships work to clients.

NodusHub runs 8 agents across 3 functional clusters:

### Engineering cluster (5 agents)

| Agent | Role |
|---|---|
| [`nodus-architect`](./nodus-architect.md) | Stack decisions, ADRs, architectural review |
| [`nodus-software-engineer`](./nodus-software-engineer.md) | Full-stack implementation (Next.js + FastAPI + Supabase) |
| [`nodus-qa-engineer`](./nodus-qa-engineer.md) | Testing, debugging, reliability, CI/CD |
| [`nodus-bot-builder`](./nodus-bot-builder.md) | WhatsApp bots — N8N + Evolution API + Claude |
| [`nodus-prompt-engineer`](./nodus-prompt-engineer.md) | System prompts for client bots, A/B testing, prompt caching |

### Commercial cluster (3 agents)

| Agent | Role |
|---|---|
| [`nodus-prospector`](./nodus-prospector.md) | Lead lists, SPIN qualification, prospect dossiers |
| [`nodus-comercial`](./nodus-comercial.md) | Proposals, follow-up, contracts, deal close |
| [`nodus-client-success`](./nodus-client-success.md) | Monthly reports, churn detection, upsell, renewal |

---

## How to install

```bash
cd ~/.claude/agents
git clone https://github.com/valterjuniorsilv/nodus-agents.git tmp
cp tmp/*.md ./
rm -rf tmp
```

Restart Claude Code. The agents appear in the routing layer automatically — Claude picks the right one based on the `description` field.

---

## How to adapt to your own operation

These agents reference the NodusHub stack and context heavily:

- **Stack mentions:** Next.js, FastAPI, Supabase, Anthropic SDK, Evolution API, N8N, Hetzner
- **Product mentions:** `lode-api`, `admine-web`, `iris-bot`, `Olympus`
- **Domain mentions:** dental clinics, traffic management, paid media

To fork for your context:

```bash
# 1. Clone
git clone https://github.com/valterjuniorsilv/nodus-agents.git ~/.claude/agents/my-agency

# 2. Find/replace mentions
cd ~/.claude/agents/my-agency
grep -rl "NodusHub" . | xargs sed -i '' 's/NodusHub/YOUR_COMPANY/g'
grep -rl "lode-api" . | xargs sed -i '' 's/lode-api/YOUR_API/g'
# ... repeat for stack, product names, domain

# 3. Move to active agents
mv ~/.claude/agents/my-agency/*.md ~/.claude/agents/
```

You're not forking a template. You're forking a **real configuration** that ships work daily.

---

## Triggers (Portuguese-first)

The `description` field of each agent is what Claude Code reads to decide routing. These are heavily optimized for **Portuguese triggers** because NodusHub operates in Brazil.

If you operate in English, you'll need to translate the triggers in each `description` field. The agent body (system prompt) is mostly in Portuguese too — translate accordingly.

---

## Architecture pattern: agents call skills

Each agent in this repo **invokes skills** for specific subtasks. Skills are reusable across agents — they live in the [claude-skills](https://github.com/valterjuniorsilv/claude-skills) repo.

Example flow:

1. User: "criar proposta para clínica X"
2. Claude Code routes to `nodus-comercial`
3. `nodus-comercial` invokes the `humanize-copy-br` skill to write the proposal in human-sounding Portuguese
4. `nodus-comercial` invokes the `persuade-copy-br` skill to add persuasion structure
5. Final output: proposal written, no AI-sounding language, with neuromarketing applied

This separation — **agents = roles, skills = capabilities** — keeps both reusable and composable.

---

## What's NOT here

Some NodusHub agents reference specific named characters internal to the operation:

- `nodus-dir-comercial` (Marcos), `nodus-dir-analytics` (Beatriz), `nodus-dir-conteudo` (Sofia), `nodus-dir-design` (Camila), `nodus-dir-engenharia` (Rafael)

These work internally because the team knows who's who. They don't translate well outside the operation, so they're not here.

Also not included: agents tightly coupled to the `Olympus` product (NodusHub's paid offering for dental clinics).

---

## Companion repos

- **[claude-skills](https://github.com/valterjuniorsilv/claude-skills)** — skills these agents invoke
- **[antigravity-lab](https://github.com/valterjuniorsilv/antigravity-lab)** — Go backend with Clean Arch + DDD + CQRS

---

## License

MIT — see [LICENSE](./LICENSE).

---

## Author

**Valter Silva** · Founder [NodusHub](https://nodushub.com.br) · Maringá, PR · 🇧🇷

> "Na area, não nas arquibancadas."
