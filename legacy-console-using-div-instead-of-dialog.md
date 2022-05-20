# Dialog-based Console
A pop-up console based on the HTML5 `<dialog>` element and its API.

## HTML

```html

<dialog class="console --hidden">
<h2 class="consoleHeading">Congratulations!</h2>
<p class="consoleParagraph">The correct answer is:</p>
<p class="consoleAnswer"></p>
<p class="consoleParagraph">You beat <strong>Wordis³h</strong> in</p>
<p class="consoleGuessNumber">0 guesses</p>
</dialog>

<dialog class="console --hidden">
<h2 class="consoleHeading">Bad Luck!</h2>
<p class="consoleParagraph">Your best guess was:</p>
<p class="consoleAnswer"></p>
<p class="consoleParagraph">Better luck with <strong>Wordis³h</strong> next time!</p>
</dialog>

```

______

## CSS

```css

.console {
  position: absolute;
  top: 0;
  left: 0;
  width: 80vw;
  max-width: 600px;
  margin: 12.5vh auto;
  padding-bottom: 54px;
  color: rgb(255, 255, 255);
  font-family: sans-serif;
  text-align: center;
  border-width: 3px;
  border-radius: 9px;
  box-shadow: 0 0 0 100vmax rgba(0, 0, 0, 0);
  transform: translateY(0);
  transition: box-shadow 1.2s ease-out, transform 0.9s ease-out 0.6s;
}

.console.\--hidden {
  transform: translateY(100vh);
}

.console.\--success {
  background-color: var(--correctBackground);
  border: var(--correctBorder);
  box-shadow: 0 0 0 100vmax rgba(0, 91, 31, 0.9);
}

.console.\--gameover {
  background-color: var(--gameoverBackground);
  border: var(--gameoverBorder);
  box-shadow: 0 0 0 100vmax rgba(127, 0, 0, 0.9);
}

[class^="console"] {
  user-select: none;
}

.consoleHeading,
.consoleGuessNumber {
  font-size: 28px;
  text-transform: uppercase;
  text-shadow: 2px 2px 2px rgba(0, 0, 0, 0.5);
}

.consoleGuessNumber {
  font-size: 24px;
  font-weight: 700;
}

.consoleAnswer {
  display: inline-block;
  padding: 6px;
  color: rgb(0, 0, 0);
  background-color: rgb(255, 255, 255);
  border-radius: 9px;
}

.console.\--success .consoleAnswer {
  border: var(--correctBorder);
}

.console.\--gameover .consoleAnswer {
  border: var(--gameoverBorder);
}

.consoleAnswer .letter {
  display: inline-block;
  width: 45px;
  height: 45px;
  margin: 3px;
}

.consoleAnswer .letter.\--transparentLetter {
  color: rgba(0, 0, 0, 0);
}

@media only screen and (max-width: 540px) {

  .console {
    width: 86vw;
  }

  .consoleHeading,
  .consoleGuessNumber {
    font-size: 5vw;
  }

  .consoleAnswer .letter {
    width: 12vw;
    height: 12vw;
    line-height: 12vw;
    font-size: 10vw;
  }
}

```
_____

## JavaScript

```js

const successConsole = document.getElementsByClassName('console')[0];
const gameoverConsole = document.getElementsByClassName('console')[1];

const closeConsole = () => {

  let header = document.querySelector('header');
  let currentConsole = document.querySelector('dialog[open]');
  document.removeEventListener('keydown', closeConsoleOnEnter);

  currentConsole.close();
  let console = '--' + document.body.dataset.console;
  document.body.removeAttribute('data-console');
  document.body.removeAttribute('style');
  header.classList.add(console);
  header.setAttribute('title', 'Click to play another game of Wordis³h...!');
  document.querySelector('h1').style.setProperty('pointer-events', 'none');
      
  document.querySelector('header').addEventListener('click', (e) => {
    window.location.reload();
  }, false);

  document.addEventListener('keydown', (e) => {
    if (e.key === 'Enter') {window.location.reload();}
  });
}


const closeConsoleOnEnter = (e) => {

  if (e.key === 'Enter') {

    closeConsole();
  }
}


const revealConsole = (consoleName) => {

  document.removeEventListener('keydown', clickKey, false);

  setTimeout(() => {

    document.body.dataset.console = consoleName;
    document.body.style.setProperty('overflow', 'hidden');
    document.body.style.setProperty('cursor', 'pointer');
    document.querySelector('.keyboard').classList.add('--disabled');

    let currentConsole;
    let consoleRowIndex;

    if (consoleName === 'gameover') {

      currentConsole = gameoverConsole;

      for (let i = 14; (i + 1) > 0; i--) {

        if (consoleRowIndex !== undefined) break;

        for (let j = 0; j < 6; j++) {

          if (rowScores[j][1] === i) {

            consoleRowIndex = j;
          }
        }
      }
    }


    else if (consoleName === 'success') {

      currentConsole = successConsole;
      consoleRowIndex = targetRowIndex;
    }

    
    for (let i = (consoleRowIndex * 5); i < (consoleRowIndex * 5 + 5); i++) {

      let consoleLetter = letters[i].cloneNode(true);
      consoleLetter.classList.remove('--current');
      consoleLetter.classList.remove('--reveal');
      consoleLetter.classList.remove('--wordRevealed');
      consoleLetter.classList.add('--transparentLetter');
      currentConsole.getElementsByClassName('consoleAnswer')[0].appendChild(consoleLetter);
    }

    currentConsole.show();
    currentConsole.classList.add('--' + consoleName);
    currentConsole.classList.remove('--hidden');
    
    [...currentConsole.getElementsByClassName('letter')].forEach((box, i) => {

      setTimeout(() => {
        box.classList.remove('--transparentLetter');
        conditionalPlay(keyPress);
      }, (1600 + (i * 300)));

      setTimeout(() => {
        box.classList.add('--reveal');
      }, (3200 + (i * 100)));

    });

    setTimeout(() => {repeatAudio(letterReveal, 5);}, 3200);

    document.addEventListener('keydown', closeConsoleOnEnter);
    document.body.addEventListener('click', closeConsole, {once: true, capture: false});

  }, 1900);
}

if (consoleCondition === true) {

  if (success === true) {
  
    revealConsole('success');
  }
  
  else if (gameover === true) {
  
    revealConsole('gameover');
  }
}

```
