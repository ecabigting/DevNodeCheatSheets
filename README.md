# Dev NodeJS Cheat Sheets
Development NodeJS Cheat Sheets

1. Automatic Server Restart with nodemon
     *   `npm install nodemon --save-dev`
         +  `--save` to list the dependency in our `package.json`.
         +  `-dev` to specify that we install `nodemon` for development only.
     *    `npm start`
          + with `nodemon` installed no longer we need to start the app with `node index.js`
          + must update the `package.json` with 
               > "scripts": {
               > "start": "nodemon index.js"
               > },

2. Template Engine : `EJS`
    *   `npm install ejs --save`
    *   Description: 
        > EJS is a simple templating language that lets us generate
HTML with plain JavaScript in simple straightforward scriplet tags i.e. <%=
… %>.

3. Using MongoDB
    *   Install via package management system
        +   Get a public GPG key first 
            >   `wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -`
        +   Create a list file for MongoDB. 
            >   For Ubuntu 20.04 (Focal) `echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list`
        +   Reload local package database
            >   `sudo apt-get update`
        +   Install the MongoDB packages
            >   `sudo apt-get install -y mongodb-org`
    *   Installing MongoDB Compass
        +   Client tool provided by the MongoDB team to help see MongoDB database visuall.
            >   `https://www.mongodb.com/try/download/compass`
