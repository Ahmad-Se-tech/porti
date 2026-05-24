---
layout: post
codemirror: true
title: Sprint 6 Final Project
permalink: /college-ready-portfolio
---

# Final Portfolio

This is my final portfolio ik its long 

## CS111 Portfolio Checklist

| Done | Category | Objective | Evidence |
|------|----------|-----------|----------|
| ☑ | OOP | Writing Classes | `GameLevelForestDeath`, `NpcAiChat`, and 5 other level classes |
| ☑ | OOP | Methods & Parameters | `cleanAndTransition(targetLevelClass, primaryGame)`, `_bubble(text, who)`, `launchSublevel(levelClass)` |
| ☑ | OOP | Instantiation & Objects | `this.classes` arrays in every level; 12 objects in `GameLevelMazeSub` |
| ☑ | OOP | Inheritance (Basic) | `GameObject → Character → Player / Npc`, plus `Barrier`, `Coin`, `GameEnvBackground` |
| ☑ | OOP | Method Overriding | `interact` and `reaction` overridden on every NPC sprite config |
| ☑ | OOP | Constructor Chaining | `new NpcAiChat(...)` x3 in `GameLevelForestWin`; `new GameControl(..., { parentControl })` |
| ☑ | Control Structures | Iteration | Fisher-Yates `for` loop, `forEach` on container children, `.map()` for door sprites, `for` loop for typing dots |
| ☑ | Control Structures | Conditionals | NPC interaction guard (`if dialogueSystem && isDialogueOpen`), Exit Warden transition, `NpcAiChat.close()` guard |
| ☑ | Control Structures | Nested Conditions | 3-level nesting in `GameLevelDoors.js`; empty-input → `try/catch` → reply/error in `NpcAiChat.send()` |
| ☑ | Data Types | Numbers | `SCALE_FACTOR`, `STEP_FACTOR`, `ANIMATION_RATE`, pixel dimensions, `Math.round(rx * width)` |
| ☑ | Data Types | Strings | `id: 'The Wraith'`, sprite `src` paths, template literals for CSS and error messages |
| ☑ | Data Types | Booleans | `GRAVITY: false/true`, `isCorrect`, `isOpen()` returning `!!this.container && ...` |
| ☑ | Data Types | Arrays | `dialogues[]`, `doorConfigs[]`, `xPositions[]`, `this.history[]`, `this.classes[]` |
| ☑ | Data Types | Objects (JSON) | Full sprite config objects (`sprite_data_r2d2`), API request body via `JSON.stringify(...)` |
| ☑ | Operators | Mathematical | `Math.round(rx * width)`, `Math.floor(Math.random() * n)`, `% taunts.length`, `_tauntIndex++` |
| ☑ | Operators | String Operations | Template literals for CSS (`justify-content:${...}`), error strings (`API ${res.status}`), key lookup with `\|\|` fallback |
| ☑ | Operators | Boolean Expressions | `&&` interaction guard, `\|\|` fallback for `parentControl`, `!` negation, `?.` and `??` in API reply |
| ☑ | Input / Output | Keyboard Input | Player controlled via engine key event listeners (arrow keys / WASD) |
| ☑ | Input / Output | Canvas Rendering | `draw()` via GameEngine; sprites, backgrounds, platforms rendered each level |
| ☑ | Input / Output | GameEnv Configuration | `gameEnv` passed into every level constructor; `GameControl` and level arrays configured in each file |
| ☑ | Input / Output | API Integration | `fetch` POST to AI NPC API in `NpcAiChat._ask()` with model and messages |
| ☑ | Input / Output | Asynchronous I/O | `async _ask()` method; `await fetch(...)` in `NpcAiChat` |
| ☑ | Input / Output | JSON Parsing | `JSON.stringify(...)` for API body; `data.content.find(b => b.type === 'text')?.text ?? '...'` |
| ☑ | Documentation | Code Comments | Inline comments throughout all level files explaining logic |
| ☑ | Documentation | Mini-Lesson Documentation | Portfolio page with embedded live game runners |
| ☑ | Documentation | Code Highlights | Annotated code snippets with explanations for every objective |
| ☑ | Debugging | Console Debugging | `console.log("Initializing GameLevelForestDeath...")`, `console.error('NPC AI error:', e)`, `console.log` in all code runners |
| ☑ | Debugging | Hit Box Visualization | Tuned widthPercentage and heightPercentage on all sprite hitboxes in GameLevelMazeSub.js by toggling the GameEngine's hitbox overlay |
| ☑ | Debugging | Source-Level Debugging | Set a breakpoint on line 54 of GameLevelForestDeath.js to step through `if (dist < 50 && !chaseState.caught)` and find the correct catch threshold |
| ☑ | Debugging | Network Debugging | Used the Network tab to find GameLevelForestSub.js returning a 404 while all other scripts loaded 200 — traced to a mismatched import path |
| ☑ | Debugging | Application Debugging | Inspected the DOM to find stale canvas elements left behind after sublevel transitions; fixed with `Array.from(gameContainer.children).forEach(...)` cleanup |
| ☑ | Debugging | Element Inspection | Console showed `TypeError: Failed to fetch dynamically imported module` from GameExecutor.js:338; traced the blob import failure back to a renamed file with an outdated import string |
| ☑ | Testing | Gameplay Testing | Live game runner embeds for every level in the portfolio |
| ☑ | Testing | Integration Testing | Live NPC AI chat in `GameLevelForestWin` |
| ☑ | Testing | API Error Handling | `try/catch` in `NpcAiChat.send()`; `console.error('NPC AI error:', e)` |
---

## Writing Classes

My game, Escape the Tower, is built around **six custom level classes** that sit on top of the GameEngine's base system. Each one is its own file and its own class — `GameLevelForestDeath` is a good example to walk through since it shows the core pattern clearly.

The constructor takes a `gameEnv` reference, then builds a `this.classes` array that the engine uses to figure out what objects to create when the level loads:

```javascript
// GameLevelForestDeath.js
class GameLevelForestDeath {
  constructor(gameEnv) {
    console.log("Initializing GameLevelForestDeath...");
    this.gameEnv = gameEnv;
    this.classes = [
      { class: GameEnvBackground, data: image_data_bg       },
      { class: Player,            data: sprite_data_player   },
      { class: Npc,               data: sprite_data_beckoner },
      { class: Npc,               data: sprite_data_victim   },
    ];
  }
}
export default GameLevelForestDeath;
```

Every one of my seven levels — `GameLevelMaze`, `GameLevelMazeSub`, `GameLevelDoors`, `GameLevelForest`, `GameLevelForestSub`, `GameLevelForestDeath`, and `GameLevelForestWin` — is built this same way. They each have their own NPC setups, sprite configs, and transition logic, but they all share this constructor pattern.

You can play the Forest Death level below to see `GameLevelForestDeath` running live:

{% capture challenge_oop_writing %}
Play the Forest Death level. Use WASD or arrow keys to move. Approach the NPCs to trigger interactions!
{% endcapture %}

{% capture code_oop_writing %}
import GameControl from "/assets/js/GameEnginev1.1/essentials/GameControl.js";
import GameLevelForestDeath from "/assets/js/GameEnginev1.1/GameLevelForestDeath.js";
import GameLevelForestSub from "/assets/js/GameEnginev1.1/GameLevelForestSub.js";

export const gameLevelClasses = [GameLevelForestDeath, GameLevelForestSub];
export { GameControl };
{% endcapture %}

{% include runners/game.html
   runner_id="game_oop_writing"
   challenge=challenge_oop_writing
   code=code_oop_writing
   height="500px"
%}

---

### Methods & Parameters

Two helpers I wrote show up repeatedly across the level files. `cleanAndTransition` in `GameLevelDoors.js` takes two parameters — the target level class and the game instance — and handles the fade + swap sequence:

```javascript
// GameLevelDoors.js
function cleanAndTransition(targetLevelClass, primaryGame) {
  // creates a fade overlay, clears the game container,
  // then calls primaryGame.transitionToLevel()
}
```

