---
name: Pull Con Edison usage in bulk with the Client Access Token and batch notifications
description: Mint a Third-Party Client Access Token with client_credentials, submit asynchronous Green Button batch requests across your whole authorized customer set, and consume the notification callback correctly.
api: openapi/con-edison-green-button-connect-my-data-swagger.json
generated: '2026-07-27'
method: generated
source: https://www.coned.com/-/media/files/coned/documents/accountandbilling/share-my-data/onboarding-doc.pdf
operations:
  - Token
  - getThirdPartyApplicationById
  - getAllUsageDataInBatch
  - getAllUsageDataForSubscriptionInBatch
  - getUsagePointBySubscriptionInBatch
  - getCustomerInformationInBatch
---

# Pull Con Edison usage in bulk with the Client Access Token

Use this when you need data for many customers at once instead of walking one subscription at a time.

## 1. Read your ApplicationInformation — `getThirdPartyApplicationById`

`GET /resource/ApplicationInformation/{applicationInformationId}` with the Registration Access Token
emailed to you at onboarding. It returns your `ClientId`, `ClientSecret`, the authorization-server and
resource endpoints, your `ThirdPartyNotifyUri`, `AuthorizationChangeNotifyUri`, granted `Scope` and
`GrantTypes`. The ApplicationInformation ID is also the `BR` (bulk resource) value used in scope
strings and the `bulkId` path parameter.

## 2. Mint the Client Access Token (CAT) — `Token`

```
POST https://api.coned.com/gbc/espi/1_1/oauth/Token
Authorization: Basic base64(client_id:client_secret)
Accept: application/json
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&scope=FB=34_35
```

The CAT covers every customer currently authorized with you, bounded by each customer's own granted
scope. Like all Con Edison tokens it lives **3600 seconds** and must be cached; the token endpoint
allows 50 calls per minute.

## 3. Submit the batch request

| Operation | Path | Use |
|---|---|---|
| `getAllUsageDataInBatch` | `/resource/Batch/Bulk/{bulkId}` | Everything for all authorized customers. `published-min` and `published-max` are **required** here. |
| `getAllUsageDataForSubscriptionInBatch` | `/resource/Batch/Subscription/{subscriptionId}` | One customer subscription |
| `getUsagePointBySubscriptionInBatch` | `/resource/Batch/Subscription/{subscriptionId}/UsagePoint/{usagePointId}` | One usage point |
| `getCustomerInformationInBatch` | `/resource/Batch/RetailCustomer/{subscriptionId}` | Retail customer records |

A batch request returns **HTTP 202** — the Data Custodian has accepted it and will assemble the data.
No payload comes back on this call.

## 4. Consume the notification

Con Edison POSTs a notification to the `ThirdPartyNotifyUri` you registered. Rules that bite in
production:

- The notification body is XML containing a **BatchList of resource URLs**; the URLs are XML-escaped
  (`&amp;`) and must be unescaped before you GET them. Use a real XML parser.
- GET each URL with the Authorization header associated with that batch request.
- The BatchList may mix Bulk resource URLs and individual ones.
- Responses larger than **200 MB** are chunked into multiple files.
- You have **2 days**. After that the assembled response is deleted and calling the URL is equivalent
  to submitting a fresh batch request.
- Repeating an identical request inside those 2 days returns the **cached** response.
- Do not resubmit while one is pending — duplicates are rejected and do not speed anything up.
- Notifications typically arrive within an hour, but up to 24 hours under heavy load. Build for the
  24-hour case; there is no polling endpoint and no delivery guarantee documented.
- Your endpoint must be reachable from the Con Edison network and support TLS 1.2+. Never rename or
  change a callback URL without emailing `ShareMyDataTech@coned.com` first — it will break the
  integration.
- No signature or shared-secret verification is documented for the inbound POST. Treat the
  notification as an untrusted trigger and authenticate the data by fetching it yourself.

## 5. Real-time variants

The same pattern exists for the non-standard real-time family — `getAllRealTimeUsageDataInBatch`,
`getAllRealTimeUsageDataForSubscriptionInBatch`, `getRealTimeUsagePointBySubscriptionInBatch` — which
use the camelCase `publishedMin` / `publishedMax` / `updatedMin` / `updatedMax` parameters. See
`skills/con-edison-real-time-electric-readings.md`.

Details of the callback surface: `asyncapi/con-edison-batch-notification-webhooks.yml`.
