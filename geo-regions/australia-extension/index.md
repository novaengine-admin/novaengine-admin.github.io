---
title: Australia Extension
parent: Geo Regions
has_children: true
nav_order: 2
permalink: /geo-regions/australia-extension/
description: Local Government Area maps for every Australian state and territory.
---

# Geo Regions — Australia Extension
{: .no_toc }

This extension adds Local Government Area (LGA) maps for Australia to Geo Regions.

1. TOC
{:toc}

---

## Packages

| Package | Purpose |
|---------|---------|
| **Geo Regions** | The map engine. Install it first. |
| **Geo Regions - Australia** | Australian map boundaries. Requires Geo Regions 0.1.0 or later. |

## Install the packages

Install **Geo Regions** first, then **Geo Regions - Australia**. Each link opens the Salesforce installer.

**Geo Regions** (core engine — install first):

| Version | Release | Production | Sandbox |
|---------|---------|------------|---------|
| 0.2.0 | 2026 October | [Install](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000jCXBBYA4) | [Install](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000jCXBBYA4) |
| 0.1.0 | 2026 October | [Install](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilBhpYAE) | [Install](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilBhpYAE) |

**Geo Regions - Australia** (Australian map boundaries):

| Version | Release | Production | Sandbox |
|---------|---------|------------|---------|
| 0.2.0 | 2026 October | [Install](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000jCXG1YAO) | [Install](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000jCXG1YAO) |
| 0.1.0 | 2026 October | [Install](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilj0LYAQ) | [Install](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilj0LYAQ) |

Version 0.1.0 includes the Victoria map only. Version 0.2.0 adds all states and territories.
{: .note }

After installing, assign the **Geo Regions User** permission set. See [Assign the permission set]({{ site.baseurl }}/geo-regions/#2-assign-the-permission-set).

---

## Available maps

| Map | Key | Regions |
|-----|-----|---------|
| Australian Capital Territory | `AU_ACT_LGA` | 3 |
| New South Wales | `AU_NSW_LGA` | 131 |
| Northern Territory | `AU_NT_LGA` | 22 |
| Other Territories | `AU_OT_LGA` | 4 |
| Queensland | `AU_QLD_LGA` | 80 |
| South Australia | `AU_SA_LGA` | 73 |
| Tasmania | `AU_TAS_LGA` | 31 |
| Victoria | `AU_VIC_LGA` | 82 |
| Western Australia | `AU_WA_LGA` | 140 |

Region counts include a few non-geographic entries such as `No usual address`.

Use the key as the `map-type` value, or pick the map from the **Region** dropdown in Lightning App Builder. New maps become available automatically after a package upgrade — no configuration needed.

The names your data must use are listed on the [Region Names]({{ site.baseurl }}/geo-regions/australia-extension/region-names/) page.
