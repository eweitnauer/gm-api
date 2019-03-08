# Quick Start

*(gmath version 2.6.6 or later)*

The GM Canvas is the top-level object that handles embedding GM into a page, is the container for all GM elements such as algebra derivations, and handles logging user actions. Most of the canvas functionality, such as drawing and erasing, can be switched off if not needed.

## Embedding in an iframe

When editing a canvas on graspablemath.com/canvas, you can use the share button to generate a code snippet that loads GM inside an iframe, as in this example html page:

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

You can customize the options in the URL of the code snippet after the `embed.html?` separate these options with `&` signs
* `load=` the ID of the document you want automatically loaded
* `eq=` a list of mathematical expressions separated by `;` that should be added to the canvas upon load e.g. `5+4;6^2=x`
* `parent_url=` there should be no need for you to change this
* `options=` a object written with JSON syntax which may have the options described **in the following** table between its `{` and `}`:

| Option | Default value | Type | Description |
| --- | --- | --- | --- |
| `content_min_width` | `"auto"` | Any value that the CSS attribute `min-width` would take | Sets the minimum width of the canvas |
| `toolbar_pos` | `"top"` | `"top"` or `"bottom"` | Warning, you will probably have problems if you change this from its default. This controls the toolbar's position. |
| `undo_btn` | `{ "type": "push", "label": "undo", "icon": "material-icons", "icon_name": "undo" }` | `false` or an object like the default value | If this is set to false, the "undo" button will not show on the toolbar
| `redo_btn` | `{ "type": "push", "label": "redo", "icon": "material-icons", "icon_name": "redo" }` |  `false` or an object like the default value | If this is set to false, the "redo" button will not show on the toolbar |
| `new_sheet_btn` | `{ "type": "push", "pull_right": true, "label": "new", "icon": "glyphicon glyphicon-file"}` | `false` or an object like the default value | If this is set to false, the "new" button will not show on the toolbar |
| `font_smaller_btn` | `{ "type": "push", "label": "larger", "icon": "material-icons", "icon_name": "format_size" }` | `false` or an object like the default value  | If this or the `font_larger_btn` setting is set to false, then both the buttons to change text size will not be on the toolbar |
| `font_larger_btn` | `{ "type": "push", "label": "smaller", "icon": "material-icons", "icon_name": "format_size", "flip": "y" }` | `false` or an object like the default value  | If this or the `font_smaller_btn` setting is set to false, then both the buttons to change text size will not be on the toolbar |
| `formula_btn` | `true` | boolean | If this is set to false, the "formulas" button will not show on the toolbar |
| `help_btn` | `{ "type": "push", "label": "help", "pull_right": true, "icon": "glyphicon glyphicon-question-sign"}` | `false` or an object like the default value | If this is set to false, the "help" button will not show on the toolbar |
| `homework_btn` | `false` | boolean | Whether to put the "problems" button on the toolbar |
| `feedback` | `false` | boolean | Whether to show the "feedback" button on the toolbar |
| `drawing` | `true` | boolean | Whether to show the "draw" button on the toolbar |
| `display_icons` | `true` | boolean | Whether to show icons on the toolbar buttons |
| `display_labels` | `true` | boolean | Whether to show labels on the toolbar buttons |
| `btn_size` | `"sm"` | `"sm"` or `"xs"` | Size of toolbar buttons. See also `display_icons` and `display_labels`. |
| `mode_btns` | `[ { "type": "radio", "group": "mode", "state": true, "label": "transform", "icon": "glyphicon glyphicon-random" }, "{ "type": "radio", "group": "mode", "state": false, "label": "keypad", "icon": "material-icons", "icon_name": "keyboard" }, "{ "type": "radio", "group": "mode", "state": false, "label": "scrub", "icon": "glyphicon glyphicon-wrench" }, "{ "type": "radio", "group": "mode", "state": false, "label": "arrange", "icon": "glyphicon glyphicon-move" } ]` | a JSON list, similar to the default, maybe with some items removed | This allows you to take out buttons from the mode button group |
| `use_toolbar` | `true` | boolean | Whether to show the toolbar |
| `toolbar_on_style` | `{ }` | JSON object containing CSS styles | CSS styles to be applied to the toolbar |
| `insert_btn` | `true` | boolean | Whether to show the "insert" button on the toolbar |
| `submit_btn` | `false` | boolean | Whether to show the "submit" button on the toolbar |
| `insert_menu_items` | `{ "derivation": true, "homework": true, "textbox": true, "ggb_graphing": true, "ggb_geometry": true, "ggb_3d": true, "video": true }` | a JSON object like the default value | Allows you to make it so some items are not on the "insert" menu |
| `share_btn` | `{ "type": "push", "label": "share", "pull_right": true, "icon": "glyphicon glyphicon-send" }` | `false` or an object like the default value | If this is set to false, the "share" button will not show on the toolbar |
| `saving_and_loading` | `true` | boolean | Whether to show the "save" and "load" buttons on the tooolbar |
| `settings_btn` | `true` | boolean | Whether to show the "settings" button on the tooolbar |
| `accept_dropped_assets` | `true` | boolean | Whether to allow users to create things on the canvas by dragging them onto the canvas. Things such as pictures, text, things from graphs, and more |
| `keyboard_max_width` | `800` | number | Number of pixels of the maximum width of the keyboard |
| `ask_confirmation_on_closing` | `true` | boolean | Whether to show a modal dialog box when a user tries to navigate away from the page, but the user has unsaved changes |
| `identification_interval` | `false` | `false` or a number | If set to a number X, will show login dialog after X ms of inactivity |
| `demo_video_sources` | `[ ]` | array of strings | A list of YouTube video IDs e.g. `[ "CiN5AzMPX80", "lraDNDuFNj8" ]`. `demo_video_idle_time` must be set to something other than false for this to have any effect |
| `demo_video_idle_time` | `false` | `false` or a number | If set to a number X, will show video dialog after X ms of inactivity |
| `disable_notifications` | `false` | boolean | Whether to show notifications, such as the banner that says to try double-clicking |
| `enable_google_classroom` | `false` | boolean | Whether to allow features involving Google Classroom, such as the option in the "share" menu |
| `enable_external_api` | `false` | boolean | Set to true to enable communication across iframe via postMessage |

## Embedding a Graspable Math Canvas

In order to include the GM canvas directly into a web page, include a script tag to load the `gm-inject.js` script. Then call the `loadGM` method to load all GM scripts and css files when you want to use GM on the page. Pass a callback to `loadGM` get notified when everything is ready. At that point, you can insert a GM canvas into the page using `gmath.insertCanvas()`.

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
  canvas = new gmath.Canvas('#gm-div');
  canvas.model.createElement('derivation', { eq: '2x+1=3', pos: { x: 'center', y: 50 } });
}
</script>
</body>
```

## Loading and Saving a Canvas State

Use the methods `canvas.toJSON()` and `canvas.loadFromJSON()` to save and load canvas states.

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
