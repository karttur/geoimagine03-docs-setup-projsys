---
layout: post
title: "Projection system setup:</br>Part 3 update db"
categories: setup
excerpt: "Update database for process access to custom projection system (SWEREF99)"
tags:
  - projection
  - system
  - sweref
  - update db
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-20 11:27'
modified: '2022-10-22 T18:17:25.000Z'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

## Introduction

The functions of Karttur's GeoImagine Framework are built up using _processes_. All _processes_ are linked to specific types of data or data-sources (e.g. _processes_ developed specifically for MODIS imagery by default only operate on MODIS system data). When creating a new projection system, you must thus update the Framework postgres database to allow selected _processes_ to access the datasets belonging to the new projection system.

## Database table process.procsys

The Framework database table that defines the links between processes and projection system is _process.procsys_.

You can check out the existing links with the SQL:

```
SELECT * FROM process.procsys;
```

While you could manually update the database table, you can use a prepared support function under <span class='package'>setup_db</span> to update _process.procsys_ using an existing projection system as template.

### Update process.procsys using template projection system

The Framework includes several default global projection systems:

- system,
- ease2t,
- ease2n,
- ease2s,
- modis,
- sentinel and
- landsat

Of these, ease2t is probably the most generic and the recommended system to use as a template. Open the python module <span class='module'></span> and edit the lines under \_\_main\_\_ that define the template system and the new system:

```
    # Run TemplateProcessSystems to generate a json file for setting up
    # processes to to handle the new projection system properly
    templateSystem = 'ease2n'
    templateEPSG = 6931
    newSystem = 'sweref'
    newEPGSG = 3006
    TemplateProcessSystem(prodDB, templateSystem, newSystem, templateEPSG, newEPGSG)
```

The function <span class='function'>TemplateProcessSystem</span> reads the database and extracts the processes that apply to the template system and writes out a json coded instruction. You have to copy this json command from the screen and save it as a <span class='file'>.json</span> file that you can run using the process <span class='process'>tableinsert</span> (note that this process is only available from the package <span class='package'>setup_db</span>).

