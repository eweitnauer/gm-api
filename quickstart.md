# Quick Start

*(gmath version 2.24.11 or later)*

The GM Canvas is the top-level object that gives you access to the GM API. It is the container for all GM elements such as algebra derivations and handles logging user actions. The appearance and behavior of the canvas and interactive math expressions can be customized through settings. The API also allows you to modify math elements and to listen & react to user interactions.

## Embedding in an iframe

A quick, but less customizable way to integrate GM into a webpage is by loading it through a iframe. To do so, go to [math.new](https://math.new), create any content and save it, then click the "share" button to generate a code snippet that loads GM inside an iframe, as in this example html page:

```html
<!doctype html>
<meta charset="utf-8">
<title>Test</title>
<body>
<!-- Include this in your page (Start) -->
<script type="text/javascript">url = parent.document.URL; document.write('<iframe style="border: none" src=\'https://graspablemath.com/canvas/embed.html?load=_e0df6d1a59896f82&options={"use_toolbar": false, "vertical_scroll": false }&parent_url='+url+'\' width=100% height=400px></iframe>')
</script>
<!-- Include this in your page (End) -->
</body>
```

See [Customizing GM embedded as an iframe](https://github.com/eweitnauer/gm-api/blob/master/customizing-gm-embedded-as-an-iframe.md) for help customizing this.

## Embedding a Graspable Math Canvas

The second way of adding a GM canvas to your web application embeds is directly into without using an iframe. This gives you full access to the API for customization and interactivity between GM and the rest of your page.

In order to include the GM canvas directly into a web page, include a script tag to load the `gm-inject.js` script. The script provides the `loadGM` method. Call this method to asynchronously load all scripts and css files and fonts that GM needs to work. Pass a callback to `loadGM` to get notified when everything is ready. At that point, a global `gmath` object gives you access to the API. To insert a GM canvas into the page, use `new gmath.Canvas(cssSelector, canvasOptions)`.

Minimal example:

```html
<!doctype html>
<meta charset="utf-8">
<title>GM Canvas Example</title>
<script src="https://graspablemath.com/shared/libs/gmath/gm-inject.js"></script>

<body>
<div id='gm-div' style="margin: 20px; height: 400px"></div>
<script>
loadGM(initCanvas, { version: 'latest' });

function initCanvas() {
  const canvasOptions = {
    // You may use options described on
    // https://github.com/eweitnauer/gm-api/blob/master/customizing-gm-embedded-as-an-iframe.md#options-that-go-in-the-options-option-described-above
  }
  canvas = new gmath.Canvas('#gm-div', canvasOptions);
  canvas.model.createElement('derivation', { eq: '2x+1=3', pos: { x: 'center', y: 50 } });
}
</script>
</body>
```

<!-- If you change the name of this section, there is at least one place in the graspable-math repository that has a link to this section (displayed on GitHub), so you will need to update that(those) hyperlink(s).  -->
## Loading and Saving a Canvas State

### If you want users to use our built-in Save button

That should work by default. The canvases will be saved on our servers.

### If you plan on storing saved canvases yourself

Then you must set the `"use_built_in_saving_backend"` and `"save_btn"` canvas options to `false`. (For more on canvas options, see [this section](https://github.com/eweitnauer/gm-api/blob/master/API.md) If you don't set `"use_built_in_saving_backend"` to `false`, any images inserted onto the canvas may be uploaded to our servers and may be deleted by us from time to time.

You may use `canvas.asyncStringify()` to save a canvas state. Use `canvas.loadFromJSON(json, clearCanvas=true, callback=null)` to load a canvas state.

## Creating Derivations

To create a derivation on an existing canvas, use the `canvas.model.createElement()` method. This method takes up to four arguments. See the code example above.

* `type`... use "derivation"
* `options`... an js object specifying settings like the position and initial equation (e.g.: `{pos: 'auto', eq: '2(a+b)'}`)
* `method`... optional, a string that is recorded during logging
* `callback`... optional, a method that is called after the derivation is initialized and displayed, the derivation is passed as the first argument

## Listening to Events

### Creation and Deletion of Derivations

Unless disabled, the user can create new derivations and remove existing ones on the canvas. To listen to create events, use `canvas.model.on('create', callback)` and `canvas.model.on('remove', callback)`. The first argument to the callback function will be an event object with these fields:

* `type`... can be 'create' or 'remove'
* `target`... a reference to the canvas element (use `target.type` to check what kind of element it is)

### Changes in the State of a Derivation

In order to react to changes a user makes to elements on a canvas, such as derivations, you can listen to `el_changed` events with `canvas.model.on('el_changed', callback)`. The event object passed to the callback function looks like this:

* `type`... 'el_changed'
* `target`... a reference to the canvas element (use `target.type` to check what kind of element it is)
* `last_eq`... only for derivations: ascii representation of the equation in the last row of the derivation

To check the state of the whole derivation, you can iterate over its rows array (`der.rows`), look at the model in each row (`der.rows[0].model`), and get an ascii representation (`der.rows[0].model.to_ascii()`).

Here is a code example that listens to change events on all derivations on a canvas and shows a "Success" notice when any of the derivations reaches the state "x=1". Add these lines the the end of the `initCanvas` function in the code above.

```js
  canvas.model.on('el_changed', function(evt) {
    console.log(evt.last_eq);
    if (evt.last_eq === 'x=1') canvas.showHint('Success :)');
  });
```
