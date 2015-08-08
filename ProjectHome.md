Javascript port of the LZJB compression algorithm (http://en.wikipedia.org/wiki/LZJB).

It makes possible to deal with or submit large data to server side. We tested many compression implementation in javascript, finally found that LZJB gets better performance and easy to implement.

For client side compression, performance is very critial. We use LZJB algorithm to do that, it suprisingly fulfills our requirement to compress string fastly and efficiently.

# Getting Started #
## Example (QUnit test case) ##
```
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

(if your server side is using python, you can see http://code.google.com/p/pylzjb/).