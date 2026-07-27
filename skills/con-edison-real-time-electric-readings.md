---
name: Read Con Edison near-real-time electric interval readings
description: Use the non-standard Green Button RealTime resource family for the last 24 hours of electric interval data, with the right expectations about latency and data quality.
api: openapi/con-edison-green-button-connect-my-data-swagger.json
generated: '2026-07-27'
method: generated
source: https://www.coned.com/-/media/files/coned/documents/accountandbilling/share-my-data/onboarding-doc.pdf
operations:
  - getAllRealTimeIntervalBlocksForUsagePointMeterReadingInSubscription
  - getRealTimeIntervalBlockForUsagePointMeterReadingInSubscription
  - getAllRealTimeReadingTypes
  - getRealTimeReadingTypeById
  - getAllRealTimeUsageDataInBatch
  - getAllRealTimeUsageDataForSubscriptionInBatch
  - getRealTimeUsagePointBySubscriptionInBatch
---

# Read Con Edison near-real-time electric interval readings

This family is an explicit, documented extension: *"Connect my data will continue to support real
time APIs even though they are not part of the GBC V3.3 Standard."* Do not expect it from other
Green Button implementations.

## Know the envelope before you build on it

- **Electric only.** Real-time APIs are not applicable to gas service.
- **Last 24 hours only.** Nothing older is available through this family — use the historic
  resources for that.
- **45-minute latency** from request processing time.
- **Provisional and unvalidated.** Con Edison states real-time interval data is "not billing quality
  data". Historical interval data is the finalized, validated record; a `ReadingQuality.quality` of
  `17` marks good, validated data.
- Granularity mirrors the historic side: 5-minute for commercial AMI electric, 15-minute for
  residential AMI and legacy interval meters.

Never reconcile bills, settle payments or trigger money movement off these readings. Use them for
alerting, load feedback and near-live displays, and reconcile against the historic resources once the
data has finalized (80–90% within 24 hours, 99% within 3 days, 99.8% within 7 days).

## Direct reads

1. `getAllRealTimeReadingTypes` — `GET /resource/RealTime/ReadingType` — and
   `getRealTimeReadingTypeById` for one id. Read these first: `powerOfTenMultiplier` and `uom` live
   here, and `Actual Consumption = IntervalReading.value × 10^powerOfTenMultiplier` (electric
   multiplier is currently 3, `uom` 72 = Watt-hours).
2. `getAllRealTimeIntervalBlocksForUsagePointMeterReadingInSubscription` —
   `GET /resource/RealTime/Subscription/{subscriptionId}/UsagePoint/{usagePointId}/MeterReading/{meterReadingId}/IntervalBlock`
3. `getRealTimeIntervalBlockForUsagePointMeterReadingInSubscription` — one block by id.

## Batch reads

`getAllRealTimeUsageDataInBatch` (`/resource/RealTime/Batch/Bulk/{bulkId}`),
`getAllRealTimeUsageDataForSubscriptionInBatch` and `getRealTimeUsagePointBySubscriptionInBatch` are
asynchronous exactly like the historic batch family: HTTP 202, then a notification POST carrying a
BatchList of resource URLs, 2-day retention, 200 MB chunking. See
`skills/con-edison-bulk-batch-and-notifications.md`.

**Parameter spelling differs here.** The real-time batch operations take `publishedMin`,
`publishedMax`, `updatedMin` and `updatedMax` (camelCase), while the historic resources take
`published-min` and `published-max` (hyphenated). Follow the spec per operation.

## Time handling

Queries are always UTC — EST is UTC−5, EDT is UTC−4. A normal day is 288 five-minute intervals; the
day DST starts has 276 and the day it ends has 300. `getLocalTimeParameters` returns the timezone and
DST parameters if you need them programmatically.

## Scope and access

Real-time data is covered by the same customer authorization and the same ESPI functional blocks as
historic consumption (`FB=1_3_4` plus the metering block). Access tokens live 3600 seconds and must be
cached; the token endpoint is capped at 50 calls per minute, and no rate limit is documented on the
resource endpoints.
