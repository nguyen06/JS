The app allows us to add incomes and expenses for a certain month, which is in this case September 2016. UI shows us the Income and Expenses. WE can delete an income ore expenses, and the account will be nicely update. 

## Project plan and Architecture: part 1

First we build an font-end

TODO list

- Add event handler

- Get input values

- Add the new item to our data structure

- add the new item to uth UI

- Calculate new budget

- update user interface


Now we are going to talk about how to struture our code, modules. 

- Modules are a vital aspect of any robust application's architecture. 

- Keep the units of code for a project both cleanly separated and organized

- Excapsulate some data into privacy and expose other data publicly.


UI Module
	- Get input values
	- Add the new item to the UI
	- Update the UI

Data module
	- Add the new item to our data struture
	- Calculate budget

Controller Module

	- add event handler

	The goal of controller module will be control the entire app, and acting as a link between these other two modules. 


## Implement the Module Pattern

	- How to use the module pattern
	- More about private and public data, encapsulation and separation of concerns

why we want to create an module because we want to keep pieces of code that are related to one another together inside of separate, independent, and organized units. In each of these modules, we will hae variables and function that are private, which means that they are only accessible inside of the module. We want that so that no other code can override our data. beside the private variable, we also have public method which means that we expose them to the public so that other functions of other module, can use them. So that is called data encapsulation. Data encapsulation allows us to hide details of a specific module, so we only expose the public interface which is sometimes called an API. 

var budgetController = (function(){ // an immediately-invoked fucntion expression that will return an object
	
	var x = 23;

	var add = function(a){
		return x + a;
	}

	return {
		publicTest: function(b){ // always get access to x and add method because of power of closure
			console.log(add(b));
		}
	}
})();


var UIController = (function(){
	// some code
})();


var Controller = (function(budgetCtrl, UICtrl){
	budgetController.publicTest(); // ne don't want it, because need to change the name everywhere if we want

})(budgetController, UIController);

## 78 Setting up the first Event Listeners

we will learn
	- how to set up event listeners for keypress events
	- How to use event object



var budgetController = (function(){ // an immediately-invoked fucntion expression that will return an object
	
})();


var UIController = (function(){
	// some code
})();


// global app controller
var Controller = (function(budgetCtrl, UICtrl){

	var ctrlAddItem  = function(){


		// 1. get input data

		// 2. add item to the budget controller

		// 3. add new item to new user interface

		//4. calculate budget 

		//5. display budget on the U
	}

	// click on the add_btn
	document.querySelector('.add__btn').addEventListener('click',ctrlAddItem);


	// key press on the keyboard
	document.addEventListener('keypress',function(event){
		//enter key is pressed
		if(event.keyCode === 13 || event.which === 13){
			ctrlAddItem();

		}
	})
})(budgetController, UIController);



## 79. Reading input data

we get input from ui controller, and then in the controller method, we call ui controller to get the data

var UIController = (function(){
	return {
		getinput: function(){
			// we should return an object with three properties
			return {

				type: document.querySelector('.add__type').value; // will be either inc or exp
				discription : document.querySelector('.add__description').value;
				value : document.querySelector('.add__value').value;
			};

		}
	}
})();

now pass data to the controller


// global app controller
var Controller = (function(budgetCtrl, UICtrl){

	var ctrlAddItem  = function(){


		// 1. get input data

		var input = UICtrl.getInput();


		// 2. add item to the budget controller

		// 3. add new item to new user interface

		//4. calculate budget 

		//5. display budget on the U
	}

	// click on the add_btn
	document.querySelector('.add__btn').addEventListener('click',ctrlAddItem);


	// key press on the keyboard
	document.addEventListener('keypress',function(event){
		//enter key is pressed
		if(event.keyCode === 13 || event.which === 13){
			ctrlAddItem();

		}
	})
})(budgetController, UIController);

we can see the html class name, so they should create a big problem when we do maintainer

so we can create an object in UIController

