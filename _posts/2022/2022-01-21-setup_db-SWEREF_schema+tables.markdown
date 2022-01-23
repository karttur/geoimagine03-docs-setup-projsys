---
layout: article
title: SWEREF_schema+tables_v090_sql.json
categories: setup_db
excerpt: Install schema+tables for SWEREF
tags::
    - SWEREF_schema+tables_v090_sql.json
date: 2022-01-19
modified: 2022-01-19
comments: true
share: true
---

# SWEREF schema+tables v090 sql.json (setup_db)

## Install schema+tables for SWEREF

The json command file <span class='file'>SWEREF_schema+tables_v090_sql.json</span> is part of karttur's GeoImagine project <span class='project'>setup_db</span>. Calling the json file will execute the following commands of the GeoImagine Framework.

```
{
  "process": [
    {
      "processid": "createschema",
      "overwrite": false,
      "delete": false,
      "parameters": {
        "db": "karttur",
        "schema": "sweref"
      }
    },
    {
      "processid": "createtable",
      "overwrite": false,
      "delete": false,
      "parameters": {
        "db": "karttur",
        "schema": "sweref",
        "table": "compdef",
        "command": [
          "compid TEXT",
          "content varchar(32)",
          "layerid varchar(64)",
          "prefix varchar(32)",
          "scalefac real",
          "offsetadd real",
          "measure char(1) NOT NULL",
          "dataunit varchar(32)",
          "title varchar(255)",
          "label varchar(255)",
          "PRIMARY KEY (compid)"
        ]
      }
    },
    {
      "processid": "createtable",
      "overwrite": false,
      "delete": false,
      "parameters": {
        "db": "karttur",
        "schema": "sweref",
        "table": "compprod",
        "command": [
          "compid TEXT",
          "system varchar(16) NOT NULL",
          "source TEXT",
          "product varchar(32)",
          "suffix varchar(64)",
          "cellnull real",
          "celltype varchar(8)",
          "masked character(1) DEFAULT 'N'",
          "title varchar(255)",
          "label varchar(255)",
          "frequency varchar(20)",
          "PRIMARY KEY (compid,source,product,suffix)"
        ]
      }
    },
    {
      "processid": "createtable",
      "overwrite": false,
      "delete": false,
      "parameters": {
        "db": "karttur",
        "schema": "sweref",
        "table": "layer",
        "command": [
          "layerid bigserial",
          "compid TEXT",
          "source TEXT",
          "product varchar(32)",
          "suffix varchar(64)",
          "acqdatestr varchar(20)",
          "acqdate date",
          "doy smallint",
          "createdate date DEFAULT CURRENT_DATE",
          "xtile smallint",
          "ytile smallint",
          "xytile char(8)",
          "PRIMARY KEY (compid,source,product,suffix,xtile,ytile,acqdatestr)"
        ]
      }
    },
    {
      "processid": "createtable",
      "overwrite": false,
      "delete": false,
      "parameters": {
        "db": "karttur",
        "schema": "sweref",
        "table": "mask",
        "command": [
          "source TEXT",
          "product varchar(32)",
          "cellnull smallint",
          "water smallint",
          "cloudshadow smallint",
          "snow smallint",
          "cloud smallint",
          "clear smallint",
          "mask smallint ARRAY[3]",
          "PRIMARY KEY (source,product)"
        ]
      }
    },
    {
      "processid": "createtable",
      "overwrite": false,
      "delete": false,
      "parameters": {
        "db": "karttur",
        "schema": "sweref",
        "table": "tilecoords",
        "command": [
          "xytile char(6)",
          "xtile smallint",
          "ytile smallint",
          "minx double precision",
          "miny double precision",
          "maxx double precision",
          "maxy double precision",
          "west double precision",
          "south double precision",
          "east double precision",
          "north double precision",
          "ullat double precision",
          "ullon double precision",
          "lrlat double precision",
          "lrlon double precision",
          "urlat double precision",
          "urlon double precision",
          "lllat double precision",
          "lllon double precision",
          "PRIMARY KEY (xytile)"
        ]
      }
    },
    {
      "processid": "createtable",
      "overwrite": false,
      "delete": false,
      "parameters": {
        "db": "karttur",
        "schema": "sweref",
        "table": "regions",
        "command": [
          "regionid varchar(64)",
          "regiontype varchar(8)",
          "xtile smallint",
          "ytile smallint",
          "PRIMARY KEY (regionid,xtile,ytile)"
        ]
      }
    }
  ]
}
```
