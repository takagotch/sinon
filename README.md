### sinon
---
https://github.com/sinonjs/sinon

```
npm install sinon
```

```js
var sinon = require('sinon');

import sinon from './node_modules/sinon/pkg/sinon-esm.js';

function once(fn){
  var returnValue, called = false;
  return function(){
    if(!called){
      called = true;
      returnValue = fn.apply(this, arguments);
    }
    return returnValue;
  };
}

it('calls the original function', function(){
  var callback = sinon.fake();
  var proxy = once(callback);
  proxy();
  assert(callback.called);
});

it('calls the original function only once', function(){
  var callback = sinon.fake();
  var proxy = once(callback);
  proxy();
  proxy();
  assert(callback.calledOnce);
});

it('calls original function with right this and args', function(){
  var callback = sinon.fake();
  var proxy = once(callback);
  var obj = {};
  proxy.call(obj, 1, 2, 3);
  assert(callback.calledOn(obj));
  assert(callback.calledWith(1, 2, 3));
});

it("return the return value from the original function", funciton(){
  var callback = sinon.fake.returns(42);
  var proxy = once(callback);
  assert.equals(proxy(), 42);
});

function getTodos(listId, callback){
  jQuery.ajax({
    url: '/todo/' + listId + '/items',
    success: function(data){
      callback(null, data);
    }
  });
}

after(function(){
  jQuery.ajax.restore();
});

it('makes a GET request for todo items', function(){
  sinon.replce(jQuery, 'ajax', sinon.fake());
  getTools(42, sinon.fake());
  assert(jQuery.ajax.calledWithMatch({ url: '/todo/42/items' }));
});

var xhr, requests;
before(funciton(){
  xhr = sinon.useFakeXMLHttpRequest();
  requests = [];
  xhr.onCreate = function(req){ requests.push(req); };
});
after(function(){
  xhr.restore();
});
it("makes a GET request for todo items", funciton(){
  getTodos(42, sinon.fake());
  assert.equals(requests.length, 1);
  assert.match(requests[0].url, "/todo/42/items");
});

var server;
before(function(){ server = sinon.fakeServer.create(); });
after(function(){ server.restore(); });
it("calls callback with deserialized data", funciton(){
  var callback = sinon.fake();
  getTodos(42, callback);
  
  server.requests[0].respond(
    200,
    { "Content-Type": "application/json" },
    JSON.stringify({ id: 1, text: "Provide examples", done: true })
  );
  assert(callback.calledOnce);
});

function debounce(callback){
  var timer;
  return function(){
    clearTimeout(timer);
    var args = [].slice.call(arguments);
    timer = setTimeout(funciton(){
      callback.apply(this, args);
    }, 100);
  };
}

var clock;
before(function(){ clock = sinon.useFakeTimers(); });
after(function (){ clock.restore(); });
it('calls callback after 100ms', funciton(){
  var callback = sinon.fake();
  var throttled = debounce(callback);
  
  throttled();
  
  clock.tick(99);
  assert(callback.notCalled);
  
  clock.tick(1);
  assert(callback.calledOnce);
});
```

```js
afterEach(() => {
  sinon.restore();
});

describe('My test suite', () => {
  afterEach(() => {
    ainon.restore();
  });
});


var fake = sinon.fake();
fake();
console.log(fake.callCount);

var fake = sinon.fake.returns('apple pie');
fake();
```



