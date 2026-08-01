---
name: Report Screenleap share usage
description: List and inspect screen share sessions to report viewers, minutes and cost.
api: openapi/screenleap-openapi.yml
operations: [listScreenShares, listRecentScreenShares, getScreenShare]
---

# Report Screenleap share usage

Aggregate screen share activity for billing, analytics, or auditing.

## Authentication
Send `accountid` and `authtoken` headers over HTTPS. Base URL: `https://api.screenleap.com/v2`.

## Steps
1. **List sessions in a window** — call `listScreenShares` (`GET /screen-shares`) with `startedAfter` / `startedBefore` / `endedAfter` / `endedBefore` (milliseconds from epoch). Filter by `externalId` to scope to one of your customers. Results cap at 1000.
2. **Or pull the recent set** — call `listRecentScreenShares` (`GET /recent-screen-shares`) for the latest sessions without building a time window.
3. **Inspect a session** — call `getScreenShare` (`GET /screen-shares/{screenShareCode}`) and read `totalViewers`, `userMinutes`, `costInCents`, and the `participants` array (each with `durationInMinutes` and `isPresenter`).

## Rules
- Times are milliseconds from epoch; pass `dateFormat` (Java SimpleDateFormat) if you want formatted dates back.
- `listScreenShares` returns at most 1000 sessions — narrow the time window to page through larger ranges.
- Handle `401` for bad credentials. See errors/screenleap-problem-types.yml.
