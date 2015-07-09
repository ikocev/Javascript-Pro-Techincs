





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