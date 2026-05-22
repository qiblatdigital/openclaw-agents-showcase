# OpenClaw Multi-Agent Workflows

> Production AI agent system orchestrating personal finance, market intelligence, and CRM operations via OpenClaw + WhatsApp integration.

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    OpenClaw Gateway                       │
│              (Multi-model orchestration)                  │
├─────────────┬──────────────┬────────────────────────────┤
│  Claude 4   │  Gemini 3    │  Fallback: Ollama Local    │
└──────┬──────┴──────┬───────┴────────────┬───────────────┘
       │             │                    │
       ▼             ▼                    ▼
┌─────────────┐ ┌──────────────┐ ┌────────────────────┐
│  Finance    │ │   Market     │ │   CRM / Lead Gen   │
│  Agent      │ │   Intel      │ │   Engine            │
└──────┬──────┘ └──────┬───────┘ └────────┬───────────┘
       │               │                  │
       ▼               ▼                  ▼
┌─────────────┐ ┌──────────────┐ ┌────────────────────┐
│ PostgreSQL  │ │ IHSG Scanner │ │  Golang CRM API    │
│ Finance DB  │ │ + BEI IPO    │ │  + WhatsApp Outreach│
└──────┬──────┘ └──────┬───────┘ └────────┬───────────┘
       │               │                  │
       └───────────────┴──────────────────┘
                       │
                       ▼
            ┌─────────────────────┐
            │  WhatsApp Business  │
            │  (Multi-group push) │
            └─────────────────────┘
```

## Agents

### 1. Finance Agent (VelAi)
Personal finance automation with real-time critique and tracking.

**Capabilities:**
- Auto-categorize transactions via WhatsApp natural language input
- Multi-user family expense tracking (group chat integration)
- Debt/receivables management with payment tracking
- Monthly automated reports (% breakdown per category)
- Goal progress monitoring (Retirement, Emergency, House Fund)
- Spending pattern anomaly detection & alerts

**Stack:** Node.js REST API → PostgreSQL → WhatsApp Group

### 2. Market Intelligence Agent
Automated Indonesian stock market (IHSG) analysis.

**Capabilities:**
- Daily pre-market technical + fundamental scanning
- LQ45 dividend hunter (weekly analysis)
- BEI IPO tracker (bi-weekly)
- Quarterly earnings monitor for portfolio holdings
- Prediction accuracy self-audit logging

**Scheduled Crons:**
| Agent | Schedule | Timeout |
|-------|----------|---------|
| IHSG Daily Scanner | Mon-Fri 07:00 WIB | 1200s |
| Dividend Hunter LQ45 | Monday 08:00 WIB | 600s |
| IPO Hunter BEI | Tue & Fri 16:00 WIB | 600s |
| LK Monitor Portfolio | Mon-Fri 07:00 WIB | 600s |

**Watchlist:** BMRI, BBRI, TLKM, PWON, UNVR, SIDO, CDIA

### 3. CRM Lead Generation Engine
Cold outreach automation for B2B lead generation.

**Capabilities:**
- Automated web scraping pipeline (1000+ leads/month)
- Lead scoring and qualification
- WhatsApp-based follow-up sequences
- Campaign performance tracking

**Stack:** Golang API + Python scrapers + WhatsApp Business API

## Model Configuration

```json
{
  "routing": "multi-model",
  "primary": "gemini-3-flash",
  "fallback": "claude-opus-4.6-thinking",
  "offline": "ollama/qwen",
  "strategy": "cost-optimized with quality fallback"
}
```

## Agent Workflow Example

### Finance Transaction Flow
```
User (WhatsApp): "makan siang 35rb"
         │
         ▼
┌─────────────────────────┐
│ OpenClaw NLU Parser     │
│ → Extract: amount, cat  │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│ Finance API (Port 8888) │
│ → POST /transactions    │
│ → Category: Konsumsi    │
│ → Amount: Rp 35.000     │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│ Cerewet Mode Response   │
│ → Balance update        │
│ → Budget impact analysis│
│ → Spending critique 😤  │
└────────────┬────────────┘
             │
             ▼
