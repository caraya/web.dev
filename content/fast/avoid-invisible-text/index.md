---
page_type: guide
title: Avoid invisible text during font loading
author: khempenius
description: |
  Fonts are often large files that take awhile to load. To deal with this, some
  browsers hide text until the font loads (the "flash of invisible text"). If
  you're optimizing for performance, you'll want to avoid the "flash of
  invisible text" and show content to users immediately using a system font.
web_lighthouse:
- font-display
web_updated_on: 2018-12-06
web_published_on: 2018-11-05
wf_blink_components: N/A
---

# Avoid invisible text during font loading

## Why should you care?

Fonts are often large files that take awhile to load. To deal with this, some
browsers hide text until the font loads (the "flash of invisible text"). If
you're optimizing for performance, you'll want to avoid the "flash of invisible
text" and show content to users immediately using a system font (the "flash of
unstyled text").

## Display text immediately

This guide outlines two ways to achieve this: the first approach is very simple
but does not have universal browser
[support](https://caniuse.com/#search=font-display); the second approach is more
work but has full browser support. The best choice for you is the one that
you'll actually implement and maintain.

## Option #1: Use font-display

<table>
<thead>
<tr>
<th>Before</th>
<th>After</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>
@font-face {
  font-family: Helvetica;
}
</code>
</td>
<td>
<code>
@font-face {
  font-family: Helvetica;
  font-display: swap;
}
</code>
</td>
</tr>
</tbody>
</table>

[`font-display`](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display)
is an API for specifying font display strategy. `swap` tells the browser that
text using this font should be displayed immediately using a system font. Once
the custom font is ready, the system font is swapped out.

If a browser does not support `font-display`, the browser continues to follow
it's default behavior for loading fonts. Check which browsers support
`font-display` [here](https://caniuse.com/#search=font-display).

These are the default font-loading behaviors for common browsers:

<table>
<thead>
<tr>
<th><strong>Browser</strong></th>
<th><strong>Default behavior if font is not ready…</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Microsoft Edge</td>
<td>Uses a system font until font is ready. Swaps out font.</td>
</tr>
<tr>
<td>Chrome</td>
<td>Will hide text for up to 3 seconds. If text is still not ready, uses a
system font until font is ready. Swaps out font.</td>
</tr>
<tr>
<td>Firefox</td>
<td>Will hide text for up to 3 seconds. If text is still not ready, uses a
system font until font is ready. Swaps out font. </td>
</tr>
<tr>
<td>Safari</td>
<td>Hides text until font is ready.</td>
</tr>
</tbody>
</table>

## Option #2: Wait to use custom fonts until they are loaded

With a bit more work, the same behavior can be implemented to work across all
browsers. Check out the step-by-step [guide](/fast/avoid-invisible-text/codelab-avoid-invisible-text.md) or the [demo](https://glitch.com/~font-observer-demo) for a more detailed
explanation of the code changes.

There are three parts to this approach:

+  Don't use a custom font on initial page load. This ensures that the
    browser displays text immediately using a system font.
+  Detect when your custom font is loaded. This can be accomplished with a
    couple lines of JavaScript code, thanks to the [FontFaceObserver](https://github.com/bramstein/fontfaceobserver) library.
+  Update page styling to use the custom font.

Here are the changes you can expect to make in order to implement this:

+  Refactor your CSS to not use a custom font on initial page load.
+  Add a script to your page. This script detects when the custom font is
    loaded and then will update the page styling.

## Verify

Run Lighthouse to verify the site is using `font-display: swap` to display
text:

1. Navigate to the **Audits** panel in DevTools.
1. Select the **Performance** checkbox.
1. Click **Run Audits** to generate a report.

Confirm this audit is passing: "Ensure text remains visible during webfont
load".
