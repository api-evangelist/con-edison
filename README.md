# Con Edison (con-edison)

Consolidated Edison Company of New York, Inc. (CECONY, trading as Con Edison) is the investor-owned electric, gas and steam utility that serves New York City and Westchester County, and together with its sibling Orange & Rockland Utilities (ORU) forms the regulated utility core of Consolidated Edison, Inc. It is a wires-and-pipes distribution utility rather than a competitive retailer: it owns and operates the distribution system, meters the customer and bills the customer, while wholesale energy markets are run by NYISO and competitive supply is sold by ESCOs. It is rate-regulated by the New York State Public Service Commission.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/con-edison/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/con-edison/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- New York
- Utilities
- Electricity
- Gas
- Steam
- Smart Metering
- Green Button
- Energy Data
- Grid
- Distribution
- Hosting Capacity
- Distributed Energy Resources
- Solar
- EV Charging
- Demand Response

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### Con Edison Green Button Connect My Data API

Con Edison and Orange & Rockland's Green Button Connect My Data (CMD) implementation, branded "Share My Data" — the NAESB REQ.21 Energy Services Provider Interface (ESPI) machine-to-machine API through which a customer-authorized third party retrieves that customer's electric and gas interval usage, usage summaries, billing information and retail customer records in Atom/ESPI XML. The published Swagger 2.0 definition ("DCX GBC API V2", basePath `/gbc/espi/1_1`) documents 37 paths across ApplicationInformation, Authorization, ReadServiceStatus, UsagePoint, MeterReading, IntervalBlock, ReadingType, LocalTimeParameters, UsageSummary, RetailCustomer, Batch and RealTime resources.

- **Human URL:** [https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party](https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party)
- **Base URL:** `https://api.coned.com/gbc/espi/1_1`

#### Properties

