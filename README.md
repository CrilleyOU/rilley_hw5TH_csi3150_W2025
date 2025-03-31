# Problem Statement

This application is a 5 question Vanilla JavaScript quiz. Using a single stylesheet, single html page, and two JavaScript files a 5 question timed quiz that repeats as much as the user wants is created. This app focuses on creating a dynamically updating html page that seamlessly allows user interaction to take a quiz with immediate feedback following each question. This app also incorporates a timed aspect to taking the quiz, moving the user along if they are not quick enough with their answers. At the conclusion of the quiz, the user is prompted with their score and asked if they want to try again, or not. This ensures that there isn’t any need to reload the page in order to regain functionality on the users half.

# Features

- Initiate Quiz Start
- Timer for questions, with bar to show progress
- Quiz Navigation (Next Question, Start/Restart Quiz, Finish)
- Display Questions
- Answer Selection via Click
- Immediate Feedback for each question (Red X, or Green Check)
- Overall Quiz Feedback (tracking of total correct answers)

# Directory Structure

- In the root of our project directory, we have one html file. Our index.html
- This index.html calls our stylesheet and JavaScript Files. It is essentially where the application is running as everything gets called here and brought together
- questions.js simply holds an array with our questions. If someone wanted to change a question, or all of the questions, they would just have to go into this file and update the array to their liking
- quizApp.js

# Code Explanations:

## index.html

Our index.html file starts off like any standard html file. We have our DOCTYPE declaration, set language to english and fill up the head with your standard html information. Also within our head we import our stylesheet, and our two JavaScript files. While importing our two javascript files you can see we use the boolean attribute defer.

```html
<script src="js/questions.js" defer></script>
<script src="js/quizApp.js" defer></script>
```

This ensures that we don’t load our scripts before the document has finished parsing.

Beyond this in our HTML we really only have content that fills the instructional section, the results, and information that fills the top of our question box.

Our start button simply is an html button element, nested within a div to help pinpoint it within our DOM, for both styling and JS purposes.

```html
<div class="start_btn"><button>Start Quiz</button></div>
```

### Instruction Box

The instruction box includes lots of content that is right in our `index.html` file. This box is not dynamically updated by any of our JavaScript functionality.  

#### HTML Structure: 

```html
<div class="info_box">
  <div class="info-title"></div>
  <div class="info-list"></div>
  <div class="buttons"></div>
</div>
```

Above is a snippet of html code outlining our DOM structure without including any content. In reality, this contains text to fill up the info box but here we'll infer based on our class identifiers what each of our divs contains.

We start with our overall info box, this holds each aspect of the instructions of our quiz. Then one level below this, we have our three parts of the info box, our title, our list of rules, and our buttons.

#### Buttons

In our info box we include some class identifiers for each of our buttons that will be referenced in our JavaScript later.

```html
<div class="buttons">
  <button class="quit">Exit Quiz</button>
  <button class="restart">Continue</button>
</div>
```

Here we add quit and restart as class identifiers to our buttons.

### Quiz Box

The quiz box is the main section where the questions and options are displayed dynamically. It is structured in the HTML and updated using JavaScript.

#### HTML Structure:

```html
<div class="quiz_box">
  <header>
    <div class="title">Quiz Application</div>
    <div class="timer">
      <div class="time_left_txt">Time Left</div>
      <div class="timer_sec">15</div>
    </div>
    <div class="time_line"></div>
  </header>
  <section>
    <div class="que_text"></div>
    <div class="option_list"></div>
  </section>
  <footer>
    <div class="total_que"></div>
    <button class="next_btn">Next</button>
  </footer>
</div>
```

This snippet includes all of the HTML content that is truly within index.html, unlike the previous that just showed the div structure. Here we can see lots of empty divs, that are only there to be referenced later by our JavaScript and Stylesheet

### Results Box

The results box is displayed at the end of the quiz to show the user's performance.

#### HTML Structure:

```html
<div class="result_box">
  <div class="icon"><i class="fas fa-crown"></i></div>
  <div class="complete_text">You've completed the Quiz!</div>
  <div class="score_text"></div>
  <div class="buttons">
    <button class="restart">Restart Quiz</button>
    <button class="quit">Quit Quiz</button>
  </div>
</div>
```

