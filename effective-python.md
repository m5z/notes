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
* Don't assign any special meaning to None returned by functions
  * Raise exceptions to indicate special situations instead
* Know how closures interact with variable scope
  * If the use of `nonlocal` gets complicated, write a class instead
  * Class with a `__call__(self, ...)` method can act as a function
* Always specify optional arguments using keyword names, instead positions
* Use keword-only arguments (`*` in arguments)
* Use positional-only arguments (`/` in arguments) - since Python 3.8
* Use `functools.wraps` for defining decorators
