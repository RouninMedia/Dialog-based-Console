# Dialog-based Console
A pop-up console based on the HTML5 `<dialog>` element and its API.

## HTML

```html

```

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

## JavaScript

```js

```