`launchSublevel` in `GameLevelForestSub.js` wraps everything needed to spin up a game-inside-a-game into a single one-parameter call:

```javascript
// GameLevelForestSub.js
function launchSublevel(levelClass) {
  const primaryGame = gameEnv.gameControl;
  // ... fade, pause, new GameControl, start
}
```

Run this to see the actual return values from both functions:

{% capture challenge_oop_methods %}
Run the code to see the method signatures from the Escape Game printed with real output.
{% endcapture %}

{% capture code_oop_methods %}
// Mirrors cleanAndTransition(targetLevelClass, primaryGame) — GameLevelDoors.js
function cleanAndTransition(targetLevelClass, primaryGame) {
  return `Transitioning to: ${targetLevelClass} via: ${primaryGame}`;
}

// Mirrors _bubble(text, who) — NpcAiChat in GameLevelForestWin.js
function _bubble(text, who) {
  const align  = who === 'user' ? 'flex-end' : 'flex-start';
  const radius = who === 'user' ? '10px 10px 2px 10px' : '10px 10px 10px 2px';
  return { text, who, align, borderRadius: radius };
}

// Mirrors launchSublevel(levelClass) — GameLevelForestSub.js
function launchSublevel(levelClass) {
  return `Launching sublevel: ${levelClass}`;
}

console.log(cleanAndTransition('GameLevelForest', 'primaryGame'));
console.log(JSON.stringify(_bubble("I came from the left.", "npc"), null, 2));
console.log(JSON.stringify(_bubble("Go right.", "user"), null, 2));
console.log(launchSublevel('GameLevelForestDeath'));
{% endcapture %}

{% include runners/code.html
   runner_id="code_oop_methods"
   language="javascript"
   challenge=challenge_oop_methods
   code=code_oop_methods
%}

---

### Instantiation & Objects

The GameEngine reads `this.classes` and calls `new class(data, gameEnv)` for each entry automatically — so my job in each level is just to build that array correctly. `GameLevelMazeSub.js` is my most object-heavy level with 12 entries total, including six barrier platforms, a coin, three NPCs, and the player:

```javascript
// GameLevelMazeSub.js
this.classes = [
  { class: GameEnvBackground, data: image_data_cave },
  { class: Barrier, data: floor },
  { class: Barrier, data: step1 },
  { class: Barrier, data: step2 },
  { class: Barrier, data: step3 },
  { class: Barrier, data: step4 },
  { class: Barrier, data: step5 },
  { class: Coin,    data: sprite_data_coin    },
  { class: Npc,     data: sprite_data_shadow  },
  { class: Npc,     data: sprite_data_lantern },
  { class: Npc,     data: sprite_data_warden  },
  { class: Player,  data: sprite_data_octopus },
];
```

`GameLevelDoors.js` takes it further by generating door instances dynamically through `.map()` and spreading them into the classes array at the end:

```javascript
// GameLevelDoors.js — dynamic instantiation via .map()
const doorSprites = doorConfigs.map((cfg, i) => {
  const isCorrect = (i === correctIndex);
  return {
    ...doorDefaults,
    id: cfg.id,
    src: cfg.src,
    INIT_POSITION: { x: xPositions[i], y: 0.5 },
    interact: isCorrect ? function() { ... } : function() { ... }
  };
});

this.classes = [
  { class: GameEnvBackground, data: image_data_water },
  { class: Player,            data: sprite_data_octopus },
  ...doorSprites.map(data => ({ class: Npc, data }))
];
```

This runner simulates what the engine does when it processes `this.classes`:

{% capture challenge_oop_instantiation %}
Run the code to simulate how the GameEngine reads this.classes and instantiates all 12 objects from GameLevelMazeSub.
{% endcapture %}

{% capture code_oop_instantiation %}
// Simulating GameEngine instantiation from this.classes

class GameEnvBackground { constructor(data) { this.type = 'Background'; this.name = data.name; } }
class Player  { constructor(data) { this.type = 'Player';  this.id = data.id; } }
class Barrier { constructor(data) { this.type = 'Barrier'; this.id = data.id; } }
class Npc     { constructor(data) { this.type = 'Npc';     this.id = data.id; } }
class Coin    { constructor(data) { this.type = 'Coin';    this.id = data.id; } }

// this.classes from GameLevelMazeSub.js — 12 entries
const classes = [
  { class: GameEnvBackground, data: { name: 'maze' } },
  { class: Barrier, data: { id: 'floor' } },
  { class: Barrier, data: { id: 'step1' } },
  { class: Barrier, data: { id: 'step2' } },
  { class: Barrier, data: { id: 'step3' } },
  { class: Barrier, data: { id: 'step4' } },
  { class: Barrier, data: { id: 'step5' } },
  { class: Coin,    data: { id: 'coin'  } },
  { class: Npc,     data: { id: 'Whispering Shadow' } },
  { class: Npc,     data: { id: 'Lantern Keeper'    } },
  { class: Npc,     data: { id: 'Exit Warden'       } },
  { class: Player,  data: { id: 'Octopus'           } },
];

// What the GameEngine does internally
const gameObjects = classes.map(entry => new entry.class(entry.data));

gameObjects.forEach(obj => {
  const label = obj.id ?? obj.name ?? '(unnamed)';
  console.log(`[${obj.type.padEnd(12)}] ${label}`);
});
console.log(`\nTotal objects instantiated: ${gameObjects.length}`);
{% endcapture %}

{% include runners/code.html
   runner_id="code_oop_instantiation"
   language="javascript"
   challenge=challenge_oop_instantiation
   code=code_oop_instantiation
%}

Play the Maze level to see all 12 objects running at once:

{% capture challenge_oop_maze %}
Play the Maze Sub Level. Climb the staircase platforms to reach the Exit Warden and trigger a level transition!
{% endcapture %}

{% capture code_oop_maze %}
import GameControl from "/assets/js/GameEnginev1.1/essentials/GameControl.js";
import GameLevelMaze from "/assets/js/GameEnginev1.1/GameLevelMaze.js";
import GameLevelMazeSub from "/assets/js/GameEnginev1.1/GameLevelMazeSub.js";

export const gameLevelClasses = [GameLevelMaze, GameLevelMazeSub];
export { GameControl };
{% endcapture %}

{% include runners/game.html
   runner_id="game_oop_maze"
   challenge=challenge_oop_maze
   code=code_oop_maze
   height="500px"
%}

---

### Inheritance (Basic)

My game plugs into the GameEngine's existing inheritance chain rather than rebuilding it from scratch. The hierarchy looks like this:

```
GameObject  (GameEngine base)
  └─ Character
       ├─ Player      (used in every level as the controllable octopus)
       └─ Npc         (used for all NPCs: Wraith, Warden, R2D2, doors, etc.)
  └─ GameEnvBackground
  └─ Barrier          (used in GameLevelMazeSub for staircase platforms)
  └─ Coin             (used in GameLevelDoors, GameLevelMazeSub, GameLevelForestWin)
```

All my level files import and use these engine classes via `extends`. `NpcAiChat` in `GameLevelForestWin.js` is a standalone class that doesn't extend anything from the engine — so that file actually shows both patterns next to each other.

Play the full game here to see every class from the hierarchy active at once:

{% capture challenge_oop_inheritance %}
Play the full Escape Game. Every class in the inheritance hierarchy is running live — Player, Npc, Barrier, Coin, and GameEnvBackground all instantiated from the same engine base.
{% endcapture %}

{% capture code_oop_inheritance %}
import GameControl from "/assets/js/GameEnginev1.1/essentials/GameControl.js";
import GameLevelMaze        from "/assets/js/GameEnginev1.1/GameLevelMaze.js";
import GameLevelMazeSub     from "/assets/js/GameEnginev1.1/GameLevelMazeSub.js";
import GameLevelDoors       from "/assets/js/GameEnginev1.1/GameLevelDoors.js";
import GameLevelForest      from "/assets/js/GameEnginev1.1/GameLevelForest.js";
import GameLevelForestSub   from "/assets/js/GameEnginev1.1/GameLevelForestSub.js";
import GameLevelForestWin   from "/assets/js/GameEnginev1.1/GameLevelForestWin.js";
import GameLevelForestDeath from "/assets/js/GameEnginev1.1/GameLevelForestDeath.js";

