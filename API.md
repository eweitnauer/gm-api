*(gmath version 0.12.6)*

## gm-inject

**loadGM**(*options*)

Including the [gm-inject.js](https://graspablemath.com/shared/libs/gmath/gm-inject.js) script in an html page provides the `loadGM()` method. Calling it injects the gmath library, the d3 library, jquery and bootstrap, as well as several stylesheet files into the page. The *options* parameter needs to specify which gmath version to load, such as `{version: '0.12.6'}`.

# Canvas

The canvas ties together a toolbar and a working area where the user can draw and erase, as well as create canvas elements and interact with them. Each canvas element has a type, such as 'derivation' or 'textbox', and a  unique id. The working area has a fixed width, but can extend vertically as needed to make space for new elements or paths.

## CanvasModel

Holds all elements on the canvas.

<a name="createElement" href="#createElement">#</a> cmodel.**createElement**(*type*, *options*, *[method]*, *[callback]*)

Creates a new canvas element. GM comes with the build-in element types 'derivation', 'textbox', 'image', 'ggb-panel' and 'ggb-element'. The *options* object is passed to the constructor of the element that is created. You should always pass `pos: {x, y}` as an option. The string passed as *method* parameter is written to the logging database, and the function passed as *callback* will be called with the created element as parameter once it is initialized and displayed.

<a name="removeElement" href="#removeElement">#</a> cmodel.**removeElement**(*element*)

Removes the passed element from the canvas.

<a name="elements" href="#elements">#</a> cmodel.**elements**()

Returns an array of all elements on the canvas.

<a name="scroll" href="#scroll">#</a> cmodel.**scroll**(*[y]*)

If called without an arguments, returns the current scrollTop position of the canvas workspace. When passed a number, it will scroll to that position.

<a name="size" href="#size">#</a> cmodel.**size**()

Returns the current size of the canvas workspace (usually bigger than the screen when the canvas is in infinite scrolling mode).

<a name="viewport" href="#viewport">#</a> cmodel.**viewport**()

Returns the currently visible rectangle of the canvas workspace.

<a name="reset" href="#reset">#</a> cmodel.**reset**()

Removes all elements and drawings from the canvas and scrolls to the top position.

<a name="showNotice" href="#showNotice">#</a> cmodel.**showNotice**(*text*)

Shows a text notice at the top of the canvas. The message is automatically dismissed when the user interacts with the canvas.

<a name="showNotice" href="#showNotice">#</a> cmodel.**hideNotice**()

Hide any currently visible text notice.

### Events

<a name="cmodel-on-create" href="#cmodel-on-create">#</a> cmodel.**events.on**(**'create'**, *callback*)

Event object passed to the callback:

```
{ type: 'create'
, target_type // element type, such as 'derivation'
, target // the element
};
```

<a name="cmodel-on-remove" href="#cmodel-on-remove">#</a> cmodel.**events.on**(**'remove'**, *callback*)

Event object passed to the callback:

```
{ type: 'remove'
, target_type // element type, such as 'derivation'
, target // the element
};
```

<a name="cmodel-on-el-changed" href="#cmodel-on-el-changed">#</a> cmodel.**events.on**(**'el_changed'**, *callback*)

Event object passed to the callback:

```
{ type: 'el_changed'
, target_type // element type, such as 'derivation'
, target // the element
, last_eq // ascii string of the last derivation row (only for derivations)
};
```

## CanvasElements

Each element on the canvas like textboxes, images, geogebra elements, and derivations all share the same base class `CanvasElement`, which handles interactions like dragging, resizing, removing.

The following options can be used with any `CanvasElement`.

### constructor options

| Option | Description | Default Value |
| --- | --- | --- |
| `pos` | initial position | `{x: 'center', y: 'center'}` |
| `size` | initial size, some element types manage the size automatically | `{width: 100, height: 60}` |
| `min_size` | minimum size during resizing | `{ width: 30, height: 50 }` |
| `resizable` | can the user resize the element? | `{ x: true, y: true }` |
| `active` | is the element currently selected? | `false` |
| `mode` | current mode, allowed modes are `edit` and `arrange` | `edit` |
| `draggable` | allow users to drag the element | `true` |
| `keep_in_container` | don't allow users to drag the element beyond the size of the container | `true` |
| `edit_mode_drag_box_width` | width of the draggable area left of the element | `20` |
| `show_bg` | only displays background color if true | `true` |

## Derivation

This canvas element represents an algebraic deriavation that can consist of several lines (rows). Each row has an `AlgebraModel`, an `AlgebraView` and for most rows an `Action` that created the row. Derivations should be created through the [createElement](#createElement) method of a CanvasModel.

### constructor options

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
| `mode` | current mode, allowed modes are `edit`, `arrange`, `inspect` | `edit` |

<a name="setExpression" href="#setExpression">#</a> derivation.**setExpression**(*expr_str*)

Set the last line of the derivation to the passed expression string.

<a name="setFontSize" href="#setFontSize">#</a> derivation.**setFontSize**(*font_size*)

Updates the font size of the derivation.

<a name="getLastRow" href="#getLastRow">#</a> derivation.**getLastModel**()



Returns the `AlgebraModel` of the last line in the derivation.

### Events

<a name="der-on-change" href="#der-on-change">#</a> derivation.**events.on**(**'change'**, *callback*)

Event object passed to the callback:

```
{ type: 'change'
, performee // id of the derivation
, row // index of the new / changed row
, model // AlgebraModel, the new / changed model
}
```

## AlgebraModel

Represents the state of a single algebraic expression.

## AlgebraView

Visualizes an AlgebraModel and allows user interaction.

## Actions

Graspable Math comes with a large library of actions that define mathematical transformations and the gestures that trigger them.
