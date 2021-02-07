## Introduction

This container prepares a MSSQL Server for Intershop development.

## Configuration & Startup of a container

Simple command
```
docker run -d -p 1433:1433 --name mssql-intershop --env ACCEPT_EULA=Y --env SA_PASSWORD=password mssql-intershop
```

Compose File
```
version: "3.4"
services:
  mssql-server:
    # build: .
    image: "intershophub/mssql-intershop:latest"
    
    ports:
    - "1433:1433"

    volumes:
      - type: volume
        source: mssqlserver
        target: /var/opt/mssql
        volume:
          nocopy: true

    environment:
      ACCEPT_EULA:  Y
      SA_PASSWORD:  password
      MSSQL_PID:    Developer
      RECREATEDB:   "false"
      RECREATEUSER: "false"
      ICM_DB_NAME:  dbname
      ICM_DB_USER:  username
      ICM_DB_PASSWORD: userpassword

volumes:
  mssqlserver:
```

The following parameter are required:

| Variable | description | possible values
|----------|-----------|-----------| 
| ACCEPT_EULA   | This confirms your acceptance of the Microsoft End-User Licensing Agreement. | Y, N |
| SA_PASSWORD   | This is the database system administrator (userid = 'sa') password used to connect to SQL Server once the container is running. Important note: This password needs to include at least 8 characters of at least three of these four categories: uppercase letters, lowercase letters, numbers and non-alphanumeric symbols. | eg. 1nstershop5A |
| MSSQL_PID     | This specifies the Product ID (PID) or Edition that the container will run with. | Developer, Express, Enterprise, EnterpriseCore
| RECREATEDB    | If this variable is true, the database will be recreated. | true, false
| RECREATEUSER  | If this variable is true, the database user will be recreated. | true, false
| ICM_DB_NAME      | Database name           | icmdb
| ICM_DB_USER      | Database user name      | intershop
| ICM_DB_PASSWORD  | Database user password  | intershop

Description for MS SQL product ids
* Developer : This will run the container using the Developer Edition (this is the default if no MSSQL_PID environment variable is supplied) 
* Express : This will run the container using the Express Edition 
* Standard : This will run the container using the Standard Edition 
* Enterprise : This will run the container using the Enterprise Edition 
* EnterpriseCore : This will run the container using the Enterprise Edition Core 

## Database Connection Parameter

| Parameter | Value
|------------------------|-----------------|
| Database name          | ```$ICM_DB_NAME``` |
| Database user name     | ```$ICM_DB_USER```|
| Database user password | ```$ICM_DB_PASSWORD```|
| Host name              | ```localhost``` |
| Port                   | ```1433``` |

The hostname is localhost as long the container runs alone on your docker host machine. If you use this container
 together with other containers it is necessary to use the service name. The external port is configurable in the docker compose file. 

## Connect Local Build Environment (ICM 7.10)

To connect your local ICM development environment with the local docker mssql database your configuration in the `environment.properties` of your development machine should look like this.

```
# Database configuration
databaseType = mssql
jdbcUrl = jdbc:sqlserver://localhost:1433;database=DB; 
databaseUser = intershop 
databasePassword = intershop
```

## Build the Container

Build the container image with Ubuntu 16.04 and MSSQL Server 2017
```
docker build . --tag mssql-intershop
```

Build the container image with Ubuntu 18.04 and MSSQL Server 2019 and tag mssql-intershop:2019-latest
```
docker build . --build-arg UBUNTUVERSION=18.04 --build-arg MSSQLVERSION=2019 --tag mssql-intershop:2019-latest
```

The following parameters are tested

| Ubuntu Version <br> UBUNTUVERSION | MSSQL Server Version <br> MSSQLVERSION |
|------------------------|----------------------|
| 16.04 (Default Value)  | 2017 (Default Value) |
| 16.04 (Default Value)  | 2019 |
| 18.04  | 2019 |

## License

Copyright 2014-2020 Intershop Communications.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

