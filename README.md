node-jdbc
=========

JDBC Wrapper for node.js

Latest Version
--------------
0.0.7

## Status
[![Build Status](https://travis-ci.org/CraZySacX/node-jdbc.png)](https://travis-ci.org/CraZySacX/node-jdbc)
[![endorse](https://api.coderwall.com/crazysacx/endorsecount.png)](https://coderwall.com/crazysacx)

Initialize
----------
```javascript
var jdbc = require('jdbc');

var config = {
  libpath: __dirname + 'path/to/jdbc.jar',
  drivername: 'com.java.driverclass',
  url: 'url/to/database'
};

jdbc.initialize(config, function(err, res) {
  if (err) {
    console.log(err);
  }
});
```

Open Connection, Execute Queries, Close
---------------------------------------
```javascript
var genericQueryHandler = function(err, results) {
  if (err) {
    console.log(err);
  } else if (results) {
    console.log(results);
  }
};

jdbc.open(function(err, conn) {
  if (conn) {
    // SELECT statements are called with executeQuery
    jdbc.executeQuery("SELECT * FROM table", genericQueryHandler);

    // Table modifying statements (UPDATE/INSERT/DELETE/etc) are called with executeUpdate
    jdbc.executeUpdate("UPDATE table SET column = value", genericQueryHandler);

    // Use non-generic callbacks to handle queries individually and/or to nest queries
    jdbc.executeUpdate("INSERT INTO table VALUES (value)", function(err, results) {
      
      if(results > some_arbitrary_value) {
        jdbc.executeQuery("SELECT * FROM table where column = value", genericQueryHandler);
      }
    
    });
  }
});

jdbc.close();
```

API
---------------------------------

### initialize(config, callback)
 - see above example for config object
 - callback(error)

### open(config, callback)
 - opens a new connection
 - callback(error)

### close(config, callback)
 - closes any existing connection
 - callback(error)

### executeQuery(sql, callback)
 - SELECT commands.
 - callback(error, rset)

### executeUpdate(sql, callback) 
 - table modifying commands (INSERT, UPDATE, DELETE, etc).
 - callback(error, num_rows)
   - where @num_rows is the number of rows modified