export const gameLevelClasses = [GameLevelMaze, GameLevelMazeSub, GameLevelDoors, GameLevelForest, GameLevelForestSub, GameLevelForestWin, GameLevelForestDeath];
export { GameControl };
{% endcapture %}

{% include runners/game.html
   runner_id="game_oop_inheritance"
   challenge=challenge_oop_inheritance
   code=code_oop_inheritance
   height="500px"
%}

---

### Method Overriding

Every NPC in the game has its own `interact` function defined directly on its sprite data object, which overrides the default behavior from the base `Npc` class. The Strange Beckoner in `GameLevelForestDeath.js` is the most involved one — it tracks its own `_tauntIndex`, cycles through a list of taunts, and includes a button that sends the player back to a previous level:

```javascript
// GameLevelForestDeath.js — sprite_data_beckoner
interact: function() {
  if (this.dialogueSystem && this.dialogueSystem.isDialogueOpen()) {
    this.dialogueSystem.closeDialogue();
    return;
  }
  if (!this.dialogueSystem) this.dialogueSystem = new DialogueSystem();

  this._tauntIndex = (this._tauntIndex || 0);
  const taunts = [
    "Oh, you came back to talk to me? Interesting.",
    "Still here? I'd have thought the shame would have driven you off.",
    "You know the right path is still there. Not that it'll help you.",
    "BAWK BAWK. Classic. An absolute classic.",
    "Fine. The fork is back that way."
  ];
  const msg = taunts[this._tauntIndex % taunts.length];
  this._tauntIndex++;

  this.dialogueSystem.showDialogue(msg, "Strange Beckoner", this.spriteData.src);
  this.dialogueSystem.addButtons([
    {
      text: "Go back to the fork",
      action: () => {
        primaryGame.levelClasses      = [GameLevelForestSub];
        primaryGame.currentLevelIndex = 0;
        primaryGame.transitionToLevel();
      }
    },
    { text: "...", action: () => this.dialogueSystem.closeDialogue() }
  ]);
}
```

Run this to watch the taunt cycle in isolation — it wraps back around after 5 presses:

{% capture challenge_oop_override %}
Run the code to see the Strange Beckoner's taunt cycle — the same logic from GameLevelForestDeath.js, cycling through taunts and wrapping back around.
{% endcapture %}

{% capture code_oop_override %}
// Strange Beckoner taunt cycle — mirrors GameLevelForestDeath.js

const taunts = [
  "Oh, you came back to talk to me? Interesting.",
  "Still here? I'd have thought the shame would have driven you off.",
  "You know the right path is still there. Not that it'll help you.",
  "BAWK BAWK. Classic. An absolute classic.",
  "Fine. The fork is back that way."
];

// Simulates pressing interact 7 times — cycles back after 5
let _tauntIndex = 0;
for (let press = 1; press <= 7; press++) {
  const msg = taunts[_tauntIndex % taunts.length];
  console.log(`Press ${press}: "${msg}"`);
  _tauntIndex++;
}
{% endcapture %}

{% include runners/code.html
   runner_id="code_oop_override"
   language="javascript"
   challenge=challenge_oop_override
   code=code_oop_override
%}

---

### Constructor Chaining

`GameLevelForestWin.js` creates three separate `NpcAiChat` instances inside its constructor, each with its own name, system prompt, and avatar image:

```javascript
// GameLevelForestWin.js — three NpcAiChat instances chained from parent constructor
const r2d2Chat     = new NpcAiChat('R2D2',          PERSONA_R2D2,    "/images/.../r2_idle.png");
const elderChat    = new NpcAiChat('Village Elder',  PERSONA_ELDER,   "/images/.../tux.png");
const villagerChat = new NpcAiChat('Villager',       PERSONA_VILLAGER,"/images/.../octocat.png");
```

`GameControl` uses a `parentControl` option so a sublevel can hold a reference back to the game that launched it, which is how the nested game-in-game structure communicates:

```javascript
// GameLevelForest.js — constructor chaining via GameControl
const gameInGame = new GameControl(gameEnv.game, levelArray, {
  parentControl: primaryGame
});
gameInGame.start();
gameInGame.gameOver = function() {
  primaryGame.resume();
};
```

Play the Forest Win level and talk to any of the three NPCs to see the separate chat instances in action:

{% capture challenge_oop_chaining %}
Play the Forest Win level. Walk up to R2D2, the Village Elder, or the Villager and interact with them. Each NPC has its own NpcAiChat instance created by constructor chaining!
{% endcapture %}

{% capture code_oop_chaining %}
import GameControl from "/assets/js/GameEnginev1.1/essentials/GameControl.js";
import GameLevelForestWin from "/assets/js/GameEnginev1.1/GameLevelForestWin.js";

export const gameLevelClasses = [GameLevelForestWin];
export { GameControl };
{% endcapture %}

{% include runners/game.html
   runner_id="game_oop_chaining"
   challenge=challenge_oop_chaining
   code=code_oop_chaining
   height="500px"
%}

---

## Control Structures

### Iteration

I used four different kinds of loops across the project, each in a spot where it made the most sense.

**`for` loop** — Fisher-Yates shuffle in `GameLevelDoors.js` randomizes which x-position each door gets assigned:

```javascript
// GameLevelDoors.js
const xPositions = [0.2, 0.35, 0.5, 0.65, 0.8];
for (let i = xPositions.length - 1; i > 0; i--) {
  const j = Math.floor(Math.random() * (i + 1));
  [xPositions[i], xPositions[j]] = [xPositions[j], xPositions[i]];
}
```

**`forEach` loop** — used on every level transition to strip old canvas elements out of the container before the new level loads:

```javascript
// GameLevelForestDeath.js
Array.from(gameContainer.children).forEach(child => {
  if (child.id !== 'promptDropDown') gameContainer.removeChild(child);
});
```

**`.map()` loop** — `GameLevelDoors.js` transforms an array of door configs into a full array of sprite data objects:

```javascript
// GameLevelDoors.js
const doorSprites = doorConfigs.map((cfg, i) => {
  const isCorrect = (i === correctIndex);
  return { ...doorDefaults, id: cfg.id, src: cfg.src, INIT_POSITION: { x: xPositions[i], y: 0.5 } };
});
```

**`for` loop** — `NpcAiChat._typingBubble()` builds 3 animated dots with staggered delays:

```javascript
// GameLevelForestWin.js
for (let i = 0; i < 3; i++) {
  const d = document.createElement('span');
  d.style.animation = `npcDot 1s ease-in-out ${i * 0.18}s infinite`;
  b.appendChild(d);
}
```

Run this to see the shuffle and `.map()` produce a fresh door layout each time:

{% capture challenge_iteration %}
Run the code to see the Fisher-Yates shuffle and .map() from GameLevelDoors.js. Run it multiple times — the door layout changes every time!
{% endcapture %}

{% capture code_iteration %}
// Iteration — for loop shuffle + .map() from GameLevelDoors.js

const doorConfigs = [
  { id: 'Blue Door'   },
  { id: 'Brown Door'  },
  { id: 'Green Door'  },
  { id: 'Orange Door' },
  { id: 'Red Door'    },
];

// for loop — Fisher-Yates shuffle
const xPositions = [0.2, 0.35, 0.5, 0.65, 0.8];
console.log("Before shuffle:", [...xPositions]);
for (let i = xPositions.length - 1; i > 0; i--) {
  const j = Math.floor(Math.random() * (i + 1));
  [xPositions[i], xPositions[j]] = [xPositions[j], xPositions[i]];
}
console.log("After shuffle: ", [...xPositions]);

// .map() — builds door sprite data from config array
const correctIndex = Math.floor(Math.random() * doorConfigs.length);
const doorSprites  = doorConfigs.map((cfg, i) => ({
  id:        cfg.id,
  isCorrect: i === correctIndex,
  x:         xPositions[i],
}));

