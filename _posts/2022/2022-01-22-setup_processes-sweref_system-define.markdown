---
layout: article
title: sweref_system-define_v090.json
categories: setup_processes
excerpt:  Define the SWEREF tile/project systems
tags::
    - sweref_system-define
date: 2022-01-22
modified: 2022-01-22
comments: true
share: true
---

# sweref system define (setup_processes)

##  Define the SWEREF tile/project systems

The json command file <span class='file'>sweref_system-define_v090.json</span> is part of Karttur's GeoImagine project <span class='project'>setup_processes</span>. Calling the json file will execute the following commands of the GeoImagine Framework.

```
{
  "userproject": {
    "userid": "karttur",
    "projectid": "karttur",
    "tractid": "karttur",
    "siteid": "*",
    "plotid": "*",
    "system": "system"
  },
  "period": {
    "timestep": "static"
  },
  "process": [
    {
      "processid": "DefineCustomSystem",
      "overwrite": false,
      "version": "0.9",
      "verbose": 3,
      "parameters": {
        "systemid": "sweref",
        "epsg": 3006,
        "minx": 200000,
        "miny": 6100000,
        "maxx": 1000000,
        "maxy": 7700000,
        "xtiles": 8,
        "ytiles": 16
      }
    }
  ]
}
```
