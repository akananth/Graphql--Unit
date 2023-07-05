# Graphql--Unit
* Install Unit on a supported operating system  https://unit.nginx.org/installation/
 
# Next follow the steps below 
*For this demo

*For Node.js-based examples, see our Express, Koa, and Docker howtos or a basic sample.

Next, install unit-Http
```
$ sudo npm install -g --unsafe-perm unit-http 
$ mkdir -p demo
$ cd demo
$ npm install express --save  #Install express framework  https://unit.nginx.org/howto/express/ 
$ npm link unit-http  #First, you need to have the unit-http module installed. If it’s global, symlink it in your project directory:
$ npm init
```
* Create your Express app; let’s store it as /demo/apollo.js. First, initialize the directory:



# Next follow the steps below to install Apollo GraphQl
```
$ cd demo
$ npm install @apollo/server express graphql cors body-parser

```
* Next, prepare the Express configuration for Unit:


{
    "listeners": {
        "*:4003": {
            "pass": "applications/express"
        }
    },

    "applications": {
        "express": {
            "type": "external",
            "user": "unit",
            "group": "unit",
            "working_directory": "/home/admin/apollo",  #this the directory where application is running 
            "executable": "/usr/bin/env",
            "arguments": [
                "node",
                "--loader",
                "unit-http/loader.mjs",
                "--require",
                "unit-http/loader",
                "apollo.js"  #name of the application
            ]
        }
    }
}

*Save this as demo.json

*Please upload the revised configuration. If the JSON file mentioned above has been added to demo.json.

*Run the below cmd to update the configuration: 

```
$ curl -X PUT --data-binary @demo.json --unix-socket   /var/run/control.unit.sock http://localhost/config -v
```
*After a successful update, you should see the app should be available on the listener’s IP address and port:

{
        "success": "Reconfiguration done."
}


## Table of contents
* [General info](#general-info)
* [Technologies](#technologies)
* [Setup](#setup)

## General info
This project is simple Lorem ipsum dolor generator.
	
## Technologies
Project is created with:
* Lorem version: 12.3
* Ipsum version: 2.33
* Ament library version: 999
	
## Setup
To run this project, install it locally using npm:

```
$ cd ../lorem
$ npm install
$ npm start
```




