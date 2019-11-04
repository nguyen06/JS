## 129 Project overview

Two folders: dist and src, we work on src folder and dist folder are already to deploy

the src folder has js folders which contain models, views and index.js and index.html


## 130 Modern JS

Now we can write js with different tool which makes it easier and better to work with. 

- NodeJS and NPM ecosystem where we can find all kind of third party open source tools such as library framework needed like REACTJS, Angular, and even Jquery. In order to use and share these packages, we need some kind of tools which is called NPM (node package manager) 
- NPM is just a simple command line interface, and also allow us to write script to use our development tools, 

- the first tool we use in JS is BABEL which convert cutting adge JS6/EXNext to ES5

- We want to make our code more modular which is in ES6, but we need to bundle these modules together into single file using something called a module bundler and the most popular one out there is called Webpack. Webpack can do so much more than just bundling modules. 

bundle and webpack are npm packages. 

## 131 A brief introduction to the command line
    touch test.js
    copy NUL test.js // in window
    
    //copy file 
    copy test.js .. // copy to parent folder
    
    //moving a file
    mv test.js ..
    
    // delete file
    rm test.js // remove permanently 
    
    // delete folder
    rm -r test
    
    rmdir /S test // in window
    
    // open file in directory
    open index.html
    

## 133 A modern Setup: installing NODE.js and NPM

moving into the project folder and do 
    
    npm init

answer some questions to create packe.json file

install webapack

    npm install webpack --save-dev
    npm install --save-dev webpack@4 webpack-cli@2 webpack-dev-server@3
    npm install --save-dev babel-core@6 babel-preset-env@1 babel-loader@7
we only need to share and another people can only do
    
    npm install

install package

    npm uninstall jquery --save

install live server

    sudo npm install live-sever --global

and run server
    
    live-server 
## 134 A Modern Setup: Confifure Webpack

learn how to create a basic webpack configuration, webpack is the most commonly used as a set bundler, not only bundel js files but also css, jpg and png files. bundle to one js file, css file, and image

If we want to use webpack, we need one source folder in the root and then in there one index.js file, and webpack then will automatically create a distribution folder (dist) and put the bundled file in there, Thatr is just for small app. for a big app we will write a configration file 


create a configration file out of scr and dist folder called

    src
    package.json
    node_modules
    dist
    package-lock.json
    webpack.config.js

in here we have one object

in webpack, there are four core concepts: entry point, the output, loaders, and plugins

+   entry point is where the webpack will start the budling, so basically this is the file where it will start looking for all the dependencies which it should then bundle together
+ next we need to specify the output property which tell webpack actually where to save our bundle file, we specify the output property and now in here we actually pass an object. In this object, we put the path, and then the file name

        const path = require('path');

        module.exports = {
            // the entry point where webpack is started bundling
            // ./ current folder
            entry: './src/js/index.js',
            // output
            output:{
                // the path need to be absolute path we need a builded node path
                // the dirname is the current directory
                path: path.resolve(__dirname,'dist/js'),
                filename: 'bundle.js',
            }
        }

now in webpack four have something called "production and development mode". So the development mode simply builds our bundler without minify our code in order to be as fast as posible. But in the production mode will automatically enable all kind of optimization, like minification and tree shaking in order to reduce the final bundle size. 

    const path = require('path');

        module.exports = {
            // the entry point where webpack is started bundling
            // ./ current folder
            entry: './src/js/index.js',
            // output
            output:{
                // the path need to be absolute path we need a builded node path
                // the dirname is the current directory
                path: path.resolve(__dirname,'dist/js'),
                filename: 'bundle.js',
            }
            mode: 'development'
    }

in order to test the code we just wrote, we create a new js file called test.js and 

test.js
    
    export default 34;

in the index.js we import it and console.log

    import num from './test';
    console.log(`i imported the ${num}`);
    
However, we can't see what happen in the browser so far, we need to some more work. in package.json

package.json

    {
        "scripts": {
            "dev":"webpack"
        }
    }

as soon as we start npm run dev, the webpack looks for the configration file here and do the work we specify here

