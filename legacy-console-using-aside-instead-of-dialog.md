# Legacy Console (using `<aside>` instead of `<dialog>`)
An alternative for older browsers which do not recognise `<dialog>`

## HTML

```html

<aside class="console --success">
<h2 class="consoleHeading">Congratulations!</h2>
<p class="consoleParagraph">The correct answer is:</p>
<p class="consoleAnswer"></p>
<p class="consoleParagraph">You beat <strong>Wordis³h</strong> in</p>
<p class="consoleGuessNumber">0 guesses</p>
</aside>

<aside class="console --gameover">
<h2 class="consoleHeading">Bad Luck!</h2>
<p class="consoleParagraph">Your best guess was:</p>
<p class="consoleAnswer"></p>
<p class="consoleParagraph">Better luck with <strong>Wordis³h</strong> next time!</p>
</aside>

```

______

## CSS

```css

.console {
  position: absolute;
  top: 0;
  left: 0;
  width: 600px;
  margin: 12.5vh calc((100vw - 600px) / 2);
  padding-bottom: 54px;
  color: rgb(255, 255, 255);
  font-family: sans-serif;
  text-align: center;
  border-radius: 9px;
  box-shadow: 0 0 0 100vmax rgba(0, 0, 0, 0);
  opacity: 0;
  box-sizing: border-box;
  transform: translateY(100vh);
  transition: box-shadow 1.2s ease-out, transform 0.9s ease-out 0.6s;
}

.console.\--open {
  opacity: 1;
  transform: translateY(0);
}

.console.\--success {
  background-color: var(--correctBackground);
  border: 12px solid var(--correctBorder);
}

.console.\--gameover {
  background-color: var(--gameoverBackground);
  border: 12px solid var(--gameoverBorder);
}

.console.\--success.\--open {
  box-shadow: 0 0 0 100vmax rgba(0, 91, 31, 0.9);
}

.console.\--gameover.\--open {
  box-shadow: 0 0 0 100vmax rgba(127, 0, 0, 0.9);
}

.console.\--success.\--closing,
.console.\--gameover.\--closing {
  transition: transform 1.2s ease-out, box-shadow 0.9s ease-out 0.6s, opacity 0s linear 1.5s;
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
  border: 6px solid var(--correctBorder);
}

.console.\--gameover .consoleAnswer {
  border: 6px solid var(--gameoverBorder);
}

.consoleAnswer .letter {
  position: relative;
  z-index: 6;
  display: inline-block;
  width: 45px;
  height: 45px;
  margin: 3px;
  border-width: 3px;
}

.consoleAnswer .letter.\--transparentLetter {
  color: rgba(0, 0, 0, 0);
  text-shadow: none;
}


@media screen and (max-width: 750px) {
  
  .console {
    width: 80vw;
    margin: 12.5vh 10vw;
  }
}


@media screen and (max-width: 500px),
       screen and (max-height: 560px) {

  .console {
    width: 92vw;
    margin: 12.5vh 4vw;
  }

  .console.\--success {
    border: 9px solid var(--correctBorder);
  }

  .console.\--gameover {
    border: 9px solid var(--gameoverBorder);
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

const successConsole = document.querySelector('.console.\--success');
const gameoverConsole = document.querySelector('.console.\--gameover');

const closeConsole = () => {

  let currentConsole = document.querySelector('.console.\--open');
  document.removeEventListener('keydown', closeConsoleOnEnter);

  currentConsole.classList.add('--closing');
  currentConsole.classList.remove('--open');

  document.body.removeAttribute('style');
  wordgrid.classList.remove('--grayscaled');
  keyboard.classList.remove('--grayscaled');

  header.classList.add('--nextGame');
  header.setAttribute('title', 'Click to play another game of Wordis³h...!');
  header.addEventListener('click', initialiseWordish, false);

  document.addEventListener('keydown', initialiseOnEnter);
}


const closeConsoleOnEnter = (e) => {

  if (e.key === 'Enter') {

    closeConsole();
  }
}


const revealConsole = (consoleName) => {
  
  document.removeEventListener('keydown', clickKey, false);

  setTimeout(() => {

    document.body.style.setProperty('cursor', 'pointer');
    keyboard.classList.add('--disabled');
    keyboard.classList.add('--grayscaled');
    wordgrid.classList.add('--grayscaled');
    header.classList.add('--' + consoleName);

    let currentConsole;
    let consoleRowIndex;

    if (consoleName === 'gameover') {

      document.body.style.setProperty('background-color', 'var(--gameoverBackground)');

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

      document.body.style.setProperty('background-color', 'var(--correctBackground)');

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

    currentConsole.classList.add('--open');
    
    [...currentConsole.getElementsByClassName('letter')].forEach((box, j) => {

      setTimeout(() => {
        box.classList.remove('--transparentLetter');
        conditionalPlay(keyPress);
      }, (1600 + (j * 300)));

      
      setTimeout(() => {
        box.classList.add('--reveal');
      }, 3200);

    });

    setTimeout(() => {repeatAudio(letterReveal, 5);}, 3200);

    document.addEventListener('keydown', closeConsoleOnEnter);
    document.body.addEventListener('click', closeConsole, {once: true, capture: false});

  }, 1900);
}

```
