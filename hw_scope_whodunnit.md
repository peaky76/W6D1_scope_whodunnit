# Scope Homework: Who Dunnit

### Learning Objectives

- Understand function scope
- Know the difference in between the let and const keywords

## Brief

Using your knowledge about scope and variable declarations in JavaScript, look at the following code snippets and predict what the output or error will be and why. Copy the following episodes into a JavaScript file and add comments under each one detailing the reason for your predicted output.

### MVP

#### Episode 1

```js
const scenario = {
  murderer: 'Miss Scarlet',
  room: 'Library',
  weapon: 'Rope'
};

const declareMurderer = function() {
  return `The murderer is ${scenario.murderer}.`;
}

const verdict = declareMurderer();
console.log(verdict);
```

The murderer is Miss Scarlett 

#### Episode 2

```js
const murderer = 'Professor Plum';

const changeMurderer = function() {
  murderer = 'Mrs. Peacock';
}

const declareMurderer = function() {
  return `The murderer is ${murderer}.`;
}

changeMurderer();
const verdict = declareMurderer();
console.log(verdict);
```

Will throw an error because can't reassign const murderer.

#### Episode 3

```js
let murderer = 'Professor Plum';

const declareMurderer = function() {
  let murderer = 'Mrs. Peacock';
  return `The murderer is ${murderer}.`;
}

const firstVerdict = declareMurderer();
console.log('First Verdict: ', firstVerdict);

const secondVerdict = `The murderer is ${murderer}.`;
console.log('Second Verdict: ', secondVerdict);
```

First verdict will be the murderer is Mrs Peacock. It calls a function which has its own murderer variable declared at block level.
Second verdict will be the murderer is Professor Plum. It calls the murder variable directly and the value for murderer held at the level of the whole file is Prof Plum. 


#### Episode 4

```js
let suspectOne = 'Miss Scarlet';
let suspectTwo = 'Professor Plum';
let suspectThree = 'Mrs. Peacock';

const declareAllSuspects = function() {
  let suspectThree = 'Colonel Mustard';
  return `The suspects are ${suspectOne}, ${suspectTwo}, ${suspectThree}.`;
}

const suspects = declareAllSuspects();
console.log(suspects);
console.log(`Suspect three is ${suspectThree}.`);
```

This will first list the suspects as Miss Scarlet, Prof Plum and Col Mustard as suspectThree is reset at block level in the declareAllSuspects function
The second output will list Mrs Peacock as suspectThree as the reassignment as Col Mustard only extends to the scope of the declareAllSuspects function

#### Episode 5

```js
const scenario = {
  murderer: 'Miss Scarlet',
  room: 'Kitchen',
  weapon: 'Candle Stick'
};

const changeWeapon = function(newWeapon) {
  scenario.weapon = newWeapon;
}

const declareWeapon = function() {
  return `The weapon is the ${scenario.weapon}.`;
}

changeWeapon('Revolver');
const verdict = declareWeapon();
console.log(verdict);
```

The weapon is the revolver. It is possible to change the properties of the object held in const scenario, so reassigning the weapon in the changeWeapon function is allowed.

#### Episode 6

```js
let murderer = 'Colonel Mustard';

const changeMurderer = function() {
  murderer = 'Mr. Green';

  const plotTwist = function() {
    murderer = 'Mrs. White';
  }

  plotTwist();
}

const declareMurderer = function () {
  return `The murderer is ${murderer}.`;
}

changeMurderer();
const verdict = declareMurderer();
console.log(verdict);
```

The murderer is Mrs White. The murderer variable is declared at block level and all the subsequent changes are within that scope.

#### Episode 7

```js
let murderer = 'Professor Plum';

const changeMurderer = function() {
  murderer = 'Mr. Green';

  const plotTwist = function() {
    let murderer = 'Colonel Mustard';

    const unexpectedOutcome = function() {
      murderer = 'Miss Scarlet';
    }

    unexpectedOutcome();
  }

  plotTwist();
}

const declareMurderer = function() {
  return `The murderer is ${murderer}.`;
}

changeMurderer();
const verdict = declareMurderer();
console.log(verdict);
```

The murderer is Mr Green. The murderer variable declared in plotTwist is a new variable which only has block scope so it won't affect the value of the murderer stored at the level of the main body of the changeMurderer function

#### Episode 8

```js
const scenario = {
  murderer: 'Mrs. Peacock',
  room: 'Conservatory',
  weapon: 'Lead Pipe'
};

const changeScenario = function() {
  scenario.murderer = 'Mrs. Peacock';
  scenario.room = 'Dining Room';

  const plotTwist = function(room) {
    if (scenario.room === room) {
      scenario.murderer = 'Colonel Mustard';
    }

    const unexpectedOutcome = function(murderer) {
      if (scenario.murderer === murderer) {
        scenario.weapon = 'Candle Stick';
      }
    }

    unexpectedOutcome('Colonel Mustard');
  }

  plotTwist('Dining Room');
}

const declareWeapon = function() {
  return `The weapon is ${scenario.weapon}.`
}

changeScenario();
const verdict = declareWeapon();
console.log(verdict);
```

The weapon is the Candle Stick. The properties of the object held in const scenario can be changed. The room is changed first in the changeScenario function, then because the tests in the if blocks of plotTwist and unexpectedOutcome pass, the murderer and weapon is changed. 

#### Episode 9

```js
let murderer = 'Professor Plum';

if (murderer === 'Professor Plum') {
  let murderer = 'Mrs. Peacock';
}

const declareMurderer = function() {
  return `The murderer is ${murderer}.`;
}

const verdict = declareMurderer();
console.log(verdict);
```

The murderer is Professor Plum. Although the if test passes it creates a new block level variable called murderer, this does not affect the variable held at the top level, which is still Professor Plum.

### Extensions

#### Episode 10

```js
let murderer = 'Miss Scarlett';

const scenario = {
  murderer: murderer,
  weapon: 'Rope',
  room: 'Conservatory'
};

let room = 'Ballroom';

const plotTwist = () => {
  scenario.room = room
}

const unexpectedOutcome = () => {
  let room = 'Kitchen'
} 

const unpredictedHappening = () => {
  room = 'Study'
}

const declareRoom = () => {
  return `The room is ${scenario.room}.`
}

unpredictedHappening()
unexpectedOutcome()
plotTwist()

const verdict = declareRoom()
console.log(verdict)
```

The room is Study. The scenario.room is originally set to Conservatory. A variable room is originally set to Ballroom. Both the unexpectedOutcome and unpredictedHappening functions change a variable called room. But in the unexpectedOutcome function this is a new block level variable called room. Only unpredictedHappening changes the original room variable held at the top level. The plotTwist function assigns to scenario.room whatever is held in that top level variable room, which once the functions have been run is Study (assigned in the unpredictedHappening function) 