console.log("\nDoor layout this run:");
doorSprites.forEach(d =>
  console.log(`  ${d.id.padEnd(14)} x=${d.x}  ${d.isCorrect ? '<-- CORRECT' : ''}`)
);
{% endcapture %}

{% include runners/code.html
   runner_id="code_iteration"
   language="javascript"
   challenge=challenge_iteration
   code=code_iteration
%}

Play the Doors level — the door order is different every single time the page loads:

{% capture challenge_iteration_game %}
Play the Doors Level! The doors are shuffled into a random order every time. Find the correct door to advance to the Forest.
{% endcapture %}

{% capture code_iteration_game %}
import GameControl from "/assets/js/GameEnginev1.1/essentials/GameControl.js";
import GameLevelDoors from "/assets/js/GameEnginev1.1/GameLevelDoors.js";

export const gameLevelClasses = [GameLevelDoors];
export { GameControl };
{% endcapture %}

{% include runners/game.html
   runner_id="game_iteration"
   challenge=challenge_iteration_game
   code=code_iteration_game
   height="500px"
%}

---

### Conditionals

**Interaction guard** — every NPC across every level file uses this same pattern to check whether a dialogue box is already open before doing anything:

```javascript
// GameLevelForest.js — sprite_data_wraith
interact: function() {
  if (this.dialogueSystem && this.dialogueSystem.isDialogueOpen()) {
    this.dialogueSystem.closeDialogue();
    return;
  }
  if (!this.dialogueSystem) {
    this.dialogueSystem = new DialogueSystem();
  }
  this.showRandomDialogue();
}
```

**State transition conditional** — the Exit Warden in `GameLevelMazeSub.js` checks whether it should use a parent controller or the current one before triggering the transition:

```javascript
// GameLevelMazeSub.js — Exit Warden
const topGame = primaryGame?.parentControl || primaryGame;
if (topGame) {
  topGame.levelClasses      = [GameLevelDoors];
  topGame.currentLevelIndex = 0;
  topGame.isPaused          = false;
  topGame.transitionToLevel();
}
```

**Guard conditional** — `NpcAiChat.close()` does nothing if the panel has already been removed from the page:

```javascript
// GameLevelForestWin.js
close() {
  if (!this.isOpen()) return;
  const panel   = this.container.querySelector('.npc-chat-panel');
  const overlay = this.container.querySelector('.npc-chat-overlay');
  if (panel)   { panel.style.opacity = '0'; }
  if (overlay) { overlay.style.opacity = '0'; }
}
```

Run this to see the dialogue guard toggle back and forth across three calls:

{% capture challenge_conditionals %}
Run the code to see the NPC dialogue guard and state transition conditional from the Escape Game. Watch how the dialogue state changes with each interact call.
{% endcapture %}

{% capture code_conditionals %}
// Conditionals — NPC dialogue guard + state transition

const dialogueSystem = {
  _open: false,
  isDialogueOpen() { return this._open; },
  openDialogue()   { this._open = true;  console.log("  -> Dialogue opened"); },
  closeDialogue()  { this._open = false; console.log("  -> Dialogue closed"); },
};

function interact(dialogueSystem) {
  if (dialogueSystem && dialogueSystem.isDialogueOpen()) {
    dialogueSystem.closeDialogue();
    return;
  }
  dialogueSystem.openDialogue();
}

console.log("First interact (dialogue closed):");
interact(dialogueSystem);
console.log("Second interact (dialogue already open):");
interact(dialogueSystem);
console.log("Third interact (closed again):");
interact(dialogueSystem);

// Mirrors Exit Warden transition conditional from GameLevelMazeSub.js
const primaryGame = { parentControl: { name: 'topLevelGame' } };
const topGame = primaryGame?.parentControl || primaryGame;
console.log(`\nTransition target: ${topGame.name}`);
{% endcapture %}

{% include runners/code.html
   runner_id="code_conditionals"
   language="javascript"
   challenge=challenge_conditionals
   code=code_conditionals
%}

---

### Nested Conditions

`GameLevelDoors.js` has three levels of nesting stacked inside a single `.map()` call. The outer layer picks the correct vs. wrong door, the middle layer checks dialogue state, and the deepest layer fires the actual level transition:

```javascript
// GameLevelDoors.js
const doorSprites = doorConfigs.map((cfg, i) => {
  const isCorrect = (i === correctIndex);           // level 1: correct door?

  return {
    interact: isCorrect
      ? function() {
          if (this.dialogueSystem && this.dialogueSystem.isDialogueOpen()) {  // level 2: dialogue open?
            this.dialogueSystem.closeDialogue();
            return;
          }
          if (!this.dialogueSystem) this.dialogueSystem = new DialogueSystem();
          this.dialogueSystem.addButtons([
            {
              text: "Enter",
              action: () => {
                this.dialogueSystem.closeDialogue();
                cleanAndTransition(GameLevelForest, gameEnv.gameControl); // level 3: fire transition
              }
            },
            { text: "Not yet", action: () => this.dialogueSystem.closeDialogue() }
          ]);
        }
      : function() {                                // else branch: dead end
          if (this.dialogueSystem && this.dialogueSystem.isDialogueOpen()) {
            this.dialogueSystem.closeDialogue();
            return;
          }
          if (!this.dialogueSystem) this.dialogueSystem = new DialogueSystem();
          this.dialogueSystem.showDialogue(cfg.deadEnd, "Dead End!", this.spriteData.src);
        }
  };
});
```

Run this to see which door gets picked and what each branch outputs:

{% capture challenge_nested %}
Run the code to see the nested door conditional logic from GameLevelDoors.js. The correct door changes every run — see which one it picks!
{% endcapture %}

{% capture code_nested %}
// Nested conditions — door picker from GameLevelDoors.js

const doorConfigs = [
  { id: 'Blue Door',   deadEnd: "The blue light flickers. Nothing lies beyond."  },
  { id: 'Brown Door',  deadEnd: "Splinters and cobwebs. Nothing lies beyond."    },
  { id: 'Green Door',  deadEnd: "Roots and earth block the way."                 },
  { id: 'Orange Door', deadEnd: "A wall of heat forces you back."                },
  { id: 'Red Door',    deadEnd: "A cold dread seizes your hand. Not this one."   },
];

const correctIndex = Math.floor(Math.random() * doorConfigs.length);
let dialogueOpen   = false;

function interact(i) {
  const isCorrect = (i === correctIndex);          // level 1

  if (dialogueOpen) {                              // level 2: dialogue already open
    console.log(`  [${doorConfigs[i].id}] Closing open dialogue`);
    dialogueOpen = false;
    return;
  }

  if (isCorrect) {
    dialogueOpen = true;
    console.log(`  [${doorConfigs[i].id}] Correct door! Showing transition prompt...`);
    console.log(`  -> Would call: cleanAndTransition(GameLevelForest, primaryGame)`); // level 3
  } else {
    dialogueOpen = true;
    console.log(`  [${doorConfigs[i].id}] Dead end: "${doorConfigs[i].deadEnd}"`);
  }
}

console.log(`Correct door this run: ${doorConfigs[correctIndex].id}\n`);
doorConfigs.forEach((_, i) => interact(i));
{% endcapture %}

{% include runners/code.html
   runner_id="code_nested"
   language="javascript"
   challenge=challenge_nested
   code=code_nested
%}

---

## Data Types

### Numbers

Every level uses numbers for position, scale, animation speed, and pixel dimensions. The octopus player config in `GameLevelForest.js` is a clear example:

```javascript
// GameLevelForest.js — numeric properties on sprite_data_octopus
const OCTOPUS_SCALE_FACTOR = 5;
const sprite_data_octopus = {
  SCALE_FACTOR:    OCTOPUS_SCALE_FACTOR,  // 5
  STEP_FACTOR:     1000,
  ANIMATION_RATE:  50,
  INIT_POSITION:   { x: 0.05, y: 0.85 },
  pixels:          { height: 250, width: 167 },
  hitbox:          { widthPercentage: 0.45, heightPercentage: 0.2 },
};
```

