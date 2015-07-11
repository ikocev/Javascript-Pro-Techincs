# Proffesional Javascript

## Adding Javascript to the page

```html
<!DOCTYPE html>
<html>
	<head>
		<!-- download should begin immediately but execution should be deferred. Wait for the page to be parsed, then execute the scripts. -->
		<script type=”text/javascript” defer src=”example2.js”></script>
		<!-- begin downloading the file immediately but do not wait for the file. Asynchronous scripts are guaranteed to execute before the page’s load event.  -->
		<script type=”text/javascript” async src=”example1.js”></script> 
	</head>
	<body>
	<!-- content here -->
	</body>
</html>
```

## Javascript basics

Local variables

```javascript
function test(){
	var message = “hi”; //local variable
}
test();
alert(message); //error!

function test(){
	message = “hi”; //global variable
}
test();
alert(message); //”hi”

//define more then one variable
var message = “hi”,
found = false,
age = 29;

```

Primitive Data Types

- “undefined” if the value is undefined
- “boolean” if the value is a Boolean
- “string” if the value is a string
- “number” if the value is a number
- “object” if the value is an object (other than a function) or null
- “function” if the value is a function


## Variables, Scopes and Memory

### Primitive and Reference values

#### Arguments passing
```javascript
function addTen(num) {
	num += 10;
	return num;
}
var count = 20;
var result = addTen(count);
alert(count); //20 - no change
alert(result); //30


function setName(obj) {
	obj.name = “Nicholas”;
}
var person = new Object();
setName(person);
alert(person.name); //”Nicholas”


function setName(obj) {
	obj.name = “Nicholas”;
	obj = new Object();
	obj.name = “Greg”;
}
var person = new Object();
setName(person);
alert(person.name); //”Nicholas”, because when new object is created, the pointer is to a local object and that object is destroyed when the function ends. (This is simular to editing image in paint and not clicking save!)
```

```javascript
function buildUrl() {
	var qs = “?debug=true”;
	with(location){
		var url = href + qs; //here location is acting like global object but only to this scope.
	}
	return url;
}
```

##### No Block-Level Scopes
```javascript

if (true) {
	var color = “blue”;
}
alert(color); //”blue”

for (var i=0; i < 10; i++){
  doSomething(i);
}
alert(i); //10
```


```javascript
(function f(){
  function f(){ return 1; }
  return f();
  function f(){ return 2; }
  })();
```

## Object-Oriented Programming

### Object Creation
The following list represent the most used patterns while creating objects in javascript:
- The Factory Pattern
- The Constructor Pattern
- The Prototype Pattern
- Combination (Constructor and Prototype)
- Dynamic Prototype Pattern
- Parasitic Constructor Pattern
- Durable Constructor Pattern

#### The Factory Pattern

```javascript
function createPerson(name, age, job){
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function(){
		alert(this.name);
	};
	return o;
}
var person1 = createPerson(“Nicholas”, 29, “Software Engineer”);
var person2 = createPerson(“Greg”, 27, “Doctor”);
```
#### The Constructor Pattern

```javascript
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function(){
		alert(this.name);
	};
}
var person1 = new Person(“Nicholas”, 29, “Software Engineer”);
var person2 = new Person(“Greg”, 27, “Doctor”);


//use as a constructor
var person = new Person(“Nicholas”, 29, “Software Engineer”);
person.sayName(); //”Nicholas”
//call as a function
Person(“Greg”, 27, “Doctor”); //adds to window
window.sayName(); //”Greg”
//call in the scope of another object
var o = new Object();
Person.call(o, “Kristen”, 25, “Nurse”);
o.sayName(); //”Kristen”
```

Problems
```javascript
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = new Function(“alert(this.name)”); //logical equivalent
}

alert(person1.sayName == person2.sayName); //false
```
#### The Prototype Pattern
Example 1: Basic use
```javascript
function Person(){
}
Person.prototype.name = “Nicholas”;
Person.prototype.age = 29;
Person.prototype.job = “Software Engineer”;
Person.prototype.sayName = function(){
	alert(this.name);
};
var person1 = new Person();
person1.sayName(); //”Nicholas”
var person2 = new Person();

person2.sayName(); //”Nicholas”
alert(person1.sayName == person2.sayName); //true
```

