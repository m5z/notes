# Effective Python

## Pythonic Thinking
* Prefer `if a is not b` over `if not a is b`
* Don't check for empty containers by checking length
* Always use f-strings to format text
* DRY - write helper functions instead of squeezing stuff in one line
* Use `enumerate`
* Use `zip`
* `for/else` - don't use it
* Assignment expression - walrus operator `:=`
  * Use in `if`
  * Use for `do/while`
  * Emulate `switch` statements

## Lists and Dictionaries
* Avoid striding and slicing in a single expression
* Catch-all unpacking: `a, b, *rest = [1, 2, 3, 4]`
  * Prefer over slicing and indexing
* Python implements stable sorting, can be used instead sorting tuples
* Starting with Python 3.6, order in dictionaries is preserved
* Missing values in `dict`
  * Use `dict.get()` to handle missing values
    * Works well with assignment operator `:=`
  * `setdefault` in `dict` sets a value if key is not present and returns it
    * Careful if the default value has high init cost or can raise exceptions
  * Prefer `defaultdict` over `setdefault`
  * Inherit from `dict` and implement `__missing__()` for more advanced cases

## Functions
* Don't unpack more than 3 multiple-return values from a function
  * If it's still needed, use a lightweight class or namedtuple
* Don't assign any special meaning to `None` returned by functions
  * Raise exceptions to indicate special situations instead
* Know how closures interact with variable scope
  * If the use of `nonlocal` gets complicated, write a class instead
  * Class with a `__call__(self, ...)` method can act as a function
* Always specify optional arguments using keyword names, instead positions
* Use docstrings to document default arguments (where they equal `None`)
* Use keword-only arguments (`*` in arguments)
* Use positional-only arguments (`/` in arguments) - since Python 3.8
* Use `functools.wraps` for defining decorators

## Comprehensions and Generators
* Use comprehensions (list, dict, set) over `map` and `filter`
* Avoid using more than two control subexpressions in a comprehension
* Assignment operator used in value part of comprehension leaks the variable (?)
* Implement iterator protocol by implementing `__iter__` as a generator
  * `iter()` on an iterator returns itself
  * `iter()` on a container returns a new iterator each time
* Generators can be cascaded
  * But be careful to use each generator only once, as they are stateful
* Use `yield from` to yield all values from generator before returning control
* Use `timeit` for quick benchmarks
* You can send values into an iterator using `it.send()`, but don't use it
  * Better use iterator as an input
* You can re-raise exceptions in an iterator using `it.throw()` - don't use it
  * Better implement iterator protocol
* Use `itertools` to work with iterators (`help(itertools)`)
  * linking together
    * `chain` - combine iterators
    * `repeat` - repeat single value endlessly or `n` amount times
    * `cycle` - cycle an iterator
    * `tee` - split an iterator into `n` parallel iterators
    * `zip_longest` - zip iterators with placeholder values if one finishes
  * filtering
    * `islice` - slicing and striding on iterators
    * `takewhile` - returns items until a predicate returns `False`
    * `dropwhile` - drops items until a predicate returns `True`
    * `filterfalse` - opposite of `filter`
  * combining
    * `accumulate` - similar to `reduce`, but returns items one at a time
    * `product` - Cartesian product iterator
    * `permutations` - `n` length permutations iterator
    * `combinations` - `n` length unrepeated combinations iterator
    * `combinations_with_replacement` - same as above, but repeated

## Classes and Interfaces
* Write classes instead of multiply-nested data structures
* Use `namedtuple`, but be careful if it's external API
* Use `@classmethod` and `cls` to define alternative constructors
* Use `super().__init__()` to run constructors in method resolution order
  * solves diamond inheritance
  * `Class.mro()` returns the MRO
* Consider using mix-ins over multiple inheritance with instance attributes
* Use private atrributes only to avoid naming conflicts with subclasses
* Use `collections.abc` classes to construct custom container types

## Metaclasses and Atributes
* Avoid getter and setter methods
  * in simple cases, use regular fields
  * in complex cases, change fields to `@property`/`@property.setter`
* Reuse behavior of `@property` methods with descriptor classes
  * define a class implementing `__get__` and `__set__`
  * remember descriptor instances are shared, use `WeakKeyDictionary`
* Use `__getattr__` and `__getattribute__` for lazy loading
  * Can be used in schemaless classes
  * `__getattr__` is called only if attribute doesn't exist
  * `__getattribute__` is always called when accessing an attribute
  * Use `setattr` and `hasattr` built-in functions
* Use `__setattr__` to do actions when setting values
* Metaclasses can be used for subclass validation
  * Inherit from `type`
  * Define `def __new__(meta, name, bases, class_dict)`
  * Use `metaclass=` in subclass definition
  * Can use `__init_subclass__(cls)` for simplification
  * `__init_subclass__` supports multiple inheritance
* Use `__init_subclass__` or metaclasses for class registration
* Use `__set_name__` for name of field corresponding to descriptor
* Use class decorators to modify class methods and attributes

## Concurrency and parallelism
* Concurrency - running seemingly in parallel
* Use `subprocess` module to run child processes from command line
  * `run` for simple usage, `Popen` for advanced
* Use Python threads with blocking I/O
* Use `threading.Lock` for mutex
* Use `Queue` class for building processing pipelines
  * Handles non-busy waiting, stopping workers, buffers, joining
* `fan-out` - spawning concurrent execution for each unit of work
  * `fan-in` - waiting concurrent lines of execution to finish
* Threads:
  * require locking for coordination
  * require around 8MB per executing thread
  * take long time to start and to context-switch
  * don't re-raise exceptions to starter
* One does not simply use `Thread`s for fan-in and fan-out
  * `Queue` can be used (improves scalability), but it's complicated
  * Consider `ThreadPoolExecutor` for concurrency/parallelism
* Use coroutines for concurrent I/O
  * `async`, `await`, `asyncio.run`, `asyncio.gather`
  * Python provides asynchronous versions of many language elements
  * Use `run_in_executor` to run synchronous functions from coroutines
  * Use `run_until_complete` to run coroutine in synchronous code
  * System calls can slow down main event loop
  * Use `debug=True` in `asyncio.run` to debug the event loop
* Consider `concurrent.futures.ProcessPoolExecutor` for true parallelism
  * Try it before resorting to `multiprocessing`
