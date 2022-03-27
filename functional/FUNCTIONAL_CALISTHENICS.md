# What is functional/object calisthenics?
- "rules of thumb" that will help push your code into good object-oriented shape.

# Object Calisthenics Rules
## one level of indentation per method
- use `Extract Method` feature of your IDE to pull out behaviors until your methods only have 1 level of indentation
  - it would be easier/more trivial to match its implementation to its name. Determining the existence of bugs in these much smaller snippets is frequently much easier.
- If you've got nested control structures in a method, you're working at multiple levels of abstraction, and that means you're doing more than thing.
## don't use the else keyword
- it's harder to use polymorphism with else keyword
- try to use Null Object pattern instead.
## Wrap all primitives and Strings
- to express the intent better
## first class collections
- any class that contains a collection should contain no other member variables.
- maybe filters for the collection become part of the new class
- maybe new class can handle activities like joining 2 groups together or applying a rule to each element of the group
## one dot per line
- if you've got more than 1 dot on any given line of code, the activity is happening in the wrong place.
- the multiple dots indicate that you're violating encapsulation. Try asking that object to do something for you, rather than poking around its insides.
## don't abbreviate
- abbreviations can be confusing and they tend to hide larger problems
- is that because you're typing the same word over and over again? perhaps, your method is used too heavily and you're missing opportunities to remove duplication.
- long method name might be a sign of a misplaced responsibility, or a missing class
- try to keep class and method names to 1-2 words, and avoid names that duplicate the context. If the class is an `Order`, the method doesn't need to be called `shipOrder()`
## keep all entities small
- no class over 50 lines and no package over 10 files
  - 50 lines classes have the added benefit of being visible on one screen without scrolling, which makes them easier to grasp quickly.
  - packages, like classes, should be cohesive and have a purpose.
## no classes, with more than 2 instance variables
- most classes should simply be responsible for handling single state variable.
- typically, there are 2 kinds of classes, those that maintain the state of a single instance variable, and those that coordinate 2 separate variable. In general, don't mix the 2 kinds of responsibilities.
- frequently, decomposition of instance variables leads to an understanding of commonality of several related instance variable.
- behavior naturally follows the instance variables into the appropriate place.
## no getters/setters/properties
- tell, don't ask.
- whatever method use object A's getter usually indicate that it should be moved into A.

# Functional Calisthenics Rules

## side effects can only occur at the top level
- we want majority of our code to be `pure functions` where the code doesn't depend on anything that is impure. This means we should pull impure functions up to the top level and isolate them as much as possible from the rest of the code. This can include:
  - user interaction
  - database access
  - API calls.

## no mutable state
- we don't want mutate state within a function either, e.g. do not re-use variables.

## expressions not statements
- if we're calling a function that does not return anything then we are probably expecting that some mutable state will be changes as a part of this call and the function will not be pure.
- object's API provides a clear separation between commands and queries [functional programming in object oriented languages](http://www.harukizaemon.com/blog/2010/03/01/functional-programming-in-object-oriented-languages/).

## Functions should have one argument
- a function with a large number of arguments smells of a `single responsibility principle` violation.
- [currying](https://en.wikipedia.org/wiki/Currying)
- an object a collection of partially applied functions. [functional programming in object oriented languages](http://www.harukizaemon.com/blog/2010/03/01/functional-programming-in-object-oriented-languages/).
  - `f(a, b, c) -> new A(a, b).f(c)`

## no explicit recursion
- prefer separating the method of recursion away from the logic that you're trying to execute

## maximum level of abstraction
- functions should take the highest level of abstraction possible, e.g., if `List` is a special case of Enumerable, then the function should take Enumerable.

## use infinite sequences
- if your function takes, or returns, a sequence of data then write the function in a way that does not exclude infinite sequences, allow for tail recursion

## no if
- "if is just a special case of pattern matching anyway"

## parse don't validate
- [CHECKS - pattern language of information integrity](http://c2.com/ppr/checks.html)
# References
- [object-calisthenics](https://www.cs.helsinki.fi/u/luontola/tdd-2009/ext/ObjectCalisthenics.pdf)
- http://c2.com/ppr/checks.html
- http://www.harukizaemon.com/blog/2010/03/01/functional-programming-in-object-oriented-languages/