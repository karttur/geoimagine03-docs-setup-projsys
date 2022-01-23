---
layout: article
title: sweref_user+project_v090.json
categories: setup_processes
excerpt:  Define SWEREF user project
tags::
    - sweref_user+project
date: 2022-01-22
modified: 2022-01-22
comments: true
share: true
---

# sweref user+project (setup_processes)

##  Define SWEREF user project

The json command file <span class='file'>sweref_user+project_v090.json</span> is part of Karttur's GeoImagine project <span class='project'>setup_processes</span>. Calling the json file will execute the following commands of the GeoImagine Framework.

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
      "processid": "ManageDefRegProj",
      "overwrite": false,
      "parameters": {
        "defaultregion": "sweref",
        "tractid": "karttur-sweref",
        "tractname": "karttur sweref",
        "projid": "karttur-sweref",
        "projname": "karttur sweref",
        "tracttitle": "Karttur sweref",
        "tractlabel": "Karttur sweref",
        "projtitle": "Karttur sweref",
        "projlabel": "Karttur sweref"
      }
    }
  ]
}
```
