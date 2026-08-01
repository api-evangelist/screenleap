---
name: Start and stop a Screenleap screen share
description: Create a screen share session, hand the viewer URL to viewers, then stop the session when done.
api: openapi/screenleap-openapi.yml
operations: [createScreenShare, stopScreenShare, getScreenShare]
---

# Start and stop a Screenleap screen share

Use the Screenleap API to programmatically start a share session, direct viewers to it, and stop it when finished.

## Authentication
Send both headers on every request over HTTPS:
- `accountid: <your account id>`
- `authtoken: <your auth token>`

Base URL: `https://api.screenleap.com/v2`

## Steps
1. **Create the session** — call `createScreenShare` (`POST /screen-shares`). The response returns `screenShareData` and `viewerUrl`.
2. **Start sharing in the browser** — include the screenleap.js library and pass the returned data to `screenleap.startSharing('DEFAULT', [screenShareData])`.
3. **Send viewers** — direct viewers to the `viewerUrl` or embed it in an iframe.
4. **(Optional) Check status** — call `getScreenShare` (`GET /screen-shares/{screenShareCode}`) to read `isActive`, `totalViewers`, `userMinutes`, and the `participants` array.
5. **Stop the session** — call `stopScreenShare` (`POST /screen-shares/{screenShareCode}/stop`) when the share is complete.

## Rules
- No idempotency key is documented — do not assume retry-safety on create; check for an existing active session first if retrying.
- Handle `401` (bad credentials), `403` (session not yours), and `404` (unknown/ended share code). See errors/screenleap-problem-types.yml.
- Attach an `externalId` when creating sessions to correlate them with your own records for later `listScreenShares` filtering.
