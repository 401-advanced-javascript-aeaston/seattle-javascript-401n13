# Demos: Data Modeling

## Tests (`demo/models/__tests__/teams.test.js`)
* Add Record - Adds one and sees the return value
* Get Record - Adds one, fetches it, and then compares the IDs to be the same
* This can require either the "monolithic" model or the "modular" one and still work the same.
  * Refactoring FTW

### Monolithic  (`demo/models/teams-model.monolithic.js`)
* Approach
  * Implements all of the CRUD methods using an in-memory database array
  * Schema describes the data shape
  * `sanitize()` method validates incoming data against the schema and rule
  * We have ample places to put business logic
    * Such As ...  Can the students provide some ideas?
* Issuses
  * How extensible is this approach?
  * How dry is this?
  * How can we improve it?

### Modular  (`demo/models/memory,js` + `demo/models/teams-model.js`)
* `memory.js`
  * Same approach as with the monolithic file
  * But where did the schema go?
* `teams-model.js`
  * Schema is in the constructor
  * Note the use of `super()` to get the original functionality
* Issuses
  * How extensible is this approach?
  * How dry is this?
  * Can you identify how this could work for SQL or other Data Stores?


## Refactored Tests (`demo/models/__tests__/teams.test.refactored.js`)
* Now that we have 2 different implementations, how can we rewire our tests to run them both?
* Introduce looping over implementations and dynamically creating instances!
* How does this help us moving forward?

Today, we're just doing an in-memory demo. The lab will be to implement this same idea using the File System (`file-model.js` at the root of the `models` folder) and this leads the way to other data stores (mongo, pg, etc)

* Can the students identify the interface pattern (the API )?
  * Same methods
  * Same arguments
  * Same return values
  * Different underlying implementations that other code need not care about
