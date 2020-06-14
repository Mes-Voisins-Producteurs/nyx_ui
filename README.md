# Nyx User Interface

![badge](https://img.shields.io/badge/made%20with-vuejs-blue.svg?style=flat-square)
![badge](https://img.shields.io/github/languages/code-size/snuids/nyx_ui)
![badge](https://img.shields.io/github/last-commit/snuids/nyx_ui)
![Azure Static Web Apps CI/CD](https://github.com/snuids/nyx_ui/workflows/Azure%20Static%20Web%20Apps%20CI/CD/badge.svg)


## Introduction

The NYX platform is an easy to use application builder that integrates various technologies such as:

* Elastic Search / Kibana
* PostGreSQL
* Jupyter Notebooks
* Node Red
* Apache ActiveMQ
* Apache Camel


The NYX UI includes an online configuration tools that can create the following pages:

* A Kibana page
* A generic table page (PostGreSQL or ElasticSearch)
* An in house controller
* An Upload page
* A Form
* A Free Text page
* A Vega graph
* External URLs

### Main Config Table
![Config1](https://raw.githubusercontent.com/snuids/nyx_ui/master/medias/app_config1.png)

### An Application configuration panel

![Config2](https://raw.githubusercontent.com/snuids/nyx_ui/master/medias/app_config2.png)

### Kibana Panel:

A kibana panel is directly linked to a kibana dashboard. It is possible to change the time selector and query the Kibana panel editor.

![Kibana1](https://raw.githubusercontent.com/snuids/nyx_ui/master/medias/app_kibana1.png)


### Generic Table:

A generic table is a view on:

* An Elastic Search Collection
* A PostgreSQL table or Query

It can include:

* a discover graph
* a map
* a query
* download options

![Table1](https://raw.githubusercontent.com/snuids/nyx_ui/master/medias/app_table1.png)
![Table2](https://raw.githubusercontent.com/snuids/nyx_ui/master/medias/app_table2.png)

### Vega Panel:

A Vega panel uses the Vega framework in order to build interactive dashboard.

![Vega](https://raw.githubusercontent.com/snuids/nyx_ui/master/medias/app_vega1.png)



## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```
Lauching application

==> http://localhost:8080/?api=https://YOUR_REST_API_SERVER/api/v1/&user=admin&password=*******#/

Rest API swagger

=> Rest API swagger https://YOUR_REST_API_SERVER/api/doc/



### Compiles and minifies for production
```
npm run build
```

### Extending the UI

To add a new specific controller:

=> put a new .vue file inside the folder components/external

To add a new specific table editor:

=> put a new .vue file inside the folder components/tableEditor

### Building the container

```
docker build .
```

### Version History

[Versions](versions.md)