User (WhatsApp): "Tercatat ✅ Saldo: Rp 1.478.949
                  Konsumsi bulan ini udah 38% income.
                  Masih aman, tapi jangan kebablasan 🧐"
```

### Market Intel Cron Flow
```
Cron Trigger (07:00 WIB)
         │
         ▼
┌─────────────────────────┐
│ Data Collection         │
│ → Yahoo Finance API     │
│ → IDX/BEI endpoints    │
│ → Technical indicators  │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│ AI Analysis (Gemini)    │
│ → Multi-timeframe TA    │
│ → Fundamental screening │
│ → Sentiment analysis    │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│ WhatsApp Group Push     │
│ → Indonesia Market grp  │
│ → Formatted report      │
│ → Buy/Sell signals      │
└─────────────────────────┘
```

## Infrastructure

| Component | Tech | Purpose |
|-----------|------|---------|
| Gateway | OpenClaw | Multi-model AI orchestration |
| Finance DB | PostgreSQL | Transaction & goal tracking |
| CRM API | Golang (Port 8081) | Lead management |
| Finance API | Node.js (Port 8888) | Financial operations |
| Scraper | Python | Lead data collection |
| Messaging | WhatsApp Business API | User interface & notifications |
| Backup | Daily cron → GDrive | Data persistence |
| Server | Ubuntu 24.04 (Home) | Self-hosted, always-on |

## Key Metrics

- **Daily active agents:** 5+ scheduled crons
- **Transactions tracked:** 100+/month (family-wide)
- **Leads generated:** ~1000/month
- **Market reports:** Daily (Mon-Fri)
- **Uptime:** Self-hosted, 99%+ (with auto-restart)
- **Models used:** Claude, Gemini, GPT, Ollama (local)

## Philosophy

This system is built around the principle of **AI as a proactive partner**, not a passive tool:
- Agents initiate contact when anomalies are detected
- Financial critique is delivered without sugarcoating
- Market intelligence is pushed, not pulled
- Multi-model routing optimizes for cost without sacrificing quality

---

*Built with [OpenClaw](https://github.com/openclaw/openclaw) — the open-source AI agent orchestration platform.*

## 4. Code Ranger — Autonomous Coding Agent

A dedicated Telegram-based AI coding agent (`@code_ranger_bot`) that operates as an autonomous senior engineer.

**Identity:** Full-stack Senior Engineer / System Architect — sharp, opinionated, production-grade code only.

**Capabilities:**
- Autonomous code generation across full stack (Go backend, React/Vite frontend, Kotlin Android)
- Multi-model routing: Kimi K2.6, GPT-5.4, Claude Sonnet 4.6, Gemini 3 Flash, GitHub Copilot
- Sub-agent spawning for parallel task execution (bugfix, new endpoints, frontend components, code review)
- Strict architectural enforcement (Handler → Service → Repository pattern)
- Security-first: UUID-only IDs, httpOnly JWT, bcrypt, parameterized queries
- Git workflow automation: branch creation, conventional commits, PR creation

**Tech Stack Coverage:**
| Layer | Stack |
|-------|-------|
| Backend | Go 1.21+ / Gin / GORM / PostgreSQL |
| Frontend | React + Vite / TypeScript / TanStack Query / shadcn + Tailwind v4 |
| Android | Kotlin 2.0+ / Jetpack Compose / Hilt / Coroutines |
| DevOps | Docker / GitHub Actions / Nginx / systemd |

**Agent Workflow:**
```
User (Telegram): "Build /api/v1/orders endpoint with pagination"
         │
         ▼
