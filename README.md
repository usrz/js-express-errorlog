Express Error Log
=================

A very simple logger for [Express](http://expressjs.com/) 4.x.

* [Install and use](#install-and-use)
* [Configuration](#configuration)
* [Sample Output](#sample-output)
* [License (MIT)](#license-mit-)

Install and use
---------------

Install as usual with _NPM_:

```bash
npm install --save express-errorlog
```

Then configure as the last route of your Express app:

```javascript
var errorlog = require('express-errorlog');
app.use(errorlog);
```

In the code, various patterns can be used:

### Direct response statuses

```javascript
app.route('/somewhere', function(req, res, next) {
  next(404); // Simply interrupt with a 404 not found
});
```

### Status, message and details

```javascript
app.route('/somewhere', function(req, res, next) {
  next({
    status: 404,
    message: 'This will override the message',
    details: { // This will appear in the response
      key: 'value',
      any: [ 'sort', 'of', stuff ]
    }
  });
});
```

### Errors to be logged only

```javascript
app.route('/somewhere', function(req, res, next) {
  next({
    status: 404,
    message: 'This will override the message',
    details: { // This will appear in the response
      key: 'value',
      any: [ 'sort', 'of', stuff ]
    },
    error: new Error("This will not be transmitted to the client")
  });
});
```

Configuration
=============

```javascript
var errorlog = require('express-errorlog');
app.use(errorlog({
  logger: function/stream,
  render: true/false
}));
```

* `logger`: A `function` receiving the message to be logged or a `stream` that
  will be written to with the date and error message.
* `render`: A _boolean_, if `true` the response will be sent to the client
  using Express' own `render(...)` function.

  For log rotation use something similar to [`logrotate-stream`](https://www.npmjs.com/package/logrotate-stream).

Sample Output
=============

Here is some sample output derived from the tests:

```text
2015-03-30T16:45:01.661Z - GET /test-1 (400) - Bad Request
2015-03-30T16:45:01.682Z - GET /test-2 (499) - Unknown status 499
2015-03-30T16:45:01.689Z - GET /test-3 (500) - Unknown status 999
2015-03-30T16:45:01.694Z - GET /test-4 (401) - message for test-4
2015-03-30T16:45:01.699Z - GET /test-5 (402) - Payment Required
  >>> { testname: 'test-5' }
2015-03-30T16:45:01.704Z - GET /test-6 (403) - message for test-6
  >>> { testname: 'test-6' }
2015-03-30T16:45:01.707Z - GET /test-7 (500) - message for test-7
  >>> { testname: 'test-7' }
2015-03-30T16:45:01.711Z - GET /test-8 (500) - exception message for test-8
  >>> [Error: exception message for test-8]
  Error: exception message for test-8
    at /Users/pier/Workspace/js-express-errorlog/test/test.js:13:57
    at Layer.handle [as handle_request] (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/layer.js:82:5)
    at next (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/route.js:110:13)
    at Route.dispatch (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/route.js:91:3)
    at Layer.handle [as handle_request] (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/layer.js:82:5)
    at /Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:267:22
    at Function.proto.process_params (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:321:12)
    at next (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:261:10)
    at expressInit (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/middleware/init.js:23:5)
    at Layer.handle [as handle_request] (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/layer.js:82:5)
2015-03-30T16:45:01.715Z - GET /test-9 (410) - exception message for test-9
  >>> { [Error: exception message for test-9]
  status: 410,
  details: { testname: 'test-9' },
  extra: 'this only gets logged!' }
  Error: exception message for test-9
    at Error (native)
    at /Users/pier/Workspace/js-express-errorlog/test/test.js:16:15
    at Layer.handle [as handle_request] (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/layer.js:82:5)
    at next (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/route.js:110:13)
    at Route.dispatch (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/route.js:91:3)
    at Layer.handle [as handle_request] (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/layer.js:82:5)
    at /Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:267:22
    at Function.proto.process_params (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:321:12)
    at next (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:261:10)
    at expressInit (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/middleware/init.js:23:5)
2015-03-30T16:45:01.718Z - GET /test-0 (411) - message for test-0
  >>> { testname: 'test-0' }
  >>> { [Error: exception message for test-0]
  more1: 'some more details for test 0',
  more2: 'even more details for test 0' }
  Error: exception message for test-0
    at Error (native)
    at /Users/pier/Workspace/js-express-errorlog/test/test.js:24:15
    at Layer.handle [as handle_request] (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/layer.js:82:5)
    at next (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/route.js:110:13)
    at Route.dispatch (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/route.js:91:3)
    at Layer.handle [as handle_request] (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/layer.js:82:5)
    at /Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:267:22
    at Function.proto.process_params (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:321:12)
    at next (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/router/index.js:261:10)
    at expressInit (/Users/pier/Workspace/js-express-errorlog/node_modules/express/lib/middleware/init.js:23:5)
```

License (MIT)
=============

Copyright (c) 2015 Pier Fumagalli and USRZ.com

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