var UIController = (function(){

	var DOMString = {
		inputType: '.add_type',
		inputDescription: '.add_description',
		inputValue: '.add_value'
	}
	return {
		getinput: function(){
			// we should return an object with three properties
			return {

				type: document.querySelector('.DOMString.inputType').value; // will be either inc or exp
				discription : document.querySelector('DOMString.inputdescription').value;
				value : document.querySelector('DOMString.inputvalue').value;
			};

		}
	}
})();

in step 5 of Controller we have to display budget on the UI, we don't really get access to the DOMString from the UIController, so we need to return another object of DOMStrinf


var UIController = (function(){

	var DOMString = {
		inputType: '.add_type',
		inputDescription: '.add_description',
		inputValue: '.add_value',
		inputBtn: '.add_btn'
	}
	return {
		getinput: function(){
			// we should return an object with three properties
			return {

				type: document.querySelector('.DOMString.inputType').value; // will be either inc or exp
				discription : document.querySelector('DOMString.inputdescription').value;
				value : document.querySelector('DOMString.inputvalue').value;
			};

		},
		getDOMStrings: function(){
			return DOMString;
		}
	}
})();

now come down the controller to get DOMString


// global app controller
var Controller = (function(budgetCtrl, UICtrl){

	var ctrlAddItem  = function(){

		var DOM = UICtrl.getDOMStrings();

		// 1. get input data

		var input = UICtrl.getInput();


		// 2. add item to the budget controller

		// 3. add new item to new user interface

		//4. calculate budget 

		//5. display budget on the U
	}

	// click on the add_btn
	document.querySelector('DOM.inputBtn').addEventListener('click',ctrlAddItem);


	// key press on the keyboard
	document.addEventListener('keypress',function(event){
		//enter key is pressed
		if(event.keyCode === 13 || event.which === 13){
			ctrlAddItem();

		}
	})
})(budgetController, UIController);

## 80. Creating an initialization function

 will learn how and why to create an initialize function


// global app controller
var Controller = (function(budgetCtrl, UICtrl){

	var setupEventListener = function(){

		var DOM = UICtrl.getDOMStrings();

		// click on the add_btn
		document.querySelector('DOM.inputBtn').addEventListener('click',ctrlAddItem);


		// key press on the keyboard
		document.addEventListener('keypress',function(event){
			//enter key is pressed
			if(event.keyCode === 13 || event.which === 13){
				ctrlAddItem();

			}
		})

	};



	var ctrlAddItem  = function(){


		// 1. get input data

		var input = UICtrl.getInput();


		// 2. add item to the budget controller

		// 3. add new item to new user interface

		//4. calculate budget 

		//5. display budget on the U
	};

	So the setupEventListener should be call for addEventListener to work, we need to return and call init

	return {
		init: function(){
			setupEventListener();
		}
	}

})(budgetController, UIController);

we call init function outside of module

controller.init();


## 81. Creating income and Expense Function Contructors

in this lecture we will think about how to store income and expenses in budgetController. We will learn

	- how to choose function construtors that meet our application's needs
	- How to set up a proper data struture for our budget controller

var budgetController = (function(){
	
	// we should have something to destinguish icome and expense, so id is best idea
	// what can we do for many object, we use function constructors to instantiate 
	var Expense = function(id, description. value){
		this.id = id;
		this.description = description;
		this.value = value;
	};

	var Expense = function(id, description. value){
		this.id = id;
		this.description = description;
		this.value = value;
	};


	Remember budget controller keep track all income and expensese. Assume users create 10 income objects, where should we store them. 

	var allExpenses = [];
	var allIncomes = [];
	var totalExpenses = 0;

	// how that is not good coding, why don't we push it in the onject

	var data = {
		allItems:{
			exp:[],
			inc:[]
		},
		totals:{
			exp: 0,
			inc: 0
		}
	};

})();


## 82. Adding a new item to out budget controller

we will learn 

	- how to avoid conflicts in our data structures
	- How and why to pass data from one module to another
we use user's input data to create a new budget controller data structure. Now create a public method in budget controller


