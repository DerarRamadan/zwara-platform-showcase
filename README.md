<div align="center">

# 🏖️ Zwara Platform

### زوارة — Vacation Rental Marketplace · Escrow Treasury · Tri-Portal SPA

**Hospitality bookings with escrow-grade money movement — built on a double-entry ledger that never lies.**

[![Private Codebase](https://img.shields.io/badge/STATUS-private_codebase_·_case_study-C85A32?style=for-the-badge)](https://derar.ly)
[![Portfolio](https://img.shields.io/badge/PORTFOLIO-derar.ly-1E1E1E?style=for-the-badge&logo=safari&logoColor=white)](https://derar.ly)
[![Email](https://img.shields.io/badge/CONTACT-walkthrough_on_request-6B655C?style=for-the-badge&logo=gmail&logoColor=white)](mailto:zzxxccz908@gmail.com)

<sub>Architected & built by [Derar Ramadan](https://github.com/DerarRamadan)</sub>

</div>

---

## Why this exists

Coastal Libya rents chalets and resorts on WhatsApp, cash, and trust. Zwara replaces that with a **vacation-rental marketplace whose financial core behaves like a bank**: bookings hold their dates inside ACID transactions while guests upload bank slips; admins verify; owners claim into a double-entry wallet. Every balance change is an immutable record.

This repository is the **public case study**. The production codebase is private — a guided walkthrough is available on request.

---

## Scale at a glance

| | |
|:---|:---|
| **27+** | API endpoints |
| **25+** | database tables |
| **3** | isolated portals — guest · owner · super-admin |
| **4** | escrow ledger operations — credit · reserve · debit · release |
| **RTL** | Arabic-first interface with full English parity |

---

## System architecture

```
┌─────────────────────────────────────────────────────────────────┐
│  Clients — React 19 SPA (tri-portal)                             │
│  ┌───────────────┐ ┌──────────────────┐ ┌─────────────────────┐ │
│  │ Guest Portal  │ │ Owner Portal     │ │ Admin Portal        │ │
│  │ · discovery & │ │ · listings &     │ │ · receipt verifica- │ │
│  │   interactive │ │   availability   │ │   tion queue        │ │
│  │   maps        │ │   matrix         │ │ · dispute & refund  │ │
│  │ · booking     │ │ · digital check- │ │   engine            │ │
│  │   holds ·     │ │   in / damage    │ │ · treasury · period │ │
│  │   checkout    │ │   handovers      │ │   closing · PDFs    │ │
│  │ · reviews     │ │ · wallet claims  │ │                     │ │
│  └───────────────┘ └──────────────────┘ └─────────────────────┘ │
└───────────────────────────┬─────────────────────────────────────┘
           REST + Zod contracts / typed WebSocket events
┌───────────────────────────┴─────────────────────────────────────┐
│  API & Real-Time Layer                                           │
│  ┌───────────────────┐ ┌──────────────────┐ ┌────────────────┐ │
│  │ Fastify HTTP 5.8+ │ │ WebSocket Bus    │ │ FCM Dispatch   │ │
│  │ · Zod v4 validation│ │ · WsNotification │ │ · multi-device │ │
│  │ · JWT phone auth  │ │   Event · Ticket │ │   token mgmt   │ │
│  │ · RBAC            │ │   Update · live  │ │ · push when    │ │
│  │                   │ │   booking status │ │   offline      │ │
│  └───────────────────┘ └──────────────────┘ └────────────────┘ │
└───────────────────────────┬─────────────────────────────────────┘
                  Drizzle queries / presigned S3
┌───────────────────────────┴─────────────────────────────────────┐
│  Core Data & Financial Engine                                     │
│  ┌───────────────────┐ ┌──────────────────┐ ┌────────────────┐ │
│  │ Drizzle · MySQL   │ │ Double-Entry     │ │ S3 Object      │ │
│  │ · ACID date-      │ │ Wallet           │ │ Storage        │ │
│  │   collision locks │ │ · credit ·       │ │ · property     │ │
│  │ · immutable audit │ │   reserve ·      │ │   galleries    │ │
│  │   trail           │ │   debit ·        │ │ · bank transfer│ │
│  │                   │ │   release        │ │   receipts     │ │
│  │                   │ │ · escrow timers  │ │                │ │
│  └───────────────────┘ └──────────────────┘ └────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

---

## Escrow lifecycle — booking to payout

| Phase | Stage | What happens |
|:---|:---|:---|
| **1** | Booking & hold | Transactional date-collision check, commission math, 2-hour hold timer. |
| **2** | Payment & verification | Bank slip upload, admin approval, booking → confirmed, push to both sides. |
| **3** | Stay & handover | Digital check-in / check-out, photo-backed damage claims, 24h dispute window. |
| **4** | Claim & payout | Wallet credit → locked payout reserve → admin marks paid → funds dispatched. |

**Financial integrity highlights:**
- Immutable `wallet_transactions` ledger — every balance change is a credit / reserve / debit / release record
- ACID date-overlap locking inside transactions to make double-booking impossible
- Expiring reservation hold timers — unpaid holds release automatically
- End-to-end payment-flow test suite (Vitest)

---

## Technology matrix

| Layer | Stack |
|:---|:---|
| Runtime | Bun 1.3+ · Turborepo 2 workspace orchestration |
| API | Fastify 5.8+ · Zod v4 · JWT phone auth (jose) · RBAC |
| Frontend | React 19 · Vite · TanStack Query v5 · Radix UI |
| Database | MySQL (PlanetScale) · Drizzle ORM · 25+ tables |
| Real-time | @fastify/websocket · typed WsEventFactory bus |
| Push | Firebase Cloud Messaging — web & mobile |
| Storage | S3 presigned uploads — galleries · bank receipts |
| Ledger | Immutable wallet_transactions — credit / reserve / debit / release |
| Testing | Vitest — end-to-end payment-flow suite |

---

## License & code availability

This showcase repository (documentation, diagrams, case-study text) is released under [MIT](./LICENSE).

The **Zwara production codebase remains private**. A guided code walkthrough is available to serious parties: **zzxxccz908@gmail.com**.

<div align="center">

<sub><i>© 2026 Derar Ramadan · [derar.ly](https://derar.ly) · Tripoli, Libya / Remote Worldwide</i></sub>

</div>
