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

> Local variables

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

### Arguments passing
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


```javascript
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