
# Running the Analysis Platform form Sources

This is a quick start guide to get the OSSMETER platform running from source.

Although these instructions may apply to other versions of Eclipse, they were tested under Eclipse Neon.3 with plug-in development support (Eclipse IDE for RCP Developers package).

A step-by-step video guide is also available at [https://youtu.be/3Ry4KKfNdYg](https://youtu.be/3Ry4KKfNdYg)

## Start MongoDB

You can download MongoDB from the [MongoDb website](http://www.mongodb.org/downloads).

Instructions for starting mongo can be found in the MongoDB [manual](http://docs.mongodb.org/manual/). For example:

````Shell
mongod --dbpath /data/db --port 27017
````

## Get the Code

Get the latest version of the code, and checkout the `dev` branch. Please don't commit to the `master` branch: see the [Development Guidelines](../development/Development-Guidelines):

If you are using __Linux / OS X__:
````Shell
git clone https://github.com/crossminer/scava.git scava
cd scava
git checkout dev
````

If you are using __Windows__ you need to do things differently due to Windows' long file name limit. In the Git shell:
````Shell
mkdir scava
cd scava
git init
git config core.longpaths true
git add remote origin https://github.com/crossminer/scava.git
git fetch
git checkout dev
````

## Setup Eclipse

Open Eclipse and import all projects from the top level directory of the Scava code (`File -> Import -> Existing projects into workspace`), and wait for all the projects to compile without errors.

## Validate and Run the Platform

Open `org.ossmeter.platform.osgi/ossmeterfromfeature.product`
  * Click the `Validate...` icon in the top right of the product configuration editor (the icon is a piece of paper with a tick)
  * If things do not validate, there's something wrong -- get in touch :) Problems related to `org.eclipse.e4.core.di` aren't critical.
  * Then, click the `Export an Eclipse product` on the left of the `Validate...` button. Uncheck the `Generate p2 repository` checkbox, select a destination directory and validate. After a while, the OSSMETER platform will be generated in the selected directory.
  * The platform can then be run using the generated `eclipse` binary; it accepts the following arguments:
    * `-apiServer`: Starts up the client API on localhost:8182
    * `-worker ${id-worker}`: Spawns a thread that analyses registered projects
  * To get a full platform running, first launch a master thread, then a slave, and finally the API server.

If you are developing code for the Scava platform, be sure to check out the [Contributing](../development/Contributing).

When starting the platform, you can pass a configuration file to control the behaviour of the platform:

````Shell
./eclipse -slave -config myconfiguration.properties
````

The configuration file is a typical Java properties file. The properties that can be configured are:

## Configure the Platform

#### `identifier=<your name>` 
The identifier of the node. If not specified, the platform will attempt to use the node's hostname, and if it cannot resolve the hostname, it will generated a random UUID. 

If you plan to multiple instances of the platform on the same machine, you should definitely specify different node identifiers.

#### `log.type=console|file|rolling`
You can specify whether to log output to the console (Log4J's ConsoleAppender), to a particular file without a size limit (Log4J's FileAppender), or to a different file per day (Log4J's DailyRollingFileAppender). If you specify `file` or `rolling`, you must complete the `log.file.path` or `log.rolling.path` property as well. 

If the property is not specified, it will default to the console logger.

#### `log.file.path=<path>`
The path to the file to store the log. E.g. `log.file.path=/tmp/lovelylog.log`

#### `log.rolling.path=<path>`
The path to the file to store the log. This is a Log4J DailyRollingFileAppender that will create separate logs for each 12 hours of the day. The date stamp will be appended to the path provided. E.g.: if you specify `log.rolling.path=/tmp/mylovelylog.log`, it will store files like so: `/tmp/mylovelylog.log.2014-12-17-00` and `/tmp/mylovelylog.log.2014-12-17-12`.

#### `maven_executable=<path>`
The path to where Maven is installed. E.g. `maven_executable=/usr/bin/mvn`

#### `storage_path=<path>`
The path to where files should be stored. E.g. `storage_path=/mnt/ossmeter/`

#### `mongo_hosts`
A comma-separated list of the hosts and ports in a replica set. E.g. `ua002:27017,ua009:27017,ua019:27017,ua020:27017`

## Run the api-gateway

  * Right click on
`scava-api-gateway/src/main/java/org.eclipse.scava.apigateway/ApiGatewayApplication.java`
  * Then click on Run As -> Java Application

## Run the authentication service

  * Right click on
`scava-auth-service/src/main/java/org.eclipse.scava.authservice/AuthServiceApplication.java`
  * Then click on Run As -> Java Application

## Run the administration dashboard

Scava Administration is a single page web application based on Angular 6 Framework. To get started with Angular, it's better to install Angular CLI tool to make application development more quicker and easier (Find more here: https://angular.io/guide/quickstart).

The following instructions show how to run the dashboard web app:
  * Enter the `administration/scava-administration/` directory within the scava repository.
  * Run the web app on port 4200 using angular-cli: `ng serve`
  * Navigate to `http://localhost:4200/`

  