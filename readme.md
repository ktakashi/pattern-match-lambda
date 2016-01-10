# pattern-match-lambda for R7RS-Scheme

## Introduction

This package is easy pattern match library for R7RS.
It provides only one macro named `pattern-match-lambda`.

## Install

This package doesn't contain install scripts.
The installation procedure depends on the Scheme implementation you're using.
Please see document of the implementation.

## Syntax
A `pattern-match-lambda`'s syntax is the following.

```scheme
(pattern-match-lambda (<pattern literal> ...) <clause> ...)
```

<clause> must have one of the following forms:

- ```(<pattern> <expr>)```
- ```(<pattern> <fender> <expr>)```

A <pattern> is either an identifier, a constant, or one of the followings.

```scheme
(<pattern> ...)
(<pattern> <pattern> ... . <pattern>)
#(<pattern> ...)
```

A <fender> is an expression which is evaluated when <pattern> is matched.
If the result of evaluation is true value, then the following <expr> is
evaluated, otherwise `pattern-match-lambda` continues matching. The following
example shows how it works:

```scheme
(define foo
  (pattern-match-lambda ()
    ((a) (eq? a 'fender) 'fender)
    ((a) 'fallback)))

(foo 'fender) ;; -> fender
(foo 'a)      ;; -> fallback
```

## Semantics

A `pattern-match-lambda` expression evaluates to a procedure that accepts a
variable number of arguments and is lexically scoped in the same manner as 
a procedure resulting from a lambda expression. When the procedure is called,
the first <clause> for which the arguments match with <pattern> is selected, 
where argument is specified as for the <pattern> of a syntax-rules like 
expression.

Difference between <pattern> of `syntax-rules` and `pattern-match-lambda` is 
ellipsis. Ellipsis is not able to use in pattern-match-lambda's <pattern>.

The variables of <pattern> are bound to fresh locations, the values of the 
arguments are stored in those locations, the <body> is evaluated in the 
extended environment, and the results of <body> are returned as the results 
of the procedure call. It is an error for the arguments not to match with 
the <pattern> of any <clause>.

An identifier appearing within a <pattern> can be an underscore (`_`), a 
literal identifier listed in the list of <pattern-literal>. All other 
identifiers appearing within a <pattern> are variables.

Variables in <pattern> match arbitrary input elements and are used to refer 
to elements of the input in the body. It is an error for the same variable 
to appear more than once in a <pattern>. Underscores also match arbitrary 
input elements but are not variables and so cannot be used to refer to those 
elements. If an underscore appears in the <pattern literal> list, then that 
takes precedence and underscores in the <pattern> match as literals. Multiple 
underscores can appear in a <pattern>.

Identifiers that appear in (<pattern literal> ...) are interpreted as literal 
identifiers to be matched against corresponding elements of the input. An 
element in the input matches a literal identifiers if and only if it is an 
symbol and equal to literal identifier in the sense of the eqv? procedure.

## License

Copyright (c) 2014 SAITO Atsushi

Redistribution and use in source and binary forms, with or without 
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, 
   this list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice, 
   this list of conditions and the following disclaimer in the documentation 
   and/or other materials provided with the distribution.
3. Neither the name of the authors nor the names of its contributors may be 
   used to endorse or promote products derived from this software without 
   specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" 
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE 
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE 
ARE DISCLAIMED.
IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY 
DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES 
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; 
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND 
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT 
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF 
THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