var budgetController = (function(){
	
	// we should have something to destinguish icome and expense, so id is best idea
	// what can we do for many object, we use function constructors to instantiate 
	var Expense = function(id, description. value){
		this.id = id;
		this.description = description;
		this.value = value;
	};

	var Expense = function(id, description. value){
		this.id = id;
		this.description = description;
		this.value = value;
	};


	Remember budget controller keep track all income and expensese. Assume users create 10 income objects, where should we store them. 

	var allExpenses = [];
	var allIncomes = [];
	var totalExpenses = 0;

	// how that is not good coding, why don't we push it in the onject

	var data = {
		allItems:{
			exp:[],
			inc:[]
		},
		totals:{
			exp: 0,
			inc: 0
		}
	};

	return{
		// SO call to this method should know income type, des, and val
		addItem: function(type, des, val){

			var newItem, ID;

			// ID is unit number that we assign to the each item in the object
			// we want id = lastID + 1;


			if(data.allItems[type].length >0)
				ID = data.allItems[type][data.allItems[type].length - 1].id + 1;
			else
				ID =0;

			// create new item base on type, des, and val
			if(type === 'exp'){
				 // we still miss the ID here
				newItem = new Expense(ID,des,val);

			}else if(type === 'inc'){

				newItem = new Income(ID,des,val);l
			}

			// push to the data object

			data.allITems[type].push(newItem);
			return newItem;
		}
	}

})();

Now go back to controller

// global app controller
var Controller = (function(budgetCtrl, UICtrl){

	var setupEventListener = function(){

		var DOM = UICtrl.getDOMStrings();

		// click on the add_btn
		document.querySelector('DOM.inputBtn').addEventListener('click',ctrlAddItem);


		// key press on the keyboard
		document.addEventListener('keypress',function(event){
			//enter key is pressed
			if(event.keyCode === 13 || event.which === 13){
				ctrlAddItem();

			}
		})

	};



	var ctrlAddItem  = function(){

		var input, newItem;

		// 1. get input data

		input = UICtrl.getInput();


		// 2. add item to the budget controller
		newItem = budgetCtrl.addItem(input.type, input.description,input.value);

		// 3. add new item to new user interface

		//4. calculate budget 

		//5. display budget on the U
	};

	So the setupEventListener should be call for addEventListener to work, we need to return and call init

	return {
		init: function(){
			setupEventListener();
		}
	}

})(budgetController, UIController);

## 83. adding a new item to the UI

finally we will add newly created object to UI which means we will do some DOM manipulation for the first time in this project. We will learn

	- A technique for adding big chuncks of HTML into the DOM
	- How to replace parts of strings
	- How to do DOM manipulation using the inserAdjcentHTML method

let review from last couple lectures. 
	- first we read input out of the fields, store it into the input variable, and then using this input variable and new item method to create a new item and then return it and store it in this variable. 


Now in UIController we add new method call addListItem

var UIController = (function(){

	var DOMString = {
		inputType: '.add_type',
		inputDescription: '.add_description',
		inputValue: '.add_value',
		inputBtn: '.add_btn'
	}
	return {
		getinput: function(){
			// we should return an object with three properties
			return {

				type: document.querySelector('.DOMString.inputType').value; // will be either inc or exp
				discription : document.querySelector('DOMString.inputdescription').value;
				value : document.querySelector('DOMString.inputvalue').value;
			};

		},

		addListItem: function(object, type){
			var html;
			// Create HTML string with placeholder text
			
			if(type==='inc'){

				html = `<div class="item clearfix" id="income-0">
					<div class="item__description">Salary</div>
					<div class="right clearfix">
						<div class="item__value">+21000</div>
						<div class="item__delete">
							<button class="item__delete--btn"><i class="ion-ios-close-outline"></i></button>
						</div>
					</div>
				</div>`
				}else{
					html = `<div class="item clearfix" id="expense-0">
						<div class="item__description">Salary</div>
							<div class="right clearfix">
								<div class="item__value">+21000</div>
								<div class="item__delete">
									<button class="item__delete--btn"><i class="ion-ios-close-outline"></i></button>
								</div>
							</div>
						</div>`

				}


			// Replace the placeholder text with some actual data

			// insert the HTML into the DOM

		},

		getDOMStrings: function(){
			return DOMString;
		}
	}
})();

Now let replace income-0 with id which is replaced for real value later

