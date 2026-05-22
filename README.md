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
