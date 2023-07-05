# Graphql--Unit
Install Unit on a supported operating system  https://unit.nginx.org/installation/
Next, install unit-Http
sudo npm install -g --unsafe-perm unit-http
 
# Next follow the steps below 
Install express framework  https://unit.nginx.org/howto/express/  Follow the steps here.
mkdir -p demo
cd demo
npm install express --save
npm link unit-http

Create your Express app; let’s store it as /demo/apollo.js. First, initialize the directory:
npm init

# Next follow the steps below to install Apollo GraphQl
cd demo
npm install @apollo/server express graphql cors body-parser
 
Next, prepare the Express configuration for Unit:



