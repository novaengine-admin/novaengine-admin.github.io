---
title: Geo Regions
nav_order: 1
has_children: true
permalink: /geo-regions/
description: Interactive choropleth maps for Salesforce Lightning pages.
---

# Geo Regions — Overview
{: .no_toc }

1. TOC
{:toc}

---

## What is Geo Regions?

Geo Regions is a managed Salesforce package that adds interactive choropleth maps to your Lightning pages. It shades geographic regions by your Salesforce data, so you can visualise metrics such as Account counts or Revenue across states, local government areas and more.

The core package is the map engine and ships no geography. Map boundaries come from country packages, so you install only the ones you need:

| Package | Purpose |
|---------|---------|
| **Geo Regions** | The map engine. Install it first. |
| [**Geo Regions - Australia**]({{ site.baseurl }}/geo-regions/australia-extension/) | Local Government Area maps for every Australian state and territory. |

---

## Getting started

### 1. Install the packages

Install **Geo Regions** first, then the country packages you need. Each link opens the Salesforce installer.

| Version | Release | Production | Sandbox |
|---------|---------|------------|---------|
| 0.2.0 | 2026 October | [Install](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000jCXBBYA4) | [Install](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000jCXBBYA4) |

For country package install links, see the [Australia extension]({{ site.baseurl }}/geo-regions/australia-extension/).

### 2. Assign the permission set

Assign the **Geo Regions User** permission set to everyone who needs access to the map.

1. Go to **Setup > Permission Sets**.
2. Find **Geo Regions User**.
3. Click **Manage Assignments > Add Assignment** and select the users.

---

## Available maps

Maps are delivered by country packages. Each map has a key, which is the value you pass as `map-type` to the Geo Map component in your wrapper LWC.

| Package | Maps |
|---------|------|
| [Geo Regions - Australia]({{ site.baseurl }}/geo-regions/australia-extension/) | Local Government Areas for ACT, NSW, NT, QLD, SA, TAS, VIC and WA |

---

## Show a map on a page

The Geo Map component is used from your own code. A developer builds a wrapper Lightning Web Component (LWC) that queries your Salesforce data, passes it to Geo Map with the `map-type` key of the map to display, and places the wrapper on a Lightning page. See the [Developer Guide]({{ site.baseurl }}/geo-regions/developer-guide/).

## Using the map

- **Hover** over a region to see its tooltip.
- **Click** a region to zoom to it.
- **Click a legend item** to dim every region outside that value range. Click it again to clear the filter. Legend items also work with the keyboard (Tab, then Enter or Space).
- Regions with no matching record in your data stay grey, so missing data is never mistaken for a low value.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "Invalid map type" panel | The `map-type` doesn't match an installed map. The panel lists the installed keys; check the country package is installed and the key is spelled correctly. |
| All regions are grey | `config.color` is missing, or `color.field` doesn't match a field in your data. |
| Some regions are grey | Those regions have no matching record. Check the names match the country package's region names exactly (case-sensitive). For Australia, see the map pages under the [Australia extension]({{ site.baseurl }}/geo-regions/australia-extension/). |
| Legend is not showing | Provide `config.color` with a `field`. |
| Map shows a spinner or doesn't load | Refresh the page to retry. If it persists, contact your administrator. |

---

## Support

For questions or issues, email [contact@novaengine.com.au](mailto:contact@novaengine.com.au).
