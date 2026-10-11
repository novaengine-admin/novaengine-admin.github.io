---
title: Geo Regions
nav_order: 1
has_children: true
permalink: /geo-regions/
---

# Geo Regions -- Subscriber Guide

## What is Geo Regions?

Geo Regions is a managed Salesforce package that adds interactive choropleth maps to your Lightning pages. It shades geographic regions by your Salesforce data -- letting you visualise metrics like Account counts, Revenue, or any numeric value across states, local government areas, and more.

**Package you install:**

* **Geo Regions** -- the map engine.

---

## Getting Started

### 1. Install the Packages

Select the release version you want to install. Install Geo Regions first, followed by the required country extension.

<label for="geo-release-version">Release version</label>

<select id="geo-release-version"> <option value="0.1.0" selected>0.1.0 — October 2026</option> <option value="future">Next release (example)</option> </select>

<div id="geo-release-details"> <h4>Geo Regions 0.1.0</h4> <p><strong>Core engine — install first</strong></p> <p> <a href="https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilBhpYAE">Install in Production</a> | <a href="https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilBhpYAE">Install in Sandbox</a> </p> </div>

<script> document.addEventListener("DOMContentLoaded", function () { const selector = document.getElementById("geo-release-version"); const details = document.getElementById("geo-release-details");

const releases = { "0.1.0": <h4>Geo Regions 0.1.0</h4> <p><strong>Core engine — install first</strong></p> <p> <a href="https://login.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilBhpYAE">Install in Production</a> | <a href="https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ilBhpYAE">Install in Sandbox</a> </p> , "future": <h4>Next release</h4> <p>Installation links will be published when this release is available.</p> };

selector.addEventListener("change", function () { details.innerHTML = releases[selector.value] || ""; }); }); </script>

### 2. Assign the Permission Set

Assign the **Geo Regions User** permission set to any users who need access to the map.

1. Go to **Setup > Permission Sets**.
2. Find **Geo Regions User**.
3. Click **Manage Assignments > Add Assignment** and select the relevant users.

---

## Available Maps

Please install available extensions to add more map types.

---

## Developer Guide

To display the map and shade it with your Salesforce data, a developer builds a wrapper Lightning Web Component (LWC) that queries data and passes it to the Geo Map component.

### Component API

```html
<novaengine-geo-map
  map-type="AU_VIC_LGA"
  data={records}
  config={mapConfig}
  height="600px"
  onregionselect={handleRegionSelect}>
</novaengine-geo-map>
```

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `map-type` | String | Yes | The map key (e.g., `AU_VIC_LGA`). See [Available Maps](#available-maps) for valid keys. |
| `data` | Array | No | Array of objects containing your Salesforce data. |
| `config` | Object | No | Configures how data maps to regions, colours, and tooltips. |
| `height` | String | No | CSS height (e.g., `600px`, `80vh`). Defaults to `520px`. |
| `onregionselect` | Function | No | Event handler called when a user clicks a region. |

### Preparing Your Data

Your data should be an array of objects. One field must contain region names that match the map's regions (see [Region Name Reference](#region-name-reference)).

```javascript
// Example: Accounts aggregated by LGA
[
  { lgaName: "Melbourne",  accountCount: 150, totalRevenue: 75000 },
  { lgaName: "Geelong",    accountCount: 85,  totalRevenue: 42000 },
  { lgaName: "Ballarat",   accountCount: 60,  totalRevenue: 31000 }
]
```

### Configuring the Map

The `config` object tells the map how to use your data:

```javascript
{
  // REQUIRED -- which field in your data holds the region name
  nameField: "lgaName",

  // OPTIONAL -- omit to show all regions in neutral grey (no shading)
  color: {
    field: "totalRevenue",    // which numeric field to shade by
    color: "#1565C0",         // base colour in hex (default: blue)
    shades: 5,                // number of colour buckets (default: 5)
    label: "Revenue"          // legend title
  },

  // OPTIONAL -- omit to show only the region name on hover
  tooltip: {
    fields: [
      { label: "Accounts", key: "accountCount", format: "number" },
      { label: "Revenue",  key: "totalRevenue", format: "currency" }
    ]
  }
}
```

**Colour options:**

| Property | Required | Default | Description |
|----------|----------|---------|-------------|
| `field` | Yes | -- | Numeric field in your data to shade by |
| `color` | No | `#1565C0` (blue) | Base hex colour. Examples: `#E53935` (red), `#43A047` (green) |
| `shades` | No | `5` | Number of legend buckets |
| `label` | No | Field name | Text shown as the legend title |

**Tooltip format values:**

| Format | Display |
|--------|---------|
| `"currency"` | `$1,234` |
| `"number"` | `1,234` |
| `"text"` | As-is |

### Handling Region Clicks

When a user clicks a region, the component fires a `regionselect` custom event. Listen for it with `onregionselect`.

**`event.detail` shape:**

| Field | Type | Description |
|-------|------|-------------|
| `name` | String | The name of the clicked region (e.g., `"Melbourne"`) |
| `color` | String | The hex colour assigned to the region (e.g., `"#1565C0"`), or `null` if no shading is configured |
| `data` | Object | The matching record from your `data` array, or `undefined` if no data was provided for this region |

```javascript
handleRegionSelect(event) {
  const { name, color, data } = event.detail;

  console.log(name);   // "Melbourne"
  console.log(color);  // "#1565C0"
  console.log(data);   // { lgaName: "Melbourne", accountCount: 150, totalRevenue: 75000 }
}
```

### Complete Example

**Apex Controller:**

```java
public with sharing class AccountMapController {
  @AuraEnabled(cacheable=true)
  public static List<Map<String, Object>> getAccountsByLGA() {
    List<AggregateResult> results = [
      SELECT BillingState, COUNT(Id) cnt, SUM(AnnualRevenue) rev
      FROM Account
      WHERE BillingState != null
      GROUP BY BillingState
    ];

    List<Map<String, Object>> output = new List<Map<String, Object>>();
    for (AggregateResult ar : results) {
      output.add(new Map<String, Object>{
        'lgaName'      => (String) ar.get('BillingState'),
        'accountCount' => (Integer) ar.get('cnt'),
        'totalRevenue' => (Decimal) ar.get('rev')
      });
    }
    return output;
  }
}
```

**LWC JavaScript:**

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
    }
  }

  handleRegionSelect(event) {
    const { name } = event.detail;
    // Navigate to a filtered view, show a detail panel, etc.
  }
}
```

**LWC Template:**

```html
<template>
  <novaengine-geo-map
    map-type="AU_VIC_LGA"
    data={records}
    config={mapConfig}
    height="600px"
    onregionselect={handleRegionSelect}>
  </novaengine-geo-map>
</template>
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "Invalid map type" message | Check that the country package is installed and `map-type` matches a valid key from [Available Maps](#available-maps). |
| All regions are grey | `config.color` is missing or `color.field` does not match a field in your data. |
| Some regions are grey | Those regions have no matching record in your data. Check names match the [Region Name Reference](#region-name-reference) exactly (case-sensitive). |
| Map shows a spinner indefinitely | The map boundary file failed to load. Refresh the page to retry. |
| Legend is not showing | Provide both `config.color` and `config.color.field`. |

---

## Support

For questions or issues, contact your Salesforce administrator or the package provider.
