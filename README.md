# curiosity-research-market-enterprise

<!-- 561-group-standards:badges:start -->
[![Coesite Operations](https://img.shields.io/badge/561%20Group-Coesite%20Operations-5d6d7e)](https://github.com/coesite-operations)
[![License: Apache 2.0](https://img.shields.io/badge/license-Apache%202.0-5d6d7e)](LICENSE)
[![Indigenous data: CARE](https://img.shields.io/badge/Indigenous%20data-CARE-4b6f44)](https://github.com/coesite-operations/.github/blob/main/GOVERNANCE.md)
[![Tips welcome](https://img.shields.io/badge/tips-welcome-d4a017)](https://github.com/561-group/.github/blob/main/TIPS.md)
<!-- 561-group-standards:badges:end -->

Provider-neutral market for buyer-authored research assignments motivated by non-evidentiary curiosity candidates.

This repository holds operation data only. The code that reads it (assignment admission, offer clearing, award settlement, procurement, award delivery, and the signed A2A faces) is in Red Cup Engineering at [`red-cup-engineering/curiosity-research-market-service`](https://github.com/red-cup-engineering/curiosity-research-market-service), under the Leader's rule `science-engineering-operations-parameterize-in-order`.

## What is here

| path | what |
|---|---|
| `content/config/` | provider bindings (analytics curiosity, research assignment, research supply offer), agent activation (assignment admission, award delivery, procurement) and market activation |
| `content/agent-cards/` | signed Agent Cards and their public JWKS |
| `content/deployment/systemd-user/` | user-service units that run the three A2A faces |

Signing keys and credentials (`data/agent-card-signing/*.pem`, `data/credentials/*.pem`, `data/a2a/*.pem`) are provisioned at deploy time and never committed.

## Running the engine against this data

The engine takes the data location as a parameter: `CURIOSITY_RESEARCH_MARKET_ROOT` (default: the working directory).

```sh
git clone https://github.com/red-cup-engineering/curiosity-research-market-service ~/services/curiosity-research-market-service
cd <this repository>
node ~/services/curiosity-research-market-service/src/serve-procurement-a2a.mjs
```

The three existing systemd user units in `content/deployment/systemd-user/` now
reference the engine at the documented **expected checkout** `/home/emsenn/services/curiosity-research-market-service`.
They use `CURIOSITY_RESEARCH_MARKET_ROOT` to locate the original instance
`content/` and `data/` under their unchanged `WorkingDirectory`, rather than
the unused legacy `CURIOSITY_RESEARCH_MARKET_DATA_ROOT` setting. The
source configuration was corrected on 27 October 2026; **no on-host checkout
placement, installed npm dependency, systemd daemon reload, service start,
live Agent Card, or end-to-end settlement has been verified**.

Before the units can be installed, an Operations receiver must independently
verify that the engine's documented checkout actually exists at that path,
the original `WorkingDirectory` contains the intended instance configuration,
the required runtime-only signing credentials are present with appropriate
permissions, and the original contract and payment asset are admitted. The
existing `content/config/activation.json` still sets
`liveProcurement: refused` and `paymentAsset: null`; source repairs
do not activate a paid research marketplace. The units' existing security
sandbox, ports, data write paths, and settlement identity are unchanged.

Received by the engine 2026-10-23 from this repository at `f5d4c261ff904fd4b1cdb5791e0db34d6f566565`.
