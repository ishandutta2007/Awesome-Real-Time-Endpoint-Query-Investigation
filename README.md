# Awesome-Real-Time-Endpoint-Query-Investigation

# Top Real-Time Endpoint Query & Investigation Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Live Fleet Querying, SQL-Based Investigation & Self-Hosted Endpoint Visibility*  
**Last updated: November 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Real-Time Endpoint Query & Investigation**. These tools enable security teams to ask questions of their entire endpoint fleet—running SQL-like or scripted queries on demand to hunt threats, verify compliance, and investigate incidents—without waiting for scheduled scans or deploying new agents.

**Examples** include CrowdStrike Falcon Live Query, Tanium Interact, osquery, Kolide, Fleet Device Management, Microsoft Defender Advanced Hunting, SentinelOne Deep Visibility, Carbon Black Live Query, Sophos Live Discover, and Uptycs (the category leaders).

**Open-source emphasis**: Real-Time Endpoint Query & Investigation is a domain where open-source tooling is **anchored by a single foundational agent but mature at the fleet-management layer**. **osquery** is the definitive open-source endpoint instrumentation framework—it exposes OS state as SQL tables, letting you query processes, open sockets, installed software, and system configuration across Windows, macOS, and Linux . **Fleet Device Management** provides the open-source fleet server for centralized osquery management with TLS enrollment and query scheduling . **Kolide** (now part of 1Password) offers a cloud-based osquery manager with Slack integration, historically priced around **$6–7.20/device/month** . **Uptycs** is a commercial platform built on a forked osquery agent, adding dashboards, threat intelligence, and compliance reporting . The critical caveat: **osquery is explicitly periodic/on-demand, not real-time**—its own documentation warns "do not use for real-time alerting; use EDR for real-time" . Commercial platforms like CrowdStrike Falcon Live Query and SentinelOne Deep Visibility leverage existing EDR agents for continuous telemetry and near-real-time search, which open-source osquery cannot replicate without a separate detection layer .

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-products)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **[CrowdStrike Falcon Live Query](https://www.crowdstrike.com/)**  
  On-demand SQL queries against online hosts via the Falcon agent; returns process, kernel module, network connection, browser plugin, and file hash data in table format. **Note**: offline endpoints cannot be queried (no caching) . Supports scheduled queries and AI-assisted query generation (Charlotte AI, credit-consuming) . **Best for CrowdStrike customers** wanting fleet-wide investigation without new agents—but offline hosts require separate handling.

- **[Tanium Interact](https://www.tanium.com/)**  
  Real-time endpoint visibility and interrogation without pre-ingestion; Tanium's advantage is **finding what you're hunting for faster across any fleet size**, especially if the agent is already deployed for IT management . **Not a detection platform**—Tanium wins on fleet visibility and integrated response, not automated detection coverage . **Best for large enterprises** wanting real-time interrogation at scale, often alongside a separate EDR for detection.

- **[Kolide](https://www.kolide.com/)**  
  Cloud-based osquery management with **Slack integration** for end-user device trust checks; historically priced around **$6–7.20/device/month**, now integrated into 1Password Extended Access Management . **Best for organizations wanting osquery without running infrastructure**—but requires Slack and is device-trust-focused rather than deep threat hunting.

- **[Microsoft Defender Advanced Hunting](https://learn.microsoft.com/)**  
  Query-based hunting within Microsoft Defender using KQL (Kusto Query Language) against 30 days of raw endpoint telemetry. **Note**: advanced hunting capabilities vary by license tier—**Plan 1 (E3) lacks automated investigation and response (AIR)**, requiring manual SOC cleanup . **Best for Microsoft Defender customers** wanting integrated hunting with existing telemetry—but verify license tier capabilities before relying on automated response.

- **[SentinelOne Deep Visibility](https://www.sentinelone.com/)**  
  Patented kernel-based monitoring providing **near real-time search across endpoints** for IOCs, with visibility into encrypted traffic without a proxy . Supports **full real-time and historic retrospective search, even for offline endpoints** . Single-agent architecture integrates visibility with mitigation and remediation . **Best for SentinelOne customers** wanting hunting integrated with EDR response—the offline endpoint search is a differentiator.

- **[Carbon Black Live Query](https://www.broadcom.com/)**  
  Query Builder with 10 pre-selected osquery tables (autosexec, chrome_extensions, file, logged_in_users, patches, process_open_sockets, processes, programs, registry, scheduled_tasks) plus full SQL Query Builder against all osquery schema tables . **Limitations**: LiveOps uses osquery under the hood and inherits its limitations—no daemon mode (evented tables), Windows filenames over U+00FF fail silently, no recursive path wildcards, 50MB file hash limit . **Best for Carbon Black customers** wanting osquery-based querying within the platform.

- **[Sophos Live Discover](https://www.sophos.com/)**  
  osquery-based endpoint querying within Sophos endpoint protection; enables threat hunting and compliance checks across managed endpoints.

- **[Uptycs](https://www.uptycs.com/)**  
  Unified CNAPP and XDR platform built on a **forked osquery agent**; adds dashboards, reports, alerts, and threat intelligence on top of osquery data collection . **Caveats**: documentation is largely inherited from open-source osquery and "never really fleshed out well"; the fork doesn't merge back with upstream; support quality is inconsistent per reviewers . **Best for organizations wanting managed osquery with security use cases** (intrusion detection, vulnerability management, compliance) —but verify documentation and support before committing.

## Open-Source GitHub Projects

### Endpoint Instrumentation Agent (osquery)

- **[osquery](https://github.com/osquery/osquery)**  
  The **foundational open-source endpoint agent** (Apache 2.0 / GPL-2.0 dual license) that exposes OS state as SQL tables for querying . Runs on Windows, macOS, Linux, and FreeBSD; queries cover processes, open sockets, installed packages, users, file attributes, and more . **Critical caveat**: osquery is **periodic/on-demand, not real-time**—its documentation explicitly warns "do not use for real-time alerting (osquery is periodic/on-demand; use EDR for real-time)" . Supports scheduled queries, differential logging (only changes), and packs for IR/compliance use cases . **Best for threat hunting and compliance visibility**—but requires a fleet management server and a separate detection layer for real-time alerting.

### Fleet Management Servers (osquery Orchestration)

- **[Fleet Device Management](https://github.com/fleetdm/fleet)**  
  The **leading open-source osquery fleet manager** (MIT for free tier, built on Go); provides TLS enrollment, centralized configuration, scheduled queries, and a web UI for managing osquery agents across fleets . **Best for organizations wanting to operationalize osquery** without a commercial platform—but it is a management layer, not a detection engine.

- **[Kolide Fleet (legacy)](https://github.com/kolide/fleet)**  
  The predecessor to FleetDM; **no longer actively maintained**—migrate to Fleet Device Management for ongoing development.

### Additional Strong Open-Source Options

- **osquery-go** — Go client library for building osquery extensions; useful for custom integrations but not a standalone investigation platform .
- **sqhunter** — Threat hunter based on osquery and SaltStack for ad-hoc/distributed queries without osquery's TLS plugin; queries open network sockets against threat intelligence sources . **Niche**—for SaltStack environments, but demonstrates the pattern of layering threat intel on osquery data.
- **Velociraptor** — While primarily a DFIR platform, Velociraptor's **VQL (Velociraptor Query Language)** provides similar endpoint querying capabilities with live response actions . **Not an osquery replacement**—different query language and architecture—but relevant for teams wanting hunting plus remediation in one open-source tool.

**Frameworks for building custom systems**: For **osquery-based visibility**, osquery + Fleet Device Management provides the mature open-source stack . For **managed osquery without infrastructure**, Kolide (Slack-based) or Uptycs (commercial, with caveats) . For **real-time hunting integrated with EDR**, commercial platforms (CrowdStrike Live Query, SentinelOne Deep Visibility) are required—open-source osquery cannot provide continuous real-time telemetry . No open-source project matches the **integrated EDR telemetry + real-time query** of commercial platforms—the detection layer must come from elsewhere.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Endpoint query platforms handling system state and telemetry must comply with organizational security policies and privacy regulations; **queries can expose sensitive system information**—access should be restricted to authorized investigators.
- **Open-source endpoint querying is mature for on-demand investigation but fundamentally limited for real-time detection**—osquery is explicitly **periodic/on-demand, not real-time** . Commercial platforms like CrowdStrike Falcon Live Query and SentinelOne Deep Visibility achieve near-real-time search because they leverage existing EDR agents with continuous telemetry . **Uptycs is built on a forked osquery agent** with known documentation and support gaps . Organizations requiring real-time alerting should pair osquery with an EDR or SIEM, not rely on osquery alone.
- Verify license terms for commercial use; osquery uses Apache 2.0 / GPL-2.0 dual license, and Fleet Device Management has a free tier (check repository for current licensing).

---

**Made for threat hunters, SOC analysts, and security engineers building endpoint investigation capabilities.**  
Let's make endpoint query and investigation more open, but with clear eyes about the on-demand vs. real-time distinction that defines what osquery can and cannot do.
