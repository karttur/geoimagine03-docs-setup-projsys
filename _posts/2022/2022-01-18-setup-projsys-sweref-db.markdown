---
layout: post
title: "Projection system setup:</br>Part 1 setup_db"
categories: setup
excerpt: "Setup database for custom projection system (SWEREF99)"
tags:
  - projection
  - system
  - sweref
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-18 11:27'
modified: '2022-01-22 T18:17:25.000Z'
comments: true
share: true
figure1: machinelarning_histo_housing
figure3A: machinelarning_linregnaive
figure3B: machinelarning_linregmodel
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

## Introduction

When you work with Karttur's GeoImagine Framework you must have a predefined geographic region linking to an also predefined projection system. You can always work with any of the predefined projection systems, including the [MODIS sinusoidal grid](https://karttur.github.io/geoimagine03-docs-main/setup/setup-custom-system/) or any of the three [ease-GRID systems](https://karttur.github.io/geoimagine03-docs-main/setup/setup-custom-system/). But you might also want to work with more specific regional projection systems. This post explains how to add a custom regional projection system to the Framework. As an example the post uses the national Swedish mapping system SWEREF99, with the EPSG code 3006. While it is possible to even use a customised projection (i.e. a projection system not having any EPSG definition), this is not covered in this post.

## Prerequisites

To create a customised projection system you must have [setup the database](https://karttur.github.io/geoimagine03-docs-main/setup/setup-setup-db/) and also [setup processes](https://karttur.github.io/geoimagine03-docs-main/setup/setup-setup-processes/).

## setup projection system

For a projection system to be applicable in the Framework, the data belonging to the system must be defined in its own database _schema_ and all the system specific processes must be opened up for the novel projection system. Projection system _schema_ typically include the following _tables_:

- compdef
- compprod
- tilelayer
- regionlayer
- mask
- tilecoords
- regions

If you have datasets that are specifically linked to the project system and want to import these layers directly to the system, rather than via the system _ancillary_, also the following tables are required:

- dscompid
- datasets
- dslayers
- authors

All the system specific processes able to operate on spatial data belong to the defined projection system must then be defined in the schema.table _process.procsys_. The new projection system and its EPSG code must also be added to the support table _system.systemepsg_.

### System schema and tables

The system specific schema and its tables must be setup from the python package [<span class='package'>setup_db</span>](https://karttur.github.io/geoimagine03-docs-main/setup/setup-setup-db/). The package contains a predefined module called _setup_db_sweref_, for setting up the projection system for the national Swedish mapping grid. Using that module as an example, and this post as a tutorial, it is fairly easy to create any custom projection system based on EPSG defined projections.

The setup includes two json processes, one for defining the schema and all the tables, and one for adding the new projection system to the processes. The former is straight forward both to run and also to alter for any other custom system. Opening spatial processes to include a novel system is a bit more complex and requires generating the records to insert in the table _process.procsys_. The latter is the topic of the next section, whereafter follows the actual database construction.

NOTE that you can also generate the system specific schema and its tables from the package <span class='package'>setup_processes</span>. As the setup is described in this tutorial that setup alternative is redundant, but it nevertheless runs through the postgres database and checks the existence of the schema and the required tables in the process <span class='process'>DefineCustomSystem</span>. For now (March 2022), I will just leave it like that.

#### Generate records for opening processes

The module <span class='module'>setup_db_sweref</span> contains the routine _TemplateProcessSystem_. Calling it requires 5 variables:

- prodDB [the production database]
- templateSystem [an existing projection system to use as template]
- newSystem [the name of the new projection system]
- templateEPSG [the EPSG integer code of the  existing template projection system]
- newEPSG [the EPSG integer code of the new projection system]

The default projection system ease2n (ease2s or ease2t) is probably the most suitable projection system to use as a template. The alternative is to use the MODIS sinusoidal grid (_modis_) projection system, but the _modis_ system includes a large number of processes that are specific for the MODIS data products. If you chose to use _modis_ as your template you should edit the values before inserting them.

To generate the records to insert for your projection system, run the routine _TemplateProcessSystem_. As noted above, the json code generated from the routine _TemplateProcessSystem_ might need some editing, and then it must be saved as a <span class='file'>.json</span> file. For the example in this post, the generated code must saved in the file [<span class='file'>SWEREF_insert-procsys_v090_sql.json</span>](../../setup_db/setup_db-SWEREF_insert-procsys/). If you use the default directory structure and paths as the original Framework the file should be saved under the directory <span class='file'>jsonsql</span>.

#### Create schema and tables

The json commands for creating the  schema and tables for the SWEREF99 projection system are in the file [<span class='file'>SWEREF_schema+tables_v090_sql.json</span>](../../setup_db/setup_db-SWEREF_schema+tables/). If you want to create another custom projection system you only need to change the name of schema:
```
"schema": "sweref"
```
to
```
"schema": "your_system"
```
by a search and replace.

#### json commands

With the "tableinsert" json commands for your projection system defined and saved as a json file, you can run the database processes that will create both the schema and tables and then insert the records.

In the \_\_main\_\_ section of the module <span class='module'></span> remove the comment signs (#) that points to the _projFPN_ <span class='file'>db_karttur_setup-sweref_20220121.txt</span> and the routine _SetupSchemasTables_:

```
  projFPN = path.join('doc','db_karttur_setup-sweref_20220121.txt')
  SetupSchemasTables(projFPN,prodDB)
```

The text file <span class='file'>db_karttur_setup-sweref_20220121.txt</span> points to the two json command files also linked above:

```
\# Install schema+tables for SWEREF
[SWEREF_schema+tables_v090_sql.json](https://karttur.github.io/geoimagine03-docs-setup-projsys/setup_db/setup_db-SWEREF_schema+tables_v090_sql.json/)

\# Insert table data for process systems
[SWEREF_insert-procsys_v090_sql.json](https://karttur.github.io/geoimagine03-docs-setup-projsys/setup_db/setup_db-SWEREF_insert-procsys_v090_sql.json/)
```

Run the python module to create all the updates to your postgres database.

The [next](../setup-projsys-sweref-processes) post in this series covers how to define the new projection system boundary and tiling, define a default region for the new system and then create a user specific project and tract.
