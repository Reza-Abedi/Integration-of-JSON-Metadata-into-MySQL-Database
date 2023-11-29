# Integration of JSON Metadata into MySQL Database

This process efficiently extracts movie metadata from a JSON file and systematically enters it into a MySQL database. Leveraging modules for file system operations and database interactions, the code establishes a connection, iterates through each movie, and logs details while using a utility function for SQL queries. This systematic approach ensures the structured integration of comprehensive movie metadata into the MySQL database.

```javascript
// Import the file system module (fs)
import fs from 'fs';

// Import the database driver
import mysql from 'mysql2/promise';

// Read the json string from file
let json = fs.readFileSync('./movie-data.json', 'utf-8');

// Convert from a string to a real data structure
let data = JSON.parse(json);

// Connection 'db' to the database
const db = await mysql.createConnection({
  // CHANGE TO 127.0.0.1 IF YOU WANT TO RUN LOCAL DB
  host: 'your_database_host',
  port: 'your_port_number',
  user: 'your_database_user',
  password: 'your_database_password',
  database: 'your-database-name'
});


// A small function for a query
async function query(sql, listOfValues) {
  let result = await db.execute(sql, listOfValues);
  return result[0];
}
for (let movie of data) {
  console.log('A MOVIE:');
  console.log(movie);
  // INSERT TO DATABASE
  let result = await query(`
    INSERT INTO movies (title, description)
    VALUES(?, ?)
  `, [movie.title, movie.description]);
  console.log(result);
}


##Break down the code and its steps in more detail
###Step1: Import Modules:
The process begins with importing necessary modules. In this case, the code imports the 'fs' module for interacting with the file system and the 'mysql2/promise' module for handling MySQL database operations.

```javascript
import fs from 'fs';
import mysql from 'mysql2/promise';


###Step2: Read Movie Data:
The code reads the content of the 'movie-data.json' file, which contains details about various movies. The information is stored as a JSON-formatted string.

```javascript
let json = fs.readFileSync('./movie-data.json', 'utf-8');
let data = JSON.parse(json);


###Step3: Database Connection Setup:
A connection to the MySQL database is established using the 'mysql2/promise' module. The connection details include the host, port, user, password, and the specific database ('movies').

```javascript
// Connection 'db' to the database
const db = await mysql.createConnection({
  // CHANGE TO 127.0.0.1 IF YOU WANT TO RUN LOCAL DB
  host: 'your_database_host',
  port: 'your_port_number',
  user: 'your_database_user',
  password: 'your_database_password',
  database: 'your-database-name'
});


###Step4: Query Function:
To interact with the database, a small utility function named query  is defined. This function takes a SQL query and a list of values as parameters, executes the query, and returns the result.

```javascript
async function query(sql, listOfValues) {
  let result = await db.execute(sql, listOfValues);
  return result[0];
}

###Step5: Iterate Over Movie Data:
The code then iterates through each movie in the data array, which was populated from the 'movie-data.json' file.
```javascript
for (let movie of data) {


###Step6: Log Movie Details:
For each movie, the code logs details to the console, indicating that a movie is being processed.
```javascript
console.log('A MOVIE:');
console.log(movie);


###Step7: Insert Data into Database:
Then uses the query function to insert the movie's title and description into the movies  table in the database.
```javascript
  let result = await query(`
    INSERT INTO movies (title, description)
    VALUES(?, ?)
  `, [movie.title, movie.description]);


###Step8: Log Result:
The result of the database insertion is logged to the console, providing feedback on the success of the operation.
```javascript
  console.log(result);


###Step9: End of Iteration:
The process repeats for each movie in the 'data' array.
```javascript
}






