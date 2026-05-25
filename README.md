# Scotia FutureFlex
> CaseHacks 2026 — Scotiabank Innovation Challenge

A mobile-first investing product concept that helps first-time investors start in under 60 seconds — no forms, no jargon, no $500 minimums.

---

## The Problem

62% of Canadians aged 18–34 want to invest. Less than 11% have started.

The barrier isn't knowledge or money. It's friction. Traditional bank onboarding requires identity verification, lengthy risk questionnaires, and fund selection before a user sees any value. Most people leave before they ever reach a portfolio.

---

## The Solution

**Goal → Portfolio → Invest → Done.**

1. User taps one goal (home deposit, travel, retirement, emergency fund)
2. Instantly receives a curated beginner portfolio — no forms, no fund codes
3. Starts with as little as $25
4. Identity verification runs in the background (progressive KYC)

---

## Features

### Onboarding
- Goal-based flow completed in under 60 seconds
- Progressive KYC — deposit first ($2,500 cap), verify later
- Beginner-friendly portfolio card with plain-language allocation breakdown

### Round-up Micro-Investing
- Every debit purchase rounds up to the next dollar
- Spare change auto-invests into TFSA, RRSP, or FHSA
- Double-up mode for 2x round-ups
- Average passive investment: ~$157/year without thinking about it

### XP Gamification Engine
- Append-only XP ledger — financially meaningful actions only
- Deposits, round-ups, streaks, financial literacy tips all earn XP
- Daily XP caps per category to prevent gaming
- 24hr grace window on streaks (no hard reset on missed days)
- Free monthly streak freeze

### Rewards System
| Reward | XP Cost | Cost to Scotia |
|---|---|---|
| XP double weekend | 150 XP | $0 |
| Smart Money insights unlock | 200 XP | $0 |
| Cineplex Tuesday ticket | 300 XP | ~$7 (shared with Cineplex) |
| Fee-free month | 500 XP | ~$0.52 |
| Scotia advisor chat | 600 XP | $0 |

All rewards tied to Scotia's real partner ecosystem — Scene+, Cineplex, Sobeys, Smart Money.

---

## Tech Stack

### Frontend
- React + Vite
- AppContext for shared XP and user state
- Inline styles — no CSS framework overhead
- Deployed on Vercel

### Backend
- FastAPI + Python 3.13
- SQLAlchemy + SQLite (MVP) — swap to PostgreSQL for production
- Pydantic for request/response validation
- Append-only XP ledger design
- Round-up processor with ceiling math
- Seed data pre-loaded with demo user Alex (620 XP, 5 transactions)

---

## Project Structure
scotia-backend/
├── main.py              # FastAPI app, CORS, router mounts
├── database.py          # SQLAlchemy engine, Base, get_db
├── models.py            # User, Portfolio, Transaction, XPEvent, Streak, Reward, Habit
├── schemas.py           # Pydantic DTOs
├── seed.py              # Demo data seeder
└── routers/
├── onboard.py       # POST /goal, POST /deposit, GET /kyc-status
├── xp.py            # GET /xp/{userId}, POST /event
├── roundups.py      # POST /process, GET /{userId}
├── rewards.py       # GET /{userId}, POST /{id}/redeem
└── habits.py        # GET /today/{userId}, POST /{id}/complete/{userId}
scotia-frontend/
├── src/
│   ├── App.jsx
│   ├── context/AppContext.jsx
│   ├── api.js
│   ├── onboarding/
│   │   ├── Welcome.jsx
│   │   ├── GoalSelect.jsx
│   │   ├── Portfolio.jsx
│   │   └── Confirm.jsx
│   └── tabs/
│       ├── HomeTab.jsx
│       ├── InvestTab.jsx
│       ├── RewardsTab.jsx
│       ├── HabitsTab.jsx
│       └── RoundupsTab.jsx
└── vite.config.js

---

## Getting Started

### Backend

```bash
cd scotia-backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

API docs available at `http://localhost:8000/docs`

### Frontend

```bash
cd scotia-frontend
npm install
npm run dev
```

App runs at `http://localhost:5173`

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/login` | Mock login — returns demo user |
| POST | `/onboard/goal` | Set investment goal |
| POST | `/onboard/deposit` | Make first deposit |
| GET | `/onboard/kyc-status/{userId}` | Get KYC status |
| GET | `/portfolio/{userId}` | Portfolio summary |
| GET | `/xp/{userId}` | XP balance, tier, history |
| POST | `/roundups/process` | Process a round-up transaction |
| GET | `/roundups/{userId}` | Round-up history |
| GET | `/rewards/{userId}` | Available rewards |
| POST | `/rewards/{rewardId}/redeem` | Redeem a reward |
| GET | `/habits/today/{userId}` | Today's habits |
| POST | `/habits/{habitId}/complete/{userId}` | Complete a habit |

---

## XP System

| Action | XP Awarded |
|---|---|
| First deposit | +100 XP |
| Subsequent deposits | +50 XP |
| First round-up | +100 XP |
| Daily round-up | +5 XP (cap: 50/day) |
| Habit completion | +10–20 XP |
| 7-day streak | +bonus XP |
| 30-day streak | +bonus XP |
| Reward redemption | −XP cost |

Level = total XP ÷ 100

---

## Team

Built in 24 hours at CaseHacks 2026.

Malika · Mamin · Dev · Varun

---

## Pitch

> *"Scotia FutureFlex turns Scotiabank's 3 million underserved account holders into first-time investors before they close the app."*