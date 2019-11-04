## Everything is an Object: Inheritance and the Prototype Chain

In jS we have to type of value, primitives and objects

Primitives
    - Numbers
    - Strings
    - Booleans
    - Undefined
    - null

Everything Else...
    - Arrays
    - fucntions
    - Objects
    - Dates
    - Wrappers of Numbers, Strings, Booleans
    
    an object

OOP
    - Objects interacting with one another through methods and properties
    - Used to store data, struture applications into modules and keepong code clean
 
example:

    var john = {
        name:'John',
        yearOfBirth: 1990,
        isMarried: false
    }
    var mark = {
        name:'Mark',
        yearOfBirth: 1992,
        isMarried: false
    }
    
we can also build something like blueprint from which we can generate as many as object as we want.

    Person 
        - name
        - YearObjectBirth
        - Job
        - CalculateAge()
in JS we call constructore or prototype, but in other programming language call class
.Based on the contructor we can create many instances as we want

Now let look at inheritance which is when one object is based on another object. When one object get access to another object's properties and methods. 

example
Assume we have an Athlete constructor

    Athlete
        - olympics
        - olympicsMedal
        - allowedOlympics()
An Athlete also have an name, yearOfBirth, and job. We can make the Athlete object inherit the properties and method from the Person obejct. So Athlete is not only access to its own properties and methods but also the ones on Person object.

Person --> Athlete

let see how JS handle inheritance with our Person and Job examples

JS is a prototype-based language, which mean that inheritance work by using somthing called prototypes. That mean each of obejct in JS has prototype property which man inheritance possible in JS

    Person
        - Prototype
            - CalculateAge()
 
When Person is the contructor and John is one of the instances, now we want john to inherit a method or a properties from the Person object, we have to add that method or property to the Person's prototype property. In this example, we have to calculate calculateAge() in prototype property and therefore John can inherits the method, and can then call it, and any other object created by the Person constructor would inherit as well. 

Prototype of an object is where we put methods or properties that we want other object is inherit. One important to know here is Person prototype is not the prototype itself, but all instance that we created through the Person blueprint. Like the Person prototype is Prototype of John. 

    John
        -John
        -1990
        -teacher
        -Prototype
The person Object itself is an instance of an even bigger constructor which is object object. Each of every object we ever created is an instance of the Object constructor. 

    Object
        -prototype
            - hasOwnProperty()
            - isPrototype()
            - toString()
            ...
 

So the prototype chain is what makes all inheritance possible. When we try to access a certain method or property of an object, JS first try to find that method on that exact object, but if it cannot find it, it will look in object's prototype, which is the prototype property of its parent. If the methos is still not there, it will continue still no more protype to look up, which is null this case undefined is returned. 

## 61 Creating Objects: function constructors

support we want to create a lot of objects with different names and ages and jobs. We can use a blueprint for that, function constructors which imply a function

var Person = function(name, yearOfBirth, job){
    this.name = name;
    this.yearOfBirth = yearOfBirth;
    this.job = job;
}

var john = new Person('John', 1990, 'teacher');

John object is an instance of Person object. The 'new' operator create an empty object, and the 'this' operator' point to the empty object, but not the global object, finally assign the new object to john

now do inheritance

var Person = function(name, yearOfBirth, job){
    this.name = name;
    this.yearOfBirth = yearOfBirth;
    this.job = job;
    this.calculateAge = function(){
        console.log(2016 - this.yearOfBirth);
    }
}

var john = new Person('John', 1990, 'teacher');
john.calculateAge(); // 26

var mark = new Person('mark', 1991, 'doctor');
var jame = new Person('jame', 1980, 'engineer');

now we can call calculateAge for mark and jame too, but that is not efficient because if we have 20 function and each function has 100 line of code, we should create 3 objects. In this case we use prototype which we put all functions and properties which is inherited by other object. 

var Person = function(name, yearOfBirth, job){
    this.name = name;
    this.yearOfBirth = yearOfBirth;
    this.job = job;
}

Person.prototype.calculateAge = function(){
    console.log(2016 - this.yearOfBirth);
}

Now we can call the calculateAge function

john.calculateAge();
mark.calculateAge();
jame.calculateAge();


we can inherit property 

Person.prototype.lastName = 'Smith';

console.log(john.lastName);
console.log(mark.lastName);
console.log(jame.lastName);