Well one more thing we need to do here is to npm install webpack-cli (command line interface)

    npm install webpack-cli --dev-save

to load the 

    npm run dev

create an index.html in dist folder and include the js script 
    
    <script src="js/bundle.js"></script>

    open dist/index.html

last thing here regarding this node that we have here so we set it to development, but once we're ready then we want to set it back to production but we have to com back to the configration file, so we can do this by

    delete the mode from module.export
    and come to npm script add to
    
    "scripts":{
        "dev": "webpack --mode development",
        "build": "webpack --mode production"
 }

now the site of bundle file is much smaller

## 135 A Modern setup: The webpack Dev Server

In order to make our life is easier, we will set up the webpack dev server to automatically reload the page and save our code. 

    npm install webpack-dev-server --save-dev
    
automatically bundle all js files and then reload the app in a browser whenever we change a file.  In order to configure that we go back to configration file and add 

    

    const path = require('path');

    module.exports = {
        // the entry point where webpack is started bundling
        // ./ current folder
        entry: './src/js/index.js',
        // output
        output:{
            // the path need to be absolute path we need a builded node path
            // the dirname is the current directory
            path: path.resolve(__dirname,'dist'),
            filename: 'bundle.js',
        }
        devServer: {
            contentBase: './dist' // we only want to server run on dist folder, we can also specify the port
        }
    }
    
let make it work by adding new script
    
    "scripts":{
        "dev": "webpack --mode development",
        "build": "webpack --mode production",
        "start": "webpack-dev-server --mode development open"
    }
        
the server will run constantly

    npm run start

Now we want to copy the index.html file from src to dist folder, we can use html-webpack-plugin 

    npm install html-webpack-plugin --save-dev
    
and then we use it by 

    const HtmlWebpackPlugin = require('html-webpack-plugin');
    const path = require('path');
    
    module.exports = {
        // the entry point where webpack is started bundling
        // ./ current folder
        entry: './src/js/index.js',
        // output
        output:{
            // the path need to be absolute path we need a builded node path
            // the dirname is the current directory
            path: path.resolve(__dirname,'dist'),
            filename: 'bundle.js',
        }
        devServer: {
            contentBase: './dist' // we only want to server run on dist folder, we can also specify the port
        }
        plugins: [
            new HtmlWebpackPlugin({
                filename: 'index.html',
                template: './src/index.html',
            })]
    }
    
    
## 136 A modern setup: babel

    babeljs.io

install couple packages

    npm install babel-core bable-preset-env babel-loader --save-dev
    
Now we need to learn the concept of loader in webpack, so loader in webpack allow us to import or to load all kind of different files and process them. Like convert SASS to CSS code or covert ES6 code to ES5 code. So we need babel loader b/c babel convert ES6 to ES5

we need to specify a module in webpack.config.js and then the rule which is an array of loaders

    module.exports = {
        // the entry point where webpack is started bundling
        // ./ current folder
        entry: './src/js/index.js',
        // output
        output:{
            // the path need to be absolute path we need a builded node path
            // the dirname is the current directory
            path: path.resolve(__dirname,'dist'),
            filename: 'bundle.js',
        }
        devServer: {
            contentBase: './dist' // we only want to server run on dist folder, we can also specify the port
        }
        plugins: [
            new HtmlWebpackPlugin({
                filename: 'index.html',
                template: './src/index.html',
            })],
        module: {
            rules: [
            {
                test: /\.js$/, test with all js file
                exclude: /node_modules/, // we don't want node_modules
                use:{
                    loader: 'babel-loader' // the packahe we install before 
                }
            }] 
        }
    }

step 2 we need to set up babel configuration file
create a file name
    
    .babelrc
    
    {
        "presets": [
            ["env": {
                "targets": {
                    "browsers": [
                        "last 5 versions",
                        "ie >= 8"
                    ]
                }
            }]]
    }
so now ES6, 7, 8, or next will be comvert to ES5, but something we still can't convert to ES5 because they are not simply presented in ES5, so these things need to have pollyfilled, and I am talking somthing like Promise and methods line array.from, polyfill will help to add them to our code. 

    npm install babel-polyfill --save

