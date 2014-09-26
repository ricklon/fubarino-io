# John-Five compatiblity :-)
[Jounny-Five](https://github.com/rwaldron/johnny-five)

## Load firmata with chipKIT code
* Use MPIDE to load standardfirmata sketch to the Fubarino board

## create a new node.js project
* create the package.json file
```
{
  "name": "fubarino-io-examples",
  "version": "3.0.0",
  "description": "Provides a standard interface to boards capable of IO (e.g. chipKIT, Arduinos, Maestros, Raspberry Pis, etc) and their firmware",
  "main": "lib/ioboard.js",
  "repository": {
    "type": "git",
    "url": "git://github.com/ricklon/fubarino-io.git"
  },
  "dependencies": {
    "grunt": "~0.4.5",
    "grunt-contrib-jshint": "~0.10.0",
    "grunt-contrib-nodeunit": "~0.4.1",
    "grunt-contrib-uglify": "~0.5.0",
    "johnny-five":"latest",
    "fubarino-io":"latest"
  },
  "keywords": [
    "node",
    "firmata",
    "fubarino-io"
  ],
  "author": "Rick Anderson",
  "license": "BSD",
  "readmeFilename": "Readme.md"
}
```
* Update the dependencies to include:
** fubarino-io
** johnny-five

* Do: ```npm install```

* create blink.js
```
var five = require("johnny-five"),
    board = new five.Board("fubarino-io");

board.on("ready", function() {
  // Create an Led on pin 13
  var led = new five.Led(21);

  // Strobe the pin on/off, defaults to 100ms phases
  led.strobe();
});
```
* run ```node blink.js```








# ioboard

An extendable implementation of Johnny Five's [IO Plugins](https://github.com/rwaldron/johnny-five/wiki/IO-Plugins).

Implements all required and optional methods, along with the required constants - `MODE`, `HIGH`, `LOW`, etc.

You should configure the relevant pins of your board in your constructor, then emit the ready event.

E.g.:

```javascript
var util = require('util'),
  IOBoard = require('fubarino-io');

MyIO = function(path, callback) {
  // call super constructor
  IOBoard.call(this);

  // connect to hardware and emit "connected" event
  this.emit("connected");

  // .. configure pins
  this._pins.push(..);

  // all done, emit ready event
  this.emit("ready");

  // finally call the passed callback
  callback();
}
util.inherits(IO, IOBoard);
```

Finally implement any of the IO Plugin methods of your choosing:

```javascript
// implement digitalWrite
MyIO.prototype.digitalWrite = function(pin, value) {
  ..
};
```

## Logging

By default IOBoard will print a message when every non-implemented method is invoked.  To prevent this, set the `quiet` property of the super constructor args to true:

```javascript
MyIO = function(path, callback) {
  // call super constructor
  IOBoard.call(this, {
    ..
    quiet: true
  });

  ..
}
util.inherits(IO, IOBoard);
```
