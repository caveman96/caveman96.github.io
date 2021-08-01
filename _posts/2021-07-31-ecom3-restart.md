---
title: E-COM 3- Regret, repent, and restart
author: Silesh Chandran
date: 2021-07-30 20:10:00 +0530
categories: [backend, projects]
tags: [backend, node, spring, rest, mongodb, projects, javascript, express]
toc: true
comments: true
---

I might have bitten of a lot more than I can chew by deciding to use MongoDB with Java. I have made the difficult decision to restart this project and build the backend in Node instead. I will elaborate on my reasoning later, but for this post, we will catch up to last post but this time building a server using Express.js and using mongoDB with Mongoose.

## Why Node and not Java?
While investigating how to insert a data into an array in a document, I realized doing it in java wasnt as straightforward as I exprected thought it would be. This was mostly because i was using spring-data-mongodb which is a wrapper written on top of the official mongoDb driver. Doing operations like this is javascript is much simpler and I started wondering if there was any benefit to sticking with Java for this project.

For an e-commerce application like the one we are building, the most important thing is being able to service customers without hiccups. There is very little if any processing we are doing in the backend, so essentialy our server exists just to facilitate the user interaction with the DB. For this purpose I believe Node is vastly superior to a java server. The non blocking nature of javascript, and incidentaly Node, ensures that every request goes through and is serviced optimally.

The same can't be said for java as we are limited by the number of threads that can be spawned by spring for each incoming request. And since network latency when reading from DB is going to be our bottleneck, there are no benefits that I can see to sticking with Spring and Java.

This is my reasoning, but I could be wrong. Feel free to correct me in the comments.

## Catching up

### Getting started

We will be using NodeJs, which is a backend javascript runtime, that allows executing javascript out of a browser, and Express, which is a web framework that will quickly allow us to create APIs. We will also make of npm, which is similar to Maven in java in the way that it allows us to manage dependancies. A version manager makes managing and installing these much easier so lets use nvm as well.