now we will add it to entry point in  webpack.config.js



    module.exports = {
        // the entry point where webpack is started bundling
        // ./ current folder
        entry: ['babel-polyfill','./src/js/index.js'],
        // output
        output:{
            // the path need to be absolute path we need a builded node path
            // the dirname is the current directory
            path: path.resolve(__dirname,'dist'),
            filename: 'bundle.js',
        }
        devServer: {
            contentBase: './dist' // we only want to server run on dist folder, we can also specify the port
        }
        plugins: [
            new HtmlWebpackPlugin({
            filename: 'index.html',
            template: './src/index.html',
        })],
        module: {
            rules: [
            {
                test: /\.js$/, test with all js file
                exclude: /node_modules/, // we don't want node_modules
                use:{
                    loader: 'babel-loader' // the packahe we install before 
                }
            }] 
        }
    }

## 137 Planning our project Architecture with MVC

Thinking about architecture is absolutely fundamental before starting any new project, no matter it's a vanilla JS project or REACTJs app or Node App.  we are going to use MVC model View Controller
many different way to implement MVC

+ Model: 
        for models, we will have different aspects of the app into different files, we will have mutiple models

+ Controller: index.js

+ view
    for view, we will have different aspects of the app into different files, we will have mutiple views. 

for example, in search Model where we do AJAX calls to get recipes for a certain search query from an API. On the other hand, in the View where we get the search query string from the user interface, and also where we print the results of the search. An the controller of course, is what brings these two together, so that Model and View never actually have to communicate. and this will make the entire app more modular and easier to maintain, 

The Model is concerned about the data and the app logic while the View gets and displays data from, and to, the user interface. 

The same logic will apply to Recipe, List, and Likes part of the app. So there are always one model and one view held together by the overall controller, but it is easier to work with just one global controller and you will see why a bit later.


## 138 How ES6 module work

How to use ES6 module

delete test.js and clean the index.js

create src/models/Search.js and src/views/searchView.js

export one thing 

Search.js

    export default 'i am tim';

and import from index.js controller

    import String from './models/Search'
    
IF we want to export multiple things

export an function
    
    export const add = (a, b) => a + b;
    export const multiply = (a,b) => a *b;
    
and in index.js

    import {add, multiply} from './views/searchView';
    
    or
    
    import {add as a, multiply as m} from './views/searchView';

    console.log(`${a(2,3)} and multiple ${m(3,4)}`)
    
We can do third way

    import * as searchView from './views/searchView';
    console.log(`${searchView.add(2,3)} and multiple ${searchView.multiply(3,4)}`)
    

## 139 make a first AJAX call

    the api we will use is food2fork.com
    
we can search and get a certain recipe 

create your own account

and we will install axios package for AJAX 

    npm install axios --save

index.js

    import axios from 'axios';

    async function getResults(query){
    const proxy = 'https://cors-anywhere.herokuapp.com/';
        const key = 'api key';
        const res = await axios(`${proxy}http://food2fork.com/api/search?key=${key}&q=${query}`); // return a promise
        const recipes = res.data.recipes;
        console.log(recipes);
    }
    
    getResults('pizza');

handle error using try catch

    
    import axios from 'axios';
    
    async function getResults(query){
        const proxy = 'https://cors-anywhere.herokuapp.com/';
        const key = 'api key';
        try{
            const res = await axios(`${proxy}http://food2fork.com/api/search?key=${key}&q=${query}`); // return a promise
            const recipes = res.data.recipes;
            console.log(recipes);
        }catch(error){
            alert(error);
        }
    }
    
    getResults('pizza');

## 140 Building a Search model

We will learn how to build a simple data model using ES6 classes, 

    import axios from 'axios';
    
    export default class Search {
        constructor(query){
            this.query = query
        }
        
        // now we copy the getResult function to the Search
        async getResults(query){
            const proxy = 'https://cors-anywhere.herokuapp.com/';
            const key = 'api key';
            try{
                const res = await axios(`${proxy}http://food2fork.com/api/search?key=${key}&q=${this.query}`); // return a promise
                this.result = res.data.recipes;
                //console.log(this.recipes);
            }catch(error){
                alert(error);
            }
        }
    }

