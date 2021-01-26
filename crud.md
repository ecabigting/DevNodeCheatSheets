# Cheat Sheet for Create, Read, Update, and Search functions using NodeJS and ExpressJS with MongoDB(mongoose) as the database.

### In the example below I use **blogpost** to refer to the collection and **post** to refer to documents as example.
1.  Require mongoose
    ```javascript
    const mongoose = require('mongoose')
    ```

2.  Import the blogpost model with the correct path
    ```javascript
    const BlogPost = require('./models/BlogPost')
    ```

3.  Connect to the db `note: if my_database is not created` it will automatically be created
    ```javascript
    mongoose.connect('mongodb://localhost/my_database',
    {
        useNewUrlParser: true,
        useUnifiedTopology: true 
    })
    ```

4.  Declare a route to home(index) or root this is to list all objects for the page in this case blogposts
    ```javascript
    app.get('/', async (req,res)=>{
        // lets retrieve all blog post and hold them in blogposts varliable
        const blogposts = await BlogPost.find({})
        // calling responds to render 'index'
        // we pass back the blogposts data back to
        // the browser by providing it as the second argument to 'render'
        res.render('index',{
            // if keyname and value name are the same we can use the same
            // so use 'blogpost' instead of 'blogpost:blogpost'
            blogposts
        })
    })
    ```

5.  Create our first post with the create function accepts 2 arguments 1st arg: data we weant to save 2nd arg: call back function
    ```javascript
    BlogPost.create({
        title: "Original This is a sample Title",
        body: "This is a huge sample of a body Lorem ispm, lfhu2 ,gasdjjg. Cghjjugjkgkajsgasjgksadgjasd"
    },(error,blogpost) => {
        // this will be called when 'create' function finish execution
        // mongoose gives us any error in the error arg if there is any during execution
        // the 2nd arg 'blogpost' returns us the newly created document in this case 'blogpost'
        console.log(' >>>> Created new blog post! with title:' + blogpost.title)
        console.log(error,blogpost)
    })
    ```

6.  Get all documents of 'BlogPost'
    ```javascript
    BlogPost.find({},(error,blogpost) => {
        console.log(' >>>> Get all blogpost!')
        console.log(error,blogpost)
    })
    ```

7.  Find a document with the specific title
    ```javascript
    BlogPost.find({
        title:'This is a sample Title'
    },(error,blogpost) => {
        console.log(' >>>> find blogpost with title:' + blogpost.title)
        console.log(error,blogpost)
    })
    ```

8.  Find a document that contains 'original' in the title
    ```javascript
    BlogPost.find({
            title:/Original/
        },(error,blogpost) => {
            console.log(' >>>> find blogpost with title that contains original')
            console.log(error,blogpost)
    })
    ```

9.  Find a blogpost with specific id
    ```javascript
    var id = "5fe86d4483b6b3159425a285";
    BlogPost.findById(id,(error,blogpost)=>{console.log(error,blogpost)})
    ```

10. Find a blogspot by id and then update the title
    ```javascript
    var id = "5fe86a731e52831481e4b0dd";
    BlogPost.findByIdAndUpdate(id,{
        title:'Choose again!'
        }, (error, blogspot) =>{
        console.log(error,blogspot)
    })
    ```

11. Find a blog post with given id and then delete it
    ```javascript
    var id = "5fe86cf496d64a155d69590b";
    BlogPost.findByIdAndDelete(id,{
        title:'A blog post title of rain!'
        }, (error, blogspot) =>{
        console.log(error,blogspot)
    })
    ```
12. Using full text search on all string fields, you need to create index for each field first.
    ```javascript
        const BlogPostSchema = new Schema({
                title: String,
                body: String
                });
        // create the indexes
        BlogPostSchema.index({title:"text",body:"text"})
    ```
    > Make sure to set **useCreateIndex:true** in your connection
    ```javascript
        mongoose.connect('mongodb://localhost/my_database',
        {useNewUrlParser: true,useUnifiedTopology: true,useCreateIndex:true }) 
    ```
    > Now on your find() use the **$text** operator, read more [here](https://docs.mongodb.com/manual/text-search/).
    ```javascript
        app.post('/posts/search',async (req,res) => { 
            const blogposts = await BlogPost.find(
                    {$text : {$search: req.body.search}},
                    {score: {$meta: "textScore"}}
                )
            res.render('index',{ blogposts })
        })
    ```
13. Receiving files from an html form
    > Install the express-fileupload package
    ```javascript
        npm install --save express-fileupload
    ```
    > Use the following code to initiate:
    ```javascript
        const fileUpload = require('express-fileupload') // adds the files property to the req object so that we can access the files using req.files
        app.use(fileUpload()) // register the package in Express
    ```
    > Usage of the file upload on a request
    ```javascript
        app.post('/posts/store', (req,res)=>{
            // create a shortcut to req.files.image
            // the req.files.image object contains properties such as mv
            // mv - is a function to move the file elsewhere on your server name
            // see complete list of properties of express-fileupload
            // https://www.npmjs.com/package/express-fileupload
            let image = req.files.image; 

            // the code below image.mv moves the uploaded file to public/img directory
            // and name it using the image.name property
            // once this is done, we create the post
            image.mv(path.resolve(__dirname,'public/img',image.name),
                // note the async statement, 
                // we only use async in the scope where we use await
                async (error) => {
                    console.log(error)
                    await BlogPost.create(req.body)
                    res.redirect('/')
                }
            )
        })
    ```
    > For the request example above to recieved the file from the html form you need to specify `enctype="multipart/form"` attributes in the form tag
    ```html
        <form action="/posts/store" method="POST" enctype="multipart/form-data">
        <!-- your forms -->
        </form>
    ```
14. Bonus tip when  writing routes in order [read here](https://www.reddit.com/r/node/comments/kxe4b7/hello_im_trying_to_learn_nodejs_by_following/gj9njj1?utm_source=share&utm_medium=web2x&context=3).