![GitHub package.json version](https://img.shields.io/github/package-json/v/zerodevx/zero-md)
![jsDelivr hits (GitHub)](https://img.shields.io/jsdelivr/gh/hm/zerodevx/zero-md)

# &lt;zero-md&gt;

> Ridiculously simple zero-config markdown displayer

A native markdown-to-html web component based on [Custom Elements V1 specs](https://www.w3.org/TR/custom-elements/)
to load and display an external MD file. Under the hood, it uses [marked](https://github.com/markedjs/marked) for
super-fast markdown transformation, and [Prism](https://github.com/PrismJS/prism) for feature-packed syntax
highlighting - automagically rendering into its own self-contained shadow DOM container, while encapsulating
implementation details into one embarassingly easy-to-use package.

**NOTE: This is the V2 branch. If you're looking for the older version, see the [V1 branch](https://github.com/zerodevx/zero-md/tree/v1).**

V2 is in pre-release and detailed documentation is work-in-progress. Stay tuned!

## Basic usage

### Via CDN (recommended)

`zero-md` is designed to be zero-config with good defaults. For most use-cases, just importing the script from CDN
and consuming the component directly should suffice.

```html
<head>
  ...
  <!-- Import element definition -->
  <script type="module" src="https://cdn.jsdelivr.net/gh/zerodevx/zero-md@2/dist/zero-md.min.js"></script>
  ...
</head>
<body>
  ...
  <!-- Profit! -->
  <zero-md src="/example.md"></zero-md>
  ...
</body>
```

**NOTE: V2 is in pre-release for now, so reference the exact version in the CDN URL.**

CDN: `https://cdn.jsdelivr.net/npm/zero-md@next/dist/zero-md.min.js`

### Use in web project

Install the package.

**NOTE: V2 is in pre-release for now, so install using the `@next` tag.**

```bash
$ npm i -D zero-md@next
```

Import the class, register the element, and use anywhere.

```js
// Import the element definition
import ZeroMd from 'zero-md'

// Register the custom element
customElements.define('zero-md', ZeroMd)

// Render anywhere
app.render(`<zero-md src=${src}></zero-md>`, target)
```

## Migrating from V1 to V2

1. Support for `<xmp>` tag is removed; use `<script type="text/markdown">` instead.

```html
<!-- Previous -->
<zero-md>
  <template>
    <xmp>
# `This` is my [markdown](example.md)
    </xmp>
  </template>
</zero-md>

<!-- Now -->
<zero-md>
  <!-- No need to wrap with <template> tag -->
  <script type="text/markdown">
# `This` is my [markdown](example.md)
  </script>
</zero-md>

<!-- If you need your code to be pretty, -->
<zero-md>
  <!-- set `data-dedent` to opt-in to dedent the text during render -->
  <script type="text/markdown" data-dedent>
    # It is important to be pretty
    So having spacing makes me happy.
  </script>
</zero-md>
```

2. Markdown source behaviour has changed. Think of `<script type="text/markdown">` as a "fallback".

```html
<!-- Previous -->
<zero-md src="will-not-render.md">
  <template>
    <xmp>
# This has first priority and will be rendered instead of `will-not-render.md`
    </xmp>
  </template>
<zero-md>

<!-- Now -->
<zero-md src="will-render-unless-falsy.md">
  <script type="text/markdown">
# This will NOT be rendered *unless* `src` resolves to falsy
  </script>
<zero-md>
```

3. The `css-urls` attribute is deprecated. Use `<link rel="stylesheet">` instead.

```html
<!-- Previously -->
<zero-md src="example.md" css-urls='["/style1.css", "/style2.css"]'><zero-md>

<!-- Now, this... -->
<zero-md src="example.md"></zero-md>

<!-- ...is actually equivalent to this -->
<zero-md src="example.md">
  <template>
    <!-- These are the default stylesheets -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/sindresorhus/github-markdown-css@4/github-markdown.min.css">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/PrismJS/prism@1/themes/prism.min.css">
  </template>
</zero-md>

<!-- So, to apply your own external stylesheets... -->
<zero-md src="example.md">
  <!-- ...you overwrite the default template -->
  <template>
    <!-- Use <link> tags to reference your own stylesheets -->
    <link rel="stylesheet" href="/style1.css">
    <link rel="stylesheet" href="/style2.css">
    <!-- You can even apply additional styles -->
    <style>
      p {
        color: red;
      }
    </style>
  </template>
</zero-md>

<!-- If you like the default stylesheets but wish to apply some overrides -->
<zero-md src="example.md">
  <!-- Set `data-merge` to "append" to apply this template AFTER the default template -->
  <!-- Or "prepend" to apply this template BEFORE. -->
  <template data-merge="append">
    <style>
      p {
        color: red;
      }
    </style>
  </template>
</zero-md>
```

4. The attributes `marked-url` and `prism-url` are deprecated. To load `marked` or `prism` from another
location, simply load their scripts *before* importing `zero-md`.

```html
<head>
  ...
  <script defer src="/lib/marked.js"></script>
  <script defer src="/lib/prism.js"></script>
  <script type="module" src="/lib/zero-md.min.js"></script>
</head>

```

5. The global config object has been renamed from `ZeroMd.config` to `ZeroMdConfig`.

6. The convenience events `zero-md-marked-ready` and `zero-md-prism-ready` are removed and **will no longer fire**.
Instead, the `zero-md-ready` event guarantees that everything is ready, and that render can begin.

## Contribute

Fork, clone, `npm i`, checkout new branch, develop, lint, test, commit, raise a PR.

Lint using [standard](https://github.com/standard/standard) rules:

```bash
$ npm run lint
```

Start test server:

```bash
$ npm run test
```

Run browser tests at http://localhost:5000

## License

ISC