```
{
  "process": [
    {
      "processid": "tableinsert",
      "overwrite": false,
      "parameters": {
        "command": {
          "columns": [
            "subprocid",
            "system",
            "srcsystem",
            "dstsystem",
            "srcdivision",
            "dstdivision",
            "srcepsg",
            "dstepsg"
          ],
          "values": [
            [
              "'DefaultRegionFromCoords'",
              "'sweref'",
              "'NA'",
              "'system'",
              "'NA'",
              "'region'",
              "0",
              "3006"
            ],
            [
              "'DefaultRegionFromVector'",
              "'sweref'",
              "'sweref'",
              "'system'",
              "'NA'",
              "'region'",
              "0",
              "3006"
            ],
            [
              "'ExtractTileCoords'",
              "'sweref'",
              "'ancillary'",
              "'sweref'",
              "'region'",
              "'tiles'",
              "0",
              "3006"
            ],
            [
              "'VectorizeRegionTiling'",
              "'sweref'",
              "'system'",
              "'sweref'",
              "'region'",
              "'region'",
              "4326",
              "3006"
            ],
            [
              "'MosaicTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'MosaicAdjacentTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ReprojectAncillaryRegion'",
              "'sweref'",
              "'ancillary'",
              "'sweref'",
              "'region'",
              "'region'",
              "0",
              "3006"
            ],
            [
              "'GdalDemRegion'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'GdalDemTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'NumpyDemRegion'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'NumpyGeomorphology'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'NumpyDemTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'NumpyGeomorphologyTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'GrassManytoManyTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'GrassDemHydroDemTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'GrassDemReInterpolateTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'BasinOutletTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExtractTilesPointList'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExtractTilesPointVector'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExtractTilesLineVector'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExtractTilesPolyVector'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'Grass1to1Tiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'GrassOnetoManyTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'TranslateRegion'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'TranslateTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExportTilesToByte'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExportShadedTilesToByte'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExportTilesToMap'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExportTilesToSvg'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExportsToMovieFrames'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExportMovieClockLayout'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ExportMovieOverlayFrames'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ZipArchiveRegion'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'ZipArchiveTiles'",
              "'sweref'",
              "'sweref'",
              "'export'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'DuplicateRegion'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'DuplicateTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'ReprojectAncillaryRegionProjSys'",
              "'sweref'",
              "'ancillary'",
              "'sweref'",
              "'region'",
              "'region'",
              "0",
              "3006"
            ],
            [
              "'FileCheckTileDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tile'",
              "'tile'",
              "3006",
              "3006"
            ],
            [
              "'GrassDemFillDirTiles'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tiles'",
              "'tiles'",
              "3006",
              "3006"
            ],
            [
              "'TileAncillaryRegion'",
              "'sweref'",
              "'ancillary'",
              "'sweref'",
              "'region'",
              "'tiles'",
              "0",
              "3006"
            ],
            [
              "'ReprojectDefaultRegionProjSys'",
              "'sweref'",
              "'system'",
              "'sweref'",
              "'region'",
              "'region'",
              "0",
              "3006"
            ],
            [
              "'RenameRegionDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'OrganizeProjSysData'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'ReprojectAnciillaryRegionProjSys'",
              "'sweref'",
              "'ancillary'",
              "'sweref'",
              "'region'",
              "'region'",
              "0",
              "3006"
            ],
            [
              "'RenameTileDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tile'",
              "'tile'",
              "3006",
              "3006"
            ],
            [
              "'LinkDefaultRegionTiles'",
              "'sweref'",
              "'system'",
              "'sweref'",
              "'region'",
              "'region'",
              "0",
              "3006"
            ],
            [
              "'LinkProSysRegion'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'RemoveRegionDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'RemoveTileDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tile'",
              "'tile'",
              "3006",
              "3006"
            ],
            [
              "'LinkProjSysRegion'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'DbCheckRegionDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'DbCheckTileDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tile'",
              "'tile'",
              "3006",
              "3006"
            ],
            [
              "'FileCheckRegionDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'MetaUpdateRegionDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'region'",
              "'region'",
              "3006",
              "3006"
            ],
            [
              "'metaUpdateTileDataset'",
              "'sweref'",
              "'sweref'",
              "'sweref'",
              "'tile'",
              "'tile'",
              "3006",
              "3006"
            ]
          ]
        },
        "db": "karttur",
        "schema": "process",
        "table": "procsys"
      },
      "processid": "tableinsert"
    },
    {
      "overwrite": false,
      "parameters": {
        "command": {
          "columns": [
            "system",
            "epsg"
          ],
          "values": [
            [
              "'sweref'",
              "3006"
            ]
          ]
        },
        "db": "karttur",
        "schema": "system",
        "table": "systemepsg"
      },
      "processid": "tableinsert"
    }
  ]
}
```

To run the <span class='process'>tableinsert</span> above you have to save it as a <span class='file'>.json</span> file, link it via text file and then run it from within the <span class='package'>setup_db</span> package. For this example I saved the json file directly under the path of <span class='package'>setup_db</span>: <span class='file'>setup_db/doc/jsonsql/SWEREF_insert-procsys_v090_sql.json</span>.

Then I linked to this file in the text file <span class='file'>setup_db/doc/db_karttur_setup-sweref_20220121.txt</span>:

```
# Insert table data for process.procsys
jsonsql/SWEREF_insert-procsys_v090_sql.json
```
Finally I run the commands from the \_\_main\_\_ section of <span class='module'>setup_db_sweref.py</span>:

```
projFPN = path.join('doc','db_karttur_setup-sweref_20220121.txt')
    SetupSchemasTables(projFPN,prodDB)
```

Query the database to check that the _processes_ are linked also to the new system (sweref):

```
SELECT * FROM process.procsys WHERE system = 'sweref';
```

## Next step

The setup of the custom projection system is now finished. The system defined in this series of tutorials is used in the project [Swedish wetlands](https://karttur.github.io/geoimagine03-proj-wetland-se/index.html).