id="income-%id%"
    <div class="item__description">%description%</div>

    <div class="item__value">%value%</div>

do the same with expenses

now replace the place holder with actual data

    newHtml = html.replace(%id%,obj.id);
now we can override with description

    newHtml = newHtml.replace(%description%,obj.description);

    newHtml = newHtml.replace(%value%,obj.value);


Now we can insert into DOM, we use element.insertAdjacentHTML(position,text)

there are 4 positions we can look at, beforebegin, afterbegin, beforeend, afterend, we are going to use beforeend because we have <div class="income__list">

we will add incomeContainer imto the DOMStrings 

    incomeContainer: '.income__list'
    expenseContainer: '.expense__list'

now base on the type of value, we have element income or expense. and we use them to add to the DOM

add 

var element into the 

    if(type === 'inc'){
    element = DOMString.incomeContainer;
    }else{
    element = DOMString.expenseContainer;
    }

now it is time to insert to DOME

document.querySelector(element).inserAdjacentHTML('beforeend',newHTML)


but in order it work we need to add this method into ctrlAddItem in UIController

    var ctrlAddItem = function(){
        UICtrl.addListItem(newItem, input.type);
    }


## 84 Clearing Our Input Fields

We will learn 
	- how to clear HTML fields
	- How to use querySelectorAll
	- How to convert a list to an array
	- A better way to loop over an array then for loops, foreach

we create a public method called clearField in UIController, 

clearFields: function(){
	var fields, fieldArr;
	fields = document.querySelectorAll(DOMStrings.inputDescription + ',' + DOMString.inputValue);

	but the querySelectorAll return a list which is simmilar with an array, we sould convert the list to the array

	we can not use slice method directly with fields, but we can call 'call' method first in the Array

	fieldArr = Array.prototype.slice.call(fields);

	now we can loop through the array, and clear the fields

	the anomymous we have access up to three arguments the current value, index, and entire array

	fieldArr.forEach(function(curr, index, array){
		current.value = "";
	})
},

Now go back to controller

add clear the fiels

UICtrl.clearFields();



## 85 Updating the budget: Controller

We will learn
 
 - How to convert field inputs to numbers
 - How to prevent false inputs

in order to avoid DRY principle, we move step 4 and 5 in ctrlAddItem to separate function called updateBuget

var updateBuget = function(){
	
	//1. Calculate the budget

	// 2. return the budget

	//3. Display the budget on the UI
}

and then in ctrlAddItem, we will add the updateBuget function

updateBuget();

The problem is the value is string, not a number, we need to convert it to number first


return{
	getInput:function(){

		return{
			type:
			description:
			value: parseFloat(document.querySelector(DOMStrings,inputValue).value);
		}
	}
}


right now if we keeo clicking on the input button, we are simply adding new input with empty line




in ctrlAddItem function we can use if-else statement

    if(input.description !== "" && !isNaN(input.value) && input.value > 0){
        newItem 
        UICtrl.addListItem(newItem, input.type);
        UICtrl.clearFields();
        updateBuget()
    }

## 86. Updateing the budget: budget Controller

We will learn 

	- how and why to create simple, reuseable function with only one purpose
	- How to sum all elements of an array using the forEach method


var updateBuget = function(){
	
	//1. Calculate the budget
	budgetCtrl.calculateBudget();

	// 2. return the budget

	//3. Display the budget on the UI
}

in the budget controller, we create and public method call calculateBudget(), 

var budgetController = (function(){
	
	var calculateTotal = function(type){
		var sum = 0;
		data.allItems[type].forEach(function(cur){
			sum += cur.value;
			});
		data.totals[type] = sum;
	}

	// add two new variable into data
	data = {
		budget:0;
		percentage:-1;
	}
	return {
		calculateBudget:function(){

			// Calculate total income and expenses, need a private method
			calculateTotal('exp');
			calculateTotal('inc');

			// Calculate budget: income - expenses
			data.budget = data.totals.inc - data.totals.exp;

			// calcualte percentage of income that we spent
			if(data.total.inc >0){
				data.percentage = Math.round((datat.totals.exp / data.totals.inc) * 100);
			}else{
				data.percentage = -1;
			}

		}
	}
});