now go ahead to import from from index.js

    import Search from './models/Search';
    
    const search = new Search('pizza');


## 141 Building the Search Controller

We will learn the concept of appliction state, and a simple way of implementing state

Each model and each view will get each own file, but for the controller, we will put all of them into the same file because that's make it a lot easier as you will see when the file gets bigger and bigger as we adding more controllers. 

So let talk a bit about the state, imagine our final app running with all the search queries, the recipe, the likes, and shopping list. So what is the state of our app in any given moment?

so think about what is the like the current search query? or what's the current recipe? or how many servings are currently being calculated? and What's currently in the shopping list?

So all few things in one given moment are the state. We want all these data in the current state in a current moment of our app, and all of this data is the state and we want that to be in one central place like one central variable like an object in which we have then. 

in index.js

    import Search from './models/Search';

    // Global state of the app
    // - Search object
    // - Current recipe object
    // - Shopping list object
    // - Liked object
    const state = {};
    
    const controlSearch = async() => {
        // 1. Get query from view
        const query = 'pizza';
        
        if(query){
            // 2. New search object and add to state
            state.search = new Search(query);
            
            // 3. Prepare UI for results
            
            // 4. Search for recipes
            await state.search.getResult();
            
            // 5. Render results on UI
            console.log(state.search.result);
        }
    }
    // search is classname of form
    document.querySelector('.search').addEventListener('submit', e => {
        e.preventDefault();
        controllerSearch();
    });
    const search = new Search('pizza');
    console.log(search);
    search.getResults();


## 142 Building the Search View - part 1

you will learn some more advanced DOM manipulation techniques, How to use ES6 template stings to render entire HTML components, and how to create a loading spinner

searchView.js

    export const getInput = () => {

    }

better to have one central element to get all element we get from the app, so we crreate a new file called base.js

base.js

    export const elements = {
        searchInput: document.querySelector('.search_field'),
        searchForm: document.querySelector('.search')
    }

in index.js

    import {elements} from './views/base';
    import * as searchView from './views/searchView';
    import Search from './models/Search';
    
    // Global state of the app
    // - Search object
    // - Current recipe object
    // - Shopping list object
    // - Liked object
    const state = {};
    
    const controlSearch = async() => {
        // 1. Get query from view
        const query = 'pizza';
        
        if(query){
            // 2. New search object and add to state
            state.search = new Search(query);
            
            // 3. Prepare UI for results
            
            // 4. Search for recipes
            await state.search.getResult();
            
            // 5. Render results on UI
            console.log(state.search.result);
            }
        }
    // search is classname of form
    elements.searchForm.addEventListener('submit', e => {
    e.preventDefault();
    controllerSearch();
    });
    const search = new Search('pizza');
    console.log(search);
    search.getResults();

in searchView.js

    import {elements} from './base';
    
    export const getInput = () => elements.searchInput.value;

