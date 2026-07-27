---
name: Read Con Edison billing summaries and retail customer records
description: Retrieve usage summaries with cost and walk the retail-customer chain — account, agreement, service location, meter — for an authorized Con Edison or Orange & Rockland customer.
api: openapi/con-edison-green-button-connect-my-data-swagger.json
generated: '2026-07-27'
method: generated
source: https://www.coned.com/-/media/files/coned/documents/accountandbilling/share-my-data/onboarding-doc.pdf
operations:
  - getAllElectricPowerUsageSummaries
  - getAllElectricPowerUsageSummariesById
  - getCustomerInformationBySubscription
  - getCustomerAccountInSubscription
  - getCustomerAccountByAccountIdInSubscription
  - getCustomerAgreementByAccountIdInSubscription
  - getCustomerAgreementByCustomerAgreementId
  - getAllServiceLocationByCustomerAgreementIdAndAccountIdInSubscription
  - getServiceLocationByServiceLocationId
  - getAllMetersForServiceLocationInSubscription
  - getMeterBySerialNumberId
---

# Read Con Edison billing summaries and retail customer records

Two different scope families are in play, and the customer may have granted one without the other.

- **Billing / usage summary** needs `FB=1_3_15` (without cost) or `FB=1_3_15_16` (with cost).
- **Retail customer** needs `FB=51_53` at minimum; add `56` for billing information, `57` for
  account-agreement information, `58` for service-location information, `60` for meter information.

Check the granted scope on the authorization (`Get Third Party Authorization by ID`) before calling —
a missing block is a 401, not an empty list.

## Usage summaries

- `getAllElectricPowerUsageSummaries` —
  `GET /resource/Subscription/{subscriptionId}/UsagePoint/{usagePointId}/UsageSummary`
- `getAllElectricPowerUsageSummariesById` —
  `POST /resource/Subscription/{subscriptionId}/UsagePoint/{usagePointId}/UsageSummary/{usageSummaryId}`
  (note: the contract declares this one as POST, not GET)

Summaries are billing-period aggregates. **ESCO charges, when the customer buys supply from a
competitive energy service company, are included in `BillLastPeriod`.** Apply the same scaling rule as
everywhere else: `value × 10^powerOfTenMultiplier`, with `uom` 72 = Watt-hours and 119 = cubic feet.
A summary such as `overallConsumptionLastPeriod` with `powerOfTenMultiplier: 0`, `uom: 72`,
`value: 4488000` is 4,488,000 Wh.

## The retail-customer chain

Walk it in order — each id comes from the level above:

1. `getCustomerInformationBySubscription` — `GET /resource/Customer/{subscriptionId}`
2. `getCustomerAccountInSubscription` — `.../CustomerAccount`
3. `getCustomerAccountByAccountIdInSubscription` — `.../CustomerAccount/{accountId}`
4. `getCustomerAgreementByAccountIdInSubscription` — `.../CustomerAccount/{accountId}/CustomerAgreement`
5. `getCustomerAgreementByCustomerAgreementId` — `.../CustomerAgreement/{customerAgreementId}`
6. `getAllServiceLocationByCustomerAgreementIdAndAccountIdInSubscription` — `.../ServiceLocation`
7. `getServiceLocationByServiceLocationId` — `.../ServiceLocation/{serviceLocationId}`
8. `getAllMetersForServiceLocationInSubscription` — `.../Meter`
9. `getMeterBySerialNumberId` — `.../Meter/{serialNumber}`

Cardinalities Con Edison publishes (see `data-model/con-edison-data-model.yml`): an account has many
usage points and many reading types; an account has many customer agreements (one per service, so
electric + gas is two); an agreement can cover many service locations; each usage point maps to
exactly one meter; each meter reading maps to one interval block.

## Your obligation on identity

Con Edison states it explicitly in the onboarding document: the Retail Customer API supplies the
Account ID and address (street, city, state, town, postal code) for a subscription, and **the third
party is required to map and maintain the Account ID for its usage data**. Interval data carries no
account number of its own — if you do not persist the mapping at authorization time you cannot
reattach usage to a customer later.

This is PII under a signed Data Security Agreement, the Share My Data terms and the New York Customer
Data Access Tariff. Retrieve only what the customer's scope grants, and honour revocation: an
authorization ends when the customer revokes it, when a fixed period ends, after a one-time 24-hour
window, or automatically after 365 days without a call from you.

## Bulk alternative

For the same records across your whole authorized customer set, use `getCustomerInformationInBatch`
(`/resource/Batch/RetailCustomer/{subscriptionId}`) with the Client Access Token and consume the
notification — see `skills/con-edison-bulk-batch-and-notifications.md`.