Example 2: "Shadowing"
```javascript
function Person(){
}
Person.prototype.name = “Nicholas”;
Person.prototype.age = 29;
Person.prototype.job = “Software Engineer”;
Person.prototype.sayName = function(){
	alert(this.name);
};
var person1 = new Person();
var person2 = new Person();
person1.name = “Greg”;
alert(person1.name); //”Greg” - from instance
alert(person2.name); //”Nicholas” - from prototype

delete person1.name;
alert(person1.name); //”Nicholas” - from the prototype
alert(“name” in person1); //true - Enumerable is true to all prototype properties
```

Example 3: Alternate Prototype Syntax
```javascript
function Person(){
}
Person.prototype = {
	constructor: Person, 
	name : “Nicholas”,
	age : 29,
	job : “Software Engineer”,
	sayName : function () {
		alert(this.name);
	}
};

var friend = new Person();
alert(friend instanceof Object); //true
alert(friend instanceof Person); //true
alert(friend.constructor == Person); //false
alert(friend.constructor == Object); //true
```

Example 4: Dynamic Nature of Prototypes
```javascript
var friend= new Person();
Person.prototype.sayHi = function(){
	alert(“hi”);
};
friend.sayHi(); //”hi” - works!


function Person(){
}

var friend = new Person();

Person.prototype = {
	constructor: Person,
	name : “Nicholas”,
	age : 29,
	job : “Software Engineer”,
	sayName : function () {
		alert(this.name);
	}
};
friend.sayName(); //error

var friend2 = new Person();
friend2.sayName(); //"Nicholas" - works
```
The instance is created before the prototype object is overwritten. The error is occured becase when friend object is created by the constructor, the object use the default prototype object, so there is none prototype objects.

Example 5: Native Prototype Objects
```javascript
String.prototype.startsWith = function (text) {
	return this.indexOf(text) == 0;
};
var msg = “Hello world!”;
alert(msg.startsWith(“Hello”)); //true
```
Example 6: Problems with prototypes
```javascript
function Person(){
}
Person.prototype = {
	constructor: Person,
	name : “Nicholas”,
	age : 29,
	job : “Software Engineer”,
	friends : [“Shelby”, “Court”],
	sayName : function () {
		alert(this.name);
	}
};
var person1 = new Person();
var person2 = new Person();
person1.friends.push(“Van”);
alert(person1.friends); //”Shelby,Court,Van”
alert(person2.friends); //”Shelby,Court,Van”
alert(person1.friends === person2.friends); //true
```
#### Combination (Constructor and Prototype)
Example 1:
```javascript
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.friends = [“Shelby”, “Court”];
}
Person.prototype = {
	constructor: Person,
	sayName : function () {
		alert(this.name);
	}
};

var person1 = new Person(“Nicholas”, 29, “Software Engineer”);
var person2 = new Person(“Greg”, 27, “Doctor”);

person1.friends.push(“Van”);

alert(person1.friends); //”Shelby,Court,Van”
alert(person2.friends); //”Shelby,Court”
alert(person1.friends === person2.friends); //false
alert(person1.sayName === person2.sayName); //true
```

#### Dynamic Prototype Pattern
Example 1:
```javascript
function Person(name, age, job){
	//properties
	this.name = name;
	this.age = age;
	this.job = job;
	//methods
	if (typeof this.sayName != “function”){
		Person.prototype.sayName = function(){
			alert(this.name);
		};
	}
}
var friend = new Person(“Nicholas”, 29, “Software Engineer”);
friend.sayName();
```
#### Parasitic Constructor Pattern
If other fails to your needs try parasitic pattern.
Example 1:
```javascript
function Person(name, age, job){
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function(){
		alert(this.name);
	};
	return o;
}
var friend = new Person(“Nicholas”, 29, “Software Engineer”);
friend.sayName(); //”Nicholas”
```
Example 2:
```javascript
function SpecialArray(){
	//create the array
	var values = new Array();
	//add the values
	values.push.apply(values, arguments);
	//assign the method
	values.toPipedString = function(){
		return this.join(“|”);
	};
	//return it
	return values;
}
var colors = new SpecialArray(“red”, “blue”, “green”);
alert(colors.toPipedString()); //”red|blue|green”
```
#### Durable Constructor Pattern
Example 1:
```javascript
function Person(name, age, job){
	//create the object to return
	var o = new Object();
	//optional: define private variables/functions here
	//attach methods
	o.sayName = function(){
		alert(name);
	};
	//return the object
	return o;
}
var friend = Person(“Nicholas”, 29, “Software Engineer”);
friend.sayName(); //”Nicholas”
```