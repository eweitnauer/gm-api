# Customizing GM embedded as an iframe

As described [here](https://github.com/eweitnauer/gm-api/blob/master/quickstart.md#embedding-in-an-iframe), GM (Graspable Math) can generate for you a snippet of code you can paste into your HTML page. That looks kind of like

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

You can customize the options in the URL of the code snippet after the `embed.html?` . Separate these options with `&` signs.
* `load=` the ID of the document you want automatically loaded
* `eq=` a list of mathematical expressions separated by `;` that should be added to the canvas upon load e.g. `5+4;6^2=x`
* `parent_url=` there should be no need for you to change this
* `options=` a object written with [JSON syntax](https://www.w3schools.com/js/js_json_syntax.asp) which may have the options described **in the following table** between its `{` and `}`.

## Options that go in the `options=` option (described above)

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
| `"vertical_scroll"` | `true` | boolean | Whether to automatically grow the canvas (infinite scrolling). This cannot be turned off if you are embedding GM in an iframe. |
| `"use_hold_menu"` | `true` | boolean | If this is true and you click & hold somewhere on some blank part of the canvas, then a circular menu will appear |