* nodejs << v8-javascript engine << written in C++
* io.js is community  - more frequesnt updates based on v8 releases.
* nodejs 4.x.x is merge of io.js 3.x + nodejs 0.12

* CommonJS  - standards for javascript module system
* require argument - file path (absolute or relative)
	* .js in file name can be ignored

* every module have access to some local variaoble this, require, module, exports, \_\_dirname, \_\_filename

* \_\_proto\_\_  -  property which refers to prototype of every object in v8

# Prototypes in Javascript
```javascript
var Person = function(name){
  this.name = name;
}
Person.prototype.greet = function(){
	console.log("Hello..", this.name);
}
var suresh = new Person("Suresh");
var sadhana = new Person("Sadhana");
suresh.greet();	//	Hello.. Suresh
sadhana.greet();	//Hello.. Sadhana
console.log(suresh.__proto__ === sadhana.__proto__);	//	true
```

# Classical Inheritance in Javascript
```javascript
var Phone = function(number){
	this.number = number;
}
Phone.prototype.makeCall = function(){
	console.log("Making a call from # : ", this.number);
}
var SmartPhone = function(number){
	Phone.apply(this, arguments);
}

SmartPhone.prototype = new Phone();
SmartPhone.prototype.constructor = SmartPhone;

SmartPhone.prototype.sendMMS = function(){
	console.log('Sending MMS from # :  ', this.number);
}
var phone = new Phone(123);
var smartPhone = new SmartPhone(456);
var anotherSmartPhone = new SmartPhone(789);
phone.makeCall();
smartPhone.makeCall();
smartPhone.sendMMS();
anotherSmartPhone.makeCall();
anotherSmartPhone.sendMMS();
```
# extending javascript object another way - similar util.inherits
```javascript
var Phone = function(number){
	this.number = number;
}
Phone.prototype.makeCall = function(){
	console.log("Making a call from # : ", this.number);
}
var SmartPhone = function(number){
	Phone.apply(this, arguments);
}

SmartPhone.prototype = Object.create(Phone.prototype, {constructor: {value: SmartPhone}});

SmartPhone.prototype.sendMMS = function(){
	console.log('Sending MMS from # :  ', this.number);
}
var phone = new Phone(123);
var smartPhone = new SmartPhone(456);
var anotherSmartPhone = new SmartPhone(789);
phone.makeCall();
smartPhone.makeCall();
smartPhone.sendMMS();
anotherSmartPhone.makeCall();
anotherSmartPhone.sendMMS();
```
* SCOPE - wher in code we have access to particular object (function or object or any var)

# require - module - exports 
* no need to put .js - file extension
* require('./greet') does below things
	1. find and load "./greet.js" file
	2. if above not found - fin "./greet" folder and load "index.js" file under that folder
	3. If not found throw error.
* require function does below (for js file)
	1. reads our file and wraps it a immediately invoking function
	2. execute it and return the module.export object where module is one of the argument to the generated wrapper function.
```javascript
var summa = function(){
	console.log("Summa");
}
```
* translated to 
```javascript
function(exports, require, module, __dirname, __filename){
	var summa = function(){
		console.log("Summa");
	}
});
```
* execute the above function through "apply"
* returns module.exports
* __REVEALING MODULE PATTERN__ is revealing only the few properties/functions from module.exports - achieved through returning an object with limited properties	
* Use module.exports instead of assigning something to exports. mutation exports work but asgning a new object to exports does not work
```javascript
// its a javascript quirk
export = function(){
	console.log("test");
}
console.log(exports === module.exports);	//	false

export.test = "test";
console.log(exports === module.exports);	//	true 
```
* If a util named as same as core library (util for ex) - make  sure to use the relative path in require

# Node and ES6
* use node --harmony <js file> - to run in es6 mode
```javascript
// file greet.js
export function greet(){
    console.log("Greeting from ES6");
}
export function greet1(){
    console.log("Greeting 1 from ES6");
}
// file index.ja
import * as greeter from "greet"
greeter.greet();
greeter.greet1();
```
* babel.js  - shim for ES6 (ES 2015) - only for browser
## server side nodejs support can be achieved through __jsconfig.json__ file
```json
{
    "compilerOptions": {
        "target": "ES6"
    }
}
```
## Template Literal
```javascript
var name = "suresh";
var greetOlder = "Hello : " + name;
var greetEs6 = `Hello : ${name}`;	//	can be multi line
console.log(greetOlder);
console.log(greetEs6);
```
## ES6 classes
```javascript
'use strict';
module.exports = class Person {
	constructor(firstname, lastname) {
		this.firstname = firstname;
		this.lastname = lastname;
	}
	
	greet() {
		console.log('Hello, ' + this.firstname + ' ' + this.lastname);
	}
}

var john = new Person('John', 'Doe');
john.greet();

var jane = new Person('Jane', 'Doe');
jane.greet();

console.log(john.__proto__);
console.log(jane.__proto__);
console.log(john.__proto__ === jane.__proto__);
```


## this on literals - always refers to the enclosing object literal - __call__ and __apply__ methods
```javascript
var xx = {
  name: "outer",
  outerPrint: function(){
    console.log("Outer Print : ", this.name);
	//console.log(this);
  },
  inner: {
    name: "Inner",
    innerPrint: function(){
      console.log("Inner Print : ", this.name);
    }
  }
};
xx.outerPrint();	//	outer
xx.inner.innerPrint();	//	inner

xx.outerPrint.call();	//	prints nothing	-	this refers to global here
xx.outerPrint.call({name: "External"});	// External
xx.inner.innerPrint.call();	//	prints nothing	-	this refers to global here 
xx.inner.innerPrint.call({name: "External for Inner call"});	// External for Inner call

xx.outerPrint.apply();	//	prints nothing	-	this refers to global here
xx.outerPrint.apply({name: "External"});	// External
xx.inner.innerPrint.apply();	//	prints nothing	-	this refers to global here 
xx.inner.innerPrint.apply({name: "External for Inner call"});	// External for Inner call

```
* apply expects arguments to passed as array - like XX.apply({}, [arg1, arg2, arg3...])
* call expects arguments to passed as additional arguments - like XX.call({}, arg1, arg2, arg3...)

# Events and EventEmitter
* Systems events(file read, file open and more - from C++, libuv),  Custom Events - as part of javascrpt code (Event Emitter)
* __MAgic Strings__ - using hard coded strings for event names - use externalized string constants for event names
* Object.create(null) - object creation without prototype = Object.create({}) - create object with prototype
## Event Emitter example 1
```javascript
var EventEmitter = require("events");
var util = require("util");
function MyConstructor(){
	EventEmitter.call(this);	//	to make sure properties added by EventEmitter to this object available for my object as well
	// blah blah blah
}
util.inherits(MyCOnstructor, EventEmitter);

var obj = new MyConstructor();
// obj.on, obj.emit - works

```
