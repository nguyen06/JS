## 48 First DOM Access and Manipulation

- how to create our fundamental game variables
- How to generate a random number;
- How to manipulate the DOM
- How to read from the DOM
- How to change CSS styles

Game rules:

- the game has 2 players, playing in rounds
- In each turn, a player rolls a dice as many times as he wishes. Each result added to his round score
- But, if the player rolls a 1, all his Round score gets lost. After that, it's the next player's turn
- The player can choose to hold, which mean that this round score gets added to his Global score. After that, it's the next player's turn
- The first player reach 100 points on Global score wins the game

var scores, roundScore, activePlayer, dice;

scores = [0,0];
roundScore = 0;
activePlayer = 0;

dice = 6;

we will use the math random with function floor to get number between 0 and 5, but we want from 1 to 6, simply add 1
http://casualsafedate.com/806970/
dice = Math.floor(Math.random()*6) + 1;

Now we want to display the score from html code

<div class="player-score" id="current-0">43</div>
use the querySelector function

document.querySelector('#current-0').textContent = dice;

I can use use the class of html to select the current player by 

document.querySelector('#current-'+ activePlayer).textContent = dice;

or we can embbed html by using innerHTML
document.querySelector('#current-'+ activePlayer).innerHTML = '<em>'+dice+'</em>';

We can also use querySelector of get value from the webpage and store in variable

var x = document.querySelector('#score-0').textContent;

we don't want to show a dice at the first play. we can hide it by

document.querySelector('.dice').style.display = 'none';

## 49. Events and Event handling: Rolling the dice

- Events: notifications that are sent to notify the code that something happened on the webpage
- Example: clicking a button, resizing a window, scrolling down or pressing a key

- Event listener: A function that performs an action based on a certain event. It waits for a specify event to happen. 

First, we need to rememeber about the Ezecution Stack. And that's because the rule is that an event can only be processed, or handled as soon as the execution stack is empty. Which mean all of the functions have returned. Si besides the Execution Stack we also have something called the Message Queue in JS engine. This is where all the events that happened in the browser are put and thay sit there and waiting to be processed. 

in the lecture we will learn

- How to set up an event handler
- What a callback function is
- What an anonymous function is
- Another way to select element by ID
- How to change the image in an <img> element

<button class="btn-roll"><i class="ion-ios-loop"></i>Roll dice</button>

we don't want to call the btn function right here, so we don't write () to btn. We want the Event Listener call the function for us, then called the callback function and that's because it's a function that is not called by us. 

document.querySelector('.btn-roll').addEventListener('click',btn);

function btn(){
    
}

or we use the anonymous function which a function doesn't have a name

document.querySelector('.btn-roll').addEventListener('click',function(){
    // 1. random number as soon as someone lick
    
    var dice = Math.floor(Math.random()*6) + 1;
    
    // 2.  display the result
    
    var diceDOM = document.querySelector('.dice');
    diceDOM.style.display = 'block';
    diceDOM.src='dice-' + dice + '.png';
    
    //3. Update the round score If the rolled number was not a 1
    
});

we can update the current score by using getElementById

document.getElementById('score-0').textContent = '0';

document.getElementById('score-1').textContent = '0';

document.getElementById('current-0').textContent = '0';

document.getElementById('current-1').textContent = '0';

## 50 Update score and changing the Active Player

document.querySelector('.btn-roll').addEventListener('click',function(){
    // 1. random number as soon as someone lick
    
    var dice = Math.floor(Math.random()*6) + 1;
    
    // 2.  display the result
    
    var diceDOM = document.querySelector('.dice');
    diceDOM.style.display = 'block';
    diceDOM.src='dice-' + dice + '.png';
    
    ### //3. Update the round score If the rolled number was not a 1
    if(dice !== 1){ // not type coercion
        // add score
        roundScore += dice;
        document.querySelector('#current-'+activePlayer).textContent = roundScore;
    }else{
    
       nextPlayer();
       
            // next player
            activePlayer === 0 ? activePlayer = 1 : activePlayer = 0;
            // the player score is 0
            roundScore = 0;
            document.getElementById('current-0').textContent = '0';
            document.getElementById('current-1').textContent = '0';


            we want to add the active to active player as soon as changing player.

            //document.querySelector('.player-0-panel').classList.remove('active');
            //document.querySelector('.player-1-panel').classList.add('active');

            we can have different way to do that, toggle is if the class is there, remove, if not, add 
            document.querySelector('.player-0-panel').classList.toggle('active');
            document.querySelector('.player-1-panel').classList.toggle('active');

            the dice is invisible when changing the player
            document.querySelector('.dice').style.display = 'none';
    }
});

