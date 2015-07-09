





```javascript
function setName(obj) {
  obj.name = “Nicholas”;
  obj = new Object();
  obj.name = “Greg”;
}
var person = new Object();
setName(person);
alert(person.name); //”Nicholas”
```


```javascript
for (var i=0; i < 10; i++){
  doSomething(i);
}
alert(i); //10
```