- [OpenAPI](openapi/con-edison-green-button-connect-my-data-swagger.json) — Swagger 2.0, harvested verbatim
- [Postman Collection](collections/con-edison-green-button-connect-my-data.postman_collection.json) — "GBC Certification Third party V3.3"
- [Postman Environment](collections/con-edison-green-button-connect-my-data.postman_environment.json)
- [Documentation — become a third party](https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party)
- [Documentation — Third-Party Technical Onboarding Document (PDF, v4.4, updated 2026-05-07)](https://www.coned.com/-/media/files/coned/documents/accountandbilling/share-my-data/onboarding-doc.pdf)
- [FAQ — Share My Data FAQ for Third Party Vendors (PDF)](https://www.coned.com/-/media/files/coned/documents/accountandbilling/share-my-data/faq.pdf)
- [Sign Up — Third-Party Company Registration Form](https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party/registration-form)
- [Standard — NAESB REQ.21 ESPI](https://www.naesb.org/espi_standards.asp)
- [Standard — Green Button Connect My Data](https://www.greenbuttonalliance.org/green-button-connect-my-data-cmd)

### Con Edison Hosting Capacity Map REST API

Con Edison's distribution-grid open data, published as anonymously readable Esri ArcGIS REST feature services behind the public Hosting Capacity Map. Con Edison's own documentation states that "REST API access allows users to overlay the Con Edison Hosting Capacity Map data with their own GIS systems and mapping tools." Coverage includes CECONY and ORU segmented and network hosting capacity, EV load-serving transformer capacity, 33kV feeders, non-wires-solutions impacted networks, Locational System Relief Value eligibility, disadvantaged and overburdened communities, and the IEDR-facing layers. No login, key, or agreement required.

- **Human URL:** [https://www.coned.com/en/business-partners/hosting-capacity](https://www.coned.com/en/business-partners/hosting-capacity)
- **Base URL:** `https://services.arcgis.com/ciPnsNFi1JLWVjva/arcgis/rest/services`

#### Properties

- [Documentation — Hosting Capacity](https://www.coned.com/en/business-partners/hosting-capacity)
- [Documentation — About the Hosting Capacity Map](https://www.coned.com/en/business-partners/hosting-capacity/about)
- [Application — Hosting Capacity Web Experience](https://experience.arcgis.com/experience/d9d758c7736b44909dc3781937ca2ed5)
- [API Reference — CECONY_NodalHCV_Prod](https://services.arcgis.com/ciPnsNFi1JLWVjva/arcgis/rest/services/CECONY_NodalHCV_Prod/FeatureServer)
- [API Reference — CECONY_EVM_Prod](https://services.arcgis.com/ciPnsNFi1JLWVjva/arcgis/rest/services/CECONY_EVM_Prod/FeatureServer)
- [API Reference — CE_NWS_Networks_Prod](https://services.arcgis.com/ciPnsNFi1JLWVjva/arcgis/rest/services/CE_NWS_Networks_Prod/FeatureServer)
- [API Reference — IEDR_CECONY](https://services.arcgis.com/ciPnsNFi1JLWVjva/arcgis/rest/services/IEDR_CECONY/FeatureServer)

## Energy data posture

- **Mandate regime:** `green-button-voluntary`. The United States has no federal energy consumer data right, and nothing equivalent to Australia's Consumer Data Right (energy) or Ontario Regulation 633/21 applies. Qualification, recorded so this is not read as pure voluntarism: Con Edison committed to implement the first phase of Green Button Connect by end of 2017 in the Joint Utilities' Supplemental DSIP (NY PSC Case 16-M-0411); the Commission issued an Order Adopting a Data Access Framework in Case 20-M-0082 (2021-04-15); and Con Edison's own FAQ binds third parties to "the requirements set forth in the Customer Data Access Tariff." That is state-regulator supervision of a voluntary standard — not a consumer data right.
- **Mandate status:** `live-implemented`, and verified rather than taken on trust. The documented production base URI `https://api.coned.com/gbc/espi/1_1/resource/ReadServiceStatus` returned HTTP 401 anonymously on 2026-07-27, and `POST /gbc/espi/1_1/oauth/Token` without credentials returned `{"Message":"Unauthorized. Access token is missing or invalid."}`. A 37-path Swagger 2.0 contract for that exact base path is published by Con Edison and parses. A Postman collection named "GBC Certification Third party V3.3" ships alongside it. The onboarding document is at version 4.4, last revised 2026-05-07. **Not verified:** Green Button Alliance certification from a primary GBA source — the GBA certification page publishes no directory and lists Con Edison only as a sponsor member.
- **Data standard:** Green Button Connect My Data — NAESB REQ.21 ESPI. Version evidence: base path `/gbc/espi/1_1` (ESPI 1.1), Swagger title "DCX GBC API V2", Postman collection "GBC Certification Third party V3.3". Atom/ESPI XML payloads; OAuth 2.0 per RFC 6749 with Bearer tokens per RFC 6750. Real-time endpoints are a documented extension "even though they are not part of the GBC V3.3 Standard." No CDR, OCPP, OCPI, OpenADR, IEEE 2030.5 or IEC CIM reference found. The grid-data side uses no energy standard — it is Esri ArcGIS REST.
- **Consumer data API:** Yes. Customer-authorized third parties retrieve electric and gas interval usage (5-minute commercial AMI, 15-minute residential AMI, 1-hour gas AMI, monthly non-interval), usage summaries, billing information and retail customer records, up to 2 years of history, plus electric-only real-time readings for the last 24 hours at 45-minute latency. Consent is customer-chosen, revocable, and auto-revoked after 365 days of inactivity.
- **Market data open:** Yes on the distribution-grid axis, anonymously and with no agreement — hosting capacity, EV transformer headroom, non-wires-solutions networks, 33kV feeders, LSRV eligibility and the IEDR layers, all HTTP 200 unauthenticated over ArcGIS REST. This is grid/system data, not wholesale market data; NYISO and EIA publish that, and none of it is claimed here. There is no `data.coned.com` and no open data portal.
- **Access gate:** `application-approval`. To reach customer data a developer must complete the Third-Party Company Registration Form, sign the Data Security Agreement and accept terms, submit a technical onboarding form (all URLs reachable from Con Edison's network, TLS 1.2+), receive credentials by email, build their own customer-authorization portal, pass supervised testing in a test environment they cannot access independently, submit the API testing checklist, and re-register for production. 30–60 days for technical onboarding; 90-day windows on both the DSA and testing; no cost. Con Edison states flatly that it "is unable to support API development for third parties." The Hosting Capacity REST services have no gate at all; Download My Data is `customer-account-required`.
- **Auth model:** OAuth 2.0 (RFC 6749) / Bearer (RFC 6750) over TLS 1.2+. Authorization endpoint on the web property (`https://www.coned.com/en/oauth/authorize`), token endpoint on the API host (`https://api.coned.com/gbc/espi/1_1/oauth/Token`), HTTP Basic client authentication. Grants: `authorization_code` (1-minute single-use code), `refresh_token` (1-year idle expiry, refresh token not stored by Con Edison), and `client_credentials` for the bulk Third-Party Client Access Token. Access tokens live 3600s; 50 token calls/minute. Scopes are ESPI functional blocks, e.g. `FB=1_3_4_5_7_8_10_15_16_51_53_56_57_58_60;IntervalDuration=…;BlockDuration=…;HistoryLength=63072000;BR=<BulkId>`. The Swagger declares no `securityDefinitions`; `/.well-known/openid-configuration` returns 404. The ArcGIS services require no auth.
- **Home market:** United States — New York (NYC and Westchester; ORU covers parts of NY, NJ and PA).

The split is the finding: the same company serves its grid data to anyone anonymously, and requires a signed contract plus per-customer OAuth consent for a single interval reading. Full probe log, HTTP statuses, and evidence are recorded in [review.yml](review.yml).

## Common Properties

- [Website](https://www.coned.com/)
- [About](https://www.coned.com/en/about-us/company-information)
- [Parent Company — Consolidated Edison, Inc.](https://www.conedison.com/)
- [Documentation — how to access customer data](https://www.coned.com/en/business-partners/access-customer-data)
- [Documentation — share energy usage data](https://www.coned.com/en/accounts-billing/share-energy-usage-data)
- [Documentation — Green Button Download My Data](https://www.coned.com/en/save-money/make-better-energychoices-with-green-button)
- [Sign Up — Third-Party Company Registration Form](https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party/registration-form)
- [Authentication — account dashboard](https://www.coned.com/en/accounts-billing/dashboard)
- [Privacy Policy](https://www.coned.com/en/conedison-privacy-statement)
- [Terms of Service](https://www.coned.com/en/accounts-billing/dashboard/billing-and-usage/terms-of-use)
- [Email — Share My Data technical onboarding team](mailto:dlsharemydatatech@coned.com)
- [LinkedIn](https://www.linkedin.com/company/con-edison)
- [Regulator — New York State Department of Public Service](https://dps.ny.gov/)
- [Standard — Green Button Alliance](https://www.greenbuttonalliance.org/)
- [Standard — NAESB ESPI](https://www.naesb.org/espi_standards.asp)
- [Sister Company — Orange & Rockland Green Button Connect My Data](https://www.oru.com/en/accounts-billing/share-energy-usage-data/become-a-third-party)

## Maintainers

- Kin Lane — kin@apievangelist.com