we can use hasOwnProperty to find if it is property of the object or prototype
instanceof property to check if it is an instance

console.info(x); to see the information of the object

## Creating Objects: Object.create

We will talk about another way that we can create an object that inherit from a prototype. That is called the object.create method. 

we first define the object that will act as the prototype and then create a new object base on that prototype. 

var personProto = {
    calculateAge: function(){
        console.log(2016- this.yearOfBirth);
    }
}

//now let create john object

var john = Object.create(personProto);

// new we can put properties of john
john.name = 'john';
john.yearOfBirth = 1990;
john.job = 'teacher';

//not ideal, because Object.create can accept another parameter

var jame = Object.create(personProto, {
    name: {value: 'Jane'},
    yearOfBirth: {value: 1969},
    job: {value: 'designer'}
});

Object.create build an object which inherit to the object on the first argument. 


## Primitives vs Objects

variable contains primitives actually hold data inside of the variable itself. On object is very different, variable associated with objects not actually contain the object, but instead they contain a reference to the place in memory where the object sits, so where the object is stored. So again the variable declared as an object does not have a real copy of the object. it just points to that object. 

//primitives
var a = 23;
var b = a; // copy the value of a to b
a = 4; // change the value of a not effect to b
console.log(a, b) // a = 4, b = 23

each variable contain its own value, it does not reference anything

let see object
var obj1 = {
    name= 'John',
    age = 27
}
var obj2 = obj1; // we didn't now create a new object, we just create a new reference which points to the first object, so the obj1 and obj2 both hold the same reference. That is why when we change the age in obj1 the change also reflected on the object two. Function work exact the same way

obj1.age = 30;

console.log(obj1.age); // 30
console.log(obj2.age); // 30

// function 

var age = 27;
var obj = {
    name: 'tim',
    city: 'vancouver'
}

function change(a,b){
    a = 30;
    b.city = 'toronto'
}

change(age,obj);

console.log(age, obj.city); // age = 27 obj.city = 'toronto'

## 65 First class functions: Passing Functions as Arguments

function is also an object, we can do somthing with function as we can do with object

- A function is an instance of the Object type
- A function behaves like any other object
- We can store function in a variable
- We can pass a function as an argument to another function
- We can return a function from a function
so we have first class function in js

var years = [1990, 1965, 1968, 2005, 1934];

we want to do some calculate with the values in the array. We can have a huge function with have all calculation that we want to perform at the same time and result all results arrays at the same time, but that is very not good practive. 

in stead, we can write a function that will receive an array, and return a new result array and do the calculations based on these values 

// fn which does calculation, arr is passed in
function arrayCalc(arr, fn){
    var arrRes = [];
    for(var i=0; i< arr.length; i++){
       // we pass function fn into push method and pass the current element of our input into the fn function 
       arrRes.push(fn(arr[i]))
    }
    return arrRes;
}

let write the fn function which is call callback function, because they are functions that we pass into functions that will then call them later. in the case our callback function is fn because they are functions that we pass into functions that will call them later.

function calculateAge(el){
    return 2016 - el;
}

var ages = arrayCalc(years, calculateAge); // we don't want to call right, but later

console.log(ages);

we can also write another callback function for arrayCalc for another purpose

function isFullAge(el){
    return el > 18;
}

var fullAges = arrayCalc(years, isFullAges);
console.log(fullAges);


## First Class Fuctions: Functions Returning functions

function returns another function

function interviewQ(job){
    if(job === 'designer'){
            return function(name){
               console.log(name + ' can you please explain what UX design is?');
            }
        }else if(job === 'teacher'){
            return function(name){
                console.log(name + ' can you please explain what UX design is?');

        }
    }
}

var teacherQuestion = interviewQ('teacher');
// because the function return a function, so we can use var teacherQuestion as a function
teacherQuestion('John');

// we can also do in one line 

var teacherQuestion = interviewQ('teacher')('John');

 // we can also create the desinger question 
 var designerQuestion = interviewQ('designer')('Tim');
 
 ## Immediately invoked Function Expression (IIFE)
 
 IIFE is extremly popular pattern in js, imagine that we want to build a liite game where we win the game if a random score from zero to nine is greater or equal to five, and lose if it's smaller. we do create a function

 function game(){
    var score = Math.random() *10;
    console.log(score >= 5);
}

game();