First download and install nvm ([**windows**](https://github.com/coreybutler/nvm-windows) [**mac/linux**](https://github.com/nvm-sh/nvm) )
then run:
```shell
nvm install 14.17.3
nvm use 14.17.3
```
to install the current version of Node and npm.
Now open the folder where you want to create the backend and run
```shell
npm init
```
this will create the package.json required for npm and dependancies. The default values for the fields can be used. We will also add the following dependancies:
1. Express - lightweight web framework
2. Cors - cross origin resource policy, to allow ajax requests to skip cors and access resources from same hosts. This allows allows configuring cors settings.
3. Mongoose - a utilty for simplifying interaction with mongoDB from Node.
4. dotenv - used to load environment variables from a file.

```shell
npm install express cors mongoose dotenv
```

 And that should be all for setting up. 

### Server code

Create a file called server.js in the new folder. This is how we will be starting our node server. I have explained what each line does in the code snippet below that we write in our server.js.

```javascript
//loading express and cors
const express = require('express');
const cors = require('cors');

//this will load environment variables into the process variable
require('dotenv').config();

//creating the express app
const app = express();
const port = process.env.PORT || 5000;

app.use(cors());
//since we will be sending and recieving data in json format
app.use(express.json());

//starting the server
app.listen(port, ()=>{
	console.log(`server is running on port: ${port}`);
});
```
We can also connect to our MongoDB by specifiying the following before starting the server:
```javascript
//loading mongoose and the mongoDB uri
const mongoose = require(mongoose);
const uri = process.env.ATLAS_URI;
//connecting to mongodb
mongoose.connect(uri, {useNewUrlParser: true, useCreateIndex: true});
const connection = mongoose.connection;
//logging once connection creation has happened
connection.once('open', () => {
    console.log("MongoDB connection established successfully");
})
```

To pass in the Atlas uri, we can create a .env file in the same folder and add:
```
ATLAS_URI=mongodb+srv://<username>:<password>@cluster0.sqirz.mongodb.net/myFirstDatabase?retryWrites=true&w=majority
```

This server can be started by running:
```
node server.js
```
Incidentaly we can make use of a tool called nodemon, which allows you to hot reload the server automatically after making changes.:
```
npm install -g nodemon
nodemon server.js
```
It should print both the console logs we have added.

### Hello world api
We can quickly verify if our server is working by creating a simple api:
```javascript
app.get("/", function(req,res){
    res.send("hello");
});
```
this api can be accessed on localhost:5000/

Lets also take a look at how to create a slightly more complex api, one that reads contents of a file as a json and returns it:

```javascript
//readfile is a promise method that will eventually resolve into the file contents after it reads them
const readFile = require('fs/promises').readFile;

//create an async function
const getapi = async function(req, res) {
    try {
		//by using await we wait for the readfile to resolve, and since our method is async, this is still non blocking
        const response = await readFile("products.json", "utf-8");
		//parse and send the response as a json
        res.json(JSON.parse(response));
    } catch(ex){
		//in case of exception 
        res.json(ex);
    }
}
//adding a get endpoint which calls getapi method
app.get("/test", getapi);
```

### Creating models
Now similar to how we created objects in java, we will create schemas for mongoose to use. I'm creating 2 models, one for the product and the other for the user, adding them under a models folder:

#### Product.model.js
```javascript
const mongoose = require('mongoose');

const Schema = mongoose.Schema;

//we have to specify the type of each object in db, otherwise mongoDB will automatically type it
const productSchema = Schema({
    name: {type: String, required: true},
    description: {type: String, required: true},
    id: {type: Schema.Types.ObjectId, required: true, unique: true, minlength: 10},
    price: {type: Schema.Types.Decimal128, required: true},
    stock: {type: Number}
});

//create the model
const Product = mongoose.model('Product', productSchema);
//export it so that it can be used by other files
module.exports = Product;
```

#### User.model.js
```javascript
const mongoose = require('mongoose');

const Schema = mongoose.Schema;

const userSchema = new Schema({
    username: {  type: Schema.Types.ObjectId, required: true, unique: true, trim: true, minlength: 6 },
    email : {type: String, required: true},

}, {
    timestamps: true,
});

const User = mongoose.model('User', userSchema);
module.Exports = User;
```

### Adding APIs and Routing for CRUD
The final step before we can add and maniputlate data is to expose the crud operation out via APIs. We will first create the api endpoints needed for both the User and Product models. We can make use of an express Router to use these in out server.js file.

#### User.js
```javascript
const router = require('express').Router();
let User = require('../models/user.model');
const mongoose = require('mongoose');


//main path /users is configured in server.js

router.route('/').get((req, res) => {
    //execute find all users and return response as json if no error
    User.find()
        .then(users=> res.json(users))
        .catch(err=> res.status(400).json('Error: ' + err));
});

router.route('/').post((req, res) => {
    //get name and email from payload
    ({name, email} = req.body);
    //create new user object
    const newUser = new User({"name": name, "email": email});
    //save new user object
    newUser.save()
        .then(() => res.json('user added successfully'))
        .catch((err)=> res.status(400).json('error: ' + err));

});

//get user by id
router.route('/:email').get((req, res) => {
	//get email from url param and pass that to the find method
	User.find({"email": req.params.email})
		.then(users=>res.json(users))
		.catch(err=> res.status(400).json('Error: ' + err));
});

//update user by id
router.route('/:email').post((req, res) => {
    ({name, email} = req.body);
	//get email from url param and pass create a query object
    var myquery = { email: req.params.email };

	User.updateOne(myquery, {"name": name, "email": email})
    .then(()=>res.json("success"))
    .catch(err=> res.status(400).json('Error: '+ err));
});

//delete a user by id
router.route('/:email').delete((req,res) => {
	//get email from url param and pass to the deleteone method
	User.deleteOne({"email": req.params.email}, function (err) {
		if (err) res.status(400).json('Error: '+ err);
		// deleted at most one document
		})
		.then(()=>res.json("success"));
});

module.exports = router;
```

We have added 5 api endpoints (one each for get, get by id, post, update and delete). The same can be added for Products as well. We have used the ```find``` method to get values, ```updateOne``` to update the first document that matches the query and ```deleteOne``` to do the delete for the same. 
 
## Registering routers in Server.js

Now that we have 2 routes, we need to add these with a corresponding path in server.js. Whenever the url follows that path, the corresponding route is used and the request is executed from there. So add the following as well to server.js and start it for testing. 

```javascript
//load routes
const userRouter = require('./routes/users');
const productRouter = require('./routes/products');

//use the routes to redirect these endpoints to the respective files
app.use('/products', productRouter);
app.use('/users', userRouter);
```

These Apis can now be tested using postman, a collection with all the 10 apis can be found in the [**github page**](https://github.com/caveman96/valdivian). You can also see the data as it is modified in MongoDB Atlas dashboard.

## Whats next?
In the next post we will go over some complex queries as well as add a cart functionality so that users can add products to their cart. Thanks for reading!