and replay the pizza in index.js

    const controlSearch = async() => {
        // 1. Get query from view;
        const query = elements.getInput();

Now let write a function in the view which can recieve all of this result and than print each of the elements and so each of the recipe in that array to the user interface. 

in searchView.js

    // we receive 30 recipes 
    export const renderResults = recipe => {
        recipe.foreach(renderRecipe);
    }
    
we also need to render one recipe

    const renderRecipe = recipe => {
        const markup = `
        <li>
            <a class="results__link" href="${recipe.recipe_id}">
                <figure class="results__fig">
                <img src="${recipe.image_url}" alt="Test">
                </figure>
                <div class="results__data">
                    <h4 class="results__name">${recipe.title}</h4>
                    <p class="results__author">${recipe.publisher}</p>
                </div>
            </a>
        </li>
        `;
    }
    
now we want to render it to the DOM

add the left element to the elements object in base.js


    export const elements = {
        searchInput: document.querySelector('.search_field'),
        searchForm: document.querySelector('.search'),
        searchResList: document.querySelector('.results_list')
    }
now add the renderRecipe


    const renderRecipe = recipe => {
        const markup = `
            <li>
                <a class="results__link" href="${recipe.recipe_id}">
                    <figure class="results__fig">
                    <img src="${recipe.image_url}" alt="Test">
                    </figure>
                <div class="results__data">
                    <h4 class="results__name">${recipe.title}</h4>
                    <p class="results__author">${recipe.publisher}</p>
                </div>
                </a>
            </li>
        `;
        elements.searchResList.insertAdjacenHTML('beforend', markup);
    }

in order to see we need to render;

    const controlSearch = async() => {
        // 1. Get query from view
        const query = 'pizza';

        if(query){
            // 2. New search object and add to state
            state.search = new Search(query);

            // 3. Prepare UI for results

            // 4. Search for recipes
            await state.search.getResult();

            // 5. Render results on UI
            searchView.renderResSult(state.search.result);
        }
    }
now we can display 30 result to the page

next we will clear the input 

    export const clearInput = () => {elements.searchInput.value = '}';

and in the index.js add
 
    // 3. Prepare UI for results
    searchView.clearInput();

Now if we try to search another like pasta, the result will add to the end of the result of pizza

We need to clear the input and result from previous search

in searchView.js

    export const clearResults = () => {
        elements.searchResList.innerHTML = '';
    }

and in index.js

    // 3. Prepare UI for results
    searchView.clearInput();
    searchView.clearResults();
    
Next thing we need to do is to shorten the title, 

## 143 Building the search View - part 2

We now reduce the title size here whenever it occupies more than one line, but we don't want to cut a word in half 

in searchView.js

    const limitRecipeTitle = (recipe, limit=17) => {
        const newTitle = [];
        if(title.length > limit){
            title.split(' ').reduce((acc,cur) => {
                if(acc + cur.length <= limit){
                    newTitle.push(cur);
                }
                return acc + cur.length;
            },0); // split words into array 
            
            // return the result
            return `${newTitle.join(' ')} ...`;
        }
        return title;
    }
    

now we just insert the function into 

    <div class="results__data">
        <h4 class="results__name">${limitRecipeTitle(recipe.title)}</h4>
        <p class="results__author">${recipe.publisher}</p>
    </div>

## 144 Rendering an AJAx Loading Spinner

we need the spinner which can be resued anywhere in the app, so we put it in the base because it is reusable

base.js

we want to pass the argument element which is the parent element. Remember we want to pass one argument into this loader, and that is the parent element and we attack the spinner as a child element of the parent. 
   
    export const renderLoader = parent =>{
        const loader =  `
        <div class="${elementString.loader}">
                <svg>
                    <use href="img/icons.svg#icon-cw"></use>
                </svg>
            </div>
        `;
        parent.insertAdjacentHTML('afterbegin', loader);
    }
Now we add the loader into the controller index.js

we add the parent elements into the base.js

    export const elements = {
        searchRes: document.querySelector('.result') 
    }

and in the index.js 

    import {elements, renderLoader} from './views/base';

    // 3. Prepare UI for results
    searchView.clearInput();
    searchView.clearResults();
    renderLoader(elements.searchRes);

the spinner works fine, but it doesn't disappreance when the data come back. 

let creat clearLoader in base.js

    export const elementStrings = {
        loader: 'loader'; 
    }
    
    export const clearLoader = () => {
        const loader = document.querySelector(`.${elementString.loader}`);
        if(loader){
            loader.parentElement.removeChild(loader);
        }
    }

when we render the result. we first moving off the loader

    import {elements, renderLoader, clearLoader} from './views/base';

    // 3. Prepare UI for results
    searchView.clearInput();
    searchView.clearResults();
    renderLoader(elements.searchRes);
    
    // 4. Search result on UI
    
    // 5. Render results to UI
    clearLoader();

## 145 Implementing Search Results Pagination

we will learn how to use the .closest method for easier event handling and how and why to use data-* attribute in HTML5

in searchView.js, we not only pass recipe to renderResult but also pass the page number to the result

    export const renderResults = (recipe, page = 1, resPerPage = 10) => {
        
        const start = (page - 1) * resPerPage;
        const end = page * resPerPage;
        
        recipe.slice(start,end).foreach(renderRecipe);
    }

Now take care rendering the actual button, we can keep it inside the renderResult, but it is better idea to keep it in the separate function for maintainable and reusable

there are several page, we should know which page we are, we can use if-else statement

    const renderButtons = (page, numResults, resPerPage) => {
        const pages = Math.ceil(numResults / resPerPage);
        const button;
        if(page === 1 && pages > 1){
            //only button to go the next page
            button = createbutton(page,'next');
        }else if(page < pages){
            // both button to go to prev and next page
            
            button = `
                ${createbutton(page,'next')}
                ${createbutton(page,'prev')}
            `;

        }else if(page === pages){
            //only button go to prev page
            button = createbutton(page,'prev');

        }
    }

we have several button repeat, we can create a function to display button

we can use data-goto to go to the page we want

type can be prev or next

    const createButton = (page, type) => `
        <button class="btn-inline results__btn--${type}" data-goto=${type==='prev'? page -1 : page + 1}>
            <svg class="search__icon">
                <use href=`img/icons.svg#icon-triangle-${type === 'prev' ? 'left' : 'right'}`></use>
            </svg>
            <span>Page ${type === 'prev' ? page -1 : page +1} </span>
        </button>

        
    `;
    
now insert it back to renderButtons above.

now we gona to add the result to the DOM 

in base.js add new element
    
    export elements = {
        searchResList: document.querySelector('.results__list'),
        searchResPages: document.querySelector('.results__pages')
    }

now from the renderButton  method we add to the DOM

    elements.searchResPages.insertAdjacentHTML('afterbegin',button);

now call the renderButton in renderResult

    renderButtons(page,recipe.length, resPerPage);

Now we can see the result of the first page but the button doesn't work when we click, because we need to add an eventhandler to this

we want to add the event handler in index.js but we have no way to get the page number, Thanks JS we have a method called closest which we can find the page number by using the classname

in index.js

    elements.searchResPages.addEventListener('click', e => {
        const btn = e.target.closest('.btn-incline');
        if(btn){
            const gotoPage = parseInt(btn.dataset.goto,10); // base 10
now all we need to do is to render to the page
we should clear result
               
            searchView.clearResults();
            searchView.renderResuls(state.search.result, gotoPage);
    
        }
    });

we should clear the button as well, we can do that by adding code in the clearResults()

    export const clearResult = () => {
        elements.searchResPages.innerHTML = '';
    }

## 146 Building the Recipe model -part 1

    import axios from 'axios';
    
    export default class Recipe{
        constructor(id){
            this.id = id;
        }
        
        async getRecipe() {
        
            try{
                
            }catch(error){
                alert(error);
            }
        }
    }

we are going to use the key and proxy as in search model again, so we will move them to the config file 

js/config.js 

    export const proxy = '';
    export const key = '';

but it is not good idea to stored the key on client side because everyone can see it, we can create a server let get the key from that before sending an a

import the in the Search.js, and the same with the Recipe.js

    import axios from 'axios';
    import {key, proxy} from '../config';
    
    export default class Recipe{
        constructor(id){
            this.id = id;
        }

        async getRecipe() {

            try{
                const res = await axios(`${proxy}html://food2fork.com/api/get?key=${key}&rId=${this.id}`);
                this.title = res.data.recipe.title;
                this.author = res.data.recipe.publisher;
                this.img = res.data.recipe.imagine_url;
                this.url = res.data.recipe.source_url;
                this.ingredients = res.data.recipe.ingredients;
            }catch(error){
                alert(error);
            }
        }
    }

