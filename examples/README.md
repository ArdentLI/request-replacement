
# Authentication

# Multipart

## multipart/form-data

### Flickr Image Upload

- https://www.flickr.com/services/api/upload.api.html

```js
request.post('https://up.flickr.com/services/upload', {
  // all meta data should be included here for proper signing
  qs: {
    title: 'My cat is awesome',
    description: 'Sent on ' + new Date(),
    is_public: 1
  },
  // again the same meta data + the actual photo
  formData: {
    title: 'My cat is awesome',
    description: 'Sent on ' + new Date(),
    is_public: 1,
    photo:fs.createReadStream('cat.png')
  },
  json: true
}, function (err, res, body) {
  // assert.equal(typeof body, 'object')
})
```

# Streams

## `POST` data

Use Request as a Writable stream to easily `POST` Readable streams (like files, other HTTP requests, or otherwise).

TL;DR: Pipe a Readable Stream onto Request via:

```
READABLE.pipe(request.post(URL));
```

A more detailed example:

```js
var fs = require('fs')
  , path = require('path')
  , http = require('http')
  , request = require('request')
  , TMP_FILE_PATH = path.join(path.sep, 'tmp', 'foo')
;

// write a temporary file:
fs.writeFileSync(TMP_FILE_PATH, 'foo bar baz quk\n');

http.createServer(function(req, res) {
  console.log('the server is receiving data!\n');
  req
    .on('end', res.end.bind(res))
    .pipe(process.stdout)
  ;
}).listen(3000).unref();

fs.createReadStream(TMP_FILE_PATH)
  .pipe(request.post('http://127.0.0.1:3000'))
;
```

# Proxys

Run tor on the terminal and try the following. (Needs `socks5-http-client` to connect to tor)

```js
var request = require('../index.js');
var Agent = require('socks5-http-client/lib/Agent');

request.get({
    url: 'http://www.tenreads.io',
    agentClass: Agent,
    agentOptions: {
        socksHost: 'localhost', // Defaults to 'localhost'.
        socksPort: 9050 // Defaults to 1080.
    }
}, function (err, res) {
    console.log(res.body);
});
```
