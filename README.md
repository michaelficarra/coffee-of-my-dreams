**NOTE:** <em>The changes listed here are not included in, but instead dependent upon
[my Kickstarter project](http://www.kickstarter.com/projects/1182995593/make-a-better-coffeescript-compiler).
The project has been funded and will allow me to make a fork that implements these changes once it is completed.</em>

I would like this language to remain as close to a superset of coffeescript as possible, unlike the ever-diverging
[Coco](https://github.com/satyr/coco). This means keeping the *incompatible change* list very small, which I believe I
have done.

# Full Change List

## incompatible changes
* newer, awesome class syntax/semantics ([#1207](https://github.com/jashkenas/coffee-script/issues/1207),
  [#640](https://github.com/jashkenas/coffee-script/issues/640#issuecomment-376129),
  [strawman:maximally_minimal_classes](http://wiki.ecmascript.org/doku.php?id=strawman:maximally_minimal_classes))
  * constructor is the unique, unnamed top-level function
  * context of class body is the prototype (which makes `@@` the constructor, see below)
  * regular assignments to @-vars for method definitions
* change comprehension syntax to {python,harmony}-style `[... for x in y]`
  ([harmony:array_comprehensions](http://wiki.ecmascript.org/doku.php?id=harmony:array_comprehensions),
  [#77](https://github.com/jashkenas/coffee-script/issues/77),
  [#2030](https://github.com/jashkenas/coffee-script/issues/2030),
  [#1191](https://github.com/jashkenas/coffee-script/issues/1191))
* bare `super` should not imply a call; it should simply be a reference to `ClassName.__super__.constructor`
* lower precedence of infix operators to allow for more paren-free invocations
  ([unfinished discussion with Jeremy](http://irclogger.com/.coffeescript/2012-04-04#1333551786))
  * also add a super-low-precedence application operator: `$` in Haskell, `<|` in LiveScript
* spaced (or newline-separated) member access to close implicit calls
  ([#1495](https://github.com/jashkenas/coffee-script/issues/1495))
* remove inline JS
* block comments are *comments*, don't pass them through to the compilation target
* `in` operator should perform an `egal` comparison, not strict equality comparison
* disallow references to magic `arguments` variable (requires as-patterns, see below)

## compatible changes
* default output indentation style determined by input indentation style
* fix loops ([@int3's blog post](http://discontinuously.com/2012/05/iteration-in-coffeescript/),
  [#1952](https://github.com/jashkenas/coffee-script/issues/1952),
  [#1208](https://github.com/jashkenas/coffee-script/issues/1208))

## additions
+ partially-applied operators and member accesses ([gkz/LiveScript@37ef73b7](https://github.com/gkz/LiveScript/commit/37ef73b702c32263aeba9bdd3ebadc3823fd5eda), [gkz/LiveScript#41](https://github.com/gkz/LiveScript/issues/41))
+ unary operators are functions ([gkz/LiveScript@c13b8a60](https://github.com/gkz/LiveScript/commit/c13b8a60564c16b78fd82548c01b092724f7e476))
+ backcalls, a flat call/cc via `<-`:
  + `<- fn a; ...; b` to `fn(a, function(){ ...; return b; })`
  + `a <- fn b; ...; c` to `fn(b, function(a){ ...; return c; })`
  + `(a, b) <- fn c, <&>, d; ...; e` to `fn(c, function(a, b){ ...; return e; }, d)`
  + preserve `this` by rewriting references as we do in bound functions
+ explicit tail calls via `recur`, [as in clojure](http://clojure.org/special_forms#Special%20Forms--\(recur%20exprs*\))
+ stepped ranges: `[0..8 by 2]` ([#835](https://github.com/jashkenas/coffee-script/issues/835))
+ Haskell's [as-patterns](http://www.haskell.org/tutorial/patterns.html): `o@{p0: a@[b, c]} = obj`, `fn = (args@[a, b, c]...) ->`
+ Haskell's function-infixing via backticks (kinda [#915](https://github.com/jashkenas/coffee-script/issues/915),
  [#1429](https://github.com/jashkenas/coffee-script/issues/1429),
  [gkz/LiveScript@fb548f23](https://github.com/gkz/LiveScript/commit/fb548f23df6273c4fc6ca4359cd8e1ee93ce42a1))
+ `%%` (mod) and `//` (integer division) operators ([#1971](https://github.com/jashkenas/coffee-script/issues/1971))
+ min/max operators (satyr/coco): `<?`, `>?`
+ sugar for accessing the last element of an array-like, syntax TBD ([#156](https://github.com/jashkenas/coffee-script/issues/156))
+ `@@` as sugar for `@constructor`
+ `@0`, `@1`, etc. for `this[0]`, `this[1]`, etc.
+ `a.0.1` for `a[0][1]` ([#918](https://github.com/jashkenas/coffee-script/issues/918),
  [#1334](https://github.com/jashkenas/coffee-script/issues/1334))
+ underscore in number literals; postfix alphabetic comments: `15_550km`
  ([#632](https://github.com/jashkenas/coffee-script/issues/632),
  [#857](https://github.com/jashkenas/coffee-script/issues/857),
  [#913](https://github.com/jashkenas/coffee-script/issues/913))
+ quote-word (`<[ word1 word2 ]>`) ([#582](https://github.com/jashkenas/coffee-script/issues/582),
  [#1211](https://github.com/jashkenas/coffee-script/issues/1211), others)
+ unary `::` operator ([#1220](https://github.com/jashkenas/coffee-script/issues/1220))
+ allow identifiers that are reserved words in JS but not coffee through use of unicode escape sequences
  ([#1452](https://github.com/jashkenas/coffee-script/issues/1452))
+ `{foo.bar}` as shorthand for `{bar: foo.bar}` ([#1089](https://github.com/jashkenas/coffee-script/issues/1089))
+ quoted member access (`a."b-c"` is `a["b-c"]`) to reserve postfix `[]` for indicating nondeterministic member access
+ unnamed splats in array destructuring and function parameters
  ([#870](https://github.com/jashkenas/coffee-script/issues/870))
+ single-value-skipping `null` syntax ([#870](https://github.com/jashkenas/coffee-script/issues/870))
+ Ruby's [`=~` regexp matching operator](http://ruby-doc.org/core/String.html#method-i-3D-7E) for the convenience of
  Ruby-ers ([#1651](https://github.com/jashkenas/coffee-script/issues/1651),
  [#1653](https://github.com/jashkenas/coffee-script/issues/1653),
  [#115](https://github.com/jashkenas/coffee-script/issues/115))
+ call superclass's `extended` method on extension ([#710](https://github.com/jashkenas/coffee-script/issues/710),
  [#841](https://github.com/jashkenas/coffee-script/issues/841#issuecomment-1300193),
  [gist:612786](https://gist.github.com/612786))
+ yada-yada ([#1142](https://github.com/jashkenas/coffee-script/issues/1142), satyr/coco)
+ `{+flag}`, `{-flag}` ([#885](https://github.com/jashkenas/coffee-script/issues/885))
+ (possibly) scheme-style argument accessor (if I can resolve ambiguity with `a <b> c` somehow)
  ([#739](https://github.com/jashkenas/coffee-script/issues/739))
