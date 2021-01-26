# Middleware with Express JS


_**Middleware**_ for Express are functions that are executed in the middle after the incoming request. These functions produces output that can either be the final output for that request or an output that can be use in the next Middleware to be executed. Throughout the executions of Middlewares, they can affect the request and response object.

> Some examples of Middleware with Express JS:
```Javascript
    // Exposing the 'public' directory
    app.use(express.static('public')) 
    // Parse incoming request, it will make the form data available under the req body property of each request
    app.use(bodyParse.json()) 
    // It parses incoming requests with urlencoded payloads
    app.use(bodyParser.urlencoded({extended:true})) 
```
> The _**use**_ function in express registers Middleware with the ExpressJS app. When a request is made in an ExpressJS app, all _**use**_ statement within the app will be executed in the order they were written before handling the request.

Another example of a Middleware that is very useful is `app.use(fileUpload())`. The _fileUpload_ Middleware handles the request object and extends it with the request.file property. Not using it would need alot more code when retrieving file uploads.


### **Custom Middleware** ### 
***
Creating your custom Middleware is possible with ExpressJS. See example code:
```javascript
    // define the middleware
    const customMW = function(req,res,next) {
        console.log(' -- Your custom Middleware is loaded! -- ')
        next()
    }

    // execute the middleware in the app when a request was received.
    // you will get the -- Your custom Middleware is loaded! -- in your app console everytime a request is made
    app.use(customMW)
```
> Good to note: the `next()` syntax in the Middleware tells ExpressJS that your Middleware is done executing and it should call the _next_ middleware in line. When `next()` is remove from your middleware method, ExpressJS will hang as it does know that your middleware is done and it can proceed. The `next()` function is called by all middleware that are loaded by `app.use`