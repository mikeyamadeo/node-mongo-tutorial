# node-mongo-tutorial
We will be building off of serving data from a server using node and introduce how we can persist data with a database using mongodb.

We will cover:

1. How to connect to mongodb
2. How to insert data into mongodb
3. How to retrieve data from mongodb

Before we do this, let's get up and running.


## Pre-reqs

- Node w/ npm must be installed. If that is not the case, learn how [here](https://nodejs.org/en/)
- MongoDB must be installed. If that is not the case, learn how [here](https://docs.mongodb.org/manual/installation/)

## Up and Running

##### 1. Clone repo
Make sure to clone this repository

##### 2. Run the server
We can make sure our server is running by executing the following in the terminal

`node api-server.js`

If we go to port /index.html on port 4000 we should see a red screen instructing us to set up our endpoint. If you are using your local machine you would go to [http://localhost:4000/index.html](http://localhost:4000/index.html)

`git clone https://github.com/mikeyamadeo/node-mongo-tutorial.git`

##### 3. Start MongoDB
If mongodb is installed, we should run the following in the terminal to get mongodb running

`mongod`

## Connecting to MongoDB

##### 1. Get access to MongoDB in our javascript
We will need to use npm here to install the javascript library that helps us communicate with mongodb. npm is a package manager that allows us to retrieve javascript code from internet and download it to our projects using the terminal. The way we can install the javascript library for mongodb:

making sure you are in the root directory of the application, execute the following in your terminal

`npm install mongodb`

What should of happened is a `node_modules` directory should have showed up and inside you should see a `mongodb` directory.

Now, in our `api-server.js` file, right below the comment that reads `var mongodb = require('mongodb');`, let's include the following line of code:

```js
var mongodb = require('mongodb');
```

What this does is instruct node to look for a library named `mongodb`. After looking within node's library of modules and not finding one, it will be smart enough to look in our `node_modules` directory and find it there.

##### 2.

Next we will open a connection to mongodb using our snazzy new javascript library. Feel free to paste the following right underneath where we declare the `mongodb` variable.

```js
// We need to work with "MongoClient" interface in order to connect to a mongodb server.
var MongoClient = mongodb.MongoClient;

// Connection URL. This is where your mongodb server is running.
var dbUrl = 'mongodb://localhost:27017/pokemon';

// we will use this variable later to insert and retrieve a "collection" of data
var collection

// Use connect method to connect to the Server
MongoClient.connect(dbUrl, function (err, db) {
  if (err) {
    console.log('Unable to connect to the mongoDB server. Error:', err);
  } else {
    // HURRAY!! We are connected. :)
    console.log('Connection established to', dbUrl);
    
    /**
     * TODO: insert data here, once we've successfully connected
     */
  }
});

```

Go ahead and run `node api-server.js` in your terminal again, and you should expect to see the `connection established` log.

## Inserting data

##### 1. Make a 'pokemon' collection
Let's put that collection variable we already declared to work. Notice that in the callback funciont we provided to `MongoClient.connect`, we expect to receive a `db` variable. We will use that to create a `pokemon` collection like so:

```js
collection = db.collection('pokemon');
```

Now we will use that collection object to insert the array `pokemon` like so:

```js
// do some work here with the database.
collection = db.collection('pokemon');

collection.insert(pokemon, function (err, result) {
  if (err) {
    console.log(err);
  } else {
    console.log('Inserted %d documents into the "pokemon" collection. The documents inserted with "_id" are:', result.length, result);
  }
  
  // Close connection
  db.close()
}
```

All said and done our code connecting to the db and inserting our array of pokemon into the database should look something like the following:

```js
// Use connect method to connect to the Server
MongoClient.connect(dbUrl, function (err, db) {
  if (err) {
    console.log('Unable to connect to the mongoDB server. Error:', err);
  } else {
    // HURRAY!! We are connected. :)
    console.log('Connection established to', dbUrl);

    // do some work here with the database.
    collection = db.collection('pokemon');
    collection.insert(pokemon, function (err, result) {
      if (err) {
        console.log(err);
      } else {
        console.log('Inserted %d documents into the "pokemon" collection. The documents inserted with "_id" are:', result.length, result);
      }

      // Close connection
      db.close();
    })
  }
});
```

If we run `node api-server.js` we should expect a log telling us we have an established connection, and a log informing us we've inserted documents.

## Retrieving data

