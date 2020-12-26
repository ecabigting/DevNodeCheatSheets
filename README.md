# DevNodeCheatSheets
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

