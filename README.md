# claudiaExpress
Express like middleware support for claudia API builder. The module is designed for developers who come from the express framework but want to try out AWS Lambda. ClaudiaJS with Claudia API builder is an easy way to start.

####Features:
- middleware support
- 4 param error handler middlewares
- tree like routing

####Install:
```
npm install claudiaexpress
```
###Usage:
Only the middlewares part:
```
const ApiBuilder = require('claudia-api-builder');
const api = new ApiBuilder();
const middlewares = require('claudiaexpress').Middlewares;

api.get('/hello', middlewares((req, res, next) => {
  res.data = 'hello claudiaExpress';
  next();
  }, (req, res, next) => {
  res.end(res.data);
  }))
```
Router:
```
index.js

const ApiBuilder = require('claudia-api-builder');
const api = new ApiBuilder();
const Router = require('claudiaexpress').Router;
const router = Router(api);

const routes = require('./routes.js');

router.use(routes);

module.exports = router;

routes.js
const Router = require('claudiaexpress').Router;
const router = Router();

router.get('/hello', (req, res, next) => {
    res.end('Hello World');
  });
```
###Middleware parameters:
req - the request object provided by claudia API builder

res - object containing end(data) and fail(err) functions
: -end will send an ok (200) response with the data object
: -fail will send a bad request (400) response with the err !stringified!
: -error codes can be configured at the API gateway, with using regexps like `.*\[404\].*`
```
...
return res.fail('[404] Resource not found');
// this will send an error response with status 404
...
```
next - will call the next middleware when called without parameter, will call error handler when called with parameter.

[err] - error object used in the error handler middlewares.

###Testing:
`npm test`
Runs all the mocha tests in the `./test` folder.

###TODOS:
- !upgrade router.use() to concat path!
- upgrade response object to be more express like

Developer:
Daniel Vincze
Coding Sans Zrt.
http://codingsans.com/
