## 104. Variable declaration with let and const
    
ES5 var is mutable 
    
    var name5 = 'tim nguyen';
    var dob = 'sep1988;
    name5 = 'tim ly';
    console.log(name5); // will give me tim ly
    
ES6 we can declare variable by two ways, either using let or const. let is mutable, but const is unmutable

    let name6 = 'tim nguyen'; 
    const dob = 'sep1988';
    dob = 'aug1988';
    console.log(dob);// will give us an error

these variables declare with var is function scope in ES5, but variables declare with ES6 is block-scoped.

ES5

    function driverLicien5(pass){
        if(pass){
            var name = 'tim nguyen';
            var dob = 'sep1988';
            console.log('congradualtion to ' + name + 'were born in ' + dob);
        }
    }

and in ES6

    function driverLicien5(pass){
        if(pass){
            let name = 'tim nguyen';
            const dob = 'sep1988';
            console.log('congradualtion to ' + name + 'were born in ' + dob);
        }
    }
there are the same, but if we try to console.log outside the if, ES5 still work, but ES6 doesn't allow

What happen if we declare the variable before the if statement

    //it still work in ES5
    function driverLicien5(pass){
        var name, bob;
        if(pass){
            name = 'tim nguyen';
            dob = 'sep1988';
        }
        console.log('congradualtion to ' + name + 'were born in ' + dob);
    }
    
    // but cause the problem with const without declaration in ES6
    function driverLicien5(pass){
        let name; 
        const bob; // can cause problem without initializing
        if(pass){
            name = 'tim nguyen';
            dob = 'sep1988';
        }
        console.log('congradualtion to ' + name + 'were born in ' + dob);
    }
    
In ES6, if we declare const dob outside the if, then the function can be executed without error

Another different is before we actually declare the var in ES5, we can console.log or use it with valua undefined

    function driverLicien5(pass){
        if(pass){
            console.log(name); // will give us undefined
            var name = 'tim nguyen';
            var dob = 'sep1988';
            console.log('congradualtion to ' + name + 'were born in ' + dob);
        }
    }
    
but in ES6, it will cause an issue during execution process

with the bock-scope we can declare the same variable name with outside the scope in ES6, 

    let i = 20; 
    for(let i = 0; i < 5; i++){
        console.log(i); // 0, 1, 2, 3, 4
    }
    console.log(i); // 20

However, the varibale with declare by var in ES5 can be mutated because it is function scope

    let i = 20; 
    for(let i = 0; i < 5; i++){
        console.log(i); // 0, 1, 2, 3, 4
    }
    console.log(i); // 5

## 105. Blocks and IIFEs

new way of creating IIFES. From the last lecture we have seen how to use const and let which are block-scope. That means we can not access these variables outside the scope. Yeah, that is data privacy. However, in ES5 we have seen immediately-invoked function expression for that. In ES6, we will see the new way of achieving data privacy because all we have to do is use the block. we just need {} and put some codes inside it. that we already create a block. 

    {
        const a = 1;
        let b = 2;
        var d = 3;
    }

    console.log(a + b); // error can not access
    console.log(d); // 3 we still can see d because of var keywork function scope
in ES5

    (function(){
        var c = 3;
    })();
    console.log(c);

## 106. String in ES6

    let firstname = 'tim';
    let lastname = 'ngoc';
    const yearOfBirth = 1988;
    
    function calcAge(year){
        return 2018 - year;
    }
    
ES 5, do we concationation by using + operand, this is a bit hard to write

    console.log('This is ' + firstName + ' ' + lastname + 'born in  ' + yearOfBirth + ' Today, he is ' + calcAge(yearOfBirth));

But in ES6, we have cool thing called template literals and it work like this

    console.log(`This is ${firstName} ${lastName} born in ${yearOfBirth}`);

we also have some more string methods

    const n = `${firstName} ${lastName}`;
    console.log(n.startWith('J')); // true or false
    console.log(n.endWith('J'));
    console.log(n.includes('J')); // in the midle
    console.log(`${firstName} `.repeat(5)); repeat 5 times on the row, john john ...
    
