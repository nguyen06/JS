## 120 An example of Asynchronous JS

let assume we have two functions calling one another

    const second = () => {
        console.log('this is seconde');
    }
    const first = () =>{
        console.log('Hey there');
        second();
        console.log('end');
    }
    
    first(); 
    
it is going to print
    
    hey there
    this is second
    end

if we use a settimeout function in second function let wait for two second. Settimeout function is a function we can pass callback and also the time, first is a callback and second is time which is wait for how long the callback function will run

    const second = () => {
        setTimeout(()=>{
            console.log('Async Hey there');
        }, 2000);
    }

that is ansynchronous programming in js


## 121 - Understanding Asynchronous JS: The Event loop (Important lecture)

we have already talked about global execution context and message queue, today we will talk about web API which is runing on web browser. The combination of all of them will make JS runtime and explain how JS work behind the scenes.

the execution start by calling the first function and as we know that the execution Context for this function will put on the top of Global Execution Context Stack. In the next line of code, the console log function is called and new Execution Context is created and the text is logged to the console, the function return and the Execution Context pops off the stack. Moving to the second function, a new Execution Context is created nd in next line the Set Time Out function is called which causes yet another Execution Context to be created. The Set Time Out function lives outside the JS engine. it is a part of the Web APIs, we have access to them because they are also in a JS runtime. This is exactly where the timer will keep running for two seconds, Asynchronously of course, so that our code can keep running without being blocled. When we call Set Time Out function, the timer is created together with callback function right inside the Web APIs environment, and it keep running until it finishes its work all in the Asynchronous way. Since the timer keep working basically in the background, we don't have to wait and can keep executing out code. Next the Set Time Out function returns and pop off the stack and so does the Execution Context of the second function. Now we about to initial first function Then the Execution Context of the console.log of the last console.log is put on the top of the stack and console log the message and then pop off the stack again. Next the function return and so we are back to our orginal state. Now let support our 2 second have passed and the timer disappeares. But what happens to our callback function now? Well, it simply moves to the Message Queue, where it waits to be executed as soon as the Execution Stack is empty. This is exactly what happen with DOM events as well. And that's where the Event Loop comes in. The job of the event loop is to constantly monitor the Message Queue and the Execution Stack, and to push the first callback function in line onto the Execution Stack as soon as the stack is empty. In our code right now the Execution Stack is empty and the Event Loop put the callback to the Stack where the new Execution context is created for that function and simply run the log function which print something to the console

Now if there are some callback waiting right now in the Message Queue like data coming back from an AJAX request, or the handler of a DOM event, then the Event Lopp would continue to push them onto the stack until all of them were processed. 


## 122 The Old way: Asynchronous JS with callbacks

Let assume we have to write a function to get recipe, first we sent an ajax request to get recipe id

    function getRecipe(){
        setTimeout(() => {
            // asume we get a array of recipe id
            const recipeID = [523,883,223,334];
            console.log(recipeID);
            
            // Now we want to get recipe based on id
            setTimeout(id => {
                const recipe = {title: 'Tomato sause with chicken', publisher:'tim nguyen'};
                console.log(`${id}: ${recipe.title}`);
                
                //Now we want to get a group of recipe with the same publisher
                setTimeout(publisher => {
                    const recipe2 = {title: 'Italian Pizza', publisher: 'Jonas'};
                    console.log(recipe);
                }, 1500, recipe.publisher);
            }, 1500, recipeID[2]);
        }, 1500);
    }

## Callback hell to Promise

promise is an ES6 feature designed specifically to deal with asynchronous JS. 

Promise
    - Object that keeps track about whether a certain event has happened already or not
    - Determines what happens after the event has happened
    - Implements the comcept of a future value that we're expecting

Promise states

before the event has happened, the promise is pending, then after the event has happened the promise is called settled or resolved which is the same thing. When the promise is successful, that's mean the result is available, and the promise is fulfilled, but if there was an error, then the promise is rejected. That is important to know bc we will handle these two different situations. 

we can produce and resume a promise. When we produce a promise, we create a new promise and send its result using that promise. Then when we consume it we can use callback functions for fulfillment and for rejecjtion of our promise. 

    const getIDs = new Promise((resolve, reject) => {
        // send a fake ajax request to server
        setTimeout(() => {
            // when we get a result we forward to resolve state 
            resolve([1,3,45,6]);
        }, 1500);
        
    });

we produces very first simple promise which is stored in this getIDs variable. Now it is actually time that we consume this promise, and in order to do that we can use two methods: then and catch

    getIDs.then(IDs => { // for resolve
        console.log(IDs);
        }). catch(error => { // for reject
        console.log(error);
        })

Now we will create a new promise for getRecipe which receive an id from getIDs

    const getRecipe = recID => {
         return new Promise((resolve,reject)=>{
            // send an ajax request to get data
            setTimeout((id)=>{
                const recipe = {title: 'tomato pause with potato'}, publisher:'tim nguyen'};
                resolve(`${id}: ${recipe.title}`);
            },1500, recID);
        });
    }

Then in the result of getIDs we should call the getRecipe, however, in order to consume the promise result we need to return it from getIDs then

    getIDs
    .then(IDs => { // for resolve
        console.log(IDs);
         return getRecipe(IDs[2]);
    })
    
    // then can handle the fulfilment of getRecipe
    .then(rec => {
        console.log(rec);
    })
    . catch(error => { // for reject
        console.log(error);
    })

