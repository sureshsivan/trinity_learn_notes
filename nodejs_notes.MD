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

* SCOPE - wher in code we have access to particular object (function or object or any var)
* require function does below
	1. reads our file and wraps it a immediately invoking function
	2. execute it and return the module.export object where module is one of the argument to the generated wrapper function.
```javascript
var summa = function(){
	console.log("Summa");
}
```
	3. translated to 
```javascript
function(exports, require, module, __dirname, __filename){
	var summa = function(){
		console.log("Summa");
	}
});
```
	4.	execute the above function through "apply"
	5. returns module.exports
	
	