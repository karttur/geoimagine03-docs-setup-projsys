---
layout: article
title: sweref_defaultregion_v090.json
categories: setup_processes
excerpt:  Define SWEREF default region
tags:: 
    - sweref_defaultregion
date: 2022-01-22
modified: 2022-01-22
comments: true
share: true
---

# sweref defaultregion (setup_processes)

##  Define SWEREF default region

The json command file <span class='file'>sweref_defaultregion_v090.json</span> is part of Karttur's GeoImagine project <span class='project'>setup_processes</span>. Calling the json file will execute the following commands of the GeoImagine Framework.

```
{
  "userproject": {
    "userid": "karttur",
    "projectid": "karttur",
    "tractid": "karttur",
    "siteid": "*",
    "plotid": "*",
    "system": "sweref"
  },
  "period": {
    "timestep": "static"
  },
  "process": [
    {
      "processid": "DefaultRegionFromCoords",
      "overwrite": false,
      "parameters": {
        "regioncat": "global",
        "regionid": "sweref",
        "regionname": "system default region",
        "parentid": "global",
        "parentcat": "global",
        "epsg": 3006,
        "stratum": "5",
        "minx": 200000,
        "miny": 6100000,
        "maxx": 1000000,
        "maxy": 7700000,
        "version": "1.0",
        "title": "system default region SWEREF 3006",
        "label": "system default region SWEREF 3006."
      },
      "dstpath": {
        "volume": "Ancillary"
      }
    }
  ]
}
```
