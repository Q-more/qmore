---
layout: post
title: Grafana with Postgre
description: >
  Sensor data visualisation with Grafana and Postgre database running in Docker.
image: /assets/img/grafana.png
image: /assets/img/postgre.png
image: /assets/img/docker.png
---

## Grafana
* with docker:

      docker run -d --network=host -p 3000:3000 --name=grafana -e "GF_INSTALL_PLUGINS=grafana-worldmap-panel,grafana-simple-json-datasource,linksmart-sensorthings-datasource" grafana/grafana

+ URL: http://localhost:3000/login
+ username : admin
+ password : admin

**Grafana uses OpenStreetMap**

## PostGIS Database
      sudo docker run --name postgis -p 25432:5432 -e POSTGRES_PASSWORD=postgres -d -t  mdillon/postgis

+ username: postgres
+ password: postgres
+ grafana host: localhost:25432
+ grafana database: postgres

- **crete tables in database with script: db_dump_ddl.sql**
- **fill database with SQL script: db_dump_dml.sql**

## Import dashboard
- in Grafana:

      create (plus icone) -> import -> Upload .json file -> sensors_dashboard.json

## Grafana World Map
#### SQL example of query:
- with longitude and latitude

      select time, value, latitude, longitude
      from "TableName" 
      where $__timeFilter("time")

- with geohash:

      select  time, value, geohas
      from "TableName" 
      where $__timeFilter("time")

- It is important to know that fields need to be named as those in this example, otherwise you have to assign alias:

      select timestamp as time, 
      valueField as value,
      st_x(location) as latitude,
      st_y(location) as longitude
      from "TableName" 
      where $__timeFilter("timestamp")


## User Interface

### Sensor values for SO2, CO and NO2

 ![Alt text](pictures/co2_co_so2_graf.png?raw=true)

### Sensor values and locations for CO 

 ![Alt text](pictures/co.png?raw=true)

### Sensor values and locations for NO

 ![Alt text](pictures/no2.png?raw=true)

### Sensor values and locations for Humidity

 ![Alt text](pictures/humidity.png?raw=true)

### Sensor values and locations for Noise

 ![Alt text](pictures/noise.png?raw=true)

### Sensor values and locations for Pressure

 ![Alt text](pictures/pressure.png?raw=true)

### Sensor values and locations for Temperature

 ![Alt text](pictures/temperature.png?raw=true)