This snippet also includes all of the HTML content for this section that is in index.html. We see another empty div of `score_text` that will be filled later with our JavaScript. 

### Styles.css

The `styles.css` file is responsible for the visual appearance of the application. It styles the quiz box, buttons, timer, progress bar, and results box.

#### Key Features:

- **Responsive Design:** Ensures the quiz looks good on different screen sizes, keeps all of the content centered on the page regardless of window size.
- **Dynamic Feedback:** Correct answers are highlighted in green, and incorrect answers in red.
- **Progress Bar Animation:** The `time_line` width is animated to visually represent the remaining time.

### quizApp.js

This is the main JavaScript file that controls the quiz's functionality.

#### Document API Usage : 
To start our main functionality, we first grab our elements from our html. Since we have a well defined DOM, using specific html elements and class names, we can easily select specific aspects of our document. 

```javascript
const start_btn = document.querySelector(".start_btn button");
```
Here, we grab our start button from our html, pointing it to a variable `start_btn` in our Javascript. 

#### Functions:

These functions are responsible for 

1. **`showQuestions(index)`**

   - Dynamically displays the question and options based on the current index. (more on this later)
   - This function builds the html needed to display our questions, saving it to  2 variables named `que_tag` and `option_tag`
   - This is then added to our document using the innerHTML property of our que_text variable that has a block scope, and our option list variable which has a global scope as it was defined earlier. 
   - The onclick attribute is then applied to all of our options that we just created using a for loop

2. **`startTimer(time)`**

   - Starts a countdown timer for each question using the `setInterval` function that is built in to JavaScript
   - This function is also responsible for what happens if the timer hits 0 and the user does nothing
        - The countdown timer gets cleared, and visibly all time related functionality of the page is idle
        - The questions array is accessed in order to find the correct answer, which is then highlighted

3. **`startTimerLine(time)`**

   - This function animates the progress bar to visually represent the remaining time.
   - This is achieved through the creation of a local function `timer` that increases the width of the `time_line` variable that was defined at the begining of the file. 
   - This local function also contains an if statement, which stops the length of the line from exceeding 549 px. 

4. **`optionSelected(answer)`**

   - Validates the user's selected answer by 
        - Creating two new variables `userAns` and `correctAns`
        - Checking if `userAns == correctAns`
   - Provides immediate feedback (correct/incorrect) and disables further interaction.
        - If `userAns == correctAns` returns true
            - Update the UI with green coloring around the selected answer and a check
            - Log that the answer was correct and increment our `userScore` variable
        - If `userAns == correctAns` returns false
            - Update the UI with red coloring around the selected answer and an X
            - Log that the answer was wrong and do nothing to the `userScore` variable

5. **`showResult()`**

   - Displays the results box with the user's score and performance feedback.
   - This hides our info box and quiz boxes from the view of the user. 
   - This function contains lots of html that is added to our `scoreText` variable pointing to our `score_text` div. 

#### Event Listeners

In this app, we trigger all of our functions using event listeners. 

##### Key Events

1. `start_btn` gets clicked
    - show our info box
2. `exit_btn` gets clicked
    - hide our info box
3. `continue_btn` gets clicked
    - Hide the info box
    - Show the quiz box
    - Call functions
        - `showQuestions`
        - `startTimer`
        - `startTimerLine`
4. `restart_btn` gets clicked




### questions.js

This file just contains the array of questions, options, and correct answers. Each question is stored as an Object Literal. 

#### Structure:

```javascript
let questions = [
  {
    numb: 1,
    question: "What does HTML stand for?",
    answer: "Hyper Text Markup Language",
    options: [
      "Hyper Text Preprocessor",
      "Hyper Text Markup Language",
      "Hyper Text Multiple Language",
      "Hyper Tool Multi Language",
    ],
  },
  {
    numb: 2,
    question: "What does CSS stand for?",
    answer: "Cascading Style Sheet",
    options: [
      "Common Style Sheet",
      "Colorful Style Sheet",
      "Computer Style Sheet",
      "Cascading Style Sheet",
    ],
```

#### How it's Reached:

- The `questions` array is imported into `index.html` before `quizApp.js` gets imported.
- The `showQuestions` function within our `quizApp.js` file dynamically retrieves and displays the question and options based on the current index, since the array is available globally. 
