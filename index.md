---
title: Home
nav_order: 0
permalink: /
description: Documentation for novaengine Salesforce managed packages.
---

# novaengine Documentation
{: .no_toc }

Guides for installing, configuring and extending novaengine's Salesforce managed packages.
{: .fs-6 .fw-300 }

[Get started with Geo Regions]({{ site.baseurl }}/geo-regions/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Developer Guide]({{ site.baseurl }}/geo-regions/developer-guide/){: .btn .fs-5 .mb-4 .mb-md-0 }

![Geo Regions choropleth map of Victorian local government areas]({{ site.baseurl }}/assets/images/maps/vic.png){: .home-hero-image }

---

## Products

<div class="product-card" markdown="1">

### Geo Regions

Interactive choropleth maps for Lightning pages. Shade geographic regions by your Salesforce data to visualise metrics such as Account counts or Revenue across states and local government areas.

- [Overview]({{ site.baseurl }}/geo-regions/): install, permission set and troubleshooting
- [Developer Guide]({{ site.baseurl }}/geo-regions/developer-guide/): component API, configuration, events and examples
- [Australia extension]({{ site.baseurl }}/geo-regions/australia-extension/): local government area maps for every state and territory

Latest release: **0.2.0** (October 2026). See the [install links]({{ site.baseurl }}/geo-regions/#1-install-the-packages).

</div>

---

## Where to start

1. **Install the packages.** Install Geo Regions, then the [Australia extension]({{ site.baseurl }}/geo-regions/australia-extension/) if you need Australian maps. See the [install links]({{ site.baseurl }}/geo-regions/#1-install-the-packages).
2. **Assign the permission set.** Give the **Geo Regions User** permission set to everyone who will see the map. See [Assign the permission set]({{ site.baseurl }}/geo-regions/#2-assign-the-permission-set).
3. **Build a wrapper component.** A developer wraps the Geo Map component in a custom LWC that queries your data and shades the map. Follow the [Developer Guide]({{ site.baseurl }}/geo-regions/developer-guide/).

Stuck? See [Troubleshooting]({{ site.baseurl }}/geo-regions/#troubleshooting).

---

## Support

Questions or issues? Email [contact@novaengine.com.au](mailto:contact@novaengine.com.au).