`GameLevelMazeSub.js` calculates all platform positions by multiplying ratios against the canvas dimensions at runtime:

```javascript
// GameLevelMazeSub.js
function b(id, rx, ry, rw, rh) {
  return {
    x:      Math.round(rx * width),
    y:      Math.round(ry * height),
    width:  Math.round(rw * width),
    height: Math.round(rh * height),
  };
}
const step3 = b('step3', 0.41, 0.40, 0.22, 0.03);
```

---

### Strings

Character IDs, image paths, dialogue text, and CSS values are all strings:

```javascript
// GameLevelForest.js
const sprite_data_wraith = {
  id:       'The Wraith',
  greeting: "...it took my family. Both paths lead somewhere.",
  src:      "/images/projects/escape-game/tux.png",
};
```

`NpcAiChat` in `GameLevelForestWin.js` uses template literals to build error messages and dynamic CSS on the fly:

```javascript
// GameLevelForestWin.js
throw new Error(`API ${res.status}`);

row.style.cssText = `display:flex;justify-content:${who === 'user' ? 'flex-end' : 'flex-start'}`;

d.style.animation = `npcDot 1s ease-in-out ${i * 0.18}s infinite`;
```

---

### Booleans

Boolean values show up in gravity flags, selection states, and open/closed checks:

```javascript
// GameLevelForest.js — GRAVITY flag
GRAVITY: false,    // octopus floats in the forest level

// GameLevelMaze.js — GRAVITY flag flipped for dungeon
GRAVITY: true,

// GameLevelDoors.js — boolean for correct door selection
const isCorrect = (i === correctIndex);   // true for exactly one door

// GameLevelForestWin.js — NpcAiChat.isOpen() returns boolean
isOpen() {
  return !!this.container && document.body.contains(this.container);
}
```

---

### Arrays

Arrays hold dialogue lines, door configurations, position sets, chat history, and level class lists:

```javascript
// GameLevelForest.js — dialogues array
dialogues: [
  "...it took my family. Both paths lead somewhere. Not all somewheres are safe.",
  "The trees shift when the fog comes in. I stopped trusting my eyes.",
  "I wandered left. I ended up here. I cannot leave.",
  "Follow the light... if you can find any."
],

// GameLevelDoors.js — array of door config objects
const doorConfigs = [
  { id: 'Blue Door',   src: "...", greeting: "...", deadEnd: "..." },
  { id: 'Brown Door',  src: "...", greeting: "...", deadEnd: "..." },
  { id: 'Green Door',  src: "...", greeting: "...", deadEnd: "..." },
  { id: 'Orange Door', src: "...", greeting: "...", deadEnd: "..." },
  { id: 'Red Door',    src: "...", greeting: "...", deadEnd: "..." },
];

// GameLevelForestWin.js — chat history as growing array
this.history = [];
this.history.push({ role: 'user',      content: text  });
this.history.push({ role: 'assistant', content: reply });
```

---

### Objects (JSON)

Every sprite is configured as a nested object literal. The R2D2 sprite in `GameLevelForestSub.js` is one of the more detailed ones:

```javascript
// GameLevelForestSub.js
const sprite_data_right = {
  id:             'R2D2',
  greeting:       "I have been waiting for someone to choose correctly.",
  src:            "/images/projects/escape-game/r2_idle.png",
  SCALE_FACTOR:   8,
  ANIMATION_RATE: 100,
  pixels:         { height: 223, width: 505 },
  INIT_POSITION:  { x: 0.82,    y: 0.35   },
  orientation:    { rows: 1, columns: 3   },
  down:           { row: 0, start: 0, columns: 3 },
  hitbox:         { widthPercentage: 0.1, heightPercentage: 0.2 },
};
```

The AI API request body is also assembled as a JSON object before being sent:

```javascript
// GameLevelForestWin.js
body: JSON.stringify({
  model:      'claude-sonnet-4-20250514',
  max_tokens: 1000,
  system:     this.systemPrompt,
  messages:   this.history,
}),
```

Run this to see all five data types printed from real sprite config values:

{% capture challenge_datatypes %}
Run the code to see all five data types — Numbers, Strings, Booleans, Arrays, and Objects — printed from real Escape Game sprite configs.
{% endcapture %}

{% capture code_datatypes %}
// All five data types from real Escape Game sprite configs

// Numbers
const OCTOPUS_SCALE_FACTOR = 5;
const sprite_data_octopus = {
  SCALE_FACTOR:   OCTOPUS_SCALE_FACTOR,
  STEP_FACTOR:    1000,
  ANIMATION_RATE: 50,
  INIT_POSITION:  { x: 0.05, y: 0.85 },
  pixels:         { height: 250, width: 167 },
};

// Strings
const sprite_data_wraith = {
  id:       'The Wraith',
  greeting: "...it took my family. Both paths lead somewhere.",
  src:      "/images/projects/escape-game/tux.png",
};

// Booleans
const forestGravity = false;
const mazeGravity   = true;
const isCorrect     = (2 === Math.floor(Math.random() * 5));

// Arrays
const dialogues  = [
  "...it took my family. Both paths lead somewhere.",
  "The trees shift when the fog comes in.",
  "I wandered left. I ended up here. I cannot leave.",
  "Follow the light... if you can find any."
];
const xPositions = [0.2, 0.35, 0.5, 0.65, 0.8];
const history    = [];
history.push({ role: 'user',      content: "Which way should I go?" });
history.push({ role: 'assistant', content: "The right path feels heavier." });

// Objects (JSON)
const sprite_data_r2d2 = {
  id:             'R2D2',
  SCALE_FACTOR:   8,
  pixels:         { height: 223, width: 505 },
  INIT_POSITION:  { x: 0.82,    y: 0.35   },
  orientation:    { rows: 1, columns: 3   },
  hitbox:         { widthPercentage: 0.1, heightPercentage: 0.2 },
};

console.log("=== Numbers ===");
console.log(`SCALE: ${sprite_data_octopus.SCALE_FACTOR}  STEP: ${sprite_data_octopus.STEP_FACTOR}  ANIM: ${sprite_data_octopus.ANIMATION_RATE}`);

console.log("\n=== Strings ===");
console.log(`id: "${sprite_data_wraith.id}"`);
console.log(`greeting: "${sprite_data_wraith.greeting}"`);

console.log("\n=== Booleans ===");
console.log(`forestGravity: ${forestGravity}  mazeGravity: ${mazeGravity}  isCorrect: ${isCorrect}`);

console.log("\n=== Arrays ===");
console.log(`dialogues[0]: "${dialogues[0]}"`);
console.log(`xPositions: [${xPositions}]`);
console.log(`history entries: ${history.length}`);

console.log("\n=== Object (JSON) ===");
console.log(JSON.stringify(sprite_data_r2d2, null, 2));
{% endcapture %}

{% include runners/code.html
   runner_id="code_datatypes"
   language="javascript"
   challenge=challenge_datatypes
   code=code_datatypes
%}

---

## Operators

### Mathematical

Barrier sizes come from multiplying ratios by canvas dimensions and rounding. Door shuffling uses `Math.floor` and `Math.random`. Typing dot delays use multiplication for staggering. Taunt cycling uses modulo so it wraps around:

```javascript
// GameLevelMazeSub.js
x:      Math.round(rx * width),
y:      Math.round(ry * height),
width:  Math.round(rw * width),
height: Math.round(rh * height),

// GameLevelDoors.js
const correctIndex = Math.floor(Math.random() * doorConfigs.length);

// GameLevelForestWin.js
d.style.animation = `npcDot 1s ease-in-out ${i * 0.18}s infinite`;

// GameLevelForestDeath.js
const msg = taunts[this._tauntIndex % taunts.length];
this._tauntIndex++;
```

---

### String Operations

Template literals handle error messages, CSS properties, and animation values throughout `GameLevelForestWin.js`. Key lookup uses an object literal as a map with a `||` fallback for unknown names:

