# Customizing GM embedded as an iframe

As described [here](https://github.com/eweitnauer/gm-api/blob/master/quickstart.md#embedding-in-an-iframe), GM (Graspable Math) can generate for you a snippet of code you can paste into your HTML page. That looks kind of like

<!-- prettier-ignore-start -->
```html
<!DOCTYPE html>
<meta charset="utf-8" />
<title>Test</title>
<body>
  <!-- Include this in your page (Start) -->
  <script type="text/javascript">url = parent.document.URL; document.write('<iframe style="border: none" src=\'https://graspablemath.com/canvas/embed.html?load=_e0df6d1a59896f82&options={"use_toolbar": false, "vertical_scroll": false }&parent_url='+url+'\' width=100% height=400px></iframe>')
  </script>
  <!-- Include this in your page (End) -->
</body>
```
<!-- prettier-ignore-end -->

You can customize the options in the URL of the code snippet after the `embed.html?` . Separate these options with `&` signs.

- `load=` the ID of the document you want automatically loaded
- `eq=` a list of mathematical expressions separated by `;` that should be added to the canvas upon load e.g. `5+4;6^2=x`
- `parent_url=` there should be no need for you to change this
- `options=` a object written with [JSON syntax](https://www.w3schools.com/js/js_json_syntax.asp) which may have the options described the the API.md document.
