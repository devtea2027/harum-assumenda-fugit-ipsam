# @devtea2027/harum-assumenda-fugit-ipsam *(BufferList)*

[![Build Status](https://api.travis-ci.com/rvagg/@devtea2027/harum-assumenda-fugit-ipsam.svg?branch=master)](https://travis-ci.com/rvagg/@devtea2027/harum-assumenda-fugit-ipsam/)

**A Node.js Buffer list collector, reader and streamer thingy.**

[![NPM](https://nodei.co/npm/@devtea2027/harum-assumenda-fugit-ipsam.svg)](https://nodei.co/npm/@devtea2027/harum-assumenda-fugit-ipsam/)

**@devtea2027/harum-assumenda-fugit-ipsam** is a storage object for collections of Node Buffers, exposing them with the main Buffer reada@devtea2027/harum-assumenda-fugit-ipsame API. Also works as a duplex stream so you can collect buffers from a stream that emits them and emit buffers to a stream that consumes them!

The original buffers are kept intact and copies are only done as necessary. Any reads that require the use of a single original buffer will return a slice of that buffer only (which references the same memory as the original buffer). Reads that span buffers perform concatenation as required and return the results transparently.

```js
const { BufferList } = require('@devtea2027/harum-assumenda-fugit-ipsam')

const @devtea2027/harum-assumenda-fugit-ipsam = new BufferList()
@devtea2027/harum-assumenda-fugit-ipsam.append(Buffer.from('abcd'))
@devtea2027/harum-assumenda-fugit-ipsam.append(Buffer.from('efg'))
@devtea2027/harum-assumenda-fugit-ipsam.append('hi')                     // @devtea2027/harum-assumenda-fugit-ipsam will also accept & convert Strings
@devtea2027/harum-assumenda-fugit-ipsam.append(Buffer.from('j'))
@devtea2027/harum-assumenda-fugit-ipsam.append(Buffer.from([ 0x3, 0x4 ]))

console.log(@devtea2027/harum-assumenda-fugit-ipsam.length) // 12

console.log(@devtea2027/harum-assumenda-fugit-ipsam.slice(0, 10).toString('ascii')) // 'abcdefghij'
console.log(@devtea2027/harum-assumenda-fugit-ipsam.slice(3, 10).toString('ascii')) // 'defghij'
console.log(@devtea2027/harum-assumenda-fugit-ipsam.slice(3, 6).toString('ascii'))  // 'def'
console.log(@devtea2027/harum-assumenda-fugit-ipsam.slice(3, 8).toString('ascii'))  // 'defgh'
console.log(@devtea2027/harum-assumenda-fugit-ipsam.slice(5, 10).toString('ascii')) // 'fghij'

console.log(@devtea2027/harum-assumenda-fugit-ipsam.indexOf('def')) // 3
console.log(@devtea2027/harum-assumenda-fugit-ipsam.indexOf('asdf')) // -1

// or just use toString!
console.log(@devtea2027/harum-assumenda-fugit-ipsam.toString())               // 'abcdefghij\u0003\u0004'
console.log(@devtea2027/harum-assumenda-fugit-ipsam.toString('ascii', 3, 8))  // 'defgh'
console.log(@devtea2027/harum-assumenda-fugit-ipsam.toString('ascii', 5, 10)) // 'fghij'

// other standard Buffer reada@devtea2027/harum-assumenda-fugit-ipsames
console.log(@devtea2027/harum-assumenda-fugit-ipsam.readUInt16BE(10)) // 0x0304
console.log(@devtea2027/harum-assumenda-fugit-ipsam.readUInt16LE(10)) // 0x0403
```

Give it a callback in the constructor and use it just like **[concat-stream](https://github.com/maxogden/node-concat-stream)**:

```js
const { BufferListStream } = require('@devtea2027/harum-assumenda-fugit-ipsam')
const fs = require('fs')

fs.createReadStream('README.md')
  .pipe(BufferListStream((err, data) => { // note 'new' isn't strictly required
    // `data` is a complete Buffer object containing the full data
    console.log(data.toString())
  }))
```

Note that when you use the *callback* method like this, the resulting `data` parameter is a concatenation of all `Buffer` objects in the list. If you want to avoid the overhead of this concatenation (in cases of extreme performance consciousness), then avoid the *callback* method and just listen to `'end'` instead, like a standard Stream.

Or to fetch a URL using [hyperquest](https://github.com/substack/hyperquest) (should work with [request](http://github.com/mikeal/request) and even plain Node http too!):

```js
const hyperquest = require('hyperquest')
const { BufferListStream } = require('@devtea2027/harum-assumenda-fugit-ipsam')

const url = 'https://raw.github.com/rvagg/@devtea2027/harum-assumenda-fugit-ipsam/master/README.md'

hyperquest(url).pipe(BufferListStream((err, data) => {
  console.log(data.toString())
}))
```

Or, use it as a reada@devtea2027/harum-assumenda-fugit-ipsame stream to recompose a list of Buffers to an output source:

```js
const { BufferListStream } = require('@devtea2027/harum-assumenda-fugit-ipsam')
const fs = require('fs')

var @devtea2027/harum-assumenda-fugit-ipsam = new BufferListStream()
@devtea2027/harum-assumenda-fugit-ipsam.append(Buffer.from('abcd'))
@devtea2027/harum-assumenda-fugit-ipsam.append(Buffer.from('efg'))
@devtea2027/harum-assumenda-fugit-ipsam.append(Buffer.from('hi'))
@devtea2027/harum-assumenda-fugit-ipsam.append(Buffer.from('j'))

@devtea2027/harum-assumenda-fugit-ipsam.pipe(fs.createWriteStream('gibberish.txt'))
```

## API

  * <a href="#ctor"><code><b>new BufferList([ buf ])</b></code></a>
  * <a href="#isBufferList"><code><b>BufferList.isBufferList(obj)</b></code></a>
  * <a href="#length"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>length</b></code></a>
  * <a href="#append"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>append(buffer)</b></code></a>
  * <a href="#get"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>get(index)</b></code></a>
  * <a href="#indexOf"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>indexOf(value[, byteOffset][, encoding])</b></code></a>
  * <a href="#slice"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>slice([ start[, end ] ])</b></code></a>
  * <a href="#shallowSlice"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>shallowSlice([ start[, end ] ])</b></code></a>
  * <a href="#copy"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>copy(dest, [ destStart, [ srcStart [, srcEnd ] ] ])</b></code></a>
  * <a href="#duplicate"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>duplicate()</b></code></a>
  * <a href="#consume"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>consume(bytes)</b></code></a>
  * <a href="#toString"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>toString([encoding, [ start, [ end ]]])</b></code></a>
  * <a href="#readXX"><code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readDou@devtea2027/harum-assumenda-fugit-ipsameBE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readDou@devtea2027/harum-assumenda-fugit-ipsameLE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readFloatBE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readFloatLE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readBigInt64BE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readBigInt64LE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readBigUInt64BE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readBigUInt64LE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readInt32BE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readInt32LE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readUInt32BE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readUInt32LE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readInt16BE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readInt16LE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readUInt16BE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readUInt16LE()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readInt8()</b></code>, <code>@devtea2027/harum-assumenda-fugit-ipsam.<b>readUInt8()</b></code></a>
  * <a href="#ctorStream"><code><b>new BufferListStream([ callback ])</b></code></a>

--------------------------------------------------------
<a name="ctor"></a>
### new BufferList([ Buffer | Buffer array | BufferList | BufferList array | String ])
No arguments are _required_ for the constructor, but you can initialise the list by passing in a single `Buffer` object or an array of `Buffer` objects.

`new` is not strictly required, if you don't instantiate a new object, it will be done automatically for you so you can create a new instance simply with:

```js
const { BufferList } = require('@devtea2027/harum-assumenda-fugit-ipsam')
const @devtea2027/harum-assumenda-fugit-ipsam = BufferList()

// equivalent to:

const { BufferList } = require('@devtea2027/harum-assumenda-fugit-ipsam')
const @devtea2027/harum-assumenda-fugit-ipsam = new BufferList()
```

--------------------------------------------------------
<a name="isBufferList"></a>
### BufferList.isBufferList(obj)
Determines if the passed object is a `BufferList`. It will return `true` if the passed object is an instance of `BufferList` **or** `BufferListStream` and `false` otherwise.

N.B. this won't return `true` for `BufferList` or `BufferListStream` instances created by versions of this library before this static method was added.

--------------------------------------------------------
<a name="length"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.length
Get the length of the list in bytes. This is the sum of the lengths of all of the buffers contained in the list, minus any initial offset for a semi-consumed buffer at the beginning. Should accurately represent the total number of bytes that can be read from the list.

--------------------------------------------------------
<a name="append"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.append(Buffer | Buffer array | BufferList | BufferList array | String)
`append(buffer)` adds an additional buffer or BufferList to the internal list. `this` is returned so it can be chained.

--------------------------------------------------------
<a name="get"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.get(index)
`get()` will return the byte at the specified index.

--------------------------------------------------------
<a name="indexOf"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.indexOf(value[, byteOffset][, encoding])
`get()` will return the byte at the specified index.
`indexOf()` method returns the first index at which a given element can be found in the BufferList, or -1 if it is not present.

--------------------------------------------------------
<a name="slice"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.slice([ start, [ end ] ])
`slice()` returns a new `Buffer` object containing the bytes within the range specified. Both `start` and `end` are optional and will default to the beginning and end of the list respectively.

If the requested range spans a single internal buffer then a slice of that buffer will be returned which shares the original memory range of that Buffer. If the range spans multiple buffers then copy operations will likely occur to give you a uniform Buffer.

--------------------------------------------------------
<a name="shallowSlice"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.shallowSlice([ start, [ end ] ])
`shallowSlice()` returns a new `BufferList` object containing the bytes within the range specified. Both `start` and `end` are optional and will default to the beginning and end of the list respectively.

No copies will be performed. All buffers in the result share memory with the original list.

--------------------------------------------------------
<a name="copy"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.copy(dest, [ destStart, [ srcStart [, srcEnd ] ] ])
`copy()` copies the content of the list in the `dest` buffer, starting from `destStart` and containing the bytes within the range specified with `srcStart` to `srcEnd`. `destStart`, `start` and `end` are optional and will default to the beginning of the `dest` buffer, and the beginning and end of the list respectively.

--------------------------------------------------------
<a name="duplicate"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.duplicate()
`duplicate()` performs a **shallow-copy** of the list. The internal Buffers remains the same, so if you change the underlying Buffers, the change will be reflected in both the original and the duplicate. This method is needed if you want to call `consume()` or `pipe()` and still keep the original list.Example:

```js
var @devtea2027/harum-assumenda-fugit-ipsam = new BufferListStream()

@devtea2027/harum-assumenda-fugit-ipsam.append('hello')
@devtea2027/harum-assumenda-fugit-ipsam.append(' world')
@devtea2027/harum-assumenda-fugit-ipsam.append('\n')

@devtea2027/harum-assumenda-fugit-ipsam.duplicate().pipe(process.stdout, { end: false })

console.log(@devtea2027/harum-assumenda-fugit-ipsam.toString())
```

--------------------------------------------------------
<a name="consume"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.consume(bytes)
`consume()` will shift bytes *off the start of the list*. The number of bytes consumed don't need to line up with the sizes of the internal Buffers&mdash;initial offsets will be calculated accordingly in order to give you a consistent view of the data.

--------------------------------------------------------
<a name="toString"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.toString([encoding, [ start, [ end ]]])
`toString()` will return a string representation of the buffer. The optional `start` and `end` arguments are passed on to `slice()`, while the `encoding` is passed on to `toString()` of the resulting Buffer. See the [Buffer#toString()](http://nodejs.org/docs/latest/api/buffer.html#buffer_buf_tostring_encoding_start_end) documentation for more information.

--------------------------------------------------------
<a name="readXX"></a>
### @devtea2027/harum-assumenda-fugit-ipsam.readDou@devtea2027/harum-assumenda-fugit-ipsameBE(), @devtea2027/harum-assumenda-fugit-ipsam.readDou@devtea2027/harum-assumenda-fugit-ipsameLE(), @devtea2027/harum-assumenda-fugit-ipsam.readFloatBE(), @devtea2027/harum-assumenda-fugit-ipsam.readFloatLE(), @devtea2027/harum-assumenda-fugit-ipsam.readBigInt64BE(), @devtea2027/harum-assumenda-fugit-ipsam.readBigInt64LE(), @devtea2027/harum-assumenda-fugit-ipsam.readBigUInt64BE(), @devtea2027/harum-assumenda-fugit-ipsam.readBigUInt64LE(), @devtea2027/harum-assumenda-fugit-ipsam.readInt32BE(), @devtea2027/harum-assumenda-fugit-ipsam.readInt32LE(), @devtea2027/harum-assumenda-fugit-ipsam.readUInt32BE(), @devtea2027/harum-assumenda-fugit-ipsam.readUInt32LE(), @devtea2027/harum-assumenda-fugit-ipsam.readInt16BE(), @devtea2027/harum-assumenda-fugit-ipsam.readInt16LE(), @devtea2027/harum-assumenda-fugit-ipsam.readUInt16BE(), @devtea2027/harum-assumenda-fugit-ipsam.readUInt16LE(), @devtea2027/harum-assumenda-fugit-ipsam.readInt8(), @devtea2027/harum-assumenda-fugit-ipsam.readUInt8()

All of the standard byte-reading methods of the `Buffer` interface are implemented and will operate across internal Buffer boundaries transparently.

See the <b><code>[Buffer](http://nodejs.org/docs/latest/api/buffer.html)</code></b> documentation for how these work.

--------------------------------------------------------
<a name="ctorStream"></a>
### new BufferListStream([ callback | Buffer | Buffer array | BufferList | BufferList array | String ])
**BufferListStream** is a Node **[Duplex Stream](http://nodejs.org/docs/latest/api/stream.html#stream_class_stream_duplex)**, so it can be read from and written to like a standard Node stream. You can also `pipe()` to and from a **BufferListStream** instance.

The constructor takes an optional callback, if supplied, the callback will be called with an error argument followed by a reference to the **@devtea2027/harum-assumenda-fugit-ipsam** instance, when `@devtea2027/harum-assumenda-fugit-ipsam.end()` is called (i.e. from a piped stream). This is a convenient method of collecting the entire contents of a stream, particularly when the stream is *chunky*, such as a network stream.

Normally, no arguments are required for the constructor, but you can initialise the list by passing in a single `Buffer` object or an array of `Buffer` object.

`new` is not strictly required, if you don't instantiate a new object, it will be done automatically for you so you can create a new instance simply with:

```js
const { BufferListStream } = require('@devtea2027/harum-assumenda-fugit-ipsam')
const @devtea2027/harum-assumenda-fugit-ipsam = BufferListStream()

// equivalent to:

const { BufferListStream } = require('@devtea2027/harum-assumenda-fugit-ipsam')
const @devtea2027/harum-assumenda-fugit-ipsam = new BufferListStream()
```

N.B. For backwards compatibility reasons, `BufferListStream` is the **default** export when you `require('@devtea2027/harum-assumenda-fugit-ipsam')`:

```js
const { BufferListStream } = require('@devtea2027/harum-assumenda-fugit-ipsam')
// equivalent to:
const BufferListStream = require('@devtea2027/harum-assumenda-fugit-ipsam')
```

--------------------------------------------------------

## Contributors

**@devtea2027/harum-assumenda-fugit-ipsam** is brought to you by the following hackers:

 * [Rod Vagg](https://github.com/rvagg)
 * [Matteo Collina](https://github.com/mcollina)
 * [Jarett Cruger](https://github.com/jcrugzz)

<a name="license"></a>
## License &amp; copyright

Copyright (c) 2013-2019 @devtea2027/harum-assumenda-fugit-ipsam contributors (listed above).

@devtea2027/harum-assumenda-fugit-ipsam is licensed under the MIT license. All rights not explicitly granted in the MIT license are reserved. See the included LICENSE.md file for more details.
