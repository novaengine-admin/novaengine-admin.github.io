---
title: Australia Extension
parent: Geo Regions
nav_order: 1
permalink: /geo-regions/australia-extension/
---

# Geo Regions -- Australia Extension

This extension provides maps related to Australia. As of today, following map types are available.

**Packages you install:**

1. **Geo Regions** -- the map engine is a prerequisite.
2. **Geo Regions - Australia** -- Australian map boundaries.

---

## Getting Started

### 1. Install the Packages

Install **Geo Regions** first, then the country package. Click the installation link for the version you need -- it will open the Salesforce installer.

**Geo Regions** (core engine -- install first):

| Version | Release | Install Link |
|---------|---------|--------------|
| 0.1.0 | 2026 October | [Install in Production](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilBhpYAE) / [Install in Sandbox](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilBhpYAE) |

**Geo Regions - Australia** (Australian map boundaries):

| Version | Release | Install Link |
|---------|---------|--------------|
| 0.1.0 | 2026 October | [Install in Production](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilj0LYAQ) / [Install in Sandbox](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilj0LYAQ) |

### 2. Assign the Permission Set

Assign the **Geo Regions User** permission set to any users who need access to the map.

1. Go to **Setup > Permission Sets**.
2. Find **Geo Regions User**.
3. Click **Manage Assignments > Add Assignment** and select the relevant users.

---

## Available Maps

| Map | Key | Description | Region Count |
|-----|-----|-------------|--------------|
| Victoria LGAs | `AU_VIC_LGA` | Local Government Areas in Victoria | 82 |

More Australian maps will be added in future releases. Each new map becomes available automatically after a package upgrade -- no configuration needed.

---

## Region Name Reference

Region names in your data must match exactly (case-sensitive).

### Victoria LGAs (`AU_VIC_LGA`)

| | | | |
|---|---|---|---|
| Alpine | Ararat | Ballarat | Banyule |
| Bass Coast | Baw Baw | Bayside | Benalla |
| Boroondara | Brimbank | Buloke | Campaspe |
| Cardinia | Casey | Central Goldfields | Colac Otway |
| Corangamite | Darebin | East Gippsland | Frankston |
| Glen Eira | Glenelg | Golden Plains | Greater Bendigo |
| Greater Dandenong | Greater Geelong | Greater Shepparton | Hepburn |
| Hindmarsh | Hobsons Bay | Horsham | Hume |
| Indigo | Kingston | Knox | Latrobe |
| Macedon Ranges | Manningham | Maribyrnong | Maroondah |
| Melbourne | Melton | Merri-bek | Mildura |
| Mitchell | Moira | Monash | Moonee Valley |
| Moorabool | Mornington Peninsula | Mount Alexander | Moyne |
| Murrindindi | Nillumbik | Northern Grampians | Port Phillip |
| Pyrenees | Queenscliffe | South Gippsland | Southern Grampians |
| Stonnington | Strathbogie | Surf Coast | Swan Hill |
| Towong | Wangaratta | Warrnambool | Wellington |
| West Wimmera | Whitehorse | Whittlesea | Wodonga |
| Wyndham | Yarra | Yarra Ranges | Yarriambiack |

---

## Support

For questions or issues, contact your Salesforce administrator or the package provider.