## 107. Arrow functions: basic
    
    const year = [ 1990, 1956, 2000, 2002];
    // then ES5 we can use map function which contains 3 paramenter, current, index, and entire array
    var ages5 = year.map(function(el){
        return 2018 - el;
    });
    console.log(ages5);
    
In ES6 we can use arrow function give the same result
    
    let ages6 = year.map(el => 2018 - el);
    console.log(ages6);
give the same result but let code, in case if we have more than one parameters

    let ages6 = year.map((el,index) => {
        return `Age element ${index + 1}: ${2018 - el}.`;
    });

## 108. Arrow functions: Lexical this keyword

Maybe the big advantage of using arrow functions is that they share the surrounding this keyword. that means, unlike normal functions, arrow functions don't get thier own this keyword. they are simply use the this keyword of the function called them. 

ES5 

    var box5 = {
        color: 'green';
        position: 1;
        clickMe: function(){
            document.querySelector('.green').addEventListener('click',function(){
                var str = 'this is box number ' + this.postion + ' ' + this.color;
            });
        }
    }
    
the result is undefined for this keyword. Why is it not reading our values from this object here?

as we mention before, only the method call, the this actually points to that object. by the regular function call, this keyword points to the global object. In this case, the function in addEventHandler is the regular function call, and the click me is the method call.

to handle this issue in ES5, we can use the hack by calling another variable call self in clickMe

    var self = this 

and change all this keywords to selft, so that should be work in ES5

However, in ES6 we can use arror function for regular function call, so this keywork can be access value from the object called it

    var box5 = {
        color: 'green';
        position: 1;
        clickMe: function(){
            document.querySelector('.green').addEventListener('click', () => {
                var str = 'this is box number ' + this.postion + ' ' + this.color;
            });
        }
    }

furthermore, if we change the method call by another arrow function, the result is undefined again because this keywork in clickMe share with surrounding this keyword which is global this keyword which is undefined

    
    var box5 = {
        color: 'green';
        position: 1;
        clickMe: () =>{
            document.querySelector('.green').addEventListener('click', () => {
                var str = 'this is box number ' + this.postion + ' ' + this.color; // undefined
            });
        }
    }
    
Let creat an function constructor in order to create a Person object

    function Person(name) {
        this.name = name;
    }
    
    Person.prototype.myFriends5 = function(friends){
        var arr = friends.map(function(el){
            return this.name + 'this friends with ' + el;
        })
    }
    
    var friends = ['Bob','Jane','Mark'];
    new Person('john').myFriends5(friends);

the this.name will return an undefined because it point to global object. Why? 
Inside the

    function(friends){
        
    }
We can definitely access to the this.name, however, we call another function which is anonymous function which make the this key word point to global object. 

we can use the trick as before, or we can use the bind method
    
    Person.prototype.myFriends5 = function(friends){
        var arr = friends.map(function(el){
            return this.name + 'this friends with ' + el;
        }.bind(this));
    }
    
now ES6 version

    Person.prototype.myFriends5 = function(friends){
    var arr = friends.map(el => `${this.name} + this friends with + ${el}`);
    }
    
## 109. Destructuring

Give us vary convenient way to extract data from data structure or an array. 

ES 5
    
    var john = ['john', 26];
    var name = john[0];
    var age = john[2];
    
ES6

    //destructuring
    const [name,age] = ['john',26];
    console.log(name); // john
    
destruturing also work with object

    const obj = {
        firstName: 'john',
        lastName: 'Smith'
    }
    
    const {firstName, lastName} = obj;
    console.log(firstName);
    
if we don't want variable match with element name, we can do like 

    const {firstName: a, lastName:b} = obj;

    console.log(a)
    
we can also apply destructuring for function

    function calcAgeRetirement(year){
        const age = new Date().getFullYear() - year;
        return [age, 65 - age];
    }
    
    const [age, retirement] = calcAgeRetirement(1990);
    console.log(age);
    console.log(retirement);
    
## 110. Arrays in ES6/ ES2015

