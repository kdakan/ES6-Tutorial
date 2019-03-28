https://kdakan.github.io/ES6-Tutorial/

# ES6 Tutorial
--------------
This is a tutorial on the ES6 (and some ES8) additions to javascript. You can run the examples directly on the Chrome developer console, without any transpiling.

## Table of contents
* [ 1. Scoped variables](#1-scoped-variables)
* [ 2. Destructuring](#2-destructuring)
* [ 3. Default parameters](#3-default-parameters)
* [ 4. Rest parameters](#4-rest-parameters)
* [ 5. Spread operator](#5-spread-operator)
* [ 6. Template literals](#6-template-literals)
* [ 7. Classes](#7-classes)
* [ 8. Arrow functions](#8-arrow-functions)
* [ 9. Generators and iterators](#9-generators-and-iterators)
* [10. Built-in objects](#10-built-in-objects)
* [11. Object.assign() (mixins)](#11-objectassign-mixins)
* [12. Object literal shortcuts](#12-object-literal-shortcuts)
* [13. Proxies](#13-proxies)
* [14. ES6 modules](#14-es6-modules)
* [15. Promises](#15-promises)
* [16. Async/await (ES8)](#16-asyncawait-es8)
* [17. Fetch api examples](#17-fetch-api-examples)

## 1. Scoped variables:
* ```let n = 1;``` defines a block scoped variable, where a block is enclosed by ```{``` and ```}```, it is only visible inside the block, and it cannot be declared again inside the same block.
  ```let``` can be used like
  ```js
  let someCondition = true;
  if (someCondition) {
    let n = 1;
  }
  ```
  where n is only visible inside the if block but not outside
* In ```es5```, ```var n = 1;``` defines a function scoped variable which is hoisted (its declaration is moved to top of the function declaring it, or becomes global if not declared inside a function), and var can define the same variable multiple times inside the same function but the last declaration will take effect, which can cause problems and it is not recommended to use var anymore
* ```const n = 1;``` defines a block scoped variable, and its reference cannot be changed, meaning that it cannot be reassigned. However, a ```const``` object is not immutable, because its members can be reassigned, like
  ```js
  {
    const o = {n: 1, s: "abc"};
    o.n = 2;
    o.s = "xyz";
  }
  ```
  If we need immutable objects, ```Object.freeze()``` or ```immutable.js``` library can be used with more handy features
  
## 2. Destructuring:
* Destructuring uses ```[``` and ```]``` to assign multiple variables at once from an array, and ```{``` and ```}``` to assign multiple variables at once from object fields, like
  ```js
  let [x, y] = [3, 5]; //x=3, y=5
  [x, y] = [y, x,]; //x any y are swapped, x value becomes y, y value becomes x
  let [, a, b, c] = [1, 3, 5]; //skips first element in the right-side array, a=3, b=5, c=undefined
  
  let {p, r, s} = {p: 3, r: 5, s: "abc"}; //p=3, r=5, s="abc"
  let {p:k, s:l} = {p: 3, r: 5, s: "abc"}; //k=3, r="abc", beware the weird ordering, that it is not k:p, but rather p:k
  let {m, n} = {m: 3, n: "abc"}; //m=3, n="abc", shorthand syntax when the variables and object fields have the same name
  
  let f = function(x, {y, z}, n) {
    //do something with x, y, z, n parameters
  }
  f(1, {y: 2, z: "abc"}); //call function f() with x=1, y=2, z="abc", n=undefined parameter values
  ```
  
## 3. Default parameters:
* Parameters can be assigned default values, like
  ```js
  let f = function(x = 1) {
    //no need to use let y = x || 1; to give a default value of 1 if x is missing
  }
  f(); //missing parameter x=default value of 1, same as f(undefined);
  f(null); //parameter x=null, because it is not missing
  f(""); //parameter x="", because it is not missing
  
  let g = function(x = 1, {y = 2, z = 3}) {
    //do something with x, y, z parameters
  }
  g(7, {z: 9}); //missing parameter y=default value of 2
  ```
  
## 4. Rest parameters:
* Rest parameters, which is an array, replace the need for ```arguments``` (```arguments``` can also be used, but it is an object, not an array). Rest parameters use ```...someVariables``` syntax, like
  ```js
  let sum = function(x, y, ...rest) {
    //here you can use rest parameter as an array
  }
  let total = sum("a", "b", 5, 7, 4, 67, 38); //parameter rest=[5, 7, 4, 67, 38]
  ```
  
## 5. Spread operator:
* Spread operator ```...someArray```, turns an array into a comma separated expression, like
  ```js
  let f = function(x, y, z) {
    //do something with x, y, z parameters
  }
  f(...[3, 5, 7]); //call f() with parameters x=3, y=5, z=7
  
  let a = [3, 5, 7];
  let b = [20, 40, ...a, 60, 80]; //b=[20, 40, 3, 5, 7, 60, 80]
  ```
  
## 6. Template literals:
* Template literals are used for concatenating strings, like
  ```js
  let id = 123;
  let domain = "abc.com";
  let url = `https://${domain}/item/${id}`; //url="https://abc.com/item/123"
  ```
  
## 7. Classes:
* Classes support constructor, methods, and property getters and setters, like
  ```js
  class Employee {
    constructor(firstname, lastname) {
      this.firstname = firstname;
      this.lastname = lastname;
    }
    work(hours) {
      console.log(this.fullname + " is working for " + hours + " hours.");
    }
    get fullname() {
      //this is a property getter
      return this.firstname + " " + this.lastname;
    }
    
    set fullname(fullname) {
      //this is a property setter
      var names = fullname.split(" ");
      this.firstname = names[0];
      this.lastname = names[1];
    }
  }
  
  var emp = new Employee("John", "Doe");
  emp.work(8); //prints "John Doe is working for 8 hours."
  ```
  is similar to the ```es5``` version
  ```js
  function Employee(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }
  
  Employee.prototype = {
    work: function(hours) {
      console.log(this.getfullname() + " is working for " + hours + " hours.");
    },
    getfullname() {
      return this.firstname + " " + this.lastname;
    },
    setfullname(fullname) {
      var names = fullname.split(" ");
      this.firstname = names[0];
      this.lastname = names[1];
    }
  };
  
  var emp = new Employee("John", "Doe");
  emp.work(8); //prints "John Doe is working for 8 hours."
  ```
* A class can ```extend``` (inherit from) a super class, like
  ```js
  class Person {
    constructor(firstname, lastname) {
      this.firstname = firstname;
      this.lastname = lastname;
    }
  }
  
  class Employee extends Person {
    constructor(title, firstname, lastname) {
      super(firstname, lastname);
      this.title = title;
    }
  }
  
  var emp = new Employee("Manager", "John", "Doe");
  console.log(emp.title + " " + emp.firstname + " " + emp.lastname); //prints "Manager John Doe"
  ```
  
## 8. Arrow functions:
* Arrow functions are the preferred way to define callback functions and short functions, and offer a shorthand syntax, like
  ```js
  //add() and add2() do the same thing
  let add = (x, y) => x + y;
  let add2 = (x, y) => {
    return x + y;
  }
  let sum = add(1, 2); //sum=3
  let sum2 = add(1, 2); //sum2=3
  //single parameter
  let square = x => x * x;
  let sqr = square(2); //sqr=4
  //parameterless
  let three = () => 3;
  let x = three(); //x=3
  
  let arr = [1, 2, 3];
  let total = 0;
  arr.forEach(x => total += x); //total=6
  let doubles = arr.map(x => x * 2); //doubles=[2, 4, 6]
  let odds = arr.filter(x => x % 2 === 1); //odds=[1, 3]
  ```
* Arrow functions use lexical scoping, thus when used as a callback function, ```this``` from the outside is visible inside the arrow function, unlike the normal callback functions declared with ```function``` keyword, like
  ```js
  class MyClass {
    total = 0;
    
    sumWithArrowFunction = function() {
      this.total = 0;
      [1, 2, 3].forEach(x => this.total += x);
    }
    
    sumWithoutArrowFunction = function() {
      this.total = 0;
      let self = this;
      [1, 2, 3].forEach(function(x) {
        return self.total += x; //this.total does not work here, we need to first set it outside to a variable like self and use self inside
      });
    }
  }
  
  let m = new MyClass();
  m.sumWithArrowFunction();
  console.log(m.total); //m.total=6
  m.sumWithoutArrowFunction();
  console.log(m.total); //m.total=6
  ```
  
## 9. Generators and iterators:
* Generator functions create iterators, and iterables can be lazily iterated using for..of syntax, like
  ```js
  let numbers = function*(max) {
    for (let i=0; i<max; i++) {
      console.log("yield " + i);
      yield i;
    }
  }
  
  for (let n of numbers(3))
    console.log("got " + n);
  ```
  Here the ```for..of``` loop prints "yield 0", "got 0", "yield "1, "got 1", "yield 2", "got 2", so the numbers are iterated lazily. This can be valuable when the iterator is doing expensive work like an expensive calculation, going to the database, or using network operations
  
## 10. Built-in objects:
* There are new built-in objects and objects with new additional methods, like ```Number```, ```Array```, ```Set``` (hashset), ```Map``` (hashmap/hashtable/dictionary), ```WeakSet```, ```WeakMap```. ```WeakSet``` and ```WeakMap``` do not hold strong pointers to their items, so that the item can be garbage collected, and they cannot be iterated. Using ```WeakMap``` and ```WeakSet``` instead of ```Map``` and ```Set``` can prevent memory leaks.

* Some examples of new ```Array``` methods:
  ```js
  let array = [1, 5, 10, 20];
  let a = array.find(item => item > 3); //returns first match, 5
  let ind = array.findIndex(item => item > 3); //returns index of first match, 1
  array.fill('a'); //fills array with 'a', array becomes  ['a', 'a', 'a, 'a']
  array.fill('x', 2, 3) // fills with 'x' starting from index 2 to 3 (excluding 3), array becomes ['a', 'a', 'x, 'a']
  let array2 = new Array(3); //creates array of length 3 with empty (undefined) items
  let array3 = Array.of(1, 2, 3); //array3=[1, 2, 3]
  let array4 = Array.from(document.querySelectorAll('div')); //creates araay from a non-array DOM object collection 

## 11. Object.assign() (mixins):
* ```Object.assign(o1, o2)``` merges members of o2 onto o1 (o2 is also called mixin), ```Object.assign()``` can have more than 2 parameters, and will merge all into the first parameter object

## 12. Object literal shortcuts:
* Object literal shortcut syntax can be used, like
  ```js
  let model = "Porche";
  let year = 2018;
  //both create an object with members model="Porche", year=2018 and run()
  let car = { model: model, year: year, run: function() {console.log("running");} }
  let car2 = { model, year, run() {console.log("running");} }
  
  let fieldname = "model";
  let fieldvalue = "Ferrari";
  //both create an object with member model="Ferrari"
  let car3 = {};
  car3[fieldname] = fieldvalue;
  let car4 = { [fieldname] : fieldvalue }
  
  ```
## 13. Proxies:
* ```Proxy``` is a wrapper around an object, and lets us intercept getting and setting properties, and also intercept calling methods on the wrapped (proxied) object.
* We can use a ```Proxy``` for getters and setters, like
  ```js
  let horse = {
    color: 'white',
    hasTail: true
  }
  
  let proxyHorse = new Proxy(horse, {
    get: function(target, property) {
      if(property === 'color')
        return 'Brilliant ' + target[property];
      else
        return target[property];
    },
    set: function(target, property, value) {
      if (property === 'hasTail' && value === false)
        console.log('You cannot set it to false!');
      else
        target[property] = value;
    },
    apply: function(target, context, args) {
      if(context !== unicorn)
        return "Only unicorn can use hornAttack";
      else
        return target.apply(context, args);
    }
  });
  
  console.log(proxyHorse.color) //prints "Brilliant white"
  proxyHorse.hasTail = false; //does not set hasTail to false, instead prints "You cannot set it to false!"
  ```
* We can also intercept calls to ```apply```, ```delete```, ```define```, ```freeze```, ```in```, ```has```, etc.

## 14. ES6 modules:
* Before ES6, there were several module systems and libraries to support them, like ```AMD``` modules and ```CommonJS``` modules. ES6 module syntax is similar to the ```CommonJS``` syntax.
* A module can be declared by using ```export``` and accessed from another module by using  ```import```. There are two kinds of exports, named export and default export. A module can "named export" from nothing to multiple things and "default export" either nothing or one thing, like
  ```js
  //inside lib.js
  export function add(x, y) { return x + y; }
  export const multiply = (x, y) => { x * y }
  export class ComplexNumber { ... }
  export default const PI = 3.1459...;
  
  //inside app.js
  import * as mathlib from './lib.js'
  let sum = mathlib.add(1, 2);
  
  import piNumber, { add, ComplexNumber } from './lib.js'
  let sum = add(1, piNumber);
  let c = new ComplexNumber(1, 2);
  ```
  We should use ```{``` and ```}``` when accessing a named export. Think of all named exports as part of an export object (as it was like this with CommonJS modules), an in a similar fashion, we can import every named exports into an object, as in the example ```import * as mathlib from './lib.js'```. The ```as``` keyword can be used both when exporting named exports and also when importing named exports. We should not use ```{``` and ```}``` when accessing a default export.
* A module can be declared by using ```export``` inside the file ```Customer.js``` in the ```crm``` folder, like
  ```js
  chargeCreditCard(cardNumber, amount) {
    //chargeCreditCard() is inaccessible from outside this module
  }
  
  class Customer {
    constructor(id) {
      ...
    }
    buy(item) {
      ...
      chargeCreditCard(this.cardNumber, item.price);
      ...
    }
  }
  
  export Customer;
  ```
  and accessed from outside the module by using ```import```, like
  ```js
  import {Customer} from './crm/Customer';
  
  let customer = new Customer(123);
  let item = {productId: 456, productName: "Watch", price: 100};
  customer.buy(item);
  ```
  This is similar to the ```iife``` (immediately invoked function expression) module definition and usage in ```es5```, like
  ```js
  (function(target) {
  
  chargeCreditCard(cardNumber, amount) {
    //chargeCreditCard() is inaccessible from outside this module
  }
  
  function Customer(id) {
    ...
  }
  
  Customer.prototype = {
    buy: function(item) {
      ...
      chargeCreditCard(this.cardNumber, item.price);
      ...
    }
  }
  
  }(window))
  
  var customer = new Customer(123);
  var item = {productId: 456, productName: "Watch", price: 100};
  customer.buy(item);
  ```
* A module may export multiple classes, functions or variables, like
  ```js
  function f() {...}
  class C {...}
  const pi = 3.14159;
  
  export f, C, pi;
  ```
  or like
  ```js
  export function f() {...}
  export class Customer {...}
  export const pi = 3.14159;
  ```
  and ```import``` and use whatever we need, like
  ```js
  import {Customer, pi} from './crm/Customer';
  
  let customer = new Customer();
  let circumference = 2 * pi * 10;
  ```
  We can also use ```export default```, like
  ```js
  class VIPCustomer {...}
  
  export default VIPCustomer;
  ```
  and import it and use it with any name we want, like
  ```js
  import Customer from './crm/VIPCustomer';
  
  let customer = new Customer();
  ```
  
## 15. Promises:
* An ```async``` functions does not return is result immediately, but instead returns a ```Promise``` object. A ```Promise``` object can be in 3 states, it starts in ```pending``` state, and when/if it ```resolve```s it switches to ```fulfilled``` state, and when/if an error occurs, it switches to ```rejected``` state. Promises can be chained, like 
  ```js
  function getOrder(orderId) {
    //returns a Promise, starts a time taking operation and resolves with the result when the operation succeeds
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve({id: 123, name: "Apple Macbook", userId: 456}), 1000);
    });
  }
  function getUser(userId) {
    //returns a Promise, starts a time taking operation and resolves with the result when the operation succeeds
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve({id: 456, name: "John Doe", companyId: 789}), 1000);
    });
  }
  function getCompany(companyId) {
    //returns a Promise, starts a time taking operation and resolves with the result when the operation succeeds
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve({id: 789, name: "MyCompany"}), 1000);
    });
  }

  getOrder(123)
  .then(order => getUser(order.userId))
  .then(user => getCompany(user.companyId))
  .then(company => console.log(company.name))
  .catch(error => console.log(error.message));
  ```
  This code above will print "MyCompany" after 3 seconds (```getOrder()```, ```getUser()```, and ```getCompany()``` each takes 1 second to complete). Note that ```.catch(error => console.log(error.message))``` works the same way as ```.then(undefined, error => console.log(error.message))``` does, but cleaner semantics. If we change ```getUser()``` to the following code,
  ```js
  function getUser(userId) {
    //returns a Promise, starts a time taking operation and rejects with an Error result when the operation fails
    return new Promise((resolve, reject) => {
      setTimeout(() => reject(new Error("An error occured while fetching user with id: " + userId)), 1000);
    });
  }
  ```
  Then the same ```getOrder(123).then(...).then(...).then(...).catch(...)``` chain call will print "An error occured while fetching user with id: 456" after 2 seconds
* ```Promise.all()``` resolves multiple promises, like
  ```js
  function getCompany(companyId) {
    let companies = {
      1: {id:1, name: "Apple"},
      2: {id:2, name: "Google"},
      3: {id:3, name: "Facebook"},
      4: {id:4, name: "Amazon"},
      5: {id:5, name: "Microsoft"}
    }
    
    //Promise.resolve could be used if we wanted to resolve immediately
    //return Promise.resolve(companies[companyId]);
    
    //returns a Promise, starts a time taking operation and resolves with the result when the operation succeeds
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve(companies[companyId]), 1000);
    });
  }
  
  let promises = [getCompany(1), getCompany(2), getCompany(3), getCompany(4), getCompany(5)];
  Promise.all(promises)
  .then(companies => companies.forEach(company => console.log(company.name)))
  .catch(error => console.log(error.message));
  ```
  This code above will print all 5 company names after 1 second (each call to getCompany() takes 1 second, but calls are done in parallel). If we change ```getCompany()``` to the following code,
  ```js
  function getCompany(companyId) {
    //returns a Promise, starts a time taking operation and rejects with an Error result when the operation fails
    return new Promise((resolve, reject) => {
      setTimeout(() => reject(new Error("An error occured while fetching company with id: " + companyId)), 1000);
    });
  }
  ```
  Then the same ```Promise.all(promises).then(...).catch(...)``` chain call will print "An error occured while fetching company with id: 1" after 1 second. 
* ```Promise.race()``` resolves the fastest completing promise. The following code will print "Apple" after 1 second, like
  ```js
  let promises = [getCompany(1), getCompany(2), getCompany(3), getCompany(4), getCompany(5)];
  Promise.race(promises)
  .then(company => console.log(company.name))
  .catch(error => console.log(error.message));
  ```
  
## 16. Async/await (ES8):
* ```async``` function wraps its return value in a Promise object, so ```async f() {... return 123;}``` is the same as ```f() {... return new Promise(...resolve(123)...);}```
* This is similar to a C# async method returning a ```Task<int> object``` wrapping its return value of type ```int```
* await resolves a Promise object, so await p resolves the Promise object p, and await f() resolves the Promise returned by function f()
* When an async function returns a Promise, js flattens it out to a single Promise, so ```async f() {... return new Promise(...resolve(123)...);}``` is the same as ```async f() {... return 123;}```
* async and await are frequently used together in the async/await pattern like ```x = await f();``` but using with promises, they don't have to be used together all the time
* Refer to https://dev.to/codeprototype/async-without-await-await-without-async--oom for more details
* To run two async functions in parallel, you cannot use ```await f1(); await f2();```, instead either use ```Promise.all()``` or ```p1 = f1(); p2 = f2(); await p1; await p2;``` (starts f1() and f2() and resolves both later
* Refer to https://medium.freecodecamp.org/avoiding-the-async-await-hell-c77a0fb71c4c for more details
 
## 17. Fetch api examples:
* Refer to https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch for more details and examples
  ```jsx
  //basic get
  fetch('https://api.github.com/users/KrunalLathiya')
  .then(response => response.json())
  .then(data => {
    console.log(data) //prints result from `response.json()` in getRequest
  })
  .catch(error => console.error(error))

  //cors
  fetch('https://api.github.com/users/KrunalLathiya', {
    credentials: 'include', //useful for including session ID (and, IIRC, authorization headers)
  })
  .then(response => response.json())
  .then(data => {
    console.log(data) //prints result from `response.json()`
  })
  .catch(error => console.error(error));
 
  //post data
  fetch('https://jsonplaceholder.typicode.com/users', {
    headers: { "Content-Type": "application/json; charset=utf-8" },
    method: 'POST',
    body: JSON.stringify({
      username: 'Elon Musk',
      email: 'elonmusk@gmail.com',
    })
  })
  
  //post data
  fetch('https://appdividend.com/api/v1/users', {
    credentials: 'same-origin', // 'include', default: 'omit'
    method: 'POST', // 'GET', 'PUT', 'DELETE', etc.
    body: JSON.stringify({user: 'Krunal'}), //coordinate the body type with 'Content-Type'
    headers: new Headers({
      'Content-Type': 'application/json'
    }),
  })
  .then(response => response.ok() response.json())
  .then(data => console.log(data)) //result from the `response.json()` call
  .catch(error => console.error(error))

  //delete data
  fetch('https://jsonplaceholder.typicode.com/users/1', { 
    method: 'DELETE' 
  });

  //use async/await
  let res = await fetch('https://api.github.com/users/KrunalLathiya');
  let data = await res.json();
  console.log(data);
 
  //upload file
  const formData = new FormData()
  const fileField = document.querySelector('input[type="file"].avatar')
  formData.append('username', 'abc123')
  formData.append('avatar', fileField.files[0])
  fetch('https://appdividend.com/api/v1/users', {
    method: 'POST', //'GET', 'PUT', 'DELETE', etc.
    body: formData  //coordinate the body type with 'Content-Type'
  })
  .then(response => response.json())
 
  //upload multiple files
  <input type='file' multiple class='files' name='files' />
  const formData = new FormData()
  const fileFields = document.querySelectorAll('input[type="file"].files')
  // Add all files to formData```
  [].forEach.call(fileFields.files, f => formData.append('files', f))
  //alternatively for PHP peeps, use `files[]` for the name to support arrays
  //Array.prototype.forEach.call(fileFields.files, f => formData.append('files[]', f))
  fetch('https://appdividend.com/api/v1/users', {
    method: 'POST', //'GET', 'PUT', 'DELETE', etc.
    body: formData  //coordinate the body type with 'Content-Type'
  })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error))
  ```
 