in index.js add

    import Recipe from './models/Recipe';
    
    // Recipe controller
    const r = new Recipe(455335);
    r.getRecipe();

we also need to display cooking time, and number of serving, so we will add two methods into Recipe class

    calcTim(){
        //assuming that we need 15 min for each 3 ingredients
        const numIng = this.ingredients.length;
        const periods = Math.ceil(numIng/3);
        
    }
    
    calcServings(){
        this.servings = 4;
    }

## 147 building the Recipe Controller.

we will learn how to read data from the page URL
How to respond to the hashchange event
How to add the same event listener to multiple events

when we click on an item on the item list the url is change with different id which we call a hash, that is event which called hash change event in js

in index,js

    const controlRecipe = async () => {
        const id = window.location.hash.replace('#','');
        
        if(id){
            // prepare U changed
            
            // Create new recipe object
            state.recipe = new Recipe(id);
            try {
                // Get recipe data and parse ingredient
                await state.recipe.getRecipe();
                
                // calculate serving and time
                state.recipe.calcTime();
                state.recipe.calServing();
                
                //render the recipe
                console.log(state.recipe);
                }catch(error){
                    alert('Error processing recipe');
                }

        }
    }
    window.addEventListener('hashchange',controlRecipe);

only happen when the hash happen, when we refesh the page theey are lost, we need a load event

