# Database-js
* [API](//github.com/mlaanderson/database-js/wiki/API)
* [Examples](//github.com/mlaanderson/database-js/wiki/Examples)
* [Drivers](//github.com/mlaanderson/database-js/wiki/Drivers)
* [In the Browser](//github.com/mlaanderson/database-js/wiki/Browsers)

# About
Database-js was started to implement a common, promise based interface for SQL database access. The concept is to copy the Java pattern of using connection strings to identify the driver. Then provide wrappers around the implemented functionality to commonize the syntax and results.

Thus if SQLite, MySQL and PostgreSQL all have a database named test with a table named states we can access the data the same way.

Since database-js is still being held onto by NPM, the package name is database-js2 in NPM.

Database-js has built-in prepared statements, even if the underlying driver does not support them. It is built on Promises, so it works well with ES7 async code. 

## SQLite
~~~~
var Connection = require('database-js2').Connection;

var conn = new Connection("database-js-sqlite:///test.sqlite");

var statement = conn.prepareStatement("SELECT * FROM states WHERE state = ?");
statement.query("South Dakota").then((results) => {
    console.log(results);
    conn.close().then(() => {
        process.exit(0);
    }).catch((reason) => {
        console.log(reason);
        process.exit(1);
    });
}).catch((reason) => {
    console.log(reaons);
    conn.close().then(() => {
        process.exit(0);
    }).catch((reason) => {
        console.log(reason);
        process.exit(1);
    });
});
~~~~

## MySQL
~~~~
var Connection = require('database-js2').Connection;

var conn = new Connection("database-js-mysql://user:password@localhost/test");

var statement = conn.prepareStatement("SELECT * FROM states WHERE state = ?");
statement.query("South Dakota").then((results) => {
    console.log(results);
    conn.close().then(() => {
        process.exit(0);
    }).catch((reason) => {
        console.log(reason);
        process.exit(1);
    });
}).catch((reason) => {
    console.log(reaons);
    conn.close().then(() => {
        process.exit(0);
    }).catch((reason) => {
        console.log(reason);
        process.exit(1);
    });
});
~~~~

## PostgreSQL
~~~~
var Connection = require('database-js2').Connection;

var conn = new Connection("database-js-postgres://user:password@localhost/test");

var statement = conn.prepareStatement("SELECT * FROM states WHERE state = ?");
statement.query("South Dakota").then((results) => {
    console.log(results);
    conn.close().then(() => {
        process.exit(0);
    }).catch((reason) => {
        console.log(reason);
        process.exit(1);
    });
}).catch((reason) => {
    console.log(reaons);
    conn.close().then(() => {
        process.exit(0);
    }).catch((reason) => {
        console.log(reason);
        process.exit(1);
    });
});
~~~~

Notice that in all three examples, the only difference is the connection URL.

# ES7 Compatibility: async
Because database-js is built on Promises, it works very well with ES7 async functions. Compare the following ES7 code to the SQLite code from above. They accomplish the same thing.
~~~~
var Connection = require('database-js2').Connection;

(async function() {
    let conn, statement, results;
    
    try {
        conn = new Connection("database-js-sqlite:///test.sqlite");
        statement = conn.prepareStatement("SELECT * FROM states WHERE state = ?");
        results = await statement.query("South Dakota");
        console.log(results);
    } catch (reason) {
        console.log(reason);
    } finally {
        await conn.close();
        process.exit(0);
    }
})();
~~~~