# api documentation for  [parse-torrent (v5.8.3)](https://github.com/webtorrent/parse-torrent#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-parse-torrent.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-parse-torrent) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-parse-torrent.svg)](https://travis-ci.org/npmdoc/node-npmdoc-parse-torrent)
#### Parse a torrent identifier (magnet uri, .torrent file, info hash)

[![NPM](https://nodei.co/npm/parse-torrent.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/parse-torrent)

- [https://npmdoc.github.io/node-npmdoc-parse-torrent/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "WebTorrent, LLC",
        "url": "https://webtorrent.io"
    },
    "bin": {
        "parse-torrent": "./bin/cmd.js"
    },
    "bugs": {
        "url": "https://github.com/webtorrent/parse-torrent/issues"
    },
    "dependencies": {
        "blob-to-buffer": "^1.2.6",
        "get-stdin": "^5.0.1",
        "magnet-uri": "^5.1.3",
        "parse-torrent-file": "^4.0.0",
        "simple-get": "^2.0.0"
    },
    "description": "Parse a torrent identifier (magnet uri, .torrent file, info hash)",
    "devDependencies": {
        "brfs": "^1.0.0",
        "standard": "*",
        "tape": "^4.0.0",
        "webtorrent-fixtures": "^1.0.0",
        "xtend": "^4.0.0",
        "zuul": "^3.0.0"
    },
    "directories": {},
    "dist": {
        "shasum": "f95ef23301239609de406794ad9f958a1bca1b6c",
        "tarball": "https://registry.npmjs.org/parse-torrent/-/parse-torrent-5.8.3.tgz"
    },
    "gitHead": "329d25ca8fee642f0b3097e5ef6ca55803702a6a",
    "homepage": "https://github.com/webtorrent/parse-torrent#readme",
    "keywords": [
        ".torrent",
        "bittorrent",
        "parse torrent",
        "peer-to-peer",
        "read torrent",
        "torrent",
        "webtorrent"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "feross"
        }
    ],
    "name": "parse-torrent",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git://github.com/webtorrent/parse-torrent.git"
    },
    "scripts": {
        "test": "standard && npm run test-node && npm run test-browser",
        "test-browser": "zuul -- test/basic.js",
        "test-browser-local": "zuul --local -- test/basic.js",
        "test-node": "tape test/*.js"
    },
    "version": "5.8.3"
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
