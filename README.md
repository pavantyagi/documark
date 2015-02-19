# Documark

> PDF generator for scripted documents.

A library that:

1. Compiles scripted document files (Jade, Markdown, and assets) into a PDF;
2. Is used as a command line interface ([`npm install -g documark-cli`][documark-cli]);
3. Can watch for files changes to recompile the document (`documark compile --watch`).

## Why?

My personally hatret towards WYSIWYG word processors sparked me to write this tool. LaTeX felt like a waste of time, so instead I figured: why not use Markdown? I like Documark because it:

1. Separates content and styling;
2. Uses mature webtechnologies like Markdown, HTML, JS, and CSS for writing/styling the document;
3. Enforces a consistent document style (no dragging around table columns and floating images);
4. Allows version control with Git or SVN;
5. Simplifies collaboration (by splitting up the document in separate files);
6. Allows you to use your favorite text editor - like Vim ❤ ;
7. Makes automating things (through plugins) real easy!

## Example

Go to the [Documark example][documark-example] repository for a generated PDF and its source code.

## Dependencies

1. Currently manually installing [wkhtmltopodf v0.12.2.1+][wkhtmltopdf-install] is still required.

## Build process

These are the steps for compiling the PDF document:

1. Input files (Jade, Markdown, and assets)
2. Generate HTML
3. Convert to DOM tree (with [CheerioJS][cheeriojs])
3. Process plugins (which can alter the DOM and PDF configuration)
4. Emit `pre-compile` event
5. Generate PDF
6. Emit `post-compile` event

## Configuration

Document configuration can be done in two ways:

1. In the document's [front matter][front-matter];
2. In a separate `config.json` file.

If there is front matter in the document, the configuration file will be ignored.

### Plugins

Add plugins via the `plugins` key in the `document.jade` front matter:

```yaml
---
title: Document
plugins:
  - documark-plugin-loader
  - documark-hr-to-page-break
---
```

__Tip:__ Use the [documark plugin loader][documark-plugin-loader] to load custom plugins!

#### Writing your own plugins is easy!

Here's a boilerplate for a plugin named `dmp-my-custom-plugin` (`dmp-` is short for Documark plugin):

```js
// Require modules outside the plugin function
var path = require('path');

// Add camel cased plugin name to function (for debugging)
module.exports = function dmpMyCustomPlugin ($, document, cb) {
	// Manipulate the DOM tree
	$('my-custom-element').replaceWith('<p>Hello world!</p>');

	// Or alter the configuration
	document.options().pdf.marginLeft = '5cm';

	// Don't forget to let Documark know the plugin is done!
	cb();
};
```

Don't forget to load the plugin!

### wkhtmltopdf

Configure wkhtmltopdf with the `pdf` object in the documents front matter. For example:

```yaml
---
title: Document
pdf:
  userStyleSheet: path/to/main.css
---
```

Note that [node-wkhtmltopdf][node-wkhtmltopdf] is used as an intermediate package, which uses camel cased (`userStyleSheet`) options instead of dashed ones (`user-style-sheet`, like in the command line tool). See [this page][wkhtmltopdf-options] for a full list of configuration options.

[documark-cli]: https://github.com/mauvm/documark-cli
[documark-example]: https://github.com/mauvm/documark-example
[wkhtmltopdf-install]: http://wkhtmltopdf.org/downloads.html
[cheeriojs]: https://github.com/cheeriojs/cheerio
[front-matter]: https://github.com/jxson/front-matter#example
[documark-plugin-loader]: https://www.npmjs.com/package/documark-plugin-loader
[node-wkhtmltopdf]: https://www.npmjs.com/package/wkhtmltopdf
[wkhtmltopdf-options]: http://wkhtmltopdf.org/usage/wkhtmltopdf.txt
