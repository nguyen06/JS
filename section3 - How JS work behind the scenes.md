## What is an execution context?

Simply put, an execution context is an abstract concept of an environment where the Javascript code is evaluated and executed. Whenever any code is run in JavaScript, it’s run inside an execution context.

### Type of Execution context

There are three types of execution context in JavaScript.

+ Global Execution Context — This is the default or base execution context. The code that is not inside any function is in the global execution context. It performs two things: it creates a global object which is a window object (in the case of browsers) and sets the value of this to equal to the global object. There can only be one global execution context in a program.

+ Functional Execution Context — Every time a function is invoked, a brand new execution context is created for that function. Each function has its own execution context, but it’s created when the function is invoked or called. There can be any number of function execution contexts. Whenever a new execution context is created, it goes through a series of steps in a defined order which I will discuss later in this article.

+ Eval Function Execution Context — Code executed inside an eval function also gets its own execution context, but as eval isn’t usually used by JavaScript developers, so I will not discuss it here.

### Execution stack

Execution stack, also known as “calling stack” in other programming languages, is a stack with a LIFO (Last in, First out) structure, which is used to store all the execution context created during the code execution.

When ths js engine first encounters your script, it creates a global execution context and pushes it to the execution stack. Whenever the engine finds a function invocation, it creates a new execution context for that function and pushes it to the top of the stack. 

The engine executes the function whose execution context is at the top of the stack. When this function completes, its execution stack is poped off from the stack, and the control reaches the context below it in the current stack.
```js
var a = "Hello world";

function first(){
    console.log('Inside first function');
    second();
    console.log('Again inside first function');
}

function second(){
    console.log('Inside second function');
}

first();
console.log('Inside Global Execution context');

```
## Execution context object

the execution context object has three properties

- Variable Object (VO) which contains variable argument in variable declaration 
- Scope chain- which contains the current variable objects as well as the variable object of all its parents
- "this" variable 

When a function is called, its function execution context is put on the top of execution context stack it happens in two phases:

- creation phase
    A) Creation of the varibale Object (VO)
    B) Creation of the scope chain
    C) determin value of the 'this' variable
    
- execution phase
    the code of the function that generate current execution context is executed
    
Inside Variable Object (VO)

The argument object is created, containing all the argument that were passed into the function

hoisting:

- Code is scannded for function declarations: for each function, a property is created  in the VO, poiting to the fucntion

- Code is scanned for variable declarations: for each variable, a property is created in the VO, and set to undefined.

the function and variable actually are declarated before execution.

## Hoisting in practice


calculateAge(1967);

function calculateAge(year){
    console.log(2018-year);
}

in the creation phase of the execution context which is in this case the global execution context. The function declaration calculateAge is stored in Variable Object and even before the code is executed. That is why when we then enter the execution phase the calculateAge function is already available for us, so we don't have to first declare the function and then use it. 

let see how does it work out with function expression.

retirement(1990); //ok, this time it doesn't work because this time this function   // is not a function declaration but it is a function expression. Hoisting function // only work with function declaration.

var retirement = function(year){
    console.log(65-(2016-year));
}

retirement(1990); // 39


another example with hoisting


var age = 23;
console.log(age); // 23

what happen if we declate before use. 

console.log(age); // undefined
var age = 23;


function foo(){
    console.log(age); //undefined, print the age before we declare it 
    var age = 65;
    console.log(age);
}

foo()
console.log(age);

the first age is put into the global execution context, the second age is created from the foo execution conext object. Theya re different

## 40. SCoping and scope chain

Now we will loook at the second step of Execution context object is scope chain. Scoping answers the question "where can we access a certain variable?"

In JS each function creates a scope: the space/environment, in which the variables it defines are accessible. (only for function)

Lexical scoping: a function that is lexical within another function gets access to the scope of the outer function. lexical function can get access to all variable and function that the parent function defines. 

example:

var a = "Hello";
first();

function first(){
    var b = 'hi';
    second();
    
    function second(){
        var c = 'Hey!";
        console.log(a + b+ c); // can access al
    }
}

function third(){
    var d = 'John';
    console.log(a+b+c+d); //only access a and d, but not b,c bc not parents
}

Global Scope [VOgloabl]
--> first() scope [VO1] + [VOglobal]
    ---> second() [VO1] + [VOglobal] + [VO2]
    
bottom-top arrow defines Scope chain, the lexical function can get access to its parents variable, if not it find the accessible on the parent of its parents. but the concept doesn't work on another direction. the parent scope doesn't get access to the child scope.

## 41. The 'this' keyword

we will learn about this keywork. The this variable is a variable that each and every execution context gets and it is stored in the execution context object. So where does the this variable, or the this keyword, point? 

    - Regular function call: the this keyword points at the global object, (the window object, in the browser)
    - Method call: the this variable points to the object that is calling the method

The this keyword is not assigned a value until a function where it is defined is actually called.

## 42. The 'this' in practice

console.log(this); // window{...}

calculateAge(1985);

function calculateAge(year){
    console.log(2018-year);
    console.log(this); // window object, bc it is regular function call
}

now let move on to the object here

var john = {
    name:'John',
    yearOfBirth: 1990;
    calculateAge: function(){
        console.log(this);
        cosole.log(2018 - this.yearOfBrith);
        
        function innerFucntion(){
            console.log(this); // window Object because, when regular call happen
        } // the default object is the window object
        innerFunction();
    }
}

john.calculateAge(1985);

this keyword now reference to john object: Object{name:'John',yearOfBirth:1990}

'This' variable only assigned a value as soon as an object calls a method 

var mike = {
    name='mike',
    yearOfBrith: 1990;
}

Now we can use borrow method in js to calucate mike calculateAge

mike.calculateAge = john.calculateAge;

mike.calculateAge(2018);
















