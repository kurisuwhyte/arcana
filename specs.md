Arcana
======

A minimal Lisp-1 dialect featuring dynamic typing, static scoping and proper
closures.


## 1) Overview

Arcana is a minimal Lisp-1 dialect meant to be easily implementable in any
sufficiently powerful language (and implementable in the rest). It has five
data types:

  - Number  (double-precision floats)
  - Boolean
  - String
  - List
  - Lambda

Besides this, Arcana is pure, statically scoped, dinamically typed and uses
call-by-value.


## 2) Semantics

All values in Arcana are immutable. Efficient data structures are trivial to
implement given the set of built-in data types in the language.

Arcana uses static scoping. Lambda constructs and `let` constructs define
binding regions. There are no macros, thus there's no way to abstract the
built-in operatives or particular patterns in the source code — shouldn't be
the goal in the language, anyways. Once set, a binding can never be changed,
but it can be used as the basis of another computation, where the value is
stored in a different binding.

Lambdas are automatically curried. All functions accept a fixed number of
formal parameters. There's no support for either variadic application or
keyword application. Functions are invoked using `call-by-value`, although
since the language is pure, no copying needs to be made.

Nil and false are the only falsy values, all the others are true.


## 4) Standard library

    (nil? a)                    Checks if a value is Nil
    (list? a)                   Checks if a value is a List
    (number? a)                 Checks if a value is a number
    (string? a)                 Checks if a value is a string
    (boolean? a)                Checks if a value is a boolean
    (lambda? a)                 Checks if a value is a lambda

    (not a)                     Returns the opposite of a

    (= a b)                     Structural equality
    (> a b)                     Number a is greater than b
    (< a b)                     Number a is lesser than b
    (>= a b)                    Number a is greater or equal to b
    (<= a b)                    Number a is lesser or equal to b

    (+ a b)                     Addition
    (- a b)                     Subtraction
    (/ a b)                     Division
    (* a b)                     Multiplication
    (mod a b)                   Modulo

    (cons a b)
    (concat a b)                Concats two lists
    (head a)                    Car of a cons cell
    (tail a)                    Cdr of a cons cell
    (reduce f i a)              Right fold a using f, starting from i
    

## 5) Formal syntax

```hs
comment :: ";" (anything but EOL)

digit  :: "0" ... "9"
sign   :: "-" | "+"
digits :: digit+
number :: sign? digits ("." digits)

char         :: "\\" anything
stringEscape :: "\\\""
stringChar   :: (not stringEscape | "\"") anything
string       :: "\"" stringChar* "\""
symbol       :: ":" (not whitespace)+

brackets  :: "(" | ")"
symbols   :: "!" | "@" | "#" | "$" | "%" | "&" | "*" | "-" | "_"
           | "=" | "+" | "^" | "~" | "?" | "/" | ">" | "<"
letter    :: "a" .. "z" | "A" .. "Z"
nameStart :: symbols | letter
nameRest  :: symbols | letter | digit
name      :: nameStart nameRest*

cons :: "(" expr "." expr ")"
list :: "(" expr* ")"

lambda     :: "(" "lambda" lambdaArgs expr* ")"
lambdaArgs :: "(" name* ")"

define :: "(" "define" name expr ")"

if  :: "(" "if" expr expr expr? ")"
and :: "(" "and" expr* ")"
or  :: "(" "or" expr* ")"


values  :: number | char | name | symbol | string | cons | list
         | lambda | if | and | or
expr    :: define | value
program :: expr*
```
