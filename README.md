# mime-db

[![NPM Version][npm-version-image]][npm-url]
[![NPM Downloads][npm-downloads-image]][npm-url]
[![Node.js Version][node-image]][node-url]
[![Build Status][ci-image]][ci-url]
[![Coverage Status][coveralls-image]][coveralls-url]

This is a large database of mime types and information about them.
It consists of a single, public JSON file and does not include any logic,
allowing it to remain as un-opinionated as possible with an API.
It aggregates data from the following sources:

- https://www.iana.org/assignments/media-types/media-types.xhtml
- https://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
- https://hg.nginx.org/nginx/raw-file/default/conf/mime.types

## Installation

```bash
npm install https://github.com/DK013/mime-db
```

### Database Download

If you're crazy enough to use this in the browser, you can just grab the
JSON file using [jsDelivr](https://www.jsdelivr.com/). It is recommended to
replace `master` with [a release tag](https://github.com/jshttp/mime-db/tags)
as the JSON format may change in the future.

```
https://cdn.jsdelivr.net/gh/dk013/mime-db@master/db.json
https://cdn.jsdelivr.net/gh/dk013/mime-db@master/db-ext.json
```

## CommonJS Usage

```js
var db = require('mime-db')

// grab data on .js files
var data = db['application/javascript'];

// grab mime types for extension
var type = db.extension['js'];
```

## ES6 Usage

```js
import db, { extension } from "mime-db";

// grab data on .js files
var data = db['application/javascript'];

// grab mime types for extension
var type = extension['js'];
```

## Data Structure

### db.json

The JSON file is a map lookup for lower-cased mime types. Each mime type has
the following properties:

- `.source` - where the mime type is defined.
    If not set, it's probably a custom media type.
    - `apache` - [Apache common media types](https://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types)
    - `iana` - [IANA-defined media types](https://www.iana.org/assignments/media-types/media-types.xhtml)
    - `nginx` - [nginx media types](https://hg.nginx.org/nginx/raw-file/default/conf/mime.types)
- `.extensions[]` - known extensions associated with this mime type.
- `.compressible` - whether a file of this type can be gzipped.
- `.charset` - the default charset associated with this type, if any.

If unknown, every property could be `undefined`.

### db-ext.json

The JSON file is a map lookup for lower-cased file extensions. Each file
extension is an array with the list of lower-cased mime types in order of
preference.

## Contributing

The primary way to contribute to this database is by updating the data in
one of the upstream sources. The database is updated from the upstreams
periodically and will pull in any changes.

### Registering Media Types

The best way to get new media types included in this library is to register
them with the IANA. The community registration procedure is outlined in
[RFC 6838 section 5](https://tools.ietf.org/html/rfc6838#section-5). Types
registered with the IANA are automatically pulled into this library.

### Direct Inclusion

If that is not possible / feasible, they can be added directly here as a
"custom" type. To do this, it is required to have a primary source that
definitively lists the media type. If an extension is going to be listed as
associateed with this media type, the source must definitively link the
media type and extension as well.

To edit the database, only make PRs against `src/custom-types.json` or
`src/custom-suffix.json`.

The `src/custom-types.json` file is a JSON object with the MIME type as the
keys and the values being an object with the following keys:

- `compressible` - leave out if you don't know, otherwise `true`/`false` to
  indicate whether the data represented by the type is typically compressible.
- `extensions` - include an array of file extensions that are associated with
  the type.
- `notes` - human-readable notes about the type, typically what the type is.
- `sources` - include an array of URLs of where the MIME type and the associated
  extensions are sourced from. This needs to be a [primary source](https://en.wikipedia.org/wiki/Primary_source);
  links to type aggregating sites and Wikipedia are _not acceptable_.

To update the build, run `npm run build`.

[ci-image]: https://badgen.net/github/checks/jshttp/mime-db/master?label=ci
[ci-url]: https://github.com/jshttp/mime-db/actions/workflows/ci.yml
[coveralls-image]: https://badgen.net/coveralls/c/github/jshttp/mime-db/master
[coveralls-url]: https://coveralls.io/r/jshttp/mime-db?branch=master
[node-image]: https://badgen.net/npm/node/mime-db
[node-url]: https://nodejs.org/en/download
[npm-downloads-image]: https://badgen.net/npm/dm/mime-db
[npm-url]: https://npmjs.org/package/mime-db
[npm-version-image]: https://badgen.net/npm/v/mime-db
