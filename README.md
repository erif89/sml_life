A simple, naive implementation of Conway's Game of Life in
Standard ML, Python, Clojure, Haskell.

These are not optimal or well-formed solutions to this problem.
They are just the first thing I hacked up. I am just learning
Standard ML, and the Python implementation is pretty much
a direct port of the Standard ML code with minor changes.

Here are the benchmark results for 100000 iterations of life on
a small, 5x5 grid with a little glider flying alone:

Standard ML compiled with MLton:

0.716 seconds (compiled)

Python run with CPython 2.7.2:

26.808 seconds

Python run with PyPy 1.7:

6.415 seconds

Clojure 1.3:

8.561 seconds (not counting JVM startup / Clojure bootstrap)

Haskell with GHC 7.0.3:

28.117 seconds (compiled with -O2 using [Int])
11.935 seconds (compiled with -O2 using UArray Int Int and a single
strictness hint)

Chicken Scheme 4.7.0

15.494 seconds (compiled with -O4)

Stalin Scheme

1.358 seconds (compiled with -On -copt -O2)

So the SML/MLton version is about 9 times faster than PyPy and 37
times faster than CPython. Clojure comes in a close 4th, just a bit
slower than PyPy. Haskell time is comparable with CPython, when compiled
with -O2 using a normal list. I switched out the list for an Unboxed
Array which shaved a few seconds off, and then with a bunch of reading
and experimentation found a single strictness hint that cut the time
roughly in half down to 11.935 seconds which is in the same order though
slower than Clojure and PyPy.

After implementing the solution in Chicken Scheme, utilizing srfi-1 and
srfi-4 to get some functions like "fold" and homogenous vectors, I found
a highly optimizing Scheme compiler called Stalin. It's R4RS compliant,
only, so I had to adapt the code, which just meant implementing "fold"
and "subvector" and using generic vectors. Stalin truly does optimize
brutally, eclipsed only by MLton (by a factor of 2) and being the fastest
dynamic language implementation.

TODO:

Numpy
Other Scheme impl
PyPy 1.8
