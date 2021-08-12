# Clean Code in Python

## General Traits of Good Code
* Design by contract
  * Preconditions, postconditions, invariants, side effects
  * If error occurs, it has to be easy to spot
* Defensive programming
  * Use `raise <e> from <original_exception>` to wrap exceptions
  * Use assertions for seemingly impossible conditions 
* Separation of concerns
  * Well-defined software will achieve high cohesion and low coupling.
* 

## SOLID Principles
* SOLID
  * S: Single responsibility principle
  * O: Open/closed principle
    * Open for extension, closed for modification
    * You don't have to modify the existing code too much when adding new things
  * L: Liskov's substitution principle
    * If S is a subtype of T, then objects of type T may be replaced by objects of type S, without breaking the program
  * I: Interface segregation principle
    * Interfaces should be small
  * D: Dependency inversion principle
    * Define abstractions and parametrize constructors
    * You can use `pinject` for dependency injection

## Unit Testing and Refactoring
* Unit tests traits:
  * isolation
  * performance
  * repeatability
  * self-validating
* `pytest`
  * `pytest.raises` allows `match` parameter to check for exception message
  * You can stack parametrizations in pytest to produce a cartesian product
* Property-based testing
  * Helps to find scenarios that will make code fail
  * `hypothesis` library
* Mutation testing
  * Production code is modified
  * Tests are checked if they fail
  * `mutpy` library

## Common Design Patterns
* Some of the patterns are embedded in Python
* Creational patterns
  * Factories
    * Not really needed in Python
    * We can use libraries like `pyinject`
  * Singleton and shared state (monostate)
    * Usually a bad practice, avoid as much as possible
    * The easiest way is by using a module
    * Shared state - several instances that synchronize state
      * Can be done with class attributes
      * Or descriptors
      * Borg pattern: store a class-level dictionary with all attributes
  * Builder
* Structural patterns
  * Adapter
  * Composite
  * Decorator
    * A set of objects with the same method
  * Facade
    * We can put them in `__init__.py` to act as an interface
* Behavioral patterns
  * ...