Assume we want three boxes have the dodgerblue color, first select all boxes

    const boxes = document.querySelectorAll('.box');
    
in ES5
    
    var boxArr5 = Array.prototype.slice.call(boxes);
    boxArr5.forEach(function(cur) {
        cur.style.backgroundColor = 'dodgerblue';
    });
In ES6

    Array.from(boxes).forEach(curr => curr.style.backgroundColor = 'dodgerblue');

for break and continue, we can only use for normal for loop in ES5

ES5
    
    for(var i = 0; i < boxesArr5.length; i++){
        if(boxesArr5[i].className === 'box blue'){
            continue;
        }
        boxesArr5[i].textContent = 'I changed to blue';
    }

in ES6, we have new for loop call for of

    for(const cur of boxesArr6){
        if(curr.className.include('blue')){
            continue;
        }
        cur.textContent = 'I changed to blue';
    }
Another usefull of ES6 to find and value in array with given condition

for example, to find value if this is full age

    var age = [12,17,18,23,14,11];
    var full = age.map(function(cur){
        return cur > 18;
    });
    
    console.log(full);
    console.log(full.indexOf(true));
    console.log(age[full.indexOf(true));
    
there is a lot of work in ES5, but in ES6 we can use find method

    ages.findIndex(cur => cur >= 18); // return the index of the true
    ages.find(cur => cur >= 18); // 23
    

## 111. The Spread Operator (...)

Assume we have a function which calculate the sume of other 4 numbers

    function addFourNum (a, b, c, d){
        return a + b + c+ d;
    }
    
    var sum1 = addFourNum(18, 30, 12, 21);
    
in ES5, we can use the apply method to input the parameters 
    
    var ages = [14, 12, 34, 45];
    var sum2 = addFourAges.apply(null, ages);
    
In ES6, we can use the spread operator ... to make the elements of array become components like 14, 12, 34, 45

    const sum3 = addFourAges(...ages);

we can use spread operator to join two arrays

    const familySmith = ['John', 'Jane','Mark'];
    const familyMiller = ['marry','Bob','Ann'];
    
    const bigFamily = [...familySmith, ...familyMiller]; ['john',...,'marry',...]

We can use spread operator to combine selector from html

    const h = document.querySelector('h1');
    const boxes = document.querySelectorAll('.box');
    
    const all = [h, ...boxes];
    // convert all the an array
    Array.from(all).forEach(cur => cur.style.color = 'purple');
    

## 112. Rest parameter

Receive couple values and then tranform them into an array. 

ES5
    
    function isFullAge5(){
        console.log(arguments);
    }
    
    isFullAge5(1990,1993,1994); // the arguments is [1990,1993,1994] is not an array yet, we need to transform
    var argsArr = Array.prototype.slice.call(arguments);
    
    argsArr.forEach(function(cur){
        console.log((2016-cur) >= 18);
    });
    
if we have 100 arguments, it still work

ES6

    function isFullAge6(...years){ //years is an array now
        console.log(years);
        years.forEach(cur => console.log((2016-cur) >= 18);
    }
    
    isFullAge6(1990,1993,1994);
    
    


## 113. Default Parameters

we use default parameters whenever we want one or more parameters of a function to be preset

ES5

    function timFamily(fn, dob, ln, nationality){
        ln === undefined ? ln = 'Nguyen': ln = ln ;
        nationality === 'undefined' ? nationality = 'Vietnamese' : nationality = nationality;
        
        this.fn = fn;
        this.dob = dob;
        this.ln = ln;
        this.nationality = nationality;
    }
    
    var tim = new timFamily('tim','1998');
    

in ES6

    function timFamily(fn, dob, ln = 'Nguyen', nationality='Vietnamese'){
        this.fn = fn;
        this.dob = dob;
        this.ln = ln;
        this.nationality = nationality;
    }

## 114. Maps (good)

like hash map, we can create a map with key and value, key anh value can be anything

    const question = new Map();
    question.set('question','What is the offical name of the latest major javascript version?');
    question.set(1,'ES2015');
    question.set(2,'ES5');
    question.set(3,'ES6');
    question.set(4,'ES7');
    question.set('correct',3);
    question.set(true,'correct answer');
    question.set(false,'wrong please try again');
    
we want to retreive data from the map

    console.log(question.get('question'));
    console.log(question.size);
    
check data is in the map

    // delete
    question.delete(4);

we need to check first

    if(question.has(4)){
        question.delete(4);
    }

we can clear everything at the same time

    question.clear();

map can be iterable 

    question.forEach((value,key)=> console.log(`this is ${key}, and it's set to ${value}`));
    
    // or use for of
    for(let [key,value] of question.entries()){
        if(typeof(key) === 'number'){
            console.log(`this is ${key}, and it's set to ${value}`);
        }
    }

    const ans = parseInt(prompt('write a correct answer')); 
    console.log(question.get(ans === question.get('correct')));
    
    

## 115. Classes

In ES5, we can create a class like

    var Person5 = function(name, yearOfBirth, job){
        this.name = name;
        this.yearOfBirth = yearOfBirth;
        this.job = job;
    }
    
    // add method
    Person5.prototype.calculateAge = function(){
        var age = new Date().getFullYear - this.yearOfBirth;
        console.log(age);
    }   
    
    var john5 = new Person5('john',1990,'teacher');
    

In ES6
    
    class Person6 {
        constructor(name, yearOfBirth,job){
            this.name = name;
            this.yearOfBirth = yearOfBirth;
            this.job = job; 
        }
        calculateAge(){
            var age = new Date().getFullYear - this.yearOfBirth;
            console.log(age);
        }
    }
    
    const john6 = new Person6('john',1990,'teacher');

we can create a static method which doesn't need to call the 'new', we can access this method directly

    static greeting(){ console.log('hi there');}
    
    
## 116. Classes with Subclasses

    Person (person inderit Athlete)
        - name
        - year of birth
        - job
        - calculateAge
    Athlete
        - olympics
        - olympicMedals
        - wonMedal()
so we want 

    Athlete
        - olympics
        - olympicMedals
        - wonMedal()
        - name
        - year of birth
        - job
        - calculateAge


in ES5

    
    var Person5 = function(name, yearOfBirth, job){
        this.name = name;
        this.yearOfBirth = yearOfBirth;
        this.job = job;
    }
    
    // add method
    Person5.prototype.calculateAge = function(){
        var age = new Date().getFullYear() - this.yearOfBirth;
        console.log(age);
    } 

    var Athlete5 = function(name,yearOfBirth,job,olympicGames,medals){
    
        Person5.call(this, name, yearOfBirth,job);
        this.olympicGames = olympicGames;
        this.medals = medals;
    }

we want Athlete5 to use the calculateAge method in Person5

    Athlete5.prototype = Object.create(Person5.prototype);
    var johnAthlete5 = new Athlete5('john','1990','swimmer',3,10);
    
    johnAthlete5.calculateAge();

we can also set methods on prototype property on Athlete5, the method only for Athlete5, the Person doesn't inherit this methos 

    Athlete5.prototype.wonMedals = function(){
        this.medals++;
        console.log(this.medals);
    }

this function must be created after the Object.created ..

in ES6

    // super class
    class Person6 {
        constructor(name, yearOfBirth,job){
            this.name = name;
            this.yearOfBirth = yearOfBirth;
            this.job = job; 
        }
        calculateAge(){
            var age = new Date().getFullYear - this.yearOfBirth;
            console.log(age);
        }
    }

    //subclass
    class Athelete6 extends Person6 {
        constructor(name, yearOfBirth, job, olympicGame, medals){
            super(name,yearOfBirth, job);
            this.olympicGames = olympicGames;
            this.medals = medals;
        }
        
        wonMedal(){
            this.medals++;
            console.log(this.medals);
        }
    }
    
    const johnAthelete6 = new Athlete6('John',1990,'swimmer',3,10);
    johnAthelete6.wonMedal();
    johnAthlete6.calculateAge(); // can use method in super class


