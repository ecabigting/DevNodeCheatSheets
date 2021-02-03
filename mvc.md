# MVC with NodeJS, Express, MongoDb, EJS

Creating an MVC app with NodeJS and mentioned technologies to handle CRUD operation.

1. Your starting **app.js** See example below:
```javascript
    //app.js
    const express = require('express') // require package express js
    const fileUpload = require('express-fileupload') // adds the files property to the req object so that we can access the uploaded files using req.files.

    const mongoose = require('mongoose') // require the mongoose package
    mongoose.connect('mongodb://localhost/efnMongoDB',{useNewUrlParser: true,useUnifiedTopology: true,useCreateIndex:true }) // connect to MongoDB on local using mongoose and set the parameters

    const app = new express() // declare `app` as a new instance of express js
    /* we need the body-parsing middleware called body-parser,
    body-parser parse incoming request bodies
    it will make the form data available under the req.body property
    */
    const bodyParser = require('body-parser')
    app.use(bodyParser.json())
    app.use(bodyParser.urlencoded({extended:true})) 
    app.use(express.static('public')) // serve public folder for our static files
    app.use(fileUpload()) // register the fileupload package in Express
    app.set('view engine','ejs') // require ejs templating language

    app.listen(1204, ()=>{ console.log('>> App listening on port 1204') }) // start the app and listen to port 1204

    // here is an example loading a view from a request
    app.get('/', (req,res) => { // declare a route to home(index) or root
        res.render('index') // since we loading the view engine above we are calling the index.ejs file here and render it in our response
    })

    // calling our root which is our homeController
    // let initate our controller to as homeController
    const homeController = require('./controllers/home')
    // since we are want to load our home controller on root '/'
    // lets listen to it and then return our home controller
    app.get('/',homeController)

```

2. Creating your **Model**.
```javascript
    // Creating your model
    // Create a folder called models on the same folder as your app.js
    // Inside the models folder create your first model BlogPost

    const mongoose = require('mongoose') // we need to call mongoose as the offical nodejs library for MongoDb
    const Schema = mongoose.Schema; // starting our schema

    const BlogPostSchema = new Schema({ // declaring our schema for blogpost
        // below are the fields of our schema with data type of each field
        title: String,
        body: String,
        createdBy : Number,
        createdDate : { type: Date, default: new Date() },
        lastEditedBy : { type: Date, default: new Date() },
        lastEditedDate : { type: Date, default: new Date() },
        image: String
    });

    /*
        lets say our Blogpost Schema is index for searching and we are indexing the 'title' field and the 'body' field we really do not need to do this but its helpful for text searching
        our BlogPostDictionairy later on
    */
    BlogPostSchema.index({title:"text",body:"text"}) 
    /*
        lets initate our Model BlogPost
    */
    const BlogPost = mongoose.model('BlogPost',BlogPostSchema);
    module.exports = BlogPost // finally export our module
```

3. Creating your **View**
```html
    <!-- Creating your View -->
    <!-- Create a folder called views on the same folder as your app.js -->
    <!-- Inside the views folder create your first view index.ejs(notice the ejs since we are using the ejs templating language) -->
    <!DOCTYPE html>
    <html lang="en">
    <%- include('shared/header'); -%>
    <body>
    <%- include('shared/navbar'); -%>
    <!-- Page Header -->
    <header class="masthead" style="background-image: url('/img/home-bg.jpg')">
        <div class="overlay"></div>
        <div class="container">
        <div class="row">
            <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
                <h1> &lt;ecabigting/&gt; </h1>
                <span class="subheading">lets code</span>
            </div>
            </div>
        </div>
        </div>
    </header>

    <!-- Main Content -->
    <div class="container">
        <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">  
            <div class="col-lg-8 col-md-10 mx-auto">
            <% for (var i = 0; i < blogposts.length; i++) { %>
                <div class="post-preview">
                <a href="/post/<%= blogposts[i]._id%>">
                <h2 class="post-title">
                    <%= blogposts[i].title %>
                </h2>
                <h3 class="post-subtitle">
                    <%= blogposts[i].body %>
                </h3>
                </a>
                <p class="post-meta">Posted by 
                <a href="#"><%= blogposts[i].createdBy %></a> 
                on <%= blogposts[i].createdDate.toDateString() %>
                </p>
                </div>
                <hr>
            <% } %>
            <!-- Pager -->
            <div class="clearfix">
            <a class="btn btn-primary float-right" href="#">Older Posts &rarr;</a>
            </div>
        </div>
        </div>
    </div>

    <hr>
    <%- include('shared/footer'); -%>
    <%- include('shared/scripts'); -%>
    </body>

    </html>
```

3. Creating your Controller
```javascript
    /* Creating your controller
       Create a folder called controllers on the same folder as your app.js
       Inside the models folder create your first controller home.js
    */ 

    /*
        since our sample view requires our list of blog post 
        lets load our blogpost model first
    */
    const BlogPost = require('../models/BlogPost.js') 
    // here is the request and response code for our home.js
    module.exports = async (req, res) =>    {
        const blogposts = await BlogPost.find({}) // we are fetching all blogpost in our mongoDb
        res.render('index',{ // render our index.ejs file
            blogposts // send our blogpost list object
        });
    }

```