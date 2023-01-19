`USING POSTMAN TO TEST THE APIS`


`Run the server type`
    npm init
    node api.js
    nodemon api

`1. Install packages:`

    npm install pg --save
    npm install express --save
    npm install body-parser --save
    npm install nodemon --save

`2. Create Database Connection:`

Create a file called **connection.js**

This file would hold the connection data as shown below:

```js
const { Client } = require("pg");

const client = new Client({
  host: "localhost",
  user: "postgres",
  port: 5432,
  password: "sa",
  database: "postgres",
});

module.exports = client;
```

`3. Create the Server and Client`

Node.js allows us to create a server. Now we need to create a second file.

Here I call it **api.js**

Write the following code inside. This code creates a server listening at **port 3300.** Then a client is create as well that connects to the server.

```js
const client = require("./connection.js");
const express = require("express");
const app = express();

app.listen(3300, () => {
  console.log("Sever is now listening at port 3000");
});

client.connect();
```

Add the BodyParser: This is used to handle conversion to and from json.

```js
const bodyParser = require("body-parser");
app.use(bodyParser.json());
```

`4. Get All Users`

for GET requests, we use **app.get() function**. This function takes two parameters: the route /users and a callback. The callback is an arrow function that executes when a request is received. The callback take two parameter: request and response. Inside the callback, we use the client to query the database and then send the result back.

```js
app.get("/users", (req, res) => {
  client.query(`Select * from users`, (err, result) => {
    if (!err) {
      res.send(result.rows);
    }
  });
  client.end;
});
```
`5. Get User By Id`

```js
// TODO: Get user by id

app.get("/users/:id", (req, res) => {
  client.query(
    `Select * from users where id=${req.params.id}`,
    (err, result) => {
      if (!err) {
        res.send(result.rows);
      }
    }
  );
  client.end;
});
```
`6. Add New User`

```js
// TODO: Add new user:

app.post('/users', (req, res)=> {
    const user = req.body;
    let insertQuery = `insert into users(id, firstname, lastname, location) 
                       values(${user.id}, '${user.firstname}','${user.lastname}', '${user.location}')`

    client.query(insertQuery, (err, result)=>{
        if(!err){
            res.send('Insertion was successful')
        }
        else{ console.log(err.message) }
    })
    client.end;
})
```

`7. Update User Details`:

```js
//TODO: update all user data

app.put('/users/:id', (req, res)=> {
    let user = req.body;
    let updateQuery = `update users
                       set firstname = '${user.firstname}',
                       lastname = '${user.lastname}',
                       location = '${user.location}'
                       where id = ${user.id}`

    client.query(updateQuery, (err, result)=>{
        if(!err){
            res.send('Update was successful')
        }
        else{ console.log(err.message) }
    })
    client.end;
})
```

`8. Delete Data`:

```js
// TODO: Delete the data 
app.delete('/users/:id', (req, res)=> {
    let insertQuery = `delete from users where id=${req.params.id}`

    client.query(insertQuery, (err, result)=>{
        if(!err){
            res.send('Deletion was successful')
        }
        else{ console.log(err.message) }
    })
    client.end;
})
```