but there is some problem with that. Let assume the only purpose that we only want to hide the score variable from outside the world, so we should create a new function and call it. However, we can do a better way. 

we can do this with IIFE

we start with a parenthese and then anonymous function so without the name, put the function content inside the anonymous function, and then invoke it by another parenthese

(function() {
    var score = Math.random() * 10;
    console.log(score >= 5);    
})();

how does it work?

if we start with an anonymous function 

function (){}

the JS will thought an error, because js treat it as function declaration, and JS parser will though an error. So we need to treat the JS parse understant it is the function expression not declaration, so we wrap entire thing with an parenthese. after that we only invoke it, because if we don't invoke it, the function would never be called. 

we can pass an parameter into the function, 

(function(goodluck) {
    var score = Math.random() * 10;
    console.log(score >= 5);    
})(5);

we purpose of the IIFE is not used for reusable, but have something is hidden from outside scope. So with this we obtain data privacy and also don;t interferce with other variable in global execution content, and code modularity.

## Closures

this is the most crucial and advance techinue in JS, and this is the most difficult concept to understand for beginners. Let assume we want to write a function return another function which calculate how many year we have left for retirement. 

function retirement(returementAge){
    var a = ' year left untill retirement.';
    return function(yearOfBirth){
        var age = 2016 - yearOFBirth;
        console.log((retirementAge - age) + a);
    }
}

var retirementUS = retirement(66);

retirementUS(1990);


we start with the retirement function and pass retirementAge to this, and then the variable a is declared, and then return a function. When the function is finished, its execution context gets popped off the stack, 

we stored the return funtion as retirementUS, here we use the a and retirementAge which are declared outside the anonymous function. Some how we still can use these variable when the retirement already stop execution. 

This is closure

An inner function has always access to the variables and parameters of its outer function, even after the outer function has returned.

the variable object still in the memory even the function is already return.(execution stack and scope chain)
how is closure usefull?

we can create three diffrerent functions for countries for different retirement ages, and use the closure over and over again. 

This is cleaner code

## The Bind, Call, and Apply

Call function is also called borrow function, let assume that John object has a function called presentation

var john = {
    name: 'John',
    age: 34,
    job:'teacher',
    presentation: function(style, timeOfDay) {
        if(type==='formal'){
            console.log('Good ' + timeOfDay + this.name + this.job);
        }
    }

}

so now we can call john.presentation('formal', 'Morning');

let assume now we have another object call Emily

var Emily = {
    name: 'Emily',
    age: 30,
    job: 'designer'
}

we want to use presentation of John for Emily, we can use call function

John.presentation.call(Emily, 'formal','Afternoon');

When we include Emily of=bject in call functin, the this in the presentation function will become the this of Emily

We also have another method which is very similar to Call method except the Apply method accept the argument as an array


John.presentation.apply(Emily, ['formal','Morning']);

but doesn't work because or function not accept the array in orginal

The bind method is very similar to Call method as well, but bind doesn't immediation call the this function, but create a copy of the function, so we can stored it some where

var johnNormal = john.presentation.bind(john, 'normal'); // bind return a function

johnNormal('Morning'); // just need to set another argument 

Now we can use it for Emily

var EmilyFormal = john.presentation.bind(emily,'formal');

emilyFormal('afternoon');

bind allow to copy the function with some preset

let look back the arrayCalc function

/ fn which does calculation, arr is passed in
function arrayCalc(arr, fn){
var arrRes = [];
for(var i=0; i< arr.length; i++){
// we pass function fn into push method and pass the current element of our input into the fn function 
arrRes.push(fn(arr[i]))
}
return arrRes;
}

let write the fn function which is call callback function, because they are functions that we pass into functions that will then call them later. in the case our callback function is fn because they are functions that we pass into functions that will call them later.

function calculateAge(el){
return 2016 - el;
}

var ages = arrayCalc(years, calculateAge); // we don't want to call right, but later

console.log(ages);

we can also write another callback function for arrayCalc for another purpose

function isFullAge(el){
return el > 18;
}

let assume that we want to calculat the full age of Japanese, 20, we need to pass another argument into the isFullAge function, we can do that with bind function

    function isFullAge(limit,el){
        return el > limit;
    }

but the only have one argument in arrRes.push(fn(arr[i]), we can do that but using bind with preset the limit

var fullJapan = arrayCalc(ages, isFullAge.bind(this, 20))



















































































 






































