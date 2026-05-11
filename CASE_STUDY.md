# Kes Track case study

## About project

Kes Track is a Shopify app case study focused on **closing the loop between store behavior and paid media measurement**. The product connects merchant storefront events to **Google Ads** (and **GA4** on higher tiers) using Shopify’s **customer events / Web Pixels** pipeline, with **OAuth-linked Google accounts**, **per-event conversion mapping**, and **Shopify subscription billing** aligned to feature gates. The write-up emphasizes practical choices: embedded admin UX, server-side secrets and token storage, plan-aware event limits, privacy-compliant webhooks, and clear boundaries between the pixel runtime, public APIs, and the admin app—plus measurable outcomes (to be filled with real metrics) and lessons for shipping reliable marketing-tech on Shopify.

## Technology stack

- **Platform**: Shopify (embedded admin app)
- **Language / UI**: TypeScript, React 18, React Router 7, Shopify Polaris + App Bridge
- **Shopify**: Admin GraphQL (e.g. web pixels), Web Pixels extension (`@shopify/web-pixels-extension`), app-specific webhooks, Shopify Billing (app subscriptions)
- **Backend / Data**: Node.js, Prisma + PostgreSQL (sessions, shop settings, pixel configs, usage counters)
- **Integrations**: Google Ads API (`google-ads-api`), Google OAuth for Ads (and GA4 auth flows in-app)
- **Hosting / ops**: Example config in README for Google Cloud Run / Render-style deployment; env-based secrets

## The client

The client is a **Shopify merchant or agency** that runs (or wants to run) **Google Ads** with trustworthy **purchase**, **add to cart**, and **checkout** signals—without fragile theme hacks. They care about accurate conversion actions, attribution-friendly context (e.g. UTM / GCLID handling in the pixel), simple onboarding inside the Shopify admin, and predictable monthly cost with trials and upgrades.

## The solution

- **Shopify-native installation & session model**: Uses `@shopify/shopify-app-react-router` with Prisma session storage so the embedded app stays authenticated and aligned with Shopify’s token lifecycle.
- **Web Pixel–based measurement core**: A customer events extension subscribes to storefront analytics events and forwards configured behavior to your backend/public endpoints, with client-side logic for UTM, GCLID persistence, referrer/first-touch style context—reducing “unknown direct” noise for reporting.
- **Google Ads conversion mapping**: Per-shop, per–Google Ads account configuration for purchase, add to cart, and begin checkout, including conversion IDs/labels and enable/disable toggles stored in `PixelConfig` and surfaced in the admin dashboard.
- **Plan-gated features & usage**: Central `plan-config` defines Basic vs Premium (and legacy Pro), trial windows, and order/event limits for funnel events; Premium unlocks add to cart / begin checkout without artificial caps and GA4 integration paths described in product copy.
- **Merchant billing in-channel**: Subscription create/update/cancel flows via Shopify’s billing APIs and webhooks (`app_subscriptions/update`), keeping entitlement state in sync with what the pixel and APIs are allowed to emit.
- **GA4 alignment (Premium)**: Server-side GA4 helpers and OAuth-style setup routes so analytics properties and measurement IDs can be wired for merchants who need Ads + GA4 in one product story.


## The results

*(Replace bullets below with your real post-launch metrics.)*

- Improved **Ads conversion reliability** by moving measurement off ad-hoc theme scripts into Shopify’s first-party pixel pipeline.
- Reduced **merchant setup time** with an in-admin flow for accounts, conversions, and embed/pixel status.
- Increased **signal depth** on Premium (funnel events + GA4) without exposing unlimited free usage that would break unit economics.
- Stronger **billing and entitlement consistency** via subscription webhooks and plan checks before expensive or gated events fire.



## Demo

- Product walkthrough: [YouTube — Kes Track functionality](https://www.youtube.com/watch?v=O-1SC7y6B04)
- App Link: [Kes Track](https://apps.shopify.com/kes-track)
