---
author: ejs.co
datetime: 2022-12-21
title: EJS-Embedded JavaScript templating
slug: "ejs"
featured: false
draft: false
tags:
  - ejs
  - server
ogImage: ""
description: EJS is a simple templating language that lets you generate HTML markup with plain JavaScript.
---

# EJS

What is the "E" for? "Embedded?" Could be. How about "Effective," "Elegant," or just "Easy"? EJS is a simple templating language that lets you generate HTML markup with plain JavaScript. No religiousness about how to organize things. No reinvention of iteration and control-flow. It's just plain JavaScript.

## Install

```js
npm install ejs
```

### Use

Pass EJS a template string and some data. BOOM, you've got some HTML.

```js
let ejs = require("ejs");
let people = ["geddy", "neil", "alex"];
let html = ejs.render('<%= people.join(", "); %>', { people: people });
```

### Docs

Example

```js
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
```

### Usage

```js
let template = ejs.compile(str, options);
template(data);
// => Rendered HTML string

ejs.render(str, data, options);
// => Rendered HTML string

ejs.renderFile(filename, data, options, function (err, str) {
  // str => Rendered HTML string
});
```

### Options

- cache Compiled functions are cached, requires filename
- filename Used by cache to key caches, and for includes
- root Set project root for includes with an absolute path (e.g, /file.ejs). Can be array to try to resolve include from multiple directories.
- views An array of paths to use when resolving includes with relative paths.
- context Function execution context
- compileDebug When false no debug instrumentation is compiled
- client Returns standalone compiled function
- delimiter Character to use for inner delimiter, by default '%'
- openDelimiter Character to use for opening delimiter, by default '<'
- closeDelimiter Character to use for closing delimiter, by default '>'
- debug Outputs generated function body
- strict When set to `true`, generated function is in strict mode
- \_with Whether or not to use with() {} constructs. If false then the locals will be stored in the locals object. (Implies `--strict`)
- localsName Name to use for the object storing local variables when not using with Defaults to locals
- rmWhitespace Remove all safe-to-remove whitespace, including leading and trailing whitespace. It also enables a safer version of -%> line slurping for all scriptlet tags (it does not strip new lines of tags in the middle of a line).
- escape The escaping function used with <%= construct. It is used in rendering and is .toString()ed in the generation of client functions. (By default escapes XML).
- outputFunctionName Set to a string (e.g., 'echo' or 'print') for a function to print output inside scriptlet tags.
- async When true, EJS will use an async function for rendering. (Depends on async/await support in the JS runtime.

### Tags

- <% 'Scriptlet' tag, for control-flow, no output
- <%\_ ‘Whitespace Slurping’ Scriptlet tag, strips all whitespace before it
- <%= Outputs the value into the template (HTML escaped)
- <%- Outputs the unescaped value into the template
- <%# Comment tag, no execution, no output
- <%% Outputs a literal '<%'
- %> Plain ending tag
- -%> Trim-mode ('newline slurp') tag, trims following newline
- \_%> ‘Whitespace Slurping’ ending tag, removes all whitespace after it

### Includes

Includes are relative to the template with the include call. (This requires the 'filename' option.) For example if you have "./views/users.ejs" and "./views/user/show.ejs" you would use <%- include('user/show'); %>.

You'll likely want to use the raw output tag (<%-) with your include to avoid double-escaping the HTML output.

```js
<ul>
  <% users.forEach(function(user){ %>
    <%- include('user/show', {user: user}); %>
  <% }); %>
</ul>
```
