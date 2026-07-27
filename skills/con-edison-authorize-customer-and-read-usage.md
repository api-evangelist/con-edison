---
name: Authorize a Con Edison customer and read their interval usage
description: Run the Green Button Connect My Data OAuth 2.0 authorization-code flow for one Con Edison or Orange & Rockland customer, then walk the subscription down to interval readings.
api: openapi/con-edison-green-button-connect-my-data-swagger.json
generated: '2026-07-27'
method: generated
source: https://www.coned.com/-/media/files/coned/documents/accountandbilling/share-my-data/onboarding-doc.pdf
operations:
  - Token
  - getAllUsagePointsBySubscription
  - getUsagePointsForSubscription
  - getAllMeterReadingsForUsagePointInSubscription
  - getAllIntervalBlocksForUsagePointMeterReadingInSubscription
  - getAllReadingTypes
  - getLocalTimeParameters
---

# Authorize a Con Edison customer and read their interval usage

Prerequisite that cannot be skipped: you must already be an onboarded Share My Data third party.
`client_id`, `client_secret` and the Registration Access Token are emailed by the Con Edison
onboarding team only after a signed Data Security Agreement and supervised certification testing
(30–60 days). There is no self-serve key.

Base URIs — production `https://api.coned.com/gbc/espi/1_1`, test `https://apit.coned.com/gbc/espi/1_1`.

## 1. Send the customer to the right data custodian

Build the authorization redirect against the custodian that owns the account:

- Con Edison production: `https://www.coned.com/en/` · test: `https://uat10.coned.com/en/`
- Orange & Rockland production: `https://www.oru.com/en/` · test: `https://uat10.oru.com/en/`

Carry the scope string you need (see `scopes/con-edison-scopes.yml`). For electric consumption the
minimum is `FB=1_3_4` plus the metering block, e.g.
`FB=1_3_4_5;IntervalDuration=Monthly_3600_900_300;BlockDuration=Monthly_Daily;HistoryLength=63072000;BR=<ApplicationInformationId>`.
`BR` is your ApplicationInformation ID — read it with `getThirdPartyApplicationById`.

The customer picks the account, the sharing duration (until revoked / fixed period / one time 24
hours) and may REMOVE scopes before accepting. On decline you receive
`?error=access_denied&error_description=Customer denied authorization.&state=...` on your redirect
URI. If any redirect parameter is missing or wrong the customer lands on an error page instead.

## 2. Exchange the code — `Token`

The authorization code expires in **60 seconds** and is single use. POST to `/oauth/Token`:

```
POST https://api.coned.com/gbc/espi/1_1/oauth/Token
Authorization: Basic base64(client_id:client_secret)
Accept: application/json
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&code=<authorization code>&redirect_uri=<redirect uri>
```

The response carries `access_token`, `refresh_token`, `token_type: Bearer`, `expires_in: 3600`,
`scope`, `resourceURI`, `authorizationURI` and `customerResourceURI`. **Persist the refresh token** —
Con Edison does not store it, and losing it forces the customer to revoke and re-authorize. Cache the
access token for its full hour with a stored timestamp; the token endpoint allows only **50 calls per
minute** (the authorization-code flow is excluded from that limit).

Renew with `grant_type=refresh_token&refresh_token=<token>&scope=<same FB string>`. A refresh token
unused for a year expires.

## 3. Walk the subscription

Every subsequent call is `Authorization: Bearer <access_token>`.

1. `getAllUsagePointsBySubscription` — `GET /resource/Subscription/{subscriptionId}/UsagePoint`.
   One usage point per service delivery point; an account may have several.
2. `getUsagePointsForSubscription` — one usage point by id, when you already know it.
3. `getAllMeterReadingsForUsagePointInSubscription` —
   `GET /resource/Subscription/{subscriptionId}/UsagePoint/{usagePointId}/MeterReading`.
4. `getAllIntervalBlocksForUsagePointMeterReadingInSubscription` —
   `.../MeterReading/{meterReadingId}/IntervalBlock`. This is where the readings live.

Filter with `published-min` / `published-max` (ISO-8601, **UTC**) and page with `startIndex`. Note the
contract spells these hyphenated on the historic resources and camelCase (`publishedMin`,
`publishedMax`, `updatedMin`, `updatedMax`) on the real-time batch resources — follow the spec per
operation.

## 4. Interpret the readings correctly

- Call `getAllReadingTypes` (and `getLocalTimeParameters`) before you trust a number.
- **Scaling is mandatory**: `Actual Consumption = IntervalReading.value × 10^powerOfTenMultiplier`.
  Con Edison currently uses `3` for electric and `0` for gas. `uom` 72 = Watt-hours, 119 = cubic feet
  (gas is billed in CCF).
- `ReadingQuality.quality = 17` means good, validated data.
- Timestamps are epoch seconds; queries must be in UTC (EST = UTC−5, EDT = UTC−4). A normal day has
  288 five-minute intervals, 276 when DST starts, 300 when DST ends.
- Solar accounts return **NET** consumption.
- Granularity: 5-minute (commercial AMI electric), 15-minute (residential AMI and legacy interval
  electric), 1-hour (gas AMI), monthly (non-interval meters). History is capped at 2 years.
- Interval data availability: 80–90% within 24 hours, 99% within 3 days, 99.8% within 7 days.

## 5. Errors and revocation

Only `400` and `401` are declared (see `errors/con-edison-problem-types.yml`). A `401` mid-stream
usually means the hour expired, the customer revoked sharing, the fixed period ended, the one-time
24-hour window closed, or the authorization auto-revoked after **365 days** without a call from you.
Touch each authorization at least once a year to keep it alive; list them with
`Get all Third Party Authorizations`.

There is no status page — report production issues to `dlsharemydatatech@coned.com`.
