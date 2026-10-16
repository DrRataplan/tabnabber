# tabnabber

A tiny, self-contained demo of [reverse tabnabbing](https://owasp.org/www-community/attacks/Reverse_Tabnabbing):
when a page opens this one via `window.open(url)` without `noopener`/`noreferrer` (and the
opener sends no `Cross-Origin-Opener-Policy` header), this page can reach back through
`window.opener` and navigate the tab that opened it — e.g. to a fake "you've been logged out,
log in again" page.

This repo intentionally collects nothing. It only demonstrates the navigation; there is no
credential form anywhere.

## Pages

- `index.html` — checks for `window.opener`, reports whether the opening tab is vulnerable,
  and (if so) takes it over after a short delay.
- `pwned.html` — the unmistakable "YOU HAVE BEEN PWNED" landing page the opener tab is
  redirected to, explaining what just happened.

## Using it

Open any page that links to this site via `window.open()` without `noopener`. If it's
vulnerable, within a few seconds the *original* tab will be replaced by `pwned.html`.

## The fix

On the opener's side:

```js
window.open(url, '_blank', 'noopener');
```

or send a `Cross-Origin-Opener-Policy: same-origin` header, or use
`<a target="_blank" rel="noopener">` instead of `window.open` where possible.
