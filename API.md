_(gmath version 2.24.11)_

# Graspable Math API Documentation

The Graspable Math library (GM) is currently used in the following places:

- https://graspablemath.com
- https://activities.graspablemath.com
- https://www.geogebra.org/notes

This document describes how to integrate GM into your own webpage or web application.

## Integrating GM into a webpage or web application

**loadGM**(_options_)

Including the [gm-inject.js](https://graspablemath.com/shared/libs/gmath/gm-inject.js) script in an html page provides the `loadGM()` method. Calling it injects the gmath library, the d3 library, jquery and bootstrap, as well as several stylesheet files into the page. The _options_ parameter needs to specify which gmath version to load, such as `{version: '2.24.11}` or `{version: 'latest'}`.

Here is a minimal example:

```html
<!DOCTYPE html>
<meta charset="utf-8" />
<title>GM Canvas Example</title>
<script src="https://graspablemath.com/shared/libs/gmath/gm-inject.js"></script>

<body>
  <div id="gm-div" style="margin: 20px; height: 400px"></div>
  <script>
    loadGM(initCanvas, { version: 'latest' });

    function initCanvas() {
      const canvasOptions = {};
      canvas = new gmath.Canvas('#gm-div', canvasOptions);
      canvas.model.createElement('derivation', {
        eq: '2x+1=3',
        pos: { x: 'center', y: 50 },
      });
    }
  </script>
</body>
```

A more complex example is [here](https://github.com/eweitnauer/gm-api/blob/master/examples/dark.html).

## Overview

Graspable Math organizes elements inside of `GM Canvas` objects, which are configurable digital whitboards. Math work is organized in derivations, which contain a list of derivation rows, which each contains an AlgebraView, an AlgebraModel, and - if that row was the result of a math action, an Action.

![image](https://user-images.githubusercontent.com/53043/112194551-60076f80-8bdf-11eb-9148-a9ca8899b511.png)

Graspable Math puts great emphasis on smooth animations of the math transformations. Here are some of the visual elements involved.

![image](https://user-images.githubusercontent.com/53043/112194634-76adc680-8bdf-11eb-8720-289ba9255f9f.png)

## Canvas

The canvas ties together a toolbar and a working area where the user can draw and erase, as well as create canvas elements and interact with them. Each canvas element has a type, such as 'derivation' or 'textbox', and a unique id. The working area has a fixed width, but can extend vertically as needed to make space for new elements or paths.

To create a canvas, use `new gmath.Canvas(containerHtmlElementOrDomSelector, options)`. See the large table on [this page](https://github.com/eweitnauer/gm-api/blob/master/customizing-gm-embedded-as-an-iframe.md) for documentation on the many things you can customize with that `options` parameter. The Canvas will be created in the element specified by `containerHtmlElementOrDomSelector`. If you create more than one Canvas, you'll need to put each into it's own container element.

### Canvas Contructor Options

<!-- prettier-ignore-start -->
| Option | Default value | Type | Description |
| --- | --- | --- | --- |
| `"accept_dropped_assets"` | `true` | boolean | Whether to allow users to create things on the canvas by dragging them onto the canvas. Things such as pictures, text, things from graphs, and more |
| `"arrange_btn"` | `true` | boolean | Whether to show the "arrange" button on the toolbar |
| `"ask_confirmation_on_closing"` | `true` | boolean | Whether to ask for confirmation when leaving the page with unsaved changes |
| `"btn_size"` | `"sm"` | `"sm"` or `"xs"` | Size of toolbar buttons. See also `display_icons` and `display_labels`. |
| `"content_min_width"` | `"auto"` | Any value that the CSS attribute `min-width` would take | Sets the minimum width of the canvas |
| `"demo_video_idle_time"` | `false` | `false` or a number | If set to a number X, will show video dialog after X ms of inactivity |
| `"demo_video_sources"` | `[ ]` | array of strings | A list of YouTube video IDs e.g. `[ "CiN5AzMPX80", "lraDNDuFNj8" ]`. `demo_video_idle_time` must be set to something other than false for this to have any effect |
| `"disable_notifications"` | `false` | boolean | Whether to show notifications, such as the banner that says to try double-clicking |
| `"display_icons"` | `true` | boolean | Whether to show icons on the toolbar buttons |
| `"display_labels"` | `true` | boolean | Whether to show labels on the toolbar buttons |
| `"draw_btn"` | `true` | boolean | Whether to show the "draw" button on the toolbar |
| `"enable_google_classroom"` | `false` | boolean | enable / disable the share to google classroom option |
| `"use_built_in_saving_backend"` | `true` | boolean | See mentions of this [here](https://github.com/eweitnauer/gm-api/blob/master/quickstart.md#loading-and-saving-a-canvas-state) for details. This must be `true` if `"save_btn"` is `true`. |
| `"erase_btn"` | `true` | boolean | Whether to show the "erase" button on the toolbar |
| `"feedback"` | `false` | boolean | Whether to show the "feedback" button on the toolbar |
| `"font_size_btns"` | `true` | boolean | Whether to show the font size buttons on the toolbar ("larger" and "smaller") |
| `"formula_btn"` | `true` | boolean | Whether to show the "formulas" button on the toolbar |
| `"fullscreen_btn"` | `false` | boolean | Whether to put the "fullscreen" button on the toolbar |
| `"help_btn"` | `true` | boolean | Whether to show the "help" button on the toolbar |
| `"homework_btn"` | `false` | boolean | Whether to show the "problems" button on the toolbar |
| `"identification_interval"` | `false` | `false` or a number | If set to a number X, will show login dialog after X ms of inactivity |
| `"insert_btn"` | `true` | boolean | Whether to show the "insert" button on the toolbar |
| `"insert_menu_items"` | `{ "derivation": true, "homework": true, "textbox": true, "ggb_graphing": true, "ggb_geometry": true, "ggb_3d": true, "video": true }` | a JSON object like the default value | Allows you to make it so some items are not on the "insert" menu |
| `"keypad_btn"` | `true` | boolean | Whether to show the "keypad" button on the toolbar |
| `"keyboard_max_width"` | `800` | number | Number of pixels of the maximum width of the keyboard |
| `"load_btn"` | `true` | boolean | Whether to show the "load" button on the tooolbar |
| `"new_sheet_btn"` | `true` | boolean | Whether to show the "new" button on the toolbar |
| `"overflow_visible"` | `false` | boolean | Whether to render parts of expressions that you drag beyond the borders of the canvas |
| `"redo_btn"` | `true` | boolean | Whether to show the "redo" button on the toolbar |
| `"save_btn"` | `true` | boolean | Whether to show the "save" button on the tooolbar |
| `"scrub_btn"` | `true` | boolean | Whether to show the "scrub" button on the toolbar |
| `"settings_btn"` | `true` | boolean | Whether to show the "settings" button on the tooolbar |
| `"share_btn"` | `true` | boolean | Whether to show the "share" button on the toolbar |
| `"submit_btn"` | `false` | boolean | Whether to show the "submit" button on the toolbar |
| `"toolbar_on_style"` | `{ }` | JSON object containing CSS styles | CSS styles to be applied to the toolbar |
| `"toolbar_pos"` | `"top"` | `"top"` or `"bottom"` | Warning, you will probably have problems if you change this from its default. This controls the toolbar's position. |
| `"transform_btn"` | `true` | boolean | Whether to show the "transform" button on the toolbar |
| `"undo_btn"` | `true` | boolean | Whether to show the "undo" button on the toolbar |
| `"use_toolbar"` | `true` | boolean | Whether to show the toolbar |
| `"horizontal_scroll"` | `true` | boolean | Whether to automatically grow the canvas horizontally, as needed |
| `"vertical_scroll"` | `true` | boolean | Whether to automatically grow the canvas vertically, as needed. This cannot be turned off if you are embedding GM in an iframe, using `embed.html`. |
| `auto_resize_on_scroll` | `true` | boolean | If true, will extend the vertical size of the canvas when the user scrolls to the bottom |
| `add_more_space_btn` | `false` | boolean | Show a button a the bottom of the canvas that adds more vertical space. Only enable if `auto_resize_on_scroll` is set to `false`. |
| `"use_hold_menu"` | `true` | boolean | If this is true and you click & hold somewhere on some blank part of the canvas, then a circular menu will appear |
<!-- prettier-ignore-end -->

## CanvasController

Manages user interactions with the canvas. When creating a new canvas with `let canvas = new gmath.Canvas(...)`, you can access the controller at `canvas.controller`.

<a name="undo" href="#cc-undo">#</a> controller.**undo**()

Undoes the last action taken on the canvas, like performing an action in a derivation or hiding and showing lines in a derivation.

<a name="redo" href="#cc-redo">#</a> controller.**redo**()

Redoes an action that was just undone.

<a name="reset" href="#cc-redo">#</a> controller.**reset**()

Removes all elements from the canvas.

## CanvasModel

Holds all elements on the canvas.

<a name="createElement" href="#createElement">#</a> cmodel.**createElement**(_type_, _options_, _[method]_, _[callback]_)

Creates a new canvas element. GM comes with the build-in element types 'derivation', 'textbox', 'image', 'ggb-panel' and 'ggb-element'. The _options_ object is passed to the constructor of the element that is created. You should always pass `pos: {x, y}` as an option. The string passed as _method_ parameter is written to the logging database, and the function passed as _callback_ will be called with the created element as parameter once it is initialized and displayed.

<a name="removeElement" href="#removeElement">#</a> cmodel.**removeElement**(_element_)

Removes the passed element from the canvas.

<a name="elements" href="#elements">#</a> cmodel.**elements**()

Returns an array of all elements on the canvas.

<a name="scroll" href="#scroll">#</a> cmodel.**scroll**(_[y]_)

If called without an arguments, returns the current scrollTop position of the canvas workspace. When passed a number, it will scroll to that position.

<a name="size" href="#size">#</a> cmodel.**size**()

Returns the current size of the canvas workspace (usually bigger than the screen when the canvas is in infinite scrolling mode).

<a name="viewport" href="#viewport">#</a> cmodel.**viewport**()

Returns the currently visible rectangle of the canvas workspace.

<a name="reset" href="#reset">#</a> cmodel.**reset**()

Removes all elements and drawings from the canvas and scrolls to the top position.

<a name="showNotice" href="#showNotice">#</a> cmodel.**showNotice**(_text_)

Shows a text notice at the top of the canvas. The message is automatically dismissed when the user interacts with the canvas.

<a name="showNotice" href="#showNotice">#</a> cmodel.**hideNotice**()

Hide any currently visible text notice.

### Events

<a name="cmodel-on-create" href="#cmodel-on-create">#</a> cmodel.**on**(**'create'**, _callback_)

Event object passed to the callback:

```js
event = {
  type: 'create',
  target_type, // element type, such as 'derivation'
  target, // the element
};
```

<a name="cmodel-on-remove" href="#cmodel-on-remove">#</a> cmodel.**on**(**'remove'**, _callback_)

Event object passed to the callback:

```js
event = {
  type: 'remove',
  target_type, // element type, such as 'derivation'
  target, // the element
};
```

<a name="cmodel-on-el-changed" href="#cmodel-on-el-changed">#</a> cmodel.**on**(**'el_changed'**, _callback_)

Like the change event, but is only emitted at the end of the user interaction, when the user let go of the mouse button. If the user performs several algebra transformations with one mouse drag, several `change` events will be emitted, but only a single `changed` event at the very end. It is save to remove the derivation at the `changed` event.

```js
event = {
  type: 'el_changed',
  target_type, // element type, such as 'derivation'
  target, // the element
  last_eq, // ascii string of the last derivation row (only for derivations)
};
```

## CanvasElements

Each element on the canvas like textboxes, images, geogebra elements, and derivations all share the same base class `CanvasElement`, which handles interactions like dragging, resizing, removing.

The following options can be used with any `CanvasElement`.

### constructor options

<!-- prettier-ignore-start -->
| Option | Description | Default Value |
| --- | --- | --- |
| `pos` | initial position | `{x: 'center', y: 'center'}` |
| `size` | initial size, some element types manage the size automatically | `{width: 100, height: 60}` |
| `min_size` | minimum size during resizing | `{ width: 30, height: 50 }` |
| `resizable` | can the user resize the element? | `{ x: true, y: true }` |
| `active` | is the element currently selected? | `false` |
| `mode` | current mode, allowed modes are `'edit'` and `'arrange'` | `'edit'` |
| `draggable` | allow users to drag the element | `true` |
| `keep_in_container` | don't allow users to drag the element beyond the size of the container | `true` |
| `edit_mode_drag_box_width` | width of the draggable area left of the element | `20` |
| `show_bg` | only displays background color if true | `true` |
| `bg_edit_active_style` | css styles for background during normal edit mode | `{ 'background-color': '#333', 'box-shadow': 'none' }` |
| `bg_edit_dragging_style` | css styles for background while dragging the element | `{ 'background-color': '#444', 'box-shadow': '0 3px 6px rgba(0,0,0,0.5)' }` |
<!-- prettier-ignore-end -->

If `pos` is set to the string `'auto'`, GM will attempt to place it at an empty, visible spot on the canvas.

## Derivation

This canvas element represents an algebraic deriavation that can consist of several lines (rows). Each row has an `AlgebraModel`, an `AlgebraView` and for most rows an `Action` that created the row. Derivations should be created through the [createElement](#createElement) method of a CanvasModel.

### constructor options

<!-- prettier-ignore-start -->
| Option | Description | Default Value |
| --- | --- | --- |
| `eq` | initial equation | - |
| `font_size` | initial fontsize | `50` |
| `collapsed_mode` | create new lines on top of previous ones | `true` |
| `auto_collapse_repeated_actions` | collapse subsequent lines triggered by same action | `true` |
| `hide_handles` | hides line handles | `false` |
| `handle_pos` | `left` or `right` | `right` |
| `wiggle` | initially wiggle some terms, e.g. `"1+2"` or `["1", "2", "3"]` | `[]` |
| `cloning_on` | allow cloning of lines of this derivation | `true` |
| `action_blacklist` | disable actions with provided names | `[]` |
| `row_padding` | padding around each derivation row | `{left: 10, right: 45, top: 7, bottom: 3 }` |
| `padding` | padding around each line's `AlgebraView` | `{left: 20, right: 5, top: 5, bottom: 5 }` |
| `handle_stroke_color` | color of the line handles | `#ddd` |
| `mode` | current mode, allowed modes are `'edit'`, `'arrange'`, `'inspect'` | `'edit'` |
| `color` | color of the main math font | `'#333'` |
| `shadow_node_color` | color used to fill the shadow terms that show where a dragged expression originated | `'white'` |
| `shadow_node_shadow` | css box-shadow or text-shadow of the shadow terms during dragging | `'0 0 1px gray, 0 0 1px black'` |
| `selection_color` | color of selected nodes | `'#4682b4'` |
| `drag_indicator_color` | color of the circle that indicates finger / mouse position during dragging | `'orange'` |
| `drag_indicator_opacity` | opacity of the circle that indicates finger / mouse position during dragging | `0.4` |
| `handle_styles` | css properties used for the handles | `handle_styles: { 'background': 'white', 'border-color': '#ddd', 'border-width': '2px' }` |
| `row_background_color` | background color of derivation rows during dragging | `'white'` |
| `row_transition_dur` | duration in ms for row unpacking animation | `250` |
| `show_area_hints` | show light blue rectangles at places a term can be dragged | `true` |
| `show_dest_hints` | show dark blue rectangles where terms will move when holding a term over a target area | `true` |
<!-- prettier-ignore-end -->

<a name="setExpression" href="#setExpression">#</a> derivation.**setExpression**(_expr_str_)

Set the last line of the derivation to the passed expression string.

<a name="startWiggle" href="#startWiggle">#</a> derivation.**startWiggle**(_sels_)

Pass a single term selector string or an array of term selector strings to wiggle nodes in the last line of the derivation. For example, in a derivation with `x+2x+3x` in the last line, call `derivation.startWiggle('2*x')` to wiggle the `2x` or call `derivation.startWiggle('+:2')` to wiggle the second `+`.

<a name="setFontSize" href="#setFontSize">#</a> derivation.**setFontSize**(_font_size_)

Updates the font size of the derivation.

<a name="getLastRow" href="#getLastRow">#</a> derivation.**getLastModel**()

Returns the `AlgebraModel` of the last line in the derivation.

### Events

<a name="der-on-change" href="#der-on-change">#</a> derivation.**events.on**(**'change'**, _callback_)

Event object passed to the callback:

```js
event = {
  type: 'change',
  performee, // id of the derivation
  row, // index of the new / changed row
  model, // AlgebraModel, the new / changed model
};
```

<a name="der-on-mistake" href="#der-on-mistake">#</a> derivation.**events.on**(**'mistake'**, _callback_)

This forwards mistake events triggered on any `AlgebraModel` in the derivation. A reference to the `AlgebraModel` is passed to the callback function. The mistake event will be fired if the user clicks on a term that does not trigger an action. This happens, for example, when clicking on the `+` in `2+3*4`, but also when clicking on `10` in `10`, or on `x` in `x=2` or on `1` in `1+2`.

<a name="der-on-undo" href="#der-on-undo">#</a> derivation.**events.on**(**'undo'**, _callback_)

This event is called when an undo is triggered for the derivation.

<a name="der-on-redo" href="#der-on-redo">#</a> derivation.**events.on**(**'redo'**, _callback_)

This event is called when an redo is triggered for the derivation.

<a name="der-on-removed" href="#der-on-removed">#</a> derivation.**events.on**(**'removed'**, _callback_)

This event is called when the derivation was removed from the canvas.

## AlgebraModel

Represents the state of a single algebraic expression. The expression is stored as a n-ary tree and each node has pointers to a parent (`parent`), a left sibling (`ls`), a right sibling (`rs`), and an array of children (`children`). The AlgebraModel also has the following methods:

- `to_latex()` returns the math expression in LaTeX format
- `to_ascii()` returns the math expression in text format
- `algExpressionsAreEqual(settings, expr1, expr2, ...)` is a static method that compares algebra models or math strings; the settings allow to ignore GM- specific sign variations such as `{-2}x+3` (adding the product of negative 2 times x and 3) versus `-{2x}+3` (subtracting the product of 2 and x then adding 3). It also allows ignoring order of commutative terms to make `a*b` and `b*a` match each other.

## AlgebraView

Visualizes an AlgebraModel and allows user interaction.

## Actions

Graspable Math comes with a large library of actions that define mathematical transformations and the gestures that trigger them.

# Logging User Interactions

Graspable Math by default logs user interactions with the canvas. Use `gmath.setupLogging(options)` to switch logging on or off and provide an ID by which the log data is grouped for later retrieval. Options: `{ experiment_id: string, enabled: Boolean }`.
