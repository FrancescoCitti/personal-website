+++
title = "ThreatMap"
description = "Static cyber threat intelligence dashboard that visualizes OSINT threat feeds on an interactive world map, deployable to GitHub Pages with zero infrastructure."
type = ["projects", "post"]
date = "2026-05-29"
[author]
name = "Francesco Citti"
+++

<a href="https://threatmap.francescocitti.com" target="_blank" rel="noopener" style="display:inline-block;margin-bottom:1.5rem;padding:0.5rem 1.2rem;background:#1a1a1a;color:#fff;border:1px solid #444;border-radius:4px;text-decoration:none;font-size:0.9rem;">&#x2197; Live Demo</a>

## Overview

ThreatMap is a static, GitHub Pages-deployable cyber threat intelligence visualization dashboard. It plots real-time indicators from public OSINT threat feeds—botnet C2 servers, top attacking IPs, known exploited vulnerabilities—on an interactive world map. No backend, no database, no paid APIs required.

## The Problem

Existing geospatial intelligence platforms are complex to self-host: they require databases, authentication layers, Docker infrastructure, and often paid API keys for map tiles or geolocation. For a security researcher who wants to monitor public threat feeds at a glance, that overhead is a significant barrier.

## How It Works

GitHub Actions runs hourly cron jobs that fetch threat feeds, geolocate IPs against the ip-api.com free tier, and write the results as static GeoJSON files committed back to the repository. The SvelteKit frontend reads those files directly—no API calls at runtime. GitHub Pages serves the whole thing as static HTML.

```
Threat feeds → GitHub Actions (hourly) → GeoJSON files → SvelteKit frontend → GitHub Pages
```

## Data Sources

| Feed | What It Shows |
|---|---|
| Feodo Tracker | Active botnet C2 server IP addresses |
| SANS ISC DShield | Top attacking source IPs |
| CISA KEV Catalog | Known exploited vulnerabilities with statistics |

Each feed is toggleable as an independent layer on the map. Indicators are color-coded by source and cluster dynamically at lower zoom levels.

## Key Features

- **Zero infrastructure**: no server, no database, no Docker—deploys entirely to GitHub Pages
- **Layer controls**: per-feed toggles and clustering for large datasets
- **Sidebar detail panel**: click any indicator to see IP, country flag, ASN, source attribution
- **Stats dashboard**: floating pill showing total indicator count and per-feed breakdowns, including 30-day additions
- **Data freshness indicator**: timestamp of last automated fetch displayed prominently
- **MIT licensed**: no usage restrictions, fully auditable static assets

## Tech Stack

| Component | Detail |
|---|---|
| Frontend | SvelteKit with `@sveltejs/adapter-static` |
| Map | MapLibre GL + CartoDB Dark Matter tile layer |
| Data pipeline | Node.js ESM scripts in GitHub Actions (hourly cron) |
| Data format | Static GeoJSON + JSON in `/static/data/` |
| Deployment | GitHub Pages |

## GitHub Repository

[github.com/FrancescoCitti/threatmap](https://github.com/FrancescoCitti/threatmap)
