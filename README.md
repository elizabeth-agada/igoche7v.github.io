<!Doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
        
        <title>Typing Game</title>
    </head>
    <style>
    body{
    margin: o;
    padding: 0;
 }

#cover{
    background-color: #5d8aab;
    height:700px;
    padding: 30px;
    
}
div{
    color: white;
    text-align: center;
}
#level{
    text-align: left;
}
.intro{
    text-align-last: center;


}
button{
    float:left;
}
hr{
    margin-left: 36%;
     width: 30%;
    align-self: center;
    border: 0.3 solid green;
}
#inner{
    background-color: #004562;
    padding: 20px;

    border-radius: 20px;
}
#h3{
    text-align: center;
    color: #fafafa;
    font-family: serif;
    transition-property: all;
    transition-delay: 0.3s;
}
input{
    margin-left: 20px;
    text-align: center;
    background-color: #5d8aab;
    border-style: none;
    height: 30px;
    width: 30%;
    border-radius: 5px;
    color: white;
}

#input2{
    font-weight: bold;
    background-color:#5d8aab;
    height: 30px;
    border-radius: 5px;
    color: white;
    margin-top: 50px;
    width: 30%;
    font-size: 9px;
}
p{
    text-align: center;
}

</style>
    <body class="ext-center">
        <div id="cover"><br>
            <div id="inner">
                <header class="text-center">
                    <h5 id="optionBtn"> <p id="level">Levels:</p></h5>
                </header>
                <button id="easy">easy 🐷</button>
                <button id="medium">med 🐼</button>
                <button id="hard">hard 🔥</button><br><br>
    <!-- Word input -->
    <div class="row">
        <div class="col-md-6 mx-auto">
            <p>Type the given word within <span class="text-success" id="seconds"></span> Seconds:</p>
            <h2 class="display-2 mb-3" id="current-word"></h2>
            <h4 class="mt-2" id="message"></h4>
            <input type="text" class="form-control form-control-lg" placeholder="Start typing..." id="word-input" autofocus col><br>
            <hr/>
        <!-- Time & Score Columns -->
    <div class="row mt-2">
        <div class="col-md-6">
            <h3>Time Left: <span id="time">0</span></h3>
        </div>
        <div class="col-md-6">
            <h3>Score: <span id="score">0</span></h3>
        </div>
        <div class="col-md-12">
            <h4>High score: <span id="high-score">0</span></h4>
        </div>
        <input type="text" placeholder="Pro Tip:Sometimes is faster to delete all and just retype!" id="input2">
         </div>
       </div>
     </div>
  </div>
 </div>
</body>
    <script>window.addEventListener('load', init);
// Available Levels
const levels = {
    easy: 5,
    medium: 3,
    hard: 2
}
// To change level
let currentLevel = levels.easy;
let time = currentLevel;
let score = 0;
let isPlaying;
let maxScore;
// DOM Elements
const wordInput = document.querySelector('#word-input');
const currentWord = document.querySelector('#current-word');
const scoreDisplay = document.querySelector('#score');
const timeDisplay = document.querySelector('#time');
const message = document.querySelector('#message');
const seconds = document.querySelector('#seconds');
const highScoreElt = document.querySelector('#high-score');
const easyBtn = document.querySelector('#easy');
const mediumBtn = document.querySelector('#medium');
const hardBtn = document.querySelector('#hard');


const words = [
    'grid',
    'swift',
    'rails',
    'ruby',
    'python',
    'java',
    'tech',
    'clear',
    'echo',
    'let',
    'wall',
    'laughter',
    'hash',
    'kotlin',
    'mobile',
    'android',
    'javascript',
    'web',
    'program',
    'coding',
    'basic',
    'foodie',
    'work',
    'case',
    'react',
    'dragon',
    'rush',
    'api',
    'virtual',
    'nerd',
    'google',
    'float',
    'docker',
    'block',
    'rank',
    'class',
    'machine',
    'perfect',
    'deploy',
    'terminal',
    'array',
    'vue',
    'node',
    'issue',
    'front',
    'grid',
    'geek',
    'mac',
    'console',
    'clone',
    'heroku',
    'slack',
    'version',
    'control',
    'data',
    'npm',
    'developer',
    'node'
];

const settingOption = document.getElementById('optionBtn');

settingOption.addEventListener('click', function(){
    
});

function setlevel(e){
    if(e.target === easyBtn){
        currentLevel = levels.easy;
    }else if(e.target === mediumBtn){
        currentLevel = levels.medium;
    }else if(e.target === hardBtn){
        currentLevel = levels.hard;
    }
    console.log(currentLevel);
    init();
}

function init(){
    // Show number of sec in UI
    seconds.innerHTML = currentLevel;
    // Load word from array
    showWord(words);
    // Start matching word input
    wordInput.addEventListener('input', startMatch);
    // Call countdown every second
    setInterval(countdown, 1000);
    // Check game status
    setInterval(checkStatus, 50);
    maxScore = localStorage.getItem('highScore');
    highScoreElt.innerHTML = maxScore;
}

function startMatch(){
    wordInput.value = wordInput.value.toLowerCase();
    if(matchWords()){
        isPlaying = true;
        time = currentLevel + 1;
        showWord(words);
        wordInput.value = '';
        score++;
    }
    
    if(score === -1){
        scoreDisplay.innerHTML = 0;
    }else{
        scoreDisplay.innerHTML = score;
        highScoreElt.innerHTML = score;
        
        if(score >= maxScore){
            localStorage.setItem('highScore',score);
        }
    }
    maxScore = localStorage.getItem('highScore');
    scoreDisplay.innerHTML = score;

}

function matchWords(){
    
        if(wordInput.value === currentWord.innerHTML){
            message.innerHTML = 'Great 👌';
            return true;
        }else{
            message.innerHTML = '🙄';
            return false;
        }
}

function showWord(word){
    // Generate random array index
    const randIndex = Math.floor(Math.random() * words.length);
    // Output random word
    currentWord.innerHTML = words[randIndex];
}

function countdown(){
    // Make sure time is not runout
    if(time > 0){
        // decrement
        time--;
    }else if(time === 0){
        // Game is over
        isPlaying = false;
    }
    // Show time
    timeDisplay.innerHTML = time;
}

function checkStatus(){
    if(!isPlaying && time === 0){
        message.innerHTML = 'Game Over!';
        score = -1;
    }
}

easyBtn.addEventListener('click', setlevel);
mediumBtn.addEventListener('click', setlevel);
hardBtn.addEventListener('click', setlevel);</script>
</html>
