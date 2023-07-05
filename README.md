# Graphql--Unit
* Install Unit on a supported operating system  https://unit.nginx.org/installation/
 
# Next follow the steps below 
*For this demo

*For Node.js-based examples, see our Express, Koa, and Docker howtos or a basic sample.

```
$ sudo npm install -g --unsafe-perm unit-http  #Next, install unit-Http
$ mkdir -p demo
$ cd demo
$ npm install express --save  #Install express framework  https://unit.nginx.org/howto/express/ 
$ npm link unit-http  #First, you need to have the unit-http module installed. If it’s global, symlink it in your project directory:
$ npm init
```
* Create your Express app; let’s store it as /demo/apollo.js. First, initialize the directory:

```
import { ApolloServer } from '@apollo/server';
import { expressMiddleware } from '@apollo/server/express4';
import { ApolloServerPluginDrainHttpServer } from '@apollo/server/plugin/drainHttpServer';        
import express from 'express';
import http from 'http';
import cors from 'cors';
import bodyParser from 'body-parser';
//import { typeDefs, resolvers } from './schema';

const typeDefs = `#graphql
  type Query {
    hello: String
  }
`;

// A map of functions which return data for the schema.
const resolvers = {
  Query: {
    hello: () => 'world',
  },
};


// Required logic for integrating with Express
const app = express();
// Our httpServer handles incoming requests to our Express app.
// Below, we tell Apollo Server to "drain" this httpServer,
// enabling our servers to shut down gracefully.
const httpServer = http.createServer(app);

// Same ApolloServer initialization as before, plus the drain plugin
// for our httpServer.
const server = new ApolloServer({
  typeDefs,
  resolvers,
  plugins: [ApolloServerPluginDrainHttpServer({ httpServer })],
});
// Ensure we wait for our server to start
await server.start();

// Set up our Express middleware to handle CORS, body parsing,
// and our expressMiddleware function.
app.use(
  '/',
  cors(),
  bodyParser.json(),
  // expressMiddleware accepts the same arguments:
  // an Apollo Server instance and optional configuration options
  expressMiddleware(server, {
    context: async ({ req }) => ({ token: req.headers.token }),
  }),
);

// Modified server startup
await new Promise((resolve) => httpServer.listen({ port: 4003 }, resolve));

```


# Next follow the steps below to install Apollo GraphQl
```
$ cd demo
$ npm install @apollo/server express graphql cors body-parser

```
* Next, prepare the Express configuration for Unit:

```
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
```
*Save this as demo.json

*Please upload the revised configuration. If the JSON file mentioned above has been added to demo.json.

*Run the below cmd to update the configuration: 

```
$ curl -X PUT --data-binary @demo.json --unix-socket   /var/run/control.unit.sock http://localhost/config -v
```
*After a successful update, you should see the app should be available on the listener’s IP address and port:

```
{
        "success": "Reconfiguration done."
}

```





