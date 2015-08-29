# jslzjb
Automatically exported from https://code.google.com/p/jslzjb/, now archived at https://code.google.com/archive/p/jslzjb/

## My particular use case

The online hex editor [HexEd.it](https://hexed.it/) supports Base64+LZJB-encoded URLs to load data into it.

This particular LZJB generates the same bytestream as HexEd.it expects.  Others, such as https://github.com/cscott/lzjb, do not.

Note that `Iuppiter.js`' Base64 implementation appears to be incompatible, incorrect, or buggy; you should use `btoa()`, e.g.:

```javascript
// "bytes" is an array of integers representing byte values 0-255
var compressed = Iuppiter.compress(bytes);
var data_cmp = new Uint8Array(compressed);
var b64encoded = btoa(String.fromCharCode.apply(null, data_cmp));
var hexedit = 'https://hexed.it#base64+lzjb:' + b64encoded;
```

Working with integer arrays can be kinda gross.  https://github.com/copy/jslzjb-k is a fork that works exclusively with `Uint8Array`s, is usually 2-3x faster in my tests, and still generates the correct bytestream for `btoa()`.

## Original description

Javascript port of the LZJB compression algorithm (http://en.wikipedia.org/wiki/LZJB).

It makes possible to deal with or submit large data to server side. We tested many compression implementation in javascript, finally found that LZJB gets better performance and easy to implement.

For client side compression, performance is very critial. We use LZJB algorithm to do that, it suprisingly fulfills our requirement to compress string fastly and efficiently.

(If your server side is using python, you can see http://code.google.com/p/pylzjb/)

### Original example (QUnit test case)

```javascript
test('jslzjb', function() {
    var s = "Hello World!!!Hello World!!!Hello World!!!Hello World!!!";
    for(var i = 0; i < 10; i++)
        s += s;
        
    var c = Iuppiter.compress(s);
        ok(c.length < s.length, c);
        
    var d = Iuppiter.decompress(c);
    ok(d == s, d);
    
    // Compressed byte array can be converted into base64 to sumbit to server side to do something.
    var b = Iuppiter.Base64.encode(c, true);
        
    var bb = Iuppiter.toByteArray(b);
    var db = Iuppiter.decompress(Iuppiter.Base64.decode(bb, true));
    ok(db == s, db);
})
```

## License

$Id: Iuppiter.js 3026 2010-06-23 10:03:13Z Bear $

Copyright (c) 2010 Nuwa Information Co., Ltd, and individual contributors.
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

  1. Redistributions of source code must retain the above copyright notice,
     this list of conditions and the following disclaimer.

  2. Redistributions in binary form must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  3. Neither the name of Nuwa Information nor the names of its contributors
     may be used to endorse or promote products derived from this software
     without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

$Author: Bear $
$Date: 2010-06-23 18:03:13 +0800 (星期三, 23 六月 2010) $
$Revision: 3026 $
