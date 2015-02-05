# Documark

PDF generator for scripted documents.

A library that:

1. Compiles scripted document files (Jade, Markdown, and assets) into a PDF;
<!-- 2. Allow usage of templates (`documark init` to pick template and configure it); -->
3. Is used as a command-line interface (`npm install documark -g`, `documark compile`);
4. Can watch for files changes (`documark compile --watch`) to recompile the document.

__Note: This is still a work in progress!__

## Dependencies

1. For now manually installing [wkhtmltopdf](http://wkhtmltopdf.org/downloads.html) is still required.

## Configuration

Document configuration can be done in two ways:

1. In the document's [front matter](https://github.com/jxson/front-matter#example);
2. In a seperate `config.json`.

If there is front matter in the document, the configuration file will be ignored.

### wkhtmltopdf

Configure wkhtmltopdf with the `pdf` object in the documents front matter. For example:

```yaml
---
title: Document
pdf:
  user-style-sheet: path/to/main.css
---
```

See [this page][wkhtmltopdf-options] for a full list of configuration options.

[wkhtmltopdf-options]: http://wkhtmltopdf.org/usage/wkhtmltopdf.txt

