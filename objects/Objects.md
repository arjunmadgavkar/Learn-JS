# Creating Objects
There are a few ways to create a JS object:
* Using the `class` keyword
* Using the `function` keyword

```js
class A {
    constructor(){}
    doSomething(){
        console.log("Hello");
    }
}
// let a = new A();

function B() {
    this.doSomething = function() {
        console.log("Hello");
    }
}
// let b = new B();
```

# Sharing Functions between Objects
There might be a situation where you have 2 different classes where both of them use a common function. How do you handle this?

## Sharing Basic Functions

You can define 2 classes and the common function. Both classes make use of the common function.
```js
function A() {
    this.doSomething = function(){
        console.log("I'm A");
    }
    this.shared = shared();
}
function B() {
    this.doSomething = function(){
        console.log("I'm B");
    }
    this.shared = function() {
        shared();
    }
}
function shared() {
    console.log("I'm a shared function.");
}
```

## Sharing Functions that Use `this` between Objects

If you want to use a function with `this` then you can use the `.apply` method. It calls the shared function and uses the `this` of the caller.

```js
function Insider(value) {
    this.value = value;
    this.printValue = function(){
        console.log("I'm an insider and my value is " + value);
    }
    this.increaseValue = function() {
        increaseValue.apply(this);
    }
}
function Outsider(value) {
    this.value = value;
    this.printValue = function(){
        console.log("I'm an outsider and my value is " + value);
    }
    this.increaseValue = function() {
        increaseValue.apply(this);
    }
}
function increaseValue() {
    this.value = this.value + 10;
    console.log(this.value); // Object's value + 10
}
```

## Sharing Pure Functions

You should use pure functions when you can. A pure function is a function that has no side effects and when it's given the same input it always return the same output (read more [here](../functions/Functions.md)). Here is an example of 2 objects sharing a pure function:
```js
// Sharing pure functions between objects
function Pure1(value) {
    this.value = value;
    this.printValue = function(){
        console.log("I'm an Pure1 and my value is " + this.value);
    }
    this.increaseValue = function(valueToIncreaseBy) {
        this.value = increaseValue(this.value, valueToIncreaseBy);
    }
}
function Pure2(value) {
    this.value = value;
    this.printValue = function(){
        console.log("I'm an Pure1 and my value is " + this.value);
    }
    this.increaseValue = function(valueToIncreaseBy) {
        this.value = increaseValue(this.value, valueToIncreaseBy);
    }
}
function increaseValue(originalValue, valueToIncreaseBy) {
    return originalValue + valueToIncreaseBy;
}
```









# Object.assign()
This method copies values from source object to target object. Let's look at an example:
```js
let source = { a: 5, c: 5 };
let target = { a: 10, b: 10, c: 10 };
Object.assign(target, source);
console.log(JSON.stringify(target)); // { a: 5, b: 10, c: 5 }
```
As you can see here, the first argument is the target object that is going to be modified and the second argument is the source object that is doing the modifying. We are going to take the source object values and put them into the target value. This means that we will overwrite the properties that have the same names, but we don't remove properties that arleady existed in the target object (even if they didn't exist the source object).

