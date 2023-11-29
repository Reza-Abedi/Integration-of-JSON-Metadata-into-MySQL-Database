# Integration of JSON Metadata into MySQL Database

This process efficiently extracts movie metadata from a JSON file and systematically enters it into a MySQL database. Leveraging modules for file system operations and database interactions, the code establishes a connection, iterates through each movie, and logs details while using a utility function for SQL queries. This systematic approach ensures the structured integration of comprehensive movie metadata into the MySQL database.

## Detailed Code Breakdown and Execution Steps
For a more in-depth understanding of the code implementation and the step-by-step execution process, please refer to the accompanying document titled "Code Breakdown and Execution Steps". 
https://docs.google.com/document/d/1VSjIBcgoU1WZ0VphFPl4l9jbBWwzNTYoyLfQPvzZGNE/edit?usp=sharing
This document provides a comprehensive breakdown of the source code, elucidating each key component and explaining the sequential execution flow.




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







