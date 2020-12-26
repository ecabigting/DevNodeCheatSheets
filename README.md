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
    *   You might need `brew` installed in your machine if this is the method you want to go to
        >   if you are using linux dont forget to add `eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)` to your `.bashrc`
    *   `brew tap mongodb/brew` to `tap` to the offical MongoDB formula repo
    *   `brew install mongodb-community@4.0` to install
        >   From the terminal: 
        >
        >   ==> Caveats for `LINUX` users
mongodb-community@4.0 is keg-only, which means it was not symlinked into /home/linuxbrew/.linuxbrew,
because this is an alternate version of another formula. If you need to have mongodb-community@4.0 first in your PATH run: `echo 'export PATH="/home/linuxbrew/.linuxbrew/opt/mongodb-community@4.0/bin:$PATH"' >> ~/.profile`
    *   You can manually execute the service instead with: `mongod --config /home/linuxbrew/.linuxbrew/etc/mongod.conf`
    *   Installing MongoDB Compass
        >   Client tool provided by the MongoDB team to help see MongoDB database visuall.
        >   `https://www.mongodb.com/try/download/compass`
