[![Build Status](https://travis-ci.org/phstc/putsreq.svg)](https://travis-ci.org/phstc/putsreq)

## PutsReq

PutsReq lets you record HTTP requests and simulate responses like no other tool available. [Try it now](http://putsreq.com)!

### Getting Started

Steps to run PutsReq in development.

#### Install MongoDB

```bash
brew install mongo

mongod
```

#### Start PutsReq

```
cd ~/workspace

git clone git@github.com:phstc/putsreq.git

cd putsreq

bundle install

rails s

open http://localhost:3000
```

### Response Builder

The Response Builder is the place where you can create your responses using JavaScript V8.

Here is the list of variables you can access to create your responses:

#### request

```javascript
// curl -X POST -H 'X-MyHeader: MyHeaderValue' -d 'name=Pablo' https://putsreq.com/123...

request.request_method;
// => POST

request.body;
// => name=Pablo

request.params.name;
// => Pablo

request.headers['HTTP_X_MYHEADER'];
//=> MyHeaderValue
```

JSON request:

```javascript
// curl -i -X POST -H 'Content-Type: application/json' -d '{"message":"Hello World"}' https://putsreq.com/123...

var parsedBody = JSON.parse(request.body);

parsedBody.message;
// => Hello World
```

#### response

```javascript
response.status  = 200;  // default value
response.headers = {};   // default value
response.body    = 'ok'; // default value
```

JSON response:

```javascript
response.headers['Content-Type'] = 'application/json';

response.body = { 'message': 'Hello World' };
```

#### forwardTo

If you only want to log your requests, you can use PutsReq as a proxy to forward them.

```javascript
request.forwardTo = 'http://example.com/api';
```

You can also modify the requests before forward them.

```javascript
// add or change a header
request.headers['X-MyNewHeader'] = 'MyHeaderValue'

var parsedBody = JSON.parse(request.body);

// add or change a value
parsedBody['my_new_key'] = 'my new value';

request.body = parsedBody;

request.forwardTo = 'http://example.com/api';
```

#### last_request & last_response

The last received request and response are also available in the Response Builder context through the variables `last_request` and `last_response`.

### Sample Integration Tests

https://github.com/phstc/putsreq_integration_sample