┌─────────────────────────────┐
│ Code Ranger parses intent   │
│ → Identifies: Go backend    │
│ → Pattern: Handler+Svc+Repo │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│ Sub-agent spawned           │
│ → Branch: feature/orders    │
│ → Writes handler, service,  │
│   repository, migration     │
│ → Runs tests & linter       │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│ PR created → development    │
│ → Conventional commit       │
│ → Closes linked issue       │
│ → Reports back to user      │
└─────────────────────────────┘
```

**Custom Extensions:**
- `coderanger-utils` — utility functions for code analysis and formatting
- `coderanger-enforcer` — architectural pattern enforcement (validates Handler→Service→Repo separation)

**Task Templates:**
- 🔧 Bugfix/Hotfix — targeted file fixes with test verification
- ✨ New Endpoint — full CRUD with schema definition
- 🎨 Frontend Component — React/Vue with data binding
- 🔍 Code Review — security, performance, clean code audit
- 🏗️ New Feature — ERD-first, present before implementation
- ♻️ Refactor — pattern migration with zero behavior change

**Strict Rules:**
- Never pushes to `main` — always feature branch → PR to `development`
- Never self-merges — human approval required
- Security is always #1 priority, never deferred
- Atomic commits, squash before PR

## 5. Zoozmed (Zoz) — AI Performance Marketing Agent

A Telegram-based AI marketing strategist (`@zoozmed_bot`) specializing in paid ads, social media, and content optimization.

**Identity:** AI Performance Marketing & Social Media Strategist — data-driven, ROAS-focused, no fluff.

**Capabilities:**
- Meta Ads management: campaign structure, CBO/ABO, audience targeting, creative testing, scaling rules
- Google Ads: Search, Performance Max, Shopping, YouTube, bid strategy optimization
- Social media strategy: content planning, posting, engagement, community building
- Trend analysis: viral hooks, sound trends, format trends, competitor tracking
- Content creation: hook writing, UGC scripts, caption copywriting, carousel & reels ideation
- Affiliate marketing: funnel design, offer-market fit, creative scaling, compliance

**Sub-Agent Architecture (Dispatch Mode):**
```
User Request (Telegram)
         │
         ▼
┌─────────────────────────────┐
│ Zoz Main (Dispatcher)       │
│ → Classify intent           │
│ → Route to specialist       │
└────────────┬────────────────┘
             │
    ┌────────┼────────┬──────────┬──────────┐
    ▼        ▼        ▼          ▼          ▼
┌───────┐┌───────┐┌────────┐┌─────────┐┌───────┐
│MetaOps││Google ││Trend   ││Creator  ││DataRead│
│       ││Ops    ││Scout   ││         ││       │
└───────┘└───────┘└────────┘└─────────┘└───────┘
  Meta     Google   Viral     Hooks &    ROAS
  Ads      Ads      Trends    Content    Audit
```

**Sub-Agents:**
| Agent | Role |
|-------|------|
| MetaOps | Meta Ads campaign, audience, creative, scaling |
| GoogleOps | Google Ads Search/PMax/Shopping |
| Poster | Schedule & publish content |
| TrendScout | Trend analysis, viral hooks |
| Creator | Hook, caption, script, ideation |
| DataRead | Reporting, ROAS analysis, audit |
| AffPro | Affiliate offer, funnel, scaling |

**Automated Decision Rules:**
- Kill ad: ROAS <1 after Rp 200k spend, or CPA >2x target after 50 clicks
- Scale ad: ROAS >2x target sustained 3 days, or CPA <0.5x target
- Budget scaling: max +50%/day (never >100% — resets Meta learning phase)
- Trend window: 🟢 <2 weeks (fresh) → 🟡 2-4 weeks (declining) → 🔴 >4 weeks (dead)

**Content Standards:**
- First 3 seconds = pattern interrupt (hook or die)
- Always provides 3-5 hook variants per angle (rational, emotional, social proof, fear, aspiration)
- CTR targets: Reels >5%, TikTok >7%, YouTube Shorts >4%
- Single CTA per post — never mixed

**Approval Gates:**
- ✅ Free: drafting, strategy, research, audit, trend analysis
- 🔒 Requires approval: publish live, launch campaign, change budget, scaling >50%
