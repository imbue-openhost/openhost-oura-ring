# Oura Connector

A self-hosted connector for your [Oura ring](https://ouraring.com/) health data
on Cloud in a Bottle. It syncs the metrics your ring collects — sleep, activity,
and readiness — from the Oura Cloud API into a private SQLite database on your
own zone, shows them in a dashboard, and re-exposes them to your other apps
through the shared
[health-data service](https://github.com/imbue-openhost/health-data-service-spec)
(the same interface the Apple Health connector provides), so anything else you
run can read your health history without going back to Oura's cloud.

**Who it's for:** Oura ring owners who want to own and explore their data off of
Oura's servers, and Cloud in a Bottle users who want a single health-data source
their other apps can build on.

## Setup

Oura's API uses OAuth, so you connect it once with your own Oura developer
credentials:

1. Create an OAuth application in the
   [Oura Cloud developer console](https://cloud.ouraring.com/oauth/applications).
   - Set the **Redirect URI** to `https://<your-app>.<your-zone>/oauth/callback`.
   - Copy the **Client ID** and **Client Secret**.
2. Open this app and go to **Setup**. Paste the Client ID and Secret and click
   connect — you'll be sent to Oura to authorize, then redirected back.
3. Run an initial **Backfill** to pull your history, then **Sync** keeps it up to
   date.

Your Oura credentials and tokens are stored only in this app's private database
on your zone.

## What it does

- **Dashboard** (`/`) — recent sleep, activity, and readiness at a glance.
- **Sync / Backfill** — pull the latest data, or backfill your full history.
- **Provides the `health-data` service** (`/api/`) — other apps on your zone can
  read your metrics, time series, sleep sessions, and workouts through the
  Cloud in a Bottle service mesh (you grant access per app).

## Access

The app is owner-only: every page and API is gated by the Cloud in a Bottle
router, so only the zone owner (auto-recognized, no separate login) can reach the
dashboard, the OAuth setup, or the data. The `health-data` service is consumed by
other apps through the service mesh, not the public web.

## Links

- Oura — https://ouraring.com/
- Oura Cloud API — https://cloud.ouraring.com/v2/docs
- Health data service spec — https://github.com/imbue-openhost/health-data-service-spec
