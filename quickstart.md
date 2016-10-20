# Quick Start

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
loadGM(initCanvas, { version: '0.12.1' });

function initCanvas() {
  console.log('using gmath version', gmath.version);
  canvas = gmath.insertCanvas('#gm-div');
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

In order to react to the mathematical state of derivations on a canvas, you can listen to change events on a derivation. If `der` is a derivation object, use `der.events.on('change', callback)`. The callback method will be called with an event object with these fields:

this.type = 'Change';
  this.performee = id;
  this.performee_type = 'DL';
  this.time = Date.now();
  this.row = row_idx;
  this.model = model;

* `type`... is 'change'
* `row`... index of the derivation row that changed or was created
* `model`... an AlgebraModel object of the changed or new row

Call `model.to_ascii()` to get a string representation of the model's state. To check the state of a whole derivation, you can iterate over its rows array (`der.rows`) and look at the model in each row (`der.rows[0].model`).

Here is a code example that listens to change events on all derivations on a canvas and creates a textbox with the text "Success!" when any of the derivations reaches the state "x=1". Call this method in the above example right after inserting the canvas.

```js
function setupListeners(canvas) {
  canvas.model.on('create', function(evt) {
    if (evt.target.type !== 'derivation') return;
    // register a change listener on each new derivation
    evt.target.events.on('change', function(evt) {
    	console.log('new state: ', evt.model.to_ascii());
      if (evt.model.to_ascii() === 'x=1') {
        canvas.model.createElement('textbox', { text: 'Success!', pos: { x: 20, y: 20 }});
      }
    });
  });
}
```