```javascript
// GameLevelForestWin.js
throw new Error(`API ${res.status}`);

row.style.cssText = `display:flex;justify-content:${who === 'user' ? 'flex-end' : 'flex-start'}`;

d.style.animation = `npcDot 1s ease-in-out ${i * 0.18}s infinite`;

// String key lookup in _greeting()
return {
  'R2D2':          'Bweeeep! You made it! Ask me anything!',
  'Village Elder': "We don't get many travellers here.",
  'Villager':      "Oh! A new face! It's been so long!",
}[this.npcName] || 'Hello, traveller.';
```

---

### Boolean Expressions

`&&` and `||` chains guard NPC interactions and handle fallback state. `?.` and `??` keep the AI reply safe when the response structure is unexpected:

```javascript
// Every NPC interact — && compound expression
if (this.dialogueSystem && this.dialogueSystem.isDialogueOpen()) {
  this.dialogueSystem.closeDialogue();
  return;
}

// GameLevelForestWin.js — && in isOpen()
return !!this.container && document.body.contains(this.container);

// GameLevelMazeSub.js — || fallback for nested game control
const topGame = primaryGame?.parentControl || primaryGame;

// GameLevelForestDeath.js — ! negation guard
if (!this.isOpen()) return;

// GameLevelForestWin.js — optional chaining + nullish coalescing
return data.content.find(b => b.type === 'text')?.text ?? '...';
```

Run this to see all three operator categories produce output from real game logic:

{% capture challenge_operators %}
Run the code to see mathematical, string, and boolean operators all producing output from real Escape Game logic.
{% endcapture %}

{% capture code_operators %}
// Operators — mathematical, string, boolean from the Escape Game

// Mathematical — barrier geometry (GameLevelMazeSub.js)
const width = 800, height = 400;
function b(id, rx, ry, rw, rh) {
  return {
    id,
    x:      Math.round(rx * width),
    y:      Math.round(ry * height),
    width:  Math.round(rw * width),
    height: Math.round(rh * height),
  };
}
const step3 = b('step3', 0.41, 0.40, 0.22, 0.03);
console.log("=== Mathematical ===");
console.log(`step3: x=${step3.x} y=${step3.y} w=${step3.width} h=${step3.height}`);

let _tauntIndex = 3;
const taunts = ["Taunt A", "Taunt B", "Taunt C"];
console.log(`Modulo taunt (3 % 3 = ${3 % 3}): "${taunts[_tauntIndex % taunts.length]}"  next index: ${++_tauntIndex}`);

const correctIndex = Math.floor(Math.random() * 5);
console.log(`Correct door index (Math.floor * Math.random): ${correctIndex}`);

// String Operations — template literals (GameLevelForestWin.js)
console.log("\n=== String Operations ===");
const who = 'user', status = 400;
console.log(`CSS justify: "justify-content:${who === 'user' ? 'flex-end' : 'flex-start'}"`);
console.log(`Error string: "API ${status}"`);
const npcName = 'R2D2';
const greeting = {
  'R2D2':          'Bweeeep! You made it! Ask me anything!',
  'Village Elder': "We don't get many travellers here.",
  'Villager':      "Oh! A new face! It's been so long!",
}[npcName] || 'Hello, traveller.';
console.log(`Key lookup for "${npcName}": "${greeting}"`);

// Boolean Expressions — &&, ||, !, ?., ??
console.log("\n=== Boolean Expressions ===");
const dialogueSystem = { _open: true, isDialogueOpen() { return this._open; } };
console.log(`&& guard (dialogue open): ${dialogueSystem && dialogueSystem.isDialogueOpen()}`);
const primaryGame = { parentControl: null };
const topGame = primaryGame?.parentControl || primaryGame;
console.log(`|| fallback — topGame is primaryGame: ${topGame === primaryGame}`);
console.log(`! negation (!false): ${!false}`);
const data = { content: [{ type: 'text', text: 'Hello traveller.' }] };
const reply = data.content.find(b => b.type === 'text')?.text ?? '...';
console.log(`?. and ?? reply: "${reply}"`);
{% endcapture %}

{% include runners/code.html
   runner_id="code_operators"
   language="javascript"
   challenge=challenge_operators
   code=code_operators
%}

Play the full game one last time — every operator, type, and control structure covered above is running inside it:

{% capture challenge_operators_game %}
Play the full Escape Game as a final demo! Every operator, data type, and control structure covered in this portfolio is running live inside.
{% endcapture %}

{% capture code_operators_game %}
import GameControl from "/assets/js/GameEnginev1.1/essentials/GameControl.js";
import GameLevelMaze        from "/assets/js/GameEnginev1.1/GameLevelMaze.js";
import GameLevelMazeSub     from "/assets/js/GameEnginev1.1/GameLevelMazeSub.js";
import GameLevelDoors       from "/assets/js/GameEnginev1.1/GameLevelDoors.js";
import GameLevelForest      from "/assets/js/GameEnginev1.1/GameLevelForest.js";
import GameLevelForestSub   from "/assets/js/GameEnginev1.1/GameLevelForestSub.js";
import GameLevelForestWin   from "/assets/js/GameEnginev1.1/GameLevelForestWin.js";
import GameLevelForestDeath from "/assets/js/GameEnginev1.1/GameLevelForestDeath.js";

export const gameLevelClasses = [GameLevelMaze, GameLevelMazeSub, GameLevelDoors, GameLevelForest, GameLevelForestSub, GameLevelForestWin, GameLevelForestDeath];
export { GameControl };
{% endcapture %}

{% include runners/game.html
   runner_id="game_operators_final"
   challenge=challenge_operators_game
   code=code_operators_game
   height="500px"
%}

---

## Input / Output

### Keyboard Input

Player movement is entirely keyboard-driven. The `keypress` map in each level's Player sprite config binds key codes to directions — `87` = W (up), `65` = A (left), `83` = S (down), `68` = D (right). The GameEngine reads this map every frame and moves the player accordingly:

```javascript
// GameLevelForest.js — keypress map on sprite_data_octopus
keypress: { up: 87, left: 65, down: 83, right: 68 }
```

Every level defines this same map on its Player config. NPC interactions are also keyboard-triggered — the E key fires the `interact` function on whatever NPC the player is currently overlapping.

### Canvas Rendering

Every sprite, background, platform, and NPC is rendered onto a `<canvas>` element each frame through the GameEngine's `draw()` loop. The engine instantiates everything in `this.classes`, then calls `draw()` on every object every tick.

Each sprite config's `ANIMATION_RATE` and `orientation` properties tell the engine which frame of the spritesheet to draw:

```javascript
// GameLevelForestDeath.js — sprite_data_beckoner animation config
ANIMATION_RATE: 80,
pixels:      { height: 255, width: 150 },
orientation: { rows: 1, columns: 1 },
down:        { row: 0, start: 0, columns: 1 },
```

### GameEnv Configuration

Every level constructor receives `gameEnv`, which gives access to the canvas dimensions, the asset path, and the active `GameControl` instance. All positions and sizes are computed from `gameEnv.innerWidth` and `gameEnv.innerHeight` so the game scales correctly at any resolution:

```javascript
// GameLevelMazeSub.js — gameEnv consumed in constructor
constructor(gameEnv) {
  let width  = gameEnv.innerWidth;
  let height = gameEnv.innerHeight;
  let path   = gameEnv.path;

  // All positions derived from width/height — never hardcoded pixels
  const ledge3 = {
    x:      Math.round(width  * 0.40),
    y:      Math.round(height * 0.43),
    width:  Math.round(width  * 0.55),
    height: 18,
  };
}
```

### DOM Output

Level transitions inject a full-screen black `<div>` overlay into the page and fade it in and out using CSS transitions:

```javascript
// GameLevelMazeSub.js — fade overlay built and injected at transition time
const fade = document.createElement('div');
Object.assign(fade.style, {
  position: 'fixed',
  top: '0', left: '0',
  width: '100%', height: '100%',
  backgroundColor: '#000',
  opacity: '0',
  transition: 'opacity 0.8s ease-in-out',
  zIndex: '9999',
  pointerEvents: 'none'
});
document.body.appendChild(fade);

requestAnimationFrame(() => {
  fade.style.opacity = '1';
  setTimeout(() => {
    fade.style.opacity = '0';
    setTimeout(() => {
      if (fade.parentNode) fade.parentNode.removeChild(fade);
    }, 800);
  }, 800);
});
```