## 87. Updating the budget: UI controller

we will learn
	- Practice DOM manipulation by updating the budget and total values

look at the html code

    <div class="budget__value">+ 2,345.64</div>

	<div class="budget__income clearfix">
	    <div class="budget__income--text">Income</div>
	    <div class="right">
	        <div class="budget__income--value">+ 4,300.00</div>
	        <div class="budget__income--percentage">&nbsp;</div>
	    </div>
	</div>

    <div class="budget__expenses clearfix">
		<div class="budget__expenses--text">Expenses</div>
		<div class="right clearfix">
		    <div class="budget__expenses--value">- 1,954.36</div>
		    <div class="budget__expenses--percentage">45%</div>
		</div>	
	</div>

	we will need to name the classes in UIController

	var UIController = (function(){
		var DOMstrings = {
			budgetLabel :'.budget__value',
			incomeLabel: '.budget__income--text',
			expensesLabel:'.budget__expenses--value',
			percentageLabel:'.budget__expenses--percentage',

		}
	})

	now we create a public method to display the budget

	displayBudget: function(obj){
		var type;
		obj.budget > 0 ? type = 'inc' : type='exp';

		// using document selector to display the budget, income, and expense

		document.querySelector(DOMstrings.budgetLabel).textContent = obj.budget;
		document.querySelector(DOMstrings.incomeLabel).textContent = obj.totalIncome;
		document.querySelector(DOMstrings.expensesLabel).textContent = obj.totalExpense;

		if(obj.percentage > 0){
			document.querySelector(DOMstrings.percentageLabel).textContent = obj.percentage + '%';

		}else{
			document.querySelector(DOMstrings.percentageLabel).textContent = '--';

		}

	}



## 88. Project planning and architecture: step 2

	- add event handler
	- delete item from our data structure
	- delete the item to the UI
	- Re-calculate budget
	- update the ui


## 89. Event Delegation

Event bubbling means that when an event is fired or triggered on some DOM element, then the exact same event is also triggered on all of the parent elements. all the way up in a DOM tree until the HTML element which is the root. So we say that the event bubbels up side the DOM tree, and that's why it's called bubbling. The element that caused the event to happen, is called the target element. The important thing is the target element is stored as a property in the event object, and we already talked about that one. This means that all the parents elements on which the event will also fire will know the target element of the event. IF we know where the event was fired then we can simly attach an event handler to a parent element and wait for the event when the button up.

The first case is when we have an element with a lots of child elements that we are interested in, in this case, instead of adding an event handler to all of these child elements, we simply add it to the parent. and then determine on which child elements the event events was fired. 

The second use case for event delegation is when we want an event handler attached to an element that is not yet in the DOM when our page is loaded.That's, of course, because we cannot add an event handler to something that's not on our page, so in a case of deprecation that we're coding. that is cal event delegation

## 90. Setting up the Delete Event Listener using Event Delegation

add the class container to DOMstring

first set an event handler on the parents of income and expense which is container class

	var controller = (function(budgetCtrl, UICtrl){
		var setupEventListener = function(){

			document.querySelector(DOM.container).addEventListener('click', ctrlDeleteItem);
		}

		var ctrlDeleteItem = function(event){
			var itemID;

			itemID = event.target.parentNode.parentNode.parentNode.parentNode.id;

			if(itemID){
				splitID = itemID.split('-');
				type = splitID[0];
				ID = parseInt(splitID[1]);

				// 1. Delete the item from the data structure
				budgetCtrl.deleteItem(type,ID);

				// 2. Delete the item from the UI

				// 3. Update and show the new budget

				// 4. Calculate and update percentage
			}
		}
	})


## 91. Deleting an item from our budget controller

create an new public function in budget controller

deleteItem: function(type, id){
	var ids, index;

	ids = data.allItem[type].map(function(current){
		return current.id;
	});

	index = ids.indexOf(id);

	if(index !== -1){
		data.allItems[type].splice(index, 1)
	}
}

## 92. Deleting an item from the UI

add a new public function in UIController

    deleteListItem: function(selectorID){
        var el = document.getElementById(selectorID);
        el.parentNode.removeChild(el);
    }

