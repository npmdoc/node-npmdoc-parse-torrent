# api documentation for  [parse-torrent (v5.8.2)](https://github.com/feross/parse-torrent)  [![npm package](https://img.shields.io/npm/v/npmdoc-parse-torrent.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-parse-torrent) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-parse-torrent.svg)](https://travis-ci.org/npmdoc/node-npmdoc-parse-torrent)
#### Parse a torrent identifier (magnet uri, .torrent file, info hash)

[![NPM](https://nodei.co/npm/parse-torrent.png?downloads=true)](https://www.npmjs.com/package/parse-torrent)

[![apidoc](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-parse-torrent_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-parse-torrent/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Feross Aboukhadijeh",
        "email": "feross@feross.org",
        "url": "http://feross.org/"
    },
    "bin": {
        "parse-torrent": "./bin/cmd.js"
    },
    "bugs": {
        "url": "https://github.com/feross/parse-torrent/issues"
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
        "shasum": "09f02ca43ec2d885d1460aacb0fb50c79b3c49f9",
        "tarball": "https://registry.npmjs.org/parse-torrent/-/parse-torrent-5.8.2.tgz"
    },
    "gitHead": "adcc793b00b2c8c88f9a96b7361b28df8d455490",
    "homepage": "https://github.com/feross/parse-torrent",
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
            "name": "feross",
            "email": "feross@feross.org"
        }
    ],
    "name": "parse-torrent",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/feross/parse-torrent.git"
    },
    "scripts": {
        "test": "standard && npm run test-node && npm run test-browser",
        "test-browser": "zuul -- test/basic.js",
        "test-browser-local": "zuul --local -- test/basic.js",
        "test-node": "tape test/*.js"
    },
    "testling": {
        "files": "test/*.js"
    },
    "version": "5.8.2"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module parse-torrent](#apidoc.module.parse-torrent)
1.  [function <span class="apidocSignatureSpan">parse-torrent.</span>remote (torrentId, cb)](#apidoc.element.parse-torrent.remote)
1.  [function <span class="apidocSignatureSpan">parse-torrent.</span>toMagnetURI (obj)](#apidoc.element.parse-torrent.toMagnetURI)
1.  [function <span class="apidocSignatureSpan">parse-torrent.</span>toTorrentFile (parsed)](#apidoc.element.parse-torrent.toTorrentFile)



# <a name="apidoc.module.parse-torrent"></a>[module parse-torrent](#apidoc.module.parse-torrent)

#### <a name="apidoc.element.parse-torrent.remote"></a>[function <span class="apidocSignatureSpan">parse-torrent.</span>remote (torrentId, cb)](#apidoc.element.parse-torrent.remote)
- description and source-code
```javascript
function parseTorrentRemote(torrentId, cb) {
  var parsedTorrent
  if (typeof cb !== 'function') throw new Error('second argument must be a Function')

  try {
    parsedTorrent = parseTorrent(torrentId)
  } catch (err) {
    // If torrent fails to parse, it could be a Blob, http/https URL or
    // filesystem path, so don't consider it an error yet.
  }

  if (parsedTorrent && parsedTorrent.infoHash) {
    process.nextTick(function () {
      cb(null, parsedTorrent)
    })
  } else if (isBlob(torrentId)) {
    blobToBuffer(torrentId, function (err, torrentBuf) {
      if (err) return cb(new Error('Error converting Blob: ' + err.message))
      parseOrThrow(torrentBuf)
    })
  } else if (typeof get === 'function' && /^https?:/.test(torrentId)) {
    // http, or https url to torrent file
    get.concat({
      url: torrentId,
      timeout: 30 * 1000,
      headers: { 'user-agent': 'WebTorrent (http://webtorrent.io)' }
    }, function (err, res, torrentBuf) {
      if (err) return cb(new Error('Error downloading torrent: ' + err.message))
      parseOrThrow(torrentBuf)
    })
  } else if (typeof fs.readFile === 'function' && typeof torrentId === 'string') {
    // assume it's a filesystem path
    fs.readFile(torrentId, function (err, torrentBuf) {
      if (err) return cb(new Error('Invalid torrent identifier'))
      parseOrThrow(torrentBuf)
    })
  } else {
    process.nextTick(function () {
      cb(new Error('Invalid torrent identifier'))
    })
  }

  function parseOrThrow (torrentBuf) {
    try {
      parsedTorrent = parseTorrent(torrentBuf)
    } catch (err) {
      return cb(err)
    }
    if (parsedTorrent && parsedTorrent.infoHash) cb(null, parsedTorrent)
    else cb(new Error('Invalid torrent identifier'))
  }
}
```
- example usage
```shell
...
### remote torrents

To support remote torrent identifiers (i.e. http/https links to .torrent files, or
filesystem paths), as well as Blobs use the 'parseTorrent.remote' function. It takes
a callback since these torrent types require async operations:

'''js
parseTorrent.remote(torrentId, function (err, parsedTorrent) {
  if (err) throw err
  console.log(parsedTorrent)
})
'''

### command line program
...
```

#### <a name="apidoc.element.parse-torrent.toMagnetURI"></a>[function <span class="apidocSignatureSpan">parse-torrent.</span>toMagnetURI (obj)](#apidoc.element.parse-torrent.toMagnetURI)
- description and source-code
```javascript
function magnetURIEncode(obj) {
  obj = extend(obj) // clone obj, so we can mutate it

  // support using convenience names, in addition to spec names
  // (example: 'infoHash' for 'xt', 'name' for 'dn')
  if (obj.infoHashBuffer) obj.xt = 'urn:btih:' + obj.infoHashBuffer.toString('hex')
  if (obj.infoHash) obj.xt = 'urn:btih:' + obj.infoHash
  if (obj.name) obj.dn = obj.name
  if (obj.keywords) obj.kt = obj.keywords
  if (obj.announce) obj.tr = obj.announce
  if (obj.urlList) {
    obj.ws = obj.urlList
    delete obj.as
  }

  var result = 'magnet:?'
  Object.keys(obj)
    .filter(function (key) {
      return key.length === 2
    })
    .forEach(function (key, i) {
      var values = Array.isArray(obj[key]) ? obj[key] : [ obj[key] ]
      values.forEach(function (val, j) {
        if ((i > 0 || j > 0) && (key !== 'kt' || j === 0)) result += '&'

        if (key === 'dn') val = encodeURIComponent(val).replace(/%20/g, '+')
        if (key === 'tr' || key === 'xs' || key === 'as' || key === 'ws') {
          val = encodeURIComponent(val)
        }
        if (key === 'kt') val = encodeURIComponent(val)

        if (key === 'kt' && j > 0) result += '+' + val
        else result += key + '=' + val
      })
    })

  return result
}
```
- example usage
```shell
...

The reverse works too. To convert an object of keys/value to a magnet uri or .torrent file
buffer, use 'toMagnetURI' and 'toTorrentFile'.

'''js
var parseTorrent = require('parse-torrent')

var uri = parseTorrent.toMagnetURI({
infoHash: 'd2474e86c95b19b8bcfdb92bc12c9d44667cfa36'
})
console.log(uri) // 'magnet:?xt=urn:btih:d2474e86c95b19b8bcfdb92bc12c9d44667cfa36'

var buf = parseTorrent.toTorrentFile({
info: {
  /* ... */
...
```

#### <a name="apidoc.element.parse-torrent.toTorrentFile"></a>[function <span class="apidocSignatureSpan">parse-torrent.</span>toTorrentFile (parsed)](#apidoc.element.parse-torrent.toTorrentFile)
- description and source-code
```javascript
function encodeTorrentFile(parsed) {
  var torrent = {
    info: parsed.info
  }

  torrent['announce-list'] = (parsed.announce || []).map(function (url) {
    if (!torrent.announce) torrent.announce = url
    url = new Buffer(url, 'utf8')
    return [ url ]
  })

  torrent['url-list'] = parsed.urlList || []

  if (parsed.created) {
    torrent['creation date'] = (parsed.created.getTime() / 1000) | 0
  }

  if (parsed.createdBy) {
    torrent['created by'] = parsed.createdBy
  }

  if (parsed.comment) {
    torrent.comment = parsed.comment
  }

  return bencode.encode(torrent)
}
```
- example usage
```shell
...
var parseTorrent = require('parse-torrent')

var uri = parseTorrent.toMagnetURI({
  infoHash: 'd2474e86c95b19b8bcfdb92bc12c9d44667cfa36'
})
console.log(uri) // 'magnet:?xt=urn:btih:d2474e86c95b19b8bcfdb92bc12c9d44667cfa36'

var buf = parseTorrent.toTorrentFile({
  info: {
    /* ... */
  }
})
console.log(buf)
'''
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