Run this to see the fade sequence logged step by step:

{% capture challenge_dom %}
Run the code to see the fade overlay transition sequence from GameLevelMazeSub.js simulated with console output.
{% endcapture %}

{% capture code_dom %}
// DOM Output — fade overlay sequencing from every level transition

function simulateFadeTransition(targetLevel) {
  const fade = {
    style: { opacity: '0', transition: 'opacity 0.8s ease-in-out' },
    parentNode: true,
  };

  console.log(`[append]   opacity=${fade.style.opacity} — overlay added to DOM`);

  fade.style.opacity = '1';
  console.log(`[rAF]      opacity=${fade.style.opacity} — fading to black`);

  console.log(`[timeout]  screen is black — transitioning to: ${targetLevel}`);

  fade.style.opacity = '0';
  console.log(`[fade out] opacity=${fade.style.opacity} — fading back in`);

  console.log(`[remove]   fade removed from DOM\n`);
}

simulateFadeTransition('GameLevelDoors');
simulateFadeTransition('GameLevelForestWin');
{% endcapture %}

{% include runners/code.html
   runner_id="code_dom"
   language="javascript"
   challenge=challenge_dom
   code=code_dom
%}

### Asynchronous I/O

Level transitions are sequenced with `requestAnimationFrame` and nested `setTimeout` calls so nothing blocks the browser. `BeckonerChaseController` in `GameLevelForestDeath.js` uses real elapsed time to gradually increase the chase speed:

```javascript
// GameLevelForestDeath.js — BeckonerChaseController.update()
update() {
  const elapsed = (Date.now() - this.startTime) / 1000;
  if (elapsed < 3) return;   // does nothing for the first 3 seconds

  // Speed increases the longer the player stays — capped at 2.5
  const speed = Math.min(1.2 + (elapsed - 3) * 0.025, 2.5);
}
```

Run this to see the speed ramp in isolation:

{% capture challenge_async %}
Run the code to see the BeckonerChaseController speed ramp from GameLevelForestDeath.js. Watch how the speed increases over time and caps at 2.5!
{% endcapture %}

{% capture code_async %}
// Async I/O — BeckonerChaseController timing from GameLevelForestDeath.js

const startTime = Date.now() - 5000;   // simulate 5 seconds already elapsed

function update() {
  const elapsed = (Date.now() - startTime) / 1000;
  if (elapsed < 3) {
    console.log(`elapsed=${elapsed.toFixed(1)}s — chase not started yet`);
    return;
  }
  const speed = Math.min(1.2 + (elapsed - 3) * 0.025, 2.5);
  console.log(`elapsed=${elapsed.toFixed(1)}s — speed=${speed.toFixed(3)} px/frame`);
}

update();

console.log('\nSpeed ramp over time:');
[0, 1, 2, 3, 5, 10, 30, 60].forEach(t => {
  const speed = t < 3 ? 0 : Math.min(1.2 + (t - 3) * 0.025, 2.5);
  const bar = '█'.repeat(Math.round(speed * 8));
  console.log(`  t=${String(t).padStart(2)}s  speed=${speed.toFixed(3)}  ${bar}`);
});
{% endcapture %}

{% include runners/code.html
   runner_id="code_async"
   language="javascript"
   challenge=challenge_async
   code=code_async
%}

### JSON Parsing

Sprite configs are deeply nested JSON-style object literals throughout the codebase. `GameLevelDoors.js` keeps things clean by defining a shared `doorDefaults` object and spreading it into each individual door config:

```javascript
// GameLevelDoors.js — spreading doorDefaults into each door sprite config
const doorDefaults = {
  SCALE_FACTOR: 8, ANIMATION_RATE: 50,
  pixels:      { height: 414, width: 252 },
  orientation: { rows: 1, columns: 1 },
  hitbox:      { widthPercentage: 0.1, heightPercentage: 0.2 },
};

const doorSprites = doorConfigs.map((cfg, i) => ({
  ...doorDefaults,           // spread shared defaults in
  id:            cfg.id,
  src:           cfg.src,
  INIT_POSITION: { x: xPositions[i], y: 0.5 },
}));
```

Run this to see the spread and `JSON.stringify` output for all five doors:

{% capture challenge_json %}
Run the code to see the object spread and JSON.stringify from GameLevelDoors.js. Each door gets all shared defaults merged with its own config!
{% endcapture %}

{% capture code_json %}
// JSON — object spread + JSON.stringify from GameLevelDoors.js

const doorDefaults = {
  SCALE_FACTOR:   8,
  ANIMATION_RATE: 50,
  pixels:         { height: 414, width: 252 },
  orientation:    { rows: 1, columns: 1 },
  hitbox:         { widthPercentage: 0.1, heightPercentage: 0.2 },
};

const doorConfigs = [
  { id: 'Blue Door',   src: 'bluedoor.png'   },
  { id: 'Brown Door',  src: 'browndoor.png'  },
  { id: 'Green Door',  src: 'greendoor.png'  },
  { id: 'Orange Door', src: 'orangedoor.png' },
  { id: 'Red Door',    src: 'reddoor.png'    },
];

const xPositions = [0.2, 0.35, 0.5, 0.65, 0.8];
const correctIndex = Math.floor(Math.random() * doorConfigs.length);

const doorSprites = doorConfigs.map((cfg, i) => ({
  ...doorDefaults,
  id:            cfg.id,
  src:           cfg.src,
  INIT_POSITION: { x: xPositions[i], y: 0.5 },
  isCorrect:     i === correctIndex,
}));

console.log('Correct door this run:', doorConfigs[correctIndex].id);
console.log('\nAll door sprites (JSON.stringify):');
doorSprites.forEach(d => {
  console.log(JSON.stringify({
    id: d.id, x: d.INIT_POSITION.x, isCorrect: d.isCorrect, pixels: d.pixels
  }));
});
{% endcapture %}

{% include runners/code.html
   runner_id="code_json"
   language="javascript"
   challenge=challenge_json
   code=code_json
%}

---

## Documentation

### Code Comments

Every level file opens with a description comment and has inline comments throughout. `GameLevelMazeSub.js` goes the furthest — it includes an ASCII art diagram of the staircase platform layout directly in the source so anyone reading the file can visualize the geometry before looking at the numbers:

```javascript
// GameLevelMazeSub.js — ASCII layout comment
//
//   ledge5  ░░░░░░░░░░░░░░░░░░░░░░░░░░░  (x 0.55–0.95, y ~0.12)
//   ledge4  ░░░░░░░░░░░░░░░░░░░░         (x 0.10–0.60, y ~0.27)
//   ledge3  ░░░░░░░░░░░░░░░░░░░░░░░░░░░  (x 0.40–0.95, y ~0.43)
//   ledge2  ░░░░░░░░░░░░░░░░░░░░         (x 0.05–0.55, y ~0.58)
//   ledge1  ░░░░░░░░░░░░░░░░░░░░░░░░░░░  (x 0.00–0.50, y ~0.78)
//   floor   ═══════════════════════════  (y 0.90)
//
// The player starts above ledge1 and must climb up-right, up-left, …
```

### Mini-Lesson Documentation

This page is my mini-lesson. Each objective section has a prose explanation of the concept, a direct excerpt from the relevant source file, a code runner where the logic runs live in the browser, and in most cases a full game runner where you can play the level that contains it. The goal was to make every single objective observable, not just readable.

---

### Code Highlights

Every section above pulls annotated excerpts straight from the actual source files, with comments pointing at the specific thing being demonstrated. The code runners are designed to strip out the game engine and run only the piece of logic being discussed — so you can see the exact output without having to play all the way through the game to hit that code path.

---

## Debugging

### Console Debugging

Every level constructor starts with a `console.log` so I could confirm loading order during development. Transition code uses `console.warn` for failures that shouldn't crash the game:

