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
```

#### Combination (Constructor and Prototype)
#### Dynamic Prototype Pattern
#### Parasitic Constructor Pattern
#### Durable Constructor Pattern