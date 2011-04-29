## change
* compile to strings as late as possible (still need to work out proper rules) (#908, #1011, #1072)
* newer, awesome class syntax/semantics (#1207)
* change/restrict comprehensions to python-style `[for ... in ...]` (kinda #1191)
* output indentation style == input indentation style
* fix parameter lists issue from #1007 (make temp vars)
* only use semicolons before `[` and `(` on new lines; omit everywhere else

## remove
- block comments
- inline JS (really, it's completely unnecessary)
- YAML-style object literals (for now; these complicate the shit out of the language)
- destructuring in list comprehensions (to allow for #866, mentioned below)
- disallow `arguments`, `eval` in parameter lists (#1007)
- disallow arguments with identical names (#1002)
- disallow top-level literals and other obvious errors (#1066, #1069, #1240)

## add
+ Haskell's function-infixing via backticks (kinda #915)
+ unnamed splats in array destructuring and function parameters (#870)
+ single-value-skipping `null` syntax (#870)
+ shorthand proposed by #1089
+ implement syntax from #866
+ force matching indents/outdents (#689, #1275, others)
+ allow `@0`, `@1`, etc. for `this[0]`, `this[1]`, etc.
+ allow `a.0.1` for `a[0][1]` (#918)
+ unary `::` operator (#1220)
+ call superclass's `extended` on extension (#710, gist:612786)
+ underscore in number literals (#632, others)
+ quote-word (<[ word1 word2 ]>) (#582, #1211, others)
+ yada-yada (#1142)
+ ES5 shims like Function::bind when they may not exist
+ (possibly) add Function::new (gist:612786)
+ (possibly) scheme-style argument accessor (if I can resolve ambiguity with `a <b> c` somehow) (#739)
+ (possibly) {+flag}, {-flag} (see #885)