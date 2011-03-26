Doug Crockford  
Google Tech Talks  
February, 2009  

[](http://www.youtube.com/watch?v=hQVTIJBZook)  
[](http://googlecode.blogspot.com/2009/03/doug-crockford-javascript-good-parts.html)

Always use `===` instead of `==` as `===` doesn't do type coercion, which leads to weird comparisons around false, 'false', 0, etc. Also, prefer `!==`.

Recommends always using curly braces on if statements.

Floating point arithmetic is fail, as if you didn't run into that before.

He doesn't use `++` or `--` because in his experience he finds them tricky.

Never intentionally fall through in a switch statement.

Good parts:
* Lambda - best thing ever in language
* Dynamic objects
* Loose typing
* [Object literals](http://www.dyn-web.com/tutorials/obj_lit.php)

[Prototypal school of inheritance](http://javascript.crockford.com/prototypal.html). No classes, but objects which inherit from other objects. Delegation is a link to other objects containing only the difference between the two.

Power constructor ([more on inheritance from Crockford](http://javascript.crockford.com/inheritance.html)):

    function myPowerConstructor(x) {
      var that = otherMarker(x);
      var secret = f(x);
      that.priv = function() {
        ... secret x that ...
      }
      return that;
    }

We have closure. A function object contains:
* a function (name, params, body)
* a refernce to the environment in which it was created (context)

Bad thing leading to forced style:

    return
    {
      ok: false
    };
    // silent error - leading { must follow return.
    // Otherwise automatic semicolon insertion in JS
    // gets in our way and a ; is inserted after the return.
    
JSON became a standard on his say so.

[jslint.com](http://jslint.com/) makes you more confident you are using the good parts.

Story behind why JSON has quoted params. Because JS disallows reserved words in the key position of an object literal (even though that shouldn't matter). On top of that, many JS reserved words are reserved unnecessarily.

"It's easy to make things bigger. It's harder to make things better."