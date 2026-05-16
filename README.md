# My POD Shop Analytics

A personal, cross-platform analytics dashboard for my own Etsy print-on-demand shop, built with Flutter and Supabase. Single-user, single-shop, non-commercial — used exclusively by me to make better decisions about pricing, inventory, and SEO.

> **Status:** 🚧 In active development. Awaiting Etsy API approval.

## Why this project exists

I run a print-on-demand store on Etsy and wanted deeper insight into my shop's performance than the built-in Shop Manager provides — specifically historical tracking of views, favorites, and revenue per listing, plus custom profit margins that account for POD supplier costs. Instead of paying $15–30/month for an off-the-shelf analytics tool, I'm building exactly what I need.

## Features (planned)

### Phase 1 — MVP
- OAuth 2.0 (PKCE) connection to my own Etsy shop
- Automated sync every 6 hours of listings, receipts, transactions, and ledger entries
- Revenue dashboard (30/90/365 days)
- Per-listing historical tracking (views, favorites, sold count over time)

### Phase 2 — Keyword Research & Rank Tracking
- Manual keyword search against Etsy's public listing search
- Daily rank-position tracking for keywords I care about
- Google Trends integration as a search-volume proxy

### Phase 3 — Competitor Intelligence
- Snapshot tracking of public listing data from shops I follow
- Estimated monthly sales heuristics

### Phase 4 — Optimization & Alerts
- Listing audit against the Etsy Seller Handbook (tags, title length, photo count, etc.)
- Push notifications for rank drops, new reviews, and significant changes

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Flutter (Android, iOS, macOS, Windows, Linux) |
| State management | Riverpod 2.x |
| Architecture | Clean Architecture, feature-first |
| Backend | Supabase (Postgres + Edge Functions + pg_cron) |
| API | Etsy Open API v3 |
| Auth | OAuth 2.0 with PKCE |
| Charts | fl_chart |
| Local cache | Drift (SQLite) |

## Etsy API Compliance

This application uses the Etsy API in strict accordance with the [Etsy API Terms of Use](https://www.etsy.com/legal/api/):

- **Personal access only.** Single-user, single-shop, non-commercial. The application is not distributed to third parties.
- **Caching honored.** Listings are cached for no more than 6 hours; other content for no more than 24 hours, per Section 4 of the API Terms.
- **No scraping.** All data is retrieved exclusively through official Etsy API v3 endpoints. The app does not scrape Etsy.com HTML.
- **No AI/ML training.** Data retrieved from the Etsy API is not used to train machine learning models.
- **Attribution.** The required attribution — *"This application uses Etsy's API, but is not endorsed or certified by Etsy, Inc."* — is displayed in the application's About screen.
- **Secure credential storage.** OAuth tokens are stored in the operating system's secure keystore (Keychain on iOS/macOS, Keystore on Android, encrypted at rest on desktop).

## Architecture Overview

```
Flutter App  ──OAuth──▶  Etsy Open API v3
     │
     │ HTTPS + JWT
     ▼
Supabase (Postgres)
     │
     │ pg_cron (every 6h)
     ▼
Edge Function: sync_etsy
     │
     └──▶ Refreshes tokens, fetches latest data,
          writes append-only snapshots for historical tracking
```

## Repository Structure

```
lib/
├── core/              # HTTP client, auth, errors, utilities
├── features/
│   ├── auth/          # Etsy OAuth flow
│   ├── dashboard/     # Revenue and overview widgets
│   ├── listings/      # Per-listing detail and history
│   ├── orders/        # Receipts, transactions, ledger
│   ├── keywords/      # Keyword research and rank tracking
│   └── competitors/   # Competitor snapshot tracking
└── main.dart

supabase/
├── migrations/        # SQL schema migrations
└── functions/         # Edge Functions (sync_etsy, etc.)
```

## Development Status

This is a personal project developed in my spare time. There is no public release, no app store listing, and no plan to distribute it. If you have arrived here from the Etsy developer portal as part of an API application review, please feel free to reach out via the contact information in my GitHub profile.

## License

[MIT](LICENSE) © 2026 David Menendez

---

*This application uses Etsy's API, but is not endorsed or certified by Etsy, Inc.*