```javascript
// Every level file — initialization confirmation
console.log("Initializing GameLevelForestDeath...");
console.log("Initializing GameLevelMazeSub...");
console.log("Initializing GameLevelDoors...");

// GameLevelDoors.js — logs the correct door at load time
console.log('Correct door this run:', doorConfigs[correctIndex].id);

// Every transition function — warn instead of crashing on optional calls
console.warn('Could not hide parent canvas state', e);
```

### Hit Box Visualization

During `GameLevelMazeSub.js` development, the GameEngine's hitbox overlay was used to verify that all six barrier platforms were positioned and sized correctly. Without it, collisions on the zigzag staircase were inconsistent. Toggling it on let me visually confirm the hitbox boundaries and tune the `widthPercentage` and `heightPercentage` values until they matched the visible sprites:

```javascript
// GameLevelMazeSub.js — narrow hitbox tuned via hitbox visualization
hitbox: { widthPercentage: 0.45, heightPercentage: 0.2 }
```

### Source-Level Debugging

The `BeckonerChaseController` catch detection in `GameLevelForestDeath.js` wasn't working correctly — the player was getting caught too early or not at all. I set a breakpoint inside `update()` and stepped through frame by frame, watching the `dist` variable change in the DevTools scope panel. That's how I found the right threshold value to use:

```javascript
// GameLevelForestDeath.js — catch threshold found via step-through debugging
const dist = Math.hypot(dx, dy) || 1;
if (dist < 50 && !chaseState.caught) {   // value found by watching dist in DevTools
  chaseState.caught = true;
  this.onCaught(beckoner, player);
}
```
![Breakpoint set on line 54 of GameLevelForestDeath.js in Chrome DevTools](images/breakpoint.png)

### Network Debugging

Early in development, some level assets were silently 404-ing because of a wrong path prefix. I opened the Network tab, filtered by status, and found `GameLevelForestSub.js` returning a 404 while every other script loaded with 200. That pointed me directly to the mismatched import path:

```javascript
// GameLevelForest.js — path prefix verified via Network tab
let path = gameEnv.path;
const sprite_data_wraith = {
  src: path + "/images/gamify/tux.png",   // Network tab confirmed this resolved correctly
};
```
![Network tab showing GameLevelForestSub.js returning a 404](images/network-error.png)

### Application Debugging

Transitioning between nested sublevels was leaving old canvas elements in the DOM, which caused visual glitches when new levels loaded on top of them. I found this by opening the Elements panel and watching the DOM during a transition. The fix was the `Array.from(gameContainer.children).forEach(...)` cleanup that now runs in every transition function:

```javascript
// GameLevelMazeSub.js — cleanup added after DOM inspection revealed stale canvases
const gameContainer = document.getElementById('gameContainer');
if (gameContainer) {
  Array.from(gameContainer.children).forEach(child => {
    if (child.id !== 'promptDropDown') gameContainer.removeChild(child);
  });
}
```

### Element Inspection

A `TypeError: Failed to fetch dynamically imported module` kept appearing in the console from `GameExecutor.js:338`. The error message included the blob URL that failed, which I cross-referenced against the `<script>` tags the engine had injected. One of them had an outdated import path pointing to a file I had renamed. Fixing the import string resolved it immediately.

![Chrome DevTools console showing a failed dynamic import error](images/element-inspect.png)

---

## Testing & Verification

### Gameplay Testing

Every level has a live game runner embed on this page. I used these during development to test each level in isolation without having to play through from the beginning every time. They're still live — clicking into any of them confirms the level is working right now:

{% capture challenge_testing_game %}
Play the full Escape Game for final verification! All levels, NPCs, transitions, and AI chat are live and testable right here.
{% endcapture %}

{% capture code_testing_game %}
import GameControl from "/assets/js/GameEnginev1.1/essentials/GameControl.js";
import GameLevelMaze        from "/assets/js/GameEnginev1.1/GameLevelMaze.js";
import GameLevelMazeSub     from "/assets/js/GameEnginev1.1/GameLevelMazeSub.js";
import GameLevelDoors       from "/assets/js/GameEnginev1.1/GameLevelDoors.js";
import GameLevelForest      from "/assets/js/GameEnginev1.1/GameLevelForest.js";
import GameLevelForestSub   from "/assets/js/GameEnginev1.1/GameLevelForestSub.js";
import GameLevelForestWin   from "/assets/js/GameEnginev1.1/GameLevelForestWin.js";
import GameLevelForestDeath from "/assets/js/GameEnginev1.1/GameLevelForestDeath.js";

export const gameLevelClasses = [GameLevelMaze, GameLevelMazeSub, GameLevelDoors, GameLevelForest, GameLevelForestSub, GameLevelForestWin, GameLevelForestDeath];
export { GameControl };
{% endcapture %}

{% include runners/game.html
   runner_id="game_testing_final"
   challenge=challenge_testing_game
   code=code_testing_game
   height="500px"
%}

### Integration Testing

The trickiest integration to verify was the nested `GameControl` chain: `GameLevelMaze` → `GameLevelMazeSub` (sublevel) → `GameLevelDoors` (via `parentControl`). I tested this by playing all the way through and checking that the parent game correctly paused when the sublevel launched, and that `topGame.transitionToLevel()` handed control back to the right level:

```javascript
// GameLevelMazeSub.js — parentControl chain tested end-to-end
const topGame = primaryGame?.parentControl || primaryGame;
if (topGame) {
  topGame.levelClasses      = [GameLevelDoors];
  topGame.currentLevelIndex = 0;
  topGame.isPaused          = false;
  topGame.transitionToLevel();
}
```

### Error Handling

Every transition wraps the optional `hideCanvasState()` call in a `try/catch` with a `console.warn` so a missing method doesn't crash the level. `BeckonerChaseController` uses optional chaining on every game object lookup to stay safe during the frames before objects finish loading:

```javascript
// GameLevelForest.js — try/catch around optional engine method
try {
  if (typeof primaryGame.hideCanvasState === 'function') {
    primaryGame.hideCanvasState();
  }
} catch(e) {
  console.warn('Could not hide parent canvas state', e);
}

// GameLevelForestDeath.js — optional chaining guards in BeckonerChaseController
const player   = this.gameEnv.gameObjects?.find(o => o?.spriteData?.id === 'Octopus');
const beckoner = this.gameEnv.gameObjects?.find(o => o?.spriteData?.id === 'Strange Beckoner');
if (!player || !beckoner) return;   // bail out safely if objects not ready yet
```

Run this to see both patterns tested against three different scenarios:

{% capture challenge_error %}
Run the code to see the try/catch and optional chaining error-handling patterns from the Escape Game tested against three different scenarios.
{% endcapture %}

{% capture code_error %}
// Error handling — try/catch + optional chaining from GameLevelForestDeath.js

function safeHideCanvas(primaryGame) {
  try {
    if (typeof primaryGame.hideCanvasState === 'function') {
      primaryGame.hideCanvasState();
      console.log('[try]  hideCanvasState() succeeded');
    } else {
      console.log('[try]  hideCanvasState not a function — skipped safely');
    }
  } catch(e) {
    console.warn('[catch] Could not hide canvas state:', e.message);
  }
}

console.log('=== Game with hideCanvasState ===');
safeHideCanvas({ hideCanvasState: () => {} });

console.log('\n=== Game without hideCanvasState ===');
safeHideCanvas({});

console.log('\n=== Game where hideCanvasState throws ===');
safeHideCanvas({ hideCanvasState: () => { throw new Error('Canvas locked'); } });

// Mirrors optional chaining in BeckonerChaseController.update()
console.log('\n=== Optional chaining guard ===');
const gameEnv = { gameObjects: null };
const player = gameEnv.gameObjects?.find(o => o?.spriteData?.id === 'Octopus');
console.log(`player found: ${player ?? 'undefined — bailed out safely'}`);
{% endcapture %}

{% include runners/code.html
   runner_id="code_error"
   language="javascript"
   challenge=challenge_error
   code=code_error
%}