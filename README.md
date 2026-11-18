# NetSBOM-Guard 🛡️

**Agentless Software Bill of Materials (SBOM) & Supply Chain Integrity Scanner for Network and Server Infrastructure**

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Status](https://img.shields.io/badge/Status-Proposal_/_Pre--Alpha-orange.svg)]()

## Overview

**NetSBOM-Guard** is an open-source, agentless CLI and self-hosted web-dashboard utility designed to bring software supply chain transparency, cryptographic integrity verification, and vulnerability auditing to deployed network infrastructure and Linux/BSD-based servers.

While modern CI/CD pipelines increasingly generate SBOMs for application code, enterprise network devices (routers, firewalls, access points, OpenWrt/embedded Linux appliances) and production servers often remain blind spots containing unverified packages, outdated open-source libraries, and untracked licenses.

## Key Features (Planned Architecture)

* **Agentless Discovery:** Secure, read-only inventory collection over **SSH** (key-based authentication) and **SNMPv3** across defined subnets/VLANs without installing agents on target devices.
* **Multi-OS Package Parsing:** Extracts installed packages (`dpkg`, `rpm`, `apk`, `opkg`), running daemon binaries, and kernel modules.
* **Cryptographic Supply Chain Verification:** Computes SHA-256 hashes of critical binaries and cross-verifies package signatures against official upstream repositories to detect unauthorized tampering.
* **Standardized Open Metadata (SBOM):** Exports machine-readable **CycloneDX (v1.5+ JSON/XML)** and **SPDX** reports including full SPDX license identifiers.
* **Vulnerability & Drift Auditing:** Integrates with the open **OSV (Open Source Vulnerabilities)** database (online & offline air-gapped cache) and provides historical SBOM diffing to alert engineers on unexpected package changes.
* **Lightweight Self-Hosted Web UI:** Single-binary / Docker Compose dashboard for visual topology and SBOM auditing.

## Architecture Overview

```text
+-------------------------------------------------------------------+
|                        NetSBOM-Guard Core                         |
|                                                                   |
|  [SSH / SNMPv3 Collector] ---> [Package & Binary Parsers]         |
|                                       |                           |
|          +----------------------------+-----------------------+   |
|          v                            v                       v   |
|  [Hash & Repo Verifier]      [CycloneDX/SPDX Engine]   [OSV Audit]|
|          |                            |                       |   |
|          +----------------------------+-----------------------+   |
|                                       |                           |
|                                       v                           |
|                        [CLI & Self-Hosted Web UI]                 |
+-------------------------------------------------------------------+
