# Clean Code in Python

## General Traits of Good Code
* Design by contract
  * Preconditions, postconditions, invariants, side effects
  * If error occurs, it has to be easy to spot
* Defensive programming
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
