About Glop
----------
This project implements a simple lisp interpreter in Go.

It does not try to provide a full lisp/scheme implementation. This is mostly a toy project; the only focus so far was to try to have some error detection and reporting, something which is often overlooked in parsers&interpreters tutorials.

It tries to map as closely as possible to Go, relying on it when possible for most mechanisms.

The initial code was inspired by [Lispy by Peter Norvig](http://norvig.com/lispy.html) - though the actual code has nothing in common and does not try to be minimalistic.

Running
-------

All commands run from the base directory of the repository.

* To run the REPL:
```bash
go run glop.go
```

* Or you can execute a glop file:
```bash
go run glop.go example.glop
```

* To run all tests:
```bash
go test ./...
```

Features
--------

File `example.glop` shows a few examples.

Available list of functions is in `runtime/runtime.go`, in the `NewContext` function. Currently:

* Basic lisp:
  * `(begin a...)`: Evaluate each parameters sequentially.
  * `(quote a)`: Return `a` without evaluating it. Alias: `'a`.
  * `(define id a)`: Define `id` to be equal to the evaluation of `a`. Alias to `set!`.
  * `(set! id a)`: Define `id` to be equal to the evaluation of `a`. Alias to `define`.
  * `(if a b c)`: Eval `a`; if true, evaluate `b`, otherwise evaluate `c`.
  * `(lambda a b)`: Define a function taking the list of parameters `a`, and evaluating `b` with those parameters when called.
  * `(cons a b)`: returns `[a] + b`.
  * `(car a)`: returns `a[0]`.
  * `(cdr a)`: returns `a[1:]`.

* Constants: `true`, `false`

* Integers and float numbers are supported, but scientific notation is not.

* Operators: `==`, `!=`, `<`, `<=`, `>`, `>=`, `+`, `-`, `*` ; they look at the
  first argument to see whether they are operating on integer or floats.

* Misc:
  * `(print a ...)`: Eval and print to stdout `a` and other parameters.
  * `(length a)`: Eval `a` and return the length of value.
  * `(type a)`: returns the type of `a`.
  * `(eval a)`: Evaluate `a`.
  * `(panic)`: Triggers a panic.


Example:
```
In [0]: (define area (lambda (r) (* 3.141592653 (* r r))))
Out [0]: <runtime.Internal>(runtime.Internal)(0x457d00)
In [1]: (area 3.0)
Out [1]: <float64>28.274333877
In [2]: (define fact (lambda (n) (if (<= n 1) 1 (* n (fact (- n 1))))))
Out [2]: <runtime.Internal>(runtime.Internal)(0x457d00)
In [3]: (fact 10)
Out [3]: <int64>3628800
```


Project license
---------------
This project is under the Apache License version 2.0.

If not explicitely mentionned otherwise, all files in this project are under those terms:

    Copyright 2014 Pierre Palatin

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