## 51 Implementing our 'hold' function and the DRY principle

<button class="btn-hold"><i class="ion-iso-download-outline"></i>Hold</button>

   document.querySelector('.btn-hold').addEventListener('click',function(){
        // Add current score to Global score
        scores[activePlayer] += roundScore;
        
        // update the UI
        document.querySelector('#score-' + activePlayer).textContent = scores[activePlayer];
        now doesn't change the player, apply DRY principle\
        
        
        // check if the player win the game
        if(score[activePlayer] >= 100){
            document.querySelector('#name-' + activePlayer).textContent = 'Winner!';
            when determined the winner, don't need to display the dice anymore
            document.querySelector('.dice').style.display = 'none';
            
            // add the winner class
            document.querySelector('.player-'+activePlayer + '-panel').classList.add('winner');
            document.querySelector('.player-'+activePlayer + '-panel').classList.remove('active');
            
        }else{
           // next player
            nextPlayer();
        }

    }

function nextPlayer(){
    // next player
        activePlayer === 0 ? activePlayer = 1 : activePlayer = 0;
        // the player score is 0
        roundScore = 0;
        document.getElementById('current-0').textContent = '0';
        document.getElementById('current-1').textContent = '0';
        
        
        we want to add the active to active player as soon as changing player.
        
        //document.querySelector('.player-0-panel').classList.remove('active');
        //document.querySelector('.player-1-panel').classList.add('active');
        
        we can have different way to do that, toggle is if the class is there, remove, if not, add 
         document.querySelector('.player-0-panel').classList.toggle('active');
        document.querySelector('.player-1-panel').classList.toggle('active');
        
        the dice is invisible when changing the player
        document.querySelector('.dice').style.display = 'none';
}



## create a game initialization
 create ab init function
 init(); // at the begining
 
 
 document.querySelector('.btn-new').addEventListener('click',function(){
     init();
 });
 
 // we can do better
  document.querySelector('.btn-new').addEventListener('click', init);
 
 function init(){
 
    scores = [0,0];
    roundScore = 0;
    activePlayer = 0;
    
    document.querySelector('.dice').style.display = 'none';
    // we can update the current score by using getElementById

    document.getElementById('score-0').textContent = '0';

    document.getElementById('score-1').textContent = '0';

    document.getElementById('current-0').textContent = '0';

    document.getElementById('current-1').textContent = '0';
    
    document.getElementById('name-0').textContent = 'Player 1';
    document.getElementById('name-1').textContent = 'Player 2';

    // removing the winner from the new game
    document.querySelector('.player-0-panel').classList.remove('winner');

    document.querySelector('.player-1-panel').classList.remove('winner');
    document.querySelector('.player-0-panel').classList.remove('active');

    document.querySelector('.player-1-panel').classList.remove('active');
    
    document.querySelector('.player-0-panel').classList.add('active');

 }

## State variable

it is very weird when the game end, but user can continue to play, so we need something call state variable which tell the status of the system


var scores, ..., gamePlaying;

init(){

  gamePlaying = true;
}

we only allow user to continue to play when the gamePlaying is true

document.querySelector('.btn-roll').addEvenetListener('click',function(){
    if(gamePlaying){
        //put all the previous code here
    }
}

where to set the gamePlaying to false
document.querySelector('.btn-hold').addEvenetListener('click',function(){
    if(gamePlaying){
    
        // put everything her
        if(scores[activePlayer] >= 20){
            gamePlaying = false;
        }
    }
    

}


















































