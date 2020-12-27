1.  require mongoose
`const mongoose = require('mongoose')`

2.  > import the blogpost model
> with the correct path
`const BlogPost = require('./models/BlogPost')`

> connect to the db
> note: if my_database is not created
> it will automatically be created
```
mongoose.connect('mongodb://localhost/my_database',
{
    useNewUrlParser: true,
    useUnifiedTopology: true 
})
```

> declare a route to home(index) or root
> this is to list all objects for the page
> in this case blogposts
```
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

> create our first post
> with the create function
> accepts 2 arguments
> 1st arg: data we weant to save 
> 2nd arg: call back function
```
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

> get all documents of 'BlogPost'
```
BlogPost.find({},(error,blogpost) => {
    console.log(' >>>> Get all blogpost!')
    console.log(error,blogpost)
})
```

> find a document with the specific title
```
BlogPost.find({
    title:'This is a sample Title'
},(error,blogpost) => {
    console.log(' >>>> find blogpost with title:' + blogpost.title)
    console.log(error,blogpost)
})
```

> find a document that contains 'original' in the title
```
BlogPost.find({
    title:/Original/
},(error,blogpost) => {
    console.log(' >>>> find blogpost with title that contains original')
    console.log(error,blogpost)
})
```

> find a blogpost with specific id
```
var id = "5fe86d4483b6b3159425a285";
BlogPost.findById(id,(error,blogpost)=>{console.log(error,blogpost)})
```

> find a blogspot by id
> and then update the title
```
var id = "5fe86a731e52831481e4b0dd";
BlogPost.findByIdAndUpdate(id,{
    title:'Choose again!'
    }, (error, blogspot) =>{
    console.log(error,blogspot)
})
```

> find a blog post with given id
> and then delete it
```
var id = "5fe86cf496d64a155d69590b";
BlogPost.findByIdAndDelete(id,{
    title:'A blog post title of rain!'
    }, (error, blogspot) =>{
    console.log(error,blogspot)
})
```