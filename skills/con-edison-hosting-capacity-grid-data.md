---
name: Query Con Edison hosting capacity and grid data anonymously
description: Read Con Edison's open distribution-grid data — segmented and network hosting capacity, EV transformer capacity, 33kV feeders, non-wires-solutions networks — straight from the public ArcGIS REST feature services, with no key and no agreement.
api: Con Edison Hosting Capacity Map REST API
generated: '2026-07-27'
method: generated
source: https://www.coned.com/en/business-partners/hosting-capacity/about
verified: live anonymous probes of the feature services on 2026-07-27
operations:
  - GET /{service}/FeatureServer?f=json
  - GET /{service}/FeatureServer/{layerId}?f=json
  - GET /{service}/FeatureServer/{layerId}/query
---

# Query Con Edison hosting capacity and grid data anonymously

Con Edison's own documentation states: *"REST API access allows users to overlay the Con Edison
Hosting Capacity Map data with their own GIS systems and mapping tools. The REST API endpoints are
available within the 'Rest API' tab on the Hosting Capacity map."* These are Esri ArcGIS feature
services — no login, no API key, no agreement. Every service below returned HTTP 200 anonymously on
2026-07-27.

Root: `https://services.arcgis.com/ciPnsNFi1JLWVjva/arcgis/rest/services`

## Services verified

| Service | What it carries |
|---|---|
| `CECONY_NodalHCV_Prod` | CECONY Segmented Hosting Capacity — layers: *Hosting Capacity for 3PH Segments*, *No Hosting Capacity for 1PH and 2PH Segments*, *Substation Level System Data* |
| `CECONY_EVM_Prod` | EV transformer capacity — 460v and 208v summer capacity |
| `CE_NWS_Networks_Prod` | Non-Wires Solutions project networks |
| `CECONY_33Kv_Prod` | CECONY 33kV feeder layer |
| `IEDR_CECONY` | Hosting capacity layers published for New York's Integrated Energy Data Resource — charge and discharge capacity for 3PH feeders plus EV transformer layers |

Equivalent `ORU_*` services exist for Orange & Rockland. The publishing ArcGIS Online account is
`HCV_Support` in the `conEd` organization; a content search of that owner returned 69 public items
covering LSRV eligibility, network and structure hosting capacity, disadvantaged and overburdened
communities, alternative fuel corridors and cost sharing.

## Three calls are all you need

1. **Service metadata** — `GET .../CECONY_NodalHCV_Prod/FeatureServer?f=json`
   Returns `serviceDescription`, `capabilities` (`Query,Extract`), `maxRecordCount` (6000 on this
   service) and the layer list with ids.
2. **Layer metadata** — `GET .../FeatureServer/0?f=json` for field definitions and extent.
3. **Query** — `GET .../FeatureServer/0/query?where=1%3D1&outFields=*&f=json`
   Standard ArcGIS query parameters apply: `where`, `outFields`, `geometry` + `geometryType` +
   `spatialRel`, `returnGeometry`, `resultOffset`, `resultRecordCount`, `f=json|geojson|pjson`.

Page with `resultOffset` / `resultRecordCount` against `maxRecordCount`; use
`returnCountOnly=true` first to size a pull.

## Fields on the segmented hosting capacity layer

Layer 0 of `CECONY_NodalHCV_Prod` returns, per feeder segment: `FEEDER_ID`, `FRIENDLY_CIRCUIT_NAME`,
`SUBSTATION`, `OPERATINGCOMPANY` (CECONY or ORU), `LOCAL_MIN` / `LOCAL_MAX` local minimum and maximum
hosting capacity in MW, `CONNECTED_DER` and `QUEUED_DER` (feeder DG connected / in queue, MW),
`ANTI_ISLANDING` limit, `LOCAL_VOLTAGE` (kV), `NYISO_LOAD_ZONE`, `SUBSTATION_PROTECTION` backfeed
protection flag, the per-constraint PV values (`PV_SECTION`, `PV_BANK_RATING`, `PV_FEEDER_RATING`,
`PV_FLICKER`, `PV_OVER_VOLTAGE`, `PV_VOLTAGE_DEVIATION`, `PV_REGULATOR_DEVIATION`, `PV_THERMAL`,
`PV_ANTI_ISLAND`), plus `HC_REFESH_DATE` and `DER_REFESH_DATE` refresh timestamps (epoch
milliseconds — note Con Edison's own spelling of those two field names) and `NOTES`.

Geometry is `esriGeometryPolyline` in WGS84 (`wkid: 4326`), so the output drops straight into any
mapping stack; ask for `f=geojson` if you want GeoJSON.

## Interpreting it honestly

- The binding constraint on a segment is the **minimum** of the PV constraint values, not the headline
  `LOCAL_MAX`. Anti-islanding often binds well below thermal.
- Refresh dates matter: hosting capacity and connected-DER figures are refreshed on different cycles,
  so `CONNECTED_DER` can be newer than the hosting capacity analysis it is compared against.
- Con Edison's caveat is explicit: the data "is being provided for informational purposes only and is
  not intended to be a substitute for the established interconnection application process."
- This is distribution-grid data, not wholesale market data. Prices, load and fuel mix for New York
  come from NYISO; national data from EIA.
- No rate limit is published; Esri ArcGIS Online service quotas apply upstream. Be polite, cache, and
  prefer a single filtered query over crawling every feature.

Contact for the grid-data side: `DGExpert@conEd.com`. Map application:
`https://experience.arcgis.com/experience/d9d758c7736b44909dc3781937ca2ed5`.
