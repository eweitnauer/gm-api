# Quick Start (gmath 0.12.6 or later)

The GM Canvas is the top-level object that handles embedding GM into a page, is the container for all GM elements such as algebra derivations, and handles logging user actions. Most of the canvas functionality, such as drawing and erasing, can be switched off if not needed.

## Embedding a Graspable Math Canvas

In order to include the GM canvas into a web page, include a script tag to load the `gm-inject.js` script. Then call the `loadGM` method to load all GM scripts and css files when you want to use GM on the page. Pass a callback to `loadGM` get notified when everything is ready. At that point, you can insert a GM canvas into the page using `gmath.insertCanvas()`.

Minimal example:

```html
<!doctype html>
<meta charset="utf-8">
<title>GM Canvas Example</title>
<script src="https://graspablemath.com/shared/libs/gmath/gm-inject.js"></script>

<body>
<div id='gm-div' style="margin: 20px; height: 400px"></div>
<script>
loadGM(initCanvas, { version: '0.12.6' });

function initCanvas() {
  console.log('using gmath version', gmath.version);
  canvas = new gmath.Canvas('#gm-div');
  canvas.model.createElement('derivation', { eq: '2x+1=3', pos: { x: 'center', y: 50 } });
}
</script>
```

## Loading and Saving a Canvas State

Use the methods `canvas.saveToJSON()` and `canvas.loadFromJSON()` to save and load canvas states.

## Creating Derivations

To create a derivation on an existing canvas, use the `canvas.model.createElement()` method. This method takes up to four arguments. See the code example above.

* `type`... must be "derivation"
* `options`... an js object specifying settings like the position and initial equation
* `method`... optional, a string that is recorded during logging
* `callback`... optional, a method that is called after the derivation is initialized and displayed, the derivation is passed as the first argument


## Listening to Events

### Creation and Deletion of Derivations

Unless disabled, the user can create new derivations and remove existing ones on the canvas. To listen to create events, use `canvas.model.on('create', callback)` and `canvas.model.on('remove', callback)`. The first argument to the callback function will be an event object with these fields:

* `type`... can be 'create' or 'remove'
* `target`... a reference to the canvas element (use `target.type` to check what kind of element it is)

### Changes in the State of a Derivation

In order to react to the mathematical state of derivations on a canvas, you can listen to `el_changed` events on a canvas with `canvas.model.on('el_changed', callback)`. The event object passed to the callback function looks like this:

* `type`... 'el_changed'
* `target`... a reference to the canvas element (use `target.type` to check what kind of element it is)
* `last_eq`... only for derivations: ascii representation of the equation in the last row of the derivation

To check the state of the whole derivation, you can iterate over its rows array (`der.rows`), look at the model in each row (`der.rows[0].model`), and get an ascii representation (`der.rows[0].model.to_ascii()`).

Here is a code example that listens to change events on all derivations on a canvas and shows a "Success" textbox when any of the derivations reaches the state "x=1". Add these lines the the end of the `initCanvas` function in the code above.

```js
  canvas.model.on('el_changed', function(evt) {
    console.log(evt.last_eq);
    if (evt.last_eq === 'x=1') canvas.showHint('Success :)');
  });
```
