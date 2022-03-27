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
## no classes, with more than 2 instance variables

# References
- [object-calisthenics](https://www.cs.helsinki.fi/u/luontola/tdd-2009/ext/ObjectCalisthenics.pdf)