Now we can crate getRelated publisher by create a new function and return promise

    const getRelated = publisher => {
        return new Promise((resolve, reject)=>{
            setTimeout((pub)=>{
                const recipe = {title: 'tomato pause with potato'}, publisher:'tim nguyen'};
                resolve(`${pub}: ${publisher}`);
            },1500,publisher);
        });
    }

similarly with getRecipe, the getRelated will be called in the then of getRecipe

    getIDs
    .then(IDs => { // for resolve
        console.log(IDs);
        return getRecipe(IDs[2]);
    })

    // then can handle the fulfilment of getRecipe
    .then(rec => {
        console.log(rec);
        return getRelated(rec.publisher);
    })
    .then(pub => {
        console.log(pub);
    })
    . catch(error => { // for reject
        console.log(error);
    })

## 124 From Promise to Async/Await

The syntax to consume promises are quite confusing and difficult to manage. There are something call Async/Await is introduced to JS, Asyns/Await was designed for us to consume promises, and not to produce them.

let assum we have promise, now don't use then and catch

    async function getRecipeAW(){
        const IDs = await getIDs;
        // how to handle the error next lecture
        const recipe = await getRecipe(IDs[2]);
        const related = await getRelated('tim');
    }
    
    getRecipeAW();

the function getRecipe() is running asynchronously in the background, if we try to return the from async function which doesn't work.  because it is still pending. We can use "then" to get the return from async function

    async function getRecipeAW(){
        const IDs = await getIDs;
        // how to handle the error next lecture
        const recipe = await getRecipe(IDs[2]);
        const related = await getRelated('tim');
        
        return recipe;
    }

    getRecipeAW().then(result => console.log(`${result} is the best result`));


## 125 AJAX and APIs

AJAX  stand for Asynchronous JavaScript and XML, basically it allows us to asynchronously communicate with remote servers. let says we have out js app running in the browser, which is called the client, and we want the app to get some data from our server. but of course without having to reload the entire page. Well with AJAX we can do a simple HTTP request to our server which will then can send back a response contanining the data that we requested. And this all happens asynchronously in the background just line the way we learned before. This is not only works for getting data from the server, but also to send data to the server by doing a post request rather than a get request. We are gona look at the fetch web API in the next lecture which allow us to make some AJAX calls in a very easy way. 

Alright, API stands for Application Programming Interface and on a very high level, it's basically a piece of software that can be used by another peice of software in order to basically allow applications to talk to each other. APIs is not the server itself, but it's like a part of the server. Like an application that receives requests and send back responses. 

Let's distinguish two types of APIs: your own APIs and External - third party APIs.

you can build your own API that can receive requests from your front end app in JS and send back the results. So that would be your own API hosted on your own server. 

But if you need some current weather information for each destination, which you don't have in your database, you can use third party API to get that weather data. 

Third party API likes including Google Map, embed YouTube video to get data about movies, even send email or text message from the Venmo app. 

## 126 Making AJAX calls with Fetch and Promise

Fetch is in morder web APIs not from JS engine

    fetch('https://www.metaweather.com/api/location/2487956/');

but we got an error called 'No Access-allow-Ogirin', the reason of that is calling the same orgin policy in JS, which basically prevent us from making AJAX requests to a domain different than your own domain. Now in order to allow developer to make requests to different domains, somthings called Cross Origin Resource Sharing or CORS was developed, but it is not included in metaweather data, but we can fortunately solve this by proxy or to chanel the request through thier own server. (Like doing AJAX request on your own server where the same origin policy doesn't exist and then send the data to the browser). We do CORS here because we don't have our own server in this example, and of course we want to keep it simple. 

crossorigin.me

A CORS proxy is a service that allows developers to access resource from other website without having to own that website. 

    fetch('https://crossorigin.me/https://www.metaweather.com/api/location/2487956/')
    .then(result => {
        console.log(result);
    })
    .catch(error => console.log(error));
    
body is readableStream so we need to convert it to js

    function getWeather(woeid){
        fetch(`https://crossorigin.me/https://www.metaweather.com/api/location/${woeid}/`)
        .then(result => {
            console.log(result);
            return result.json();
            }).then(data => {
                console.log(data);
            })
        .catch(error => console.log(error));
    }
    
    getWeather(2487956);

## 127 Making AJAX calls with Fetch and Async/Await

    async function getWeather(weoid{
        const result = await fetch(`https://crossorigin.me/https://www.metaweather.com/api/location/${woeid}/`);
        const data = await result.json();
        console.log(data);
    }

    getWeather(2487956);

Now how to catch error from Async/Await, we can use try/catch

    async function getWeather(weoid){
        try{
            const result = await fetch(`https://crossorigin.me/https://www.metaweather.com/api/location/${woeid}/`);
            const data = await result.json();
            console.log(data);
        }catch(error){
            alert(error);
        }

    }

    getWeather(2487956);

we want to return some data from this function

    async function getWeather(weoid){
        try{
            const result = await fetch(`https://crossorigin.me/https://www.metaweather.com/api/location/${woeid}/`);
            const data = await result.json();
            console.log(data);
            return data;
        }catch(error){
            alert(error);
        }

    }

The below line of code doesn't becuase it return a promise, 

    const dataLondon =  getWeather(2487956);
    console.log(dataLondon); // don't work because we don't have data

then we need to use then method

    let dataLondon;
    getWeather(44418).then(data => dataLondon = data);
    console.log(dataLondon);

doesn't work either

    let dataLondon;
    getWeather(44418).then(data => {
        dataLondon = data;
        console.log(dataLondon);
    });
  
work fine