in the controller we will cal the deleteListItem

    UICtrl.deleteListItem(itemID);


 now update and show the budget

// 3. 
    updateBudget();

## 93. Project planning and architecture: part 3

	- Calculate percentages
	- Update percentages in UI
	- Display the current month and year
	- Number formatting
	- improving input field UX

## 95. Updating the percentage: budget controller

How to make our budget interact with the Expense prototype

create a new method in controller

    var updatePercentage = function(){
        //1. Calculate percentage in budget controller

        //2. Read percentage from the budget controller

        //3. Update th UI with the new percentage
    }

in budget controller we create an public method

    calculatePercentage: function()
    {
        data.allItems.exp.forEach(function(cur){
            cur.calcPercentage(data.totals.inc);
        })

calcPercentage is the prototype method of Expense which helps to calculate percentage of each expense

    Expense.prototype.calcPercentage = function(totalIncome){
        if(totalIncome > 0){
            this.percentage = Math.roud(this.value/totalIncome)*100);
        }else{
            this.percentage = -1;

        }
    }

and then create another prototype which return the percentage

    Expense.prototype.getPercentage = function(){
        return this.percentage;
    }

also have an public method of get percentage

    getPercentages: function(){
        var allPerc = data.allItems.exp.map(function(cur){
            return cur.getPercentage();
        });
        return allPerc;
    }

## 96. updating the Percentage: UI controlle

in UI controller create a new public method call

    displayPErcentage: function(percentages){
        var fields = document.querySelectorAll(DOMstrings.expensesPercLabel);

	nodeListForEach(fields, function(current,index){
		if(percentages[index] > 0){
			current.textContent = percentages[index] + '%';
		}else{
			current.textContent = '---';
		}
	});
}

create an private function

    var nodeListForEach = function(list, callback){
        for(var i =0; i<list.length;i++){
        callback(list[i],i);
    }
    }

in controller

    var updatePErcentages = function(){
        //1
        //2
        //3 update the UI with the new percentagea

        UICtrl.displayPercentage(percentages)
    }

## 97 Formatting our budget number: String Manipulation

we will learn 
	- how to use different string method to manipulate strings
	- All numbers have decimal part with two number like .00
	- have minus sign at the font of expense value, + sign for income
	- if the number is more than thousand, then we have an , 
	- 

	create an private method in UIController

	var formatNumber = function(num,type){

		var numSplit, int, dec;

		num = Math.abs(num).toFixed(2);

		numSplit = num.split('.');

		int = numSplit[0];

		if(int.length >3){
			int = int.substr(0, int.length -3) + ',' + int.substr(int.length,3);
		}

		dec = numSplit[1];

		return (type === 'exp' ? '-' : '+') + int + '.'+dec;
	}

	now use the format number in 

	newHtml = html.replace('%id%', formatNumber(obj.id, type));
	newHtml = newHtml.replace('%value%', formatNumber(obj.value, type))

	do the same in displayBudget

	displayBudget: function(obj){
		var type;

		obj.budget > 0 ? type = 'inc' : type = 'exp';

		documebt.querySelector(DOMstrings.budgetLabel).textContent = formatNumber(obj.budget, type);
	}

## 98. display current month and year

	create a public method in UIController 

	displayMonth: function(){
		var now, Months, Month, year;

		now = new Date();

		Months = ['january',...];
		month = now.getMonth();
		year = now.getFullYear();

		document.querySelector(DOMstrings.dataLabel).textContent = months[month] + ' ' + year;
	},

	include this method in the init function 

## 99. Finishing Touches: improve the UX

	create an public method in UI controller

	changedType: function(){
		var fields = document.querySelectorAll(
			DOMstrings.inputType + ',' +
			DOMstrings.inputDescription + ',' +
			DOMstrings.inputValue
		);

		move the nodeListForEach to private method and use it here
		nodeListForEach(fields, function(curr){
			curr.classList.toggle('red-focus');
		});

		// also make the color change in the button
		document.querySelector(DOMstrings.inputBtn).classList.toggle('red');
	}


DONE















