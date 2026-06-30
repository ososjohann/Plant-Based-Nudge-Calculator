# Embedding the Plant-Based Nudge Calculator

`index.html` is a single, self-contained file (inline CSS/JS, images embedded
as base64). The only external request it makes is for the Lato web font from
Google Fonts. You can host it anywhere that serves files over HTTPS.

## 1. Host the file on the BFF server

Upload `index.html` to any path on the BFF web host, e.g.

    https://betterfoodfoundation.org/tools/nudge-calculator/index.html

(Or keep the current GitHub Pages copy and just point the iframe at it.)

## 2. Paste this snippet into the BFF page

Drop this into an "Embed / Custom HTML / Code" block on the BFF CMS page.
It creates a responsive iframe that auto-resizes to the calculator's height
(no scrollbar, no fixed height to guess at).

```html
<iframe
  id="nudgeCalc"
  src="https://betterfoodfoundation.org/tools/nudge-calculator/index.html"
  title="Plant-Based Nudge Calculator"
  style="width:100%;border:0;display:block;overflow:hidden;"
  scrolling="no"
  loading="lazy"
  height="1200"></iframe>
<script>
  window.addEventListener('message', function (e) {
    if (e.data && e.data.type === 'nudge-calc-height') {
      document.getElementById('nudgeCalc').style.height = e.data.height + 'px';
    }
  });
</script>
```

Change the `src` to wherever you uploaded `index.html`. The `height="1200"`
is only a fallback for the first paint before the auto-resize kicks in.

## Notes

- **Auto-resize**: `index.html` posts its height to the parent page via
  `postMessage` (`{ type: 'nudge-calc-height', height: <px> }`). The snippet
  above listens for it. If your CMS strips the `<script>`, the iframe still
  works — just set a fixed `height` that fits.
- **No build step, no dependencies.** To update the calculator later, replace
  the hosted `index.html`; every embed updates automatically.
- **Squarespace/Wix/Webflow**: use their "Embed" / "Custom Code" block and
  paste the same snippet.
