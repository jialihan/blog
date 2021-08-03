## JavaScript - Facade Design Pattern

#### I. [What is Decorator Pattern?](#chapter1)

#### II. [Code Example 1](#chapter2)

#### III. [Reference and Useful Links](#chapter6)

<div id="chapter1" />

### I. What is Decorator Pattern?

The mediator pattern alleviates this situation promoting loose coupling and helping improve maintainability. In this pattern the independent objects (colleagues) do not communicate directly, but through a mediator object.

![image](../assets/mediator_pattern_image.png)

The participating objects will be:

- Player 1
- Player 2
- Scoreboard
- Mediator

<div id="chapter2" />

### II. Code Example - mediator setup

```js
var mediator = {
  // all the players
  players: {},

  // initialization
  setup: function () {
    var players = this.players;
    players.home = new Player("Home");
    players.guest = new Player("Guest");
  },

  // someone plays, update the score
  played: function () {
    var players = this.players,
      score = {
        Home: players.home.points,
        Guest: players.guest.points
      };

    scoreboard.update(score);
  },

  // handle user interactions
  keypress: function (e) {
    e = e || window.event; // IE
    if (e.which === 49) {
      // key "1"
      mediator.players.home.play();
      return;
    }
    if (e.which === 48) {
      // key "0"
      mediator.players.guest.play();
      return;
    }
  }
};
```

How to init & play the game?

```js
// go!
mediator.setup();
window.onkeypress = mediator.keypress;
// game over in 30 seconds
setTimeout(function () {
  window.onkeypress = null;
  alert("Game over!");
}, 30000);
```

<div id="chapter3" />

### III. Reference and Useful Links

- https://learning.oreilly.com/library/view/javascript-patterns/9781449399115/ch07.html
