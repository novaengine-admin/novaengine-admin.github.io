---
title: Developer Guide
parent: Geo Regions
nav_order: 1
permalink: /geo-regions/developer-guide/
description: Component API, configuration and examples for the Geo Map LWC.
---

# Developer Guide
{: .no_toc }

To display the map and shade it with your Salesforce data, build a wrapper Lightning Web Component (LWC) that queries your data and passes it to the Geo Map component.

1. TOC
{:toc}

---

## Component API

```html
<novaengine-geo-map
  map-type="AU_VIC_LGA"
  data={records}
  config={mapConfig}
  height="600px"
  onregionselect={handleRegionSelect}
>
</novaengine-geo-map>
```

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `map-type` | String | Yes | The map key, for example `AU_VIC_LGA`. Valid keys are listed under [Available maps]({{ site.baseurl }}/geo-regions/#available-maps). |
| `data` | Array | No | Array of objects containing your Salesforce data. |
| `config` | Object | No | Configures how data maps to regions, colours and tooltips. |
| `height` | String | No | CSS height, for example `600px` or `80vh`. Defaults to `520px`. An invalid value falls back to the default. |
| `onregionselect` | Function | No | Called when a user clicks a region. |

## Preparing your data

Your data is an array of objects. One field must hold region names that match the map's region names **exactly** (case-sensitive). Region names for each Australian map are on that map's page under the [Australia extension]({{ site.baseurl }}/geo-regions/australia-extension/).

```javascript
// Example: Accounts aggregated by LGA
[
  { lgaName: "Melbourne",       accountCount: 150, totalRevenue: 75000 },
  { lgaName: "Greater Geelong", accountCount: 85,  totalRevenue: 42000 },
  { lgaName: "Ballarat",        accountCount: 60,  totalRevenue: 31000 }
]
```

If two records use the same region name, the last one wins.
{: .note }

## Configuring the map

The `config` object tells the map how to use your data.

```javascript
{
  // REQUIRED to match data to regions: the field holding the region name
  nameField: "lgaName",

  // OPTIONAL: omit to show every region in neutral grey
  color: {
    field: "totalRevenue",   // numeric field to shade by
    color: "#1565C0",        // base colour in hex (default: blue)
    shades: 5,               // number of colour buckets (default: 5)
    label: "Revenue"         // legend title
  },

  // OPTIONAL: omit to show only the region name on hover
  tooltip: {
    fields: [
      { label: "Accounts", key: "accountCount", format: "number" },
      { label: "Revenue",  key: "totalRevenue", format: "currency" }
    ]
  }
}
```

### Colour options

The `color` object contains a property also called `color`, which is the base hex colour.
{: .note }

| Property | Required | Default | Description |
|----------|----------|---------|-------------|
| `field` | Yes | — | Numeric field in your data to shade by. |
| `color` | No | `#1565C0` (blue) | Base hex colour in `#RRGGBB` form, for example `#E53935` (red) or `#43A047` (green). An invalid value falls back to the default. |
| `shades` | No | `5` | Number of legend buckets (minimum 1). |
| `label` | No | Field name | Legend title. |

How shading works:

- Regions are split into equal-width value buckets, light to dark; darker means higher.
- A missing or non-numeric value counts as `0`.
- If every value is the same, there is a single bucket in the darkest shade.
- Regions with no matching record stay grey (`#B0BEC5`).

### Tooltip formats

| Format | Display |
|--------|---------|
| `"currency"` | `$1,234` (the `$` symbol is fixed) |
| `"number"` | `1,234` |

Tooltip values are always formatted as numbers, and a missing value shows as `0`. Don't use tooltip fields for text values.
{: .warning }

## Handling region clicks

Clicking a region zooms to it and fires a `regionselect` event that bubbles up through the component hierarchy.

| `event.detail` field | Type | Description |
|----------------------|------|-------------|
| `name` | String | The clicked region's name, for example `"Melbourne"`. |
| `color` | String | The hex colour of the region. Grey (`#B0BEC5`) when the region has no data or no shading is configured. |
| `data` | Object | The matching record from your `data` array, or `undefined` if there is none. |

```javascript
handleRegionSelect(event) {
  const { name, color, data } = event.detail;

  console.log(name);   // "Melbourne"
  console.log(color);  // "#1565C0"
  console.log(data);   // { lgaName: "Melbourne", accountCount: 150, totalRevenue: 75000 }
}
```

## Complete example

This example assumes a custom text field `LGA__c` on Account that holds an LGA name exactly as it appears in the map.

**Apex controller**

```java
public with sharing class AccountMapController {
  @AuraEnabled(cacheable=true)
  public static List<Map<String, Object>> getAccountsByLGA() {
    List<AggregateResult> results = [
      SELECT LGA__c, COUNT(Id) cnt, SUM(AnnualRevenue) rev
      FROM Account
      WHERE LGA__c != null
      GROUP BY LGA__c
    ];

    List<Map<String, Object>> output = new List<Map<String, Object>>();
    for (AggregateResult ar : results) {
      output.add(new Map<String, Object>{
        'lgaName'      => (String) ar.get('LGA__c'),
        'accountCount' => (Integer) ar.get('cnt'),
        'totalRevenue' => (Decimal) ar.get('rev')
      });
    }
    return output;
  }
}
```

**LWC JavaScript**

```javascript
import { LightningElement, wire } from "lwc";
import getAccountsByLGA from "@salesforce/apex/AccountMapController.getAccountsByLGA";

export default class AccountMap extends LightningElement {
  records = [];

  mapConfig = {
    nameField: "lgaName",
    color: {
      field: "totalRevenue",
      color: "#1565C0",
      shades: 5,
      label: "Total Revenue"
    },
    tooltip: {
      fields: [
        { label: "Accounts", key: "accountCount", format: "number" },
        { label: "Revenue",  key: "totalRevenue", format: "currency" }
      ]
    }
  };

  @wire(getAccountsByLGA)
  wiredAccounts({ data, error }) {
    if (data) {
      this.records = data;
    } else if (error) {
      console.error(error);
    }
  }

  handleRegionSelect(event) {
    const { name } = event.detail;
    // Navigate to a filtered view, show a detail panel, etc.
  }
}
```

**LWC template**

```html
<template>
  <novaengine-geo-map
    map-type="AU_VIC_LGA"
    data={records}
    config={mapConfig}
    height="600px"
    onregionselect={handleRegionSelect}
  >
  </novaengine-geo-map>
</template>
```
