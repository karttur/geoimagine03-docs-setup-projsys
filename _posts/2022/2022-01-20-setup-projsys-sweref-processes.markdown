---
layout: post
title: "Projection system setup:</br>Part 2 setup_processes"
categories: setup
excerpt: "Setup boundaries, tiling, regions and user project for custom projection system (SWEREF99)"
tags:
  - projection
  - system
  - sweref
  - setup_processes
  - tiling
  - extent
  - user
  - tract
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-20 11:27'
modified: '2022-01-22 T18:17:25.000Z'
comments: true
share: true
figure1: machinelarning_histo_housing
figure3A: machinelarning_linregnaive
figure3B: machinelarning_linregmodel
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>
## Introduction

When you work with Karttur's GeoImagine Framework you must have a predefined geographic region linking to an also predefined projection system. In the [previous](../setup-projsys-sweref-db) post you updated the Framework postgres database to include the projection system "sweref" (the Swedish national grid). The definition, however, only included the name and EPSG code of the projection. Plus a database schema with empty tables where all the spatial data belonging to this system will be registered. In this post you will run three different Framework processes for completing the definition of the projection system, add a default region and a user project based on the new projection system:

- DefineCustomSystem [for defining the boundary and tiling of the new projection system]
- DefaultRegionFromCoords [for defining a default region that exactly matches the system boundary]
- ManageDefRegProj [for defining a user project and tract that matches the default region and system boundary]

The three processes are accessible from the Framework module <span class='module'>setup_processes</span>.

## Prerequisites

You must have updated the database to include the projection system as prescribed in the [previous](../setup-projsys-sweref-db) post.

## Process chain

The Framework processes that are required for preparing the new projection system are compiled in the command file <span class='file'>sweref_setup_20220122.txt</span>:

```
\#\# Define the SWEREF tile/project systems
[sweref_system-define_v090.json](https://karttur.github.io/geoimagine03-docs-setup-projsys/setup_processes/setup_processes-sweref_system-define/)

\#\# Define SWEREF default region
[sweref_defaultregion_v090.json](https://karttur.github.io/geoimagine03-docs-setup-projsys/setup_processes/setup_processes-sweref_defaultregion/)

\#\# Define SWEREF user project
[sweref_user+project_v090.json](https://karttur.github.io/geoimagine03-docs-setup-projsys/setup_processes/setup_processes-sweref_user+project/)
```

### DefineCustomSystem

The Framework process DefineCustomSystem defines the boundary and tiling of a projection system. For the Swedish national projection grid, SWEREF99, I defined the boundary and tiling following the default map sheet system (see post on [accessing open data from Lantmäteriet, the Swedish mapping, cadastral and land registration authority](https://karttur.github.io/geoimagine03-proj-wetland-se/wetkand-se_download-lantmateriet)): [sweref_system-define_v090.json](../../setup_processes/setup_processes-sweref_system-define/).

<figure>
<img src="../../images/sweref_epsg3006_boundary_se.png">
<figcaption>Boundary of the sweref projection system compared with Sweden national terrestrial and maritime boundaries.</figcaption>
</figure>

### DefaultRegionFromCoords

To access the complete region covered by the projection system (sweref) I added a default region, also called _sweref_, that exactly matches the boundaries of the projection system. The default region is defined using the same projection (EPSG) as the system: [sweref_defaultregion_v090.json](../../setup_processes/setup_processes-sweref_defaultregion/)

When registered in the database the geographic corners in longitude and latitude are automatically calculated and added. You can inspect the new default region directly in the [postgres database](https://karttur.github.io/setup-ide/setup-ide/install-postgres#TablePlus) with the query:

```
SELECT * FROM system.regions WHERE regionid='sweref';
```

### ManageDefRegProj

To access the new projection system and its default region a Framework user must create a _project_ that includes a _tract_ that is defined from the default region (_sweref_ in this example). I defined the new project using the system default user, _karttur_, and then called the new project _karttur-sweref_. Then I also called the new _tract_ _karttur-sweref_: [sweref_user+project_v090.json](../../setup_processes/setup_processes-sweref_user+project/).

If you want to check out that the _project_ was added to the database, use the query:

```
SELECT * FROM userlocale.userprojects WHERE projid = 'karttur-sweref';
```

And to check out the _tract_:

```
SELECT * FROM regions.tracts WHERE tractid = 'karttur-sweref';
```

## Next step

The new projection system, (sweref) is not ready to use in the Framework. The Framework project [_Swedish wetlands_](#) use this system combined with open source data.