in index.js we will add

    window.addEventListener('load',controlRecipe);

we can do


    ['hashchange', 'load'].foreach(event => window.addEventListener(event,controllRecipe);
    

## 148 Building the Recipe Model - part 2

We will learn how to use array method like map, slice, and findIndex and includes;
How and why to use eval()

we are trying to find a way of separate each ingredients as number, unit and the name of ingrdient

    parseIngredients(){
    
        const unitLong = ['tablespoons','tablespoon','ounces', 'ounce','teaspoons','teaspoon','cups','pounds'] 
        
        const unitShort = ['tbsp','tbsp','oz','oz','tsp','cup','pound'];
        
        const newIngredients = this.ingrdients.map(el => {
            // 1. Uniform units
            let ingredient = el.toLowerCase();
            unitsLong.forEach((unit,i)=>{
                ingrdient = ingreditent.replace(unit,unitsShort[i]);
            });
            // 2. Remove parenthese, google it
            ingredient =ingredient.replace(/[\])}[{(]/g, '');
            
            // 3. Parse ingredients into count, unit, and ingredients
            const arrIng = ingredient.split(' ');
            const unitIndex = arrIng.findIndex(el2 => unitsShot.includes(el2));
in each element in the arrayIng, it will perform this test, if the unitsShot include this el2, then return the index of the element which the test return true
                    
            let objIng;
            if(unitIndex > -1){
                // there is an unit 4 1/2 cup
                const arrCount = arrIng.slice(0, unitIndex); // 4 1/2 cups, --> [4, 1/2]
                
                if(arrCount.length === 1){
                    // somce cases we have 1-1/2, we need to replace - with +
                    count = arrIng[0].replace('-','+');
                    }else{
                        //[4, 1.2]
                        count = eval(arr.Ing.slice(0,unitIndex).join('+')); // 4.5
                    }
                    objIng = {
                        count
                        unit: arrIng[unitIndex],
                        ingredient: arrIng.slice(unitIndex + 1).join(' ');
                    }
            }else if(parseInt(arrIng[0],10)){
                // there is No unit, but 1st element is number
                
                objIng = {
                    count: parseInt(arrIng[0],10),
                    unit: '',
                    ingredient: arrIng.slice(1).join(' '); // put back to string
                }
                
            }else if(unitIndex === -1){
                // there is no unit and no number in 1st position
                objIng = {
                    count: 1,
                    unit: '',
                    ingredient
                }
            }
            return objIng;
        })

    }

now add it to index.js

    try {
        // Get recipe data and parse ingredient
        await state.recipe.getRecipe();
        state.recipe.parseIngredients();
        
        // calculate serving and time
        state.recipe.calcTime();
        state.recipe.calServing();

        //render the recipe
        console.log(state.recipe);
        }catch(error){
            alert('Error processing recipe');
    }

## 149 Building the recipe view - part 1

now we have recipe data, and we just want to display them to the DOM

crearte a recipeView.js in views folder

recipeView.js
 
     const createIngredient = ingredient => `
         <li class="recipe__item">
            <svg class="recipe__icon">
                <use href="img/icons.svg#icon-check"></use>
            </svg>
            <div class="recipe__count">${ingredient.count}</div>
            <div class="recipe__ingredient">
            <span class="recipe__unit">${ingredient.unit}</span>
                ${ingredient.ingredient)}
            </div>
         </li>
     `;
    export const renderRecipe = recipe => {
        const markup = `
        
        <figure class="recipe__fig">
            <img src="${recipe.img}" alt="${recipe.title}" class="recipe__img">
            <h1 class="recipe__title">
                <span>${recipe.title}</span>
            </h1>
        </figure>
        <div class="recipe__details">
            <div class="recipe__info">
                <svg class="recipe__info-icon">
                    <use href="img/icons.svg#icon-stopwatch"></use>
                </svg>
                <span class="recipe__info-data recipe__info-data--minutes">${recipe.time}</span>
                <span class="recipe__info-text"> minutes</span>
            </div>
            <div class="recipe__info">
                <svg class="recipe__info-icon">
                    <use href="img/icons.svg#icon-man"></use>
                </svg>
                <span class="recipe__info-data recipe__info-data--people">${recipe.servings}</span>
                <span class="recipe__info-text"> servings</span>
        
                <div class="recipe__info-buttons">
                    <button class="btn-tiny">
                        <svg>
                            <use href="img/icons.svg#icon-circle-with-minus"></use>
                        </svg>
                    </button>
                    <button class="btn-tiny">
                        <svg>
                            <use href="img/icons.svg#icon-circle-with-plus"></use>
                        </svg>
                    </button>
                </div>
        
            </div>
            <button class="recipe__love">
                <svg class="header__likes">
                    <use href="img/icons.svg#icon-heart-outlined"></use>
                </svg>
            </button>
        </div>
About the ingredient, we don't know how many of them, so we need a loop 

        <div class="recipe__ingredients">
            <ul class="recipe__ingredient-list">
loop throught

            ${recipe.ingredients.map(el => createIngredient(el)).join('')} // before return an array, but we join it as a string
    
            </ul>
        
            <button class="btn-small recipe__btn">
                <svg class="search__icon">
                    <use href="img/icons.svg#icon-shopping-cart"></use>
                </svg>
                <span>Add to shopping list</span>
            </button>
        </div>
        
        <div class="recipe__directions">
            <h2 class="heading-2">How to cook it</h2>
            <p class="recipe__directions-text">
                This recipe was carefully designed and tested by
                <span class="recipe__by">${recipe.title}</span>. Please check out directions at their website.
            </p>
            <a class="btn-small recipe__btn" href="${recipe.url}" target="_blank">
                <span>Directions</span>
                <svg class="search__icon">
                    <use href="img/icons.svg#icon-triangle-right"></use>
                </svg>
            </a>
        </div>
        `;
in the base create new element
        
        recipe: document.querySelector('.recipe');

and back to the index.js

    elements.recipe.insertAdjacentHTML('afterbegin',markup);
        
    }

in index.js we will add the render recipe
    
    const controlRecipe = async () => {
        const id = window.location.hash.replace('#','');
        
        if(id){
            // prepare UI changed
            recipeView.clearRecipe();
            renderLoader(elements.recipe);
            
            // Create new recipe object
            state.recipe = new Recipe(id);
            try {
                // Get recipe data and parse ingredient
                await state.recipe.getRecipe();
                
                // calculate serving and time
                state.recipe.calcTime();
                state.recipe.calServing();
                
                //render the recipe
                clearLoader();
                recipeView.renderRecipe(state.recipe);
            }catch(error){
                alert('Error processing recipe');
            }
        
        }
    }

we should clear recipe in recipeView.js

    export const clearRecipe = () =>{
        elements.recipe.innerHTML = ''; 
    };
    

## 150 Building the Recipe view -- part 2

in this lecture we will learn how to display 4.5 to 4 1/2, 0.5 to 1/2. in order to do that we need a third party package call Fraction. 
    
    npm instal fractional --save
    
    (new Fraction(5,10)).numerator // 5
    (new Fraction(3,4)).denominator // 4

recipeView.js

    import { Fraction } 
