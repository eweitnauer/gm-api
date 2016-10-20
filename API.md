## gm-inject

**loadGM**(*options*)

Including the [gm-inject.js](https://graspablemath.com/shared/libs/gmath/gm-inject.js) script in an html page provides the `loadGM()` method. Calling it injects the gmath library, the d3 library, jquery and bootstrap, as well as several stylesheet files into the page. The *options* parameter needs to specify which gmath version to load, such as `{version: '0.12.2'}`.

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
