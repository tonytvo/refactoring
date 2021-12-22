# To do

- re-organize this notes by common sense, topics instead of chapters
- review other summary notes
- add questions and answers section
- add quotes section
- add references section

# References

https://gist.github.com/jonnyjava/42883d4e464167f81e2ee60a488a5ded
https://understandlegacycode.com/blog/key-points-of-working-effectively-with-legacy-code/
https://dev.to/dannypsnl/reflection-on-working-effectively-with-legacy-code-chapter-6-to-10-55g6
https://gist.github.com/jonnyjava/42883d4e464167f81e2ee60a488a5ded
https://biratkirat.medium.com/working-effectively-with-legacy-code-series-251b9c7d8c15
https://martinfowler.com/articles/patterns-legacy-displacement/
https://quizlet.com/ca/186072277/working-effectively-with-legacy-code-flash-cards/
https://www.infoq.com/podcasts/working-effectively-legacy-code/

# WORKING EFFECTIVELY WITH LEGACY CODE

To me, legacy code is simply code without tests. I’ve gotten some grief for this definition. What do tests have to do with whether code is bad?
To me, the answer is straightforward, and it is a point that I elaborate throughout the book: Code without tests is bad code. It doesn’t matter how well written it is; it doesn’t matter how pretty or object-oriented or well-encapsulated it is.
With tests, we can change the behavior of our code quickly and verifiably. Without them, we really don’t know if our code is getting better or worse.

## Chapter 1 Changing Software

Four Reasons to Change Software:
For simplicity’s sake, let’s look at four primary reasons to change software.
1. Adding a feature
2. Fixing a bug
3. Improving the design
4. Optimizing resource usage

Behavior is the most important thing about software. It is what users depend on. Users like it when we add behavior (provided it is what they really wanted), but if we change or remove behavior they depend on (introduce bugs), they stop trusting us.

It's all about changes.
Am I ready to do it? Well, I guess I have to.
1. What changes do we have to make?
2. How will we know that we’ve done them correctly?
3. ‎How will we know that we haven’t broken anything?

## Chapter 2 Working with Feedback

Changes in a system can be made in two primary ways. I like to call them _Edit and Pray_ and _Cover and Modify_.
A unit test that takes 1/10th of a second to run is a slow unit test.

**The Legacy Code Dilemma**: When we change code, we should have tests in place. To put tests in place, we often have to change code.

### The Legacy Code Change Algorithm

When you have to make a change in a legacy code base, here is an algorithm you can use.
1. Identify change points.
2. Find test points.
3. ‎Break dependencies.
4. ‎Write tests.
5. ‎Make changes and refactor.

## Chapter 3 Sensing and Separation

Generally, when we want to get tests in place, there are two reasons to break dependencies: sensing and separation.
1. Sensing: We break dependencies to sense when we can’t access values our code computes.
2. Separation: We break dependencies to separate when we can’t even get a piece of code into a test harness to run.

## Chapter 4 The Seam Model

Seam A seam is a place where you can alter behavior in your program without editing in that place. Where is the enabling point?
If you use link seams, make sure that the difference between test and production environments is obvious.
It is important to choose the right type of seam when you want to get pieces of code under test. In general, object seams are the best choice in object-oriented languages. Preprocessing seams and link seams can be useful at times but they are not as explicit as object seams. In addition, tests that depend upon them can be hard to maintain. I like to reserve preprocessing seams and link seams for cases where dependencies are pervasive and there are no better alternatives.

## Chapter 5 Tools

- Automated Refactoring Tools
- Mock Objects
- Unit-Testing Harnesses

## Chapter 6 I Don’t Have Much Time and I Have to Change It

### Sprout Method

When you need to add a feature to a system and it can be formulated completely as new code, write the code in a new method. Call it from the places where the new functionality needs to be. You might not be able to get those call points under test easily, but at the very least, you can write tests for the new code.

#### Advantages and Disadvantages

What are the downsides of Sprout Method? For one thing, when you use it, in effect you essentially are saying that you are giving up on the source method and its class for the moment. You aren’t going to get it under test, and you aren’t going to make it better; you are just going to add some new functionality in a new method. Giving up on a method or a class is the practical choice sometimes, but it still is kind of sad. It leaves your code in limbo. The source method might contain a lot of complicated code and a single sprout of a new method. Sometimes it isn’t clear why only that work is happening someplace else, and it leaves the source method in an odd state. But at least that points to some additional work that you can do when you get the source class under test later.
Although there are some disadvantages, there are a couple of key advantages. When you use Sprout Method, you are clearly separating new code from old code. Even if you can’t get the old code under test immediately, you can at least see your changes separately and have a clean interface between the new code and the old code. You see all of the variables affected, and this can make it easier to determine whether the code is right in context.

### Sprout Class

Essentially two cases lead us to Sprout Class. In one case, your changes lead you toward adding an entirely new responsibility to one of your classes.
The other case is the one we led off this chapter with. We have a small bit of functionality that we could place into an existing class, but we can’t get the class into a test harness. If we could get it to at least compile into a harness, we could attempt to use Sprout Method, but sometimes we’re not even that lucky.

#### Advantages and Disadvantages

The key advantage of Sprout Class is that it allows you to move forward with your work with more confidence than you could have if you were making invasive changes.
The key disadvantage of Sprout Class is conceptual complexity. As programmers learn new code bases, they develop a sense of how the key classes work together. When you use Sprout Class, you start to gut the abstractions and do the bulk of the work in other classes.

### Wrap Method

This is one form of Wrap Method. We create a method with the name of the original method and have it delegate to our old code. We use this when we want to add behavior to existing calls of the original method.
Wrap Method is a good way of getting new, tested functionality into an application when we can’t easily write tests for the calling code. The primary disadvantage of Wrap Method is that it can lead to poor names.

### Wrap Class

The class-level companion to Wrap Method is Wrap Class. Wrap Class uses pretty much the same concept. If we need to add behavior in a system, we can add it to an existing method, but we can also add it to something else that uses that method. In Wrap Class, that something else is another class.
This technique is called the decorator pattern. We create objects of a class that wraps another class and pass them around. The class that wraps should have the same interface as the class it is wrapping so that clients don’t know that they are working with a wrapper.
Generally two cases tip me toward using Wrap Class:
1. The behavior that I want to add is completely independent, and I don’t want to pollute the existing class with behavior that is low level or unrelated.
2. The class has grown so large that I really can’t stand to make it worse. In a case like this, I wrap just to put a stake in the ground and provide a roadmap for later changes.

## Chapter 7 It Takes Forever to Make a Change
Systems that are broken up into small, well-named, understandable pieces enable faster work.

### Lag Time

Changes often take a long time for another very common reason: lag time. Lag time is the amount of time that passes between a change that you make and the moment that you get real feedback about the change.

### The Dependency Inversion Principle

When your code depends on an interface, that dependency is usually very minor and unobtrusive. Your code doesn’t have to change unless the inter-face changes, and interfaces typically change far less often than the code behind them. When you have an interface, you can edit classes that implement that interface or add new classes that implement the interface, all without impacting code that uses the interface.
For this reason, it is better to depend on interfaces or abstract classes than it is to depend on concrete classes. When you depend on less volatile things, you minimize the chance that particular changes will trigger massive recompilation.

## Chapter 8 How Do I Add a Feature?

### TDD and Legacy Code

One of the most valuable things about TDD is that it lets us concentrate on one thing at a time. We are either writing code or refactoring; we are never doing both at once. That separation is particularly valuable in legacy code because it lets us write new code independently of new code. After we have written some new code, we can refactor to remove any duplication between it and the old code.

### Programming by Difference

It allows us to make changes quickly, and we can use tests to move to a cleaner design. But to do it well, we have to look out for a couple of gotchas. One of them is Liskov substitution principle (LSP) violation.
The LSP implies that clients of a class should be able to use objects of a subclass without having to know that they are objects of a subclass. There aren’t any mechanical ways to completely avoid LSP violations.
Whether a class is LSP conformant depends upon the clients that it has and what they expect. However, some rules of thumb help:
1. Whenever possible, avoid overriding concrete methods.
2. If you do, see if you can call the method you are overriding in the overriding method.

## Chapter 9 I Can’t Get This Class into a Test Harness

### The case of the irritating parameter

When you are writing tests and an object requires a parameter that is hard to construct, consider just passing null instead. If the parameter is used in the course of your test execution, the code will throw an exception and the test harness will catch the exception. If you need behavior that really requires an object, you can construct it and pass it as a parameter at that point.
The Null Object Pattern is a way of avoiding the use of null in programs

### The Case of the Hidden Dependency

The fundamental problem here is that the dependency on mail_service is hidden in the mailing_list_dispatcher constructor. If there was some way to replace the mail_service object with a fake, we could sense through the fake and get some feedback as we change the class.
One of the techniques we can use is Parameterize Constructor. With this technique, we externalize a dependency that we have in a constructor by passing it into the constructor.

### The Case of the Construction Blob

Parameterize Constructor is one of the easiest techniques that we can use to break hidden dependencies in a constructor, and it is the one that I often turn to first. Unfortunately, it isn’t always the best choice. If a constructor constructs a large number of objects internally or accesses a large number of globals, we could end up with a very large parameter list. In worse situations, a constructor creates a few objects and then uses them to create other objects.

### The Case of the Irritating Global Dependency

Frameworks solve many problems, and they do give us a boost when we start projects, but this isn’t the kind of reuse that people really expected early on in software development. Old-style reuse happens when we find some class or set of classes that we want to use in our application and we just do it. We just add them to a project and use them.

### The Case of the Horrible Include Dependencies

We can get some reuse for the fakes that we create this way. Instead of putting the definitions of classes such as SchedulerDisplay inline in the test file, we can put them in a separate include file that can be used across a set of test files:
```
#include "TestHarness.h"
#include "Scheduler.h"
#include “Fakes.h“
TEST(create,Scheduler) {   Scheduler scheduler("fred"); }
```
After doing it a couple of times, getting a C++ class instantiable in a harness like this is pretty easy and pretty mechanical.
There are a couple of very serious downsides. We have to create that separate program, and we really aren’t breaking dependencies at the language level, so we aren’t making the code cleaner as we break dependencies. Worse, those duplicate definitions that we put in the test file (SchedulerDisplay::displayEntry in this example) have to be maintained as long as we keep this set of tests in place.

### The Case of the Onion Parameter

It is great to be able to decide to create a class and then just type in a constructor call and have a nice live, working object available to use. But in many cases, it can be hard to create objects. Every object needs to be set up in a good state, a state that makes it ready for additional work. In many cases, this means we have to supply it with objects that are set up properly themselves. Those objects might require other objects so that they can be set up also, so we end up having to create objects to create objects to create objects to create a parameter for a constructor of the class that we want to test.
In any language where we can create interfaces or classes that act like interfaces, we can systematically use them to break dependencies.

### The Case of the Aliased Parameter
Interfaces are great for breaking dependencies, but when we get to the point that we have nearly a one-to-one relationship between classes and interfaces, the design gets cluttered.
Another strategy that we can use is Subclass and Override Method (401). We can make a class called FakeOriginationPermit that supplies methods that make it easy to change the validation flag. Then, in subclasses, we can override the validate method and set the validation flag any way that we need to while we are testing the IndustrialFacility class.

## Chapter 10 I Can’t Run This Method in a Test Harness

Here are some of the problems that we can run into.
- The method might not be accessible to the test. It could be private or have some other accessibility problem.
- It might be hard to call the method because it is hard to construct the parameters we need to call it.
- The method might have bad side effects (modifying a database, launching a cruise missile, and so on), so it is impossible to run in a test harness.
- We might need to sense through some object that the method uses.

### The Case of the Hidden Method
We need to make a change to a method in a class, but it’s a private method. What should we do? The first question to ask is whether we can test through a public method. If we can, it is a worthwhile thing to do. It saves us the trouble of trying to find a way of accessing the private method, and it has another benefit. If we test through public methods, we are guaranteed to be testing the method as it is used in the code.
If we need to test a private method, we should make it public. If making it public bothers us, in most cases, it means that our class is doing too much and we ought to fix it.

### The Case of the “Helpful” Language Feature

When we depend directly on libraries that are out of our control, we are just asking for trouble.

### The Case of the Undetectable Side Effect

Command/Query Separation is a design principle first described by Bertrand Meyer. Simply put, it is this: A method should be a command or a query, but not both. A command is a method that can modify the state of the object but that doesn’t return a value. A query is a method that returns a value but that does not modify the object. Why is this principle important? There are a number of reasons, but the most primary is communication. If a method is a query, we shouldn’t have to look at its body to discover whether we can use it several times in a row without causing some side effect.

## Chapter 11 I Need to Make a Change. What Methods Should I Test?

### Reasoning About Effects

For every functional change in software, there is some associated chain of effects.
In programs that aren’t written very well, we often find it very difficult to figure out why the results we are looking at are what they are. When we are at that point, we have a debugging problem and we have to reason backward from the problem to its source.
When we are working with legacy code, we often have to ask a different question: If we make a particular change, how could it possibly affect the rest of the results of the program? This involves reasoning forward from points of change. When you get a good handle on this sort of reasoning, you have the beginnings of a technique for finding good places to write tests.

#### The impact of changes in legacy code.

We need to find places to test, and the first step is figuring out where change can be detected: what the effects of the change are. When we know where we can detect effects, we can pick and choose among them when we write our tests.

### Reasoning Forward

When we are writing characterization tests we look at a set of objects and try to figure out what will change downstream if they stop working.
When you are sketching effects, make sure that you have found all of the clients of the class you are examining. If your class has a superclass or subclasses, there might be other clients that you haven’t considered.

#### Effect Propagation

Effects can also propagate in silent, sneaky ways. If we have an object that accepts some object as a parameter, it can modify its state, and the change is reflected back into the rest of the application. Each language has rules about how parameters to methods are handled. The default in many cases is to pass references to objects by value. This is the default in Java and C#. Objects aren’t passed to methods; instead, “handles” to objects are passed. As a result, any method can change the state of objects through the handle they were passed. Some of these languages have keywords that you can use to make it impossible to modify the state of an object that is passed to them. In C++, the const keyword does this when you use it in the declaration of a method parameter.
Effects propagate in code in three basic ways:
1. Return values that are used by a caller
2. Modification of objects passed as parameters that are used later
3. Modification of static or global data that is used later
Some languages provide additional mechanisms. For instance, in aspect-oriented languages, programmers can write constructs called aspects that affect the behavior of code in other areas of the system.
Here is a heuristic that I use when looking for effects:
 1. Identify a method that will change.
 2. If the method has a return value, look at its callers.
 3. See if the method modifies any values. If it does, look at the methods that use those values, and the methods that use those methods.
 4. Make sure you look for superclasses and subclasses that might be users of these instance variables and methods also.
 5. Look at parameters to the methods. See if they or any objects that their methods return are used by the code that you want to change.
 6. Look for global variables and static data that is modified in any of the methods you’ve identified.

### Tools for Effect Reasoning
The most important tool that we have in our arsenal is knowledge of our programming language. In every language, there are little “firewalls,” things that prevent effect propagation. If we know what they are, we know that we don’t have to look past them.

#### Effects and Encapsulation
One of the often mentioned benefits of object orientation is encapsulation. Many times when I show people the dependency breaking techniques in this book, they point out that many of them break encapsulation. That’s true. Many of them do. Encapsulation is important, but the reason why it is important is more important. Encapsulation helps us reason about our code. In well encapsulated code, there are fewer paths to follow as you try to understand it. For instance, if we add another parameter to a constructor to break a dependency as we do in the Parameterize Constructor refactoring, we have one more path to follow when we are reasoning about effects. Breaking encapsulation can make reasoning about our code harder, but it can make it easier if we end up with good explanatory tests afterward. When we have test cases for a class, we can use them to reason about our code more directly. We can also write new tests for any questions that we might have about the behavior of the code. Encapsulation and test coverage aren’t always at odds, but when they are, I bias toward test coverage. Often it can help me get more encapsulation later. Encapsulation isn’t an end in itself; it is a tool for understanding.

## Chapter 12 I Need to Make Many Changes in One Area. Do I Have to Break Dependencies for All the Classes Involved?

Higher-level tests can be useful in refactoring. Often people prefer them to finely grained tests at each class because they think that change is harder when lots of little tests are written against an interface that has to change. In fact, changes are often easier than you would expect because you can make changes to the tests and then make changes to the code, moving the structure along in small safe increments. While higher-level tests are an important tool, they shouldn’t be a substitute for unit tests. Instead, they should be a first step toward getting unit tests in place.

### Interception Points

An interception point is simply a point in your program where you can detect the effects of a particular change.
In general, it is a good idea to pick interception points that are very close to your change points, for a couple of reasons. The first reason is safety. Every step between a change point and an interception point is like a step in a logical argument. Essentially, we are saying, "We can test here because this affects this and that affects this other thing, which affects this thing that we are testing." The more steps you have in the argument, the harder it is know that you have it right. Sometimes the only way you can be sure is by writing tests at the interception point and then going back to the change point to alter the code a little bit and see if the test fails. Sometimes you have to fall back on that technique, but you shouldn’t have to do it all the time. Another reason why more distant interception points are worse, is that it is often harder to setup tests at them. This isn’t always true; it depends on the code. What can make it harder is, again, the number of steps between the change and the interception point.
An ideal interception point? It is a single point that we can use to detect effects from changes in a cluster of classes.

### Pinch Point

A pinch point is a narrowing in an effect sketch, a place where tests against a couple of methods can detect changes in many methods.
Another way of finding a pinch point is to look for common usage across an effect sketch. A method or variable might have three users, but that doesn’t mean that it is being used in three distinct ways.
What is a pinch point, really? A pinch point is a natural encapsulation boundary. When you find a pinch point, you’ve found a narrow funnel for all of the effects of a large piece of code.
This is pretty much the definition of encapsulation: We don’t have to care about the internals, but when we do, we don’t have to look at the externals to understand them. Often when I look for pinch points, I start to notice how responsibilities can be reallocated across classes to give better encapsulation.
Writing tests at pinch points is an ideal way to start some invasive work inpart of a program. You make an investment by carving out a set of classes and getting them to the point that you can instantiate them together in a test harness. After you write your characterization tests, you can make changes with impunity.

## Chapter 13 I Need to Make a Change, but I Don’t Know What Tests to Write

No, finding bugs in legacy code usually isn’t a problem. In terms of strategy, it can actually be misdirected effort. It is usually better to do something that helps your team start to write correct code consistently. The way to win is to concentrate effort on not putting bugs into code in the first place. In the natural flow of development, tests that specify become tests that preserve. You will find bugs, but usually not the first time that a test is run. You find bugs in later runs when you change behavior that you didn’t expect to.

### Characterization Tests

The tests documents the actual current behavior of the system.
Characterization tests record the actual behavior of a piece of code. If we find something unexpected when we write them, it pays to get some clarification. It could be a bug. That doesn’t mean that we don’t include the test in our test suite; instead, we should mark it as suspicious and find out what the effect would be of fixing it.

### When You Find Bugs

When you characterize legacy code, you will find bugs throughout the entire process. All legacy code has bugs, usually in direct proportion to how little it is understood. What should you do when you find a bug? The answer depends on the situation. If the system has never been deployed, the answer is simple: You should fix the bug. If the system has been deployed, you need to examine the possibility that someone is depending on that behavior, even though you see it as a bug. Often it takes a bit of analysis to figure out how to fix a bug without causing ripple effects. My bias is toward fixing bugs as soon as they are found. When behavior is clearly in error, it should be fixed. If you suspect that some behavior is wrong, mark it in the test code as suspicious and then escalate it. Find out as quickly as you can whether it is a bug and how best to deal with it.
When we refactor, we generally have to check for two things: Does the behavior exist after the refactoring, and is it connected correctly? Many characterization tests look like “sunny day” tests. They don’t test many special conditions; they just verify that particular behaviors are present. From their presence, we can infer that refactorings that we’ve done to move or extract code have preserved behavior.

### A Heuristic for Writing Characterization Tests
1. Write tests for the area where you will make your changes. Write as many cases as you feel you need to understand the behavior of the code.
2. After doing this, take a look at the specific things you are going to change, and attempt to write tests for those.
3. If you are attempting to extract or move some functionality, write tests that verify the existence and connection of those behaviors on a case-bycase basis. Verify that you are exercising the code that you are going to move and that it is connected properly. Exercise conversions.

## Chapter 14 Dependencies on Libraries Are Killing Me

Avoid littering direct calls to library classes in your code. You might think that you’ll never change them, but that can become a self-fulfilling prophecy. Sometimes the best thing you can do is write a thin wrapper over the classes that you need to separate out.

## Chapter 15 My Application Is All API Calls
Build, buy, or borrow. It’s a choice we all have to make when we develop software.
Systems that are littered with library calls are harder to deal with than homegrown systems, in many respects. The first reason is that it is often hard to see how to make the structure better because all you can see are the API calls. Anything that would’ve been a hint at a design just isn’t there. The second reason that API-intensive systems are difficult is that we don’t own the API. If we did, we could rename interfaces, classes, and methods to make things clearer for us, or add methods to classes to make them available to different parts of the code.
How do we move forward? There are essentially two approaches:
 1. Skin and Wrap the API
 2. Responsibility-Based Extraction
How do we choose between Skin and Wrap the API and Responsibility Based Extraction?
Here are the trade-offs: Skin and Wrap the API is good in these circumstances:
 - The API is relatively small.
 - You want to completely separate out dependencies on a third-party library.
 - You don’t have tests, and you can’t write them because you can’t test through the API.
Responsibility-Based Extraction is good in these circumstances:
 - The API is more complicated.
 - You have a tool that provides a safe extract method support, or you feel confident that you can do the extractions safely by hand.
Many teams use both techniques: a thin wrapper for testing and a higherlevel wrapper to present a better interface to their application.

## Chapter 16 I Don’t Understand the Code Well Enough to Change It

Ways of gaining understanding:
 - Notes/Sketching
 - Scratch Refactoring
Check out the code from your version-control system. Forget about writing tests. Extract methods, move variables, refactor it whatever way you want to get a better understanding of it; just don’t check it in again.
Throw that code away. This is called Scratch refactoring.
- Delete Unused Code
If the code you are looking at is confusing and you can determine that some of it isn’t used, delete it. It isn’t doing anything for you except getting in your way. Sometimes people feel that deleting code is wasteful.
After all, someone spent time writing that code—maybe it can be used in the future. Well, that is what version-control systems are for. That code will be in earlier versions. You can always look for if you ever decide that you need it.

## Chapter 17 My Application Has No Structure

When teams aren’t aware of their architecture, it tends to degrade. What gets in the way of this awareness?
 - The system can be so complex that it takes a long time to get the big picture.
 - The system can be so complex that there is no big picture.
 - The team is in a very reactive mode, dealing with emergency after emergency so much that they lose sight of the big picture.

The brutal truth is that architecture is too important to be left exclusively to a few people. It’s fine to have an architect, but the key way to keep an architecture intact is to make sure that everyone on the team knows what it is and has a stake in it.
Every person who is touching the code should know the architecture, and everyone else who touches the code should be able to benefit from what that person has learned. When everyone is working off of the same set of ideas, the overall system intelligence of the team is amplified.

## Chapter 20 This Class Is Too Big and I Don’t Want It to Get Any Bigger

### Single-Responsibility Principle (SRP)

Every class should have a single responsibility: It should have a single purpose in the system, and there should be only one reason to change it.

### Seeing responsibilities

 - Heuristic #1: **Group Methods** Look for similar method names. Write down all of the methods on a class, along with their access types (public, private, and so on), and try to find ones that seem to go together.
 - Heuristic #2: **Look at Hidden Methods** Pay attention to private and protected methods. If a class has many of them, it often indicates that there is another class in the class dying to get out.
 - Heuristic #3: **Look for Decisions That Can Change** Look for decisions, not decisions that you are making in the code, but decisions that you’ve already made. Is there some way of doing something (talking to a database, talking to another set of objects, and so on) that seems hard-coded? Can you imagine it changing?
 - Heuristic #4: **Look for Internal Relationships** Look for relationships between instance variables and methods. Are certain instance variables used by some methods and not others? Make a little sketch of the relationships inside a class. These are called feature sketches. They show which methods and instance variables each method in a class uses. These sketches are disposable tools. They are the sort of thing that you sit down and draw up with apartner for about 10 minutes before you make your changes. Afterward you throwthem away. There is little value in keeping them around, so there is little likelihoodthat they will be confused with each other.
 - Heuristic #5: **Look for the Primary Responsibility** Try to describe the responsibility of the class in a single sentence.  There are two ways to violate the Single Responsibility Principle. It can be violated at the interface level and at the implementation level. SRP is violated at the interface level when a class presents an interface that makes it appear that it is responsible for a very large number of things. The SRP violation that we care most about is violation at the implementation level. Plainly put, we care whether the class really does all of that stuff or whether it just delegates to a couple of other classes. If it delegates, we don’t have a large monolithic class; we just have a class that is a facade, a front end for a bunch of little classes and that can be easier to manage. Interface Segregation Principle (ISP) When a class is large, rarely do all of its clients use all of its methods. Often we can see different groupings of methods that particular clients use. If we create an interface for each of these groupings and have the large class implement those interfaces, each client can see the big class through that particular interface. This helps us hide information and also decreases dependency in the system. The clients no longer have to recompile whenever the large class does.
 - Heuristic #6: **When All Else Fails, Do Some Scratch Refactoring** If you are having a lot of trouble seeing responsibilities in a class, do some scratch refactoring.
 - Heuristic #7: **Focus on the Current Work** Pay attention to what you have to do right now. If you are providing a different way of doing anything, you might have identified a responsibility that you should extract and then allow substitution for.

The way to really get better at identification is to read more. Read books about design patterns. More important, read other people’s code. Look at open-source projects, and just take some time to browse and see how other people do things. Pay attention to how classes are named and the correspondence between class names and the names of methods. Over time, you’ll get better at identifying hidden responsibilities, and you’ll just start to see them when you browse unfamiliar code.

### Moving Forward

When you’ve identified a bunch of different responsibilities in a large class, there are only two other issues to deal with: strategy and tactics

#### Strategy

The best approach to breaking down big classes is to identify the responsibilities, make sure that everyone else on the team understands them, and then break down the class on an as-needed basis. When you do that, you spread out the risk of the changes and can get other work done as you go.

#### Tactics

If you are able to get tests in place, you can start to extract a class in a very straightforward way, using the Extract Class refactoring described in Martin Fowler’s book Refactoring: Improving the Design of Existing Code

## Chapter 21 I’m Changing the Same Code All Over the Place

When two methods look roughly the same, extract the differences to other methods. When you do that, you can often make them exactly the same and get rid of one.
Orthogonality is a fancy word for independence. If you want to change existing behavior in your code and there is exactly one place you have to go to make that change, you’ve got orthogonality. It is as if your application is a big box with knobs surrounding the outside. If there is only one knob per behavior for your system, changes are easy to make. When you have rampant duplication, you have more than one knob for each behavior.

### Open/Closed Principle

The open/closed principle is a principle that was first articulated by Bertrand Meyer. The idea behind it is that code should be open for extension but closed to modification. What does that mean? It means that when we have good design, we just don’t have to change code much to add new features. When we remove duplication, our code often naturally starts to fall in line with the Open/Closed Principle

## Chapter 22 I Need to Change a Monster Method and I Can’t Write Tests for It

Introduce Sensing Variable.
Extract small pieces first.
Extract What You Know
The key thing to pay attention to when you do these little extractions is the coupling count of the extraction. The coupling count is the number of values that pass into and out of the method you are extracting. If extractions with a low coupling count are safer, then extractions with a count of 0 must be the safest of all—and they are. You can make a lot of headway in a monster method by just extracting methods that don’t accept any parameters and don’t return any values. These methods are really commands to do something.

## Chapter 23 How Do I Know That I’m Not Breaking Anything?

Anything that helps us know how we are affecting software when we type can help us reduce bugs. Test-driven development is very powerful in this way.
Hyperaware editing is a flow state, a state in which you can just shut out the world and work sensitively with the code. Programming is the art of doing one thing at a time
Pair Programming: working in legacy code is surgery, and doctors never operate alone.

## Chapter 25 Dependency-Breaking Techniques

These refactorings are intended to be done without tests, to get tests in place.
 - **Safety first**. Once you have tests in place, you can make invasive changes much more confidently.
 - **Adapt Parameter**: when you can’t use Extract Interface on a parameter’s class or when a parameter is difficult to fake
 - **Break Out Method Object**. In a nutshell, the idea behind this refactoring is to move a long method to a new class. Break Out Method Object has several variations. In the simplest case, the original method doesn’t use any instance variables or methods from the original class. We don’t need to pass it a reference to the original class. In other cases, the method only uses data from the original class. At times, it makes sense to put this data into a new data-holding class and pass it as an argument to the method object.
 - **Definition Completion** In some languages, we can declare a type in one place and define it in another. The languages in which this capability is most apparent are C and C++. In both of them, we can declare a function or method in one place and define it someplace else, usually in an implementation file. When we have this capability, we can use it to break dependencies.
 - **Encapsulate Global References** When you are trying to test code that has problematic dependencies on globals, you essentially have three choices. You can try to make the globals act differently under test, you can link to different globals, or you can encapsulate the globals so that you can decouple things further. The last option is called Encapsulate Global References. To Encapsulate Global References, follow these steps:
 1. Identify the globals that you want to encapsulate.
 2. Create a class that you want to reference them from.
 3. Copy the globals into the class.
 - **Expose Static Method** If you have a method that doesn’t use instance data or methods, you can turn it into a static method. When it is static, you can get it under test without having to instantiate the class. When a method is static, you know that it doesn’t access any of the private data of the class; it is just a utility method. If you make the method public, you can write tests for it. Those tests will support you if you choose to move the method to another class later. Static methods and data really do act as if they are part of a different class. Static data lives for the life of a program, not the life of an instance, and statics are accessible without an instance.
 - **Extract and Override Call** is a very useful refactoring; I use it very often. It is an ideal way to break dependencies on global variables and static methods. Is what Fowler calss extract method.
 - **Extract and Override Factory Method**, follow these steps:
 1. Identify an object creation in a constructor.
 2. Extract all of the work involved in the creation into a factory method.
 3. Create a testing subclass and override the factory method in it to avoid dependencies on problematic types under test.
 - **Extract and Override Getter** Lazy getters create the object they are supposed to return the first time they are called. Lazy getters are also used in the Singleton Design Pattern. When there is just a single method on an object that is problematic, it is far easier to use Extract and Override Call. But, Extract and Override Getter is a better choice when there are many problematic methods on the same object. If you can get rid of all of those problems by extracting a getter and overriding it, it is a clear win.
 - **Extract Implementer**, follow these steps: 1. Make a copy of the source class’s declaration. Give it a different name. It’s useful to have a naming convention for classes you’ve extracted. I often use the prefix Production to indicate that the new class is the production code implementer of an interface. 2. Turn the source class into an interface by deleting all non-public methods and all variables.3 Make your production class implement the new interface.
 - **Extract Interface** In many languages, Extract Interface is a one of the safest dependency-breaking techniques. If you get a step wrong, the compiler tells you immediately, so there is very little chance of introducing a bug. The gist of it is that you create an interface for a class with declarations for all of the methods that you want to use in some context. When you’ve done that, you can implement the interface to sense or separate, passing a fake object into the class you want to test.
 - **Introduce Instance Delegator**. when you use this technique, you can get an object seam in place easily and substitute different behaviors under test. Over time, you might get to the point that every call to the utility class comes through the delegating methods. At that time, you can move the bodies of the static methods into the instance methods and delete the static methods. To Introduce Instance Delegator, follow these steps: 1. Identify a static method that is problematic to use in a test. 2. Create an instance method for the method on the class. Remember to Preserve Signatures (312). Make the instance method delegate to the static method. 3. Find places where the static methods are used in the class you have under test. Use Parameterize Method (383) or another dependency-breaking technique to supply an instance to the location where the static method call was made.
 - **Introduce Static Setter**. Add a static setter to the singleton to replace the instance, and then make the constructor protected.
 - **Link Substitution**. Create classes with the same names and methods, and change your classpath so that calls resolve to them rather than the classes with bad dependencies.
 - **Parameterize Constructor** If you are creating an object in a constructor, often the easiest way to replace it is to externalize its creation, create the object outside the class, and make clients pass it into the constructor as a parameter.
 - **Parameterize Method** You have a method that creates an object internally, and you want to replace the object to sense or separate. Often the easiest way to do this is to pass the object from the outside
 - **Primitivize Parameter**, follow these steps: 1. Develop a free function that does the work you would need to do on the class. In the process, develop an intermediate representation that you can use to do the work. 2. Add a function to the class that builds up the representation and delegates it to the new function.
 - **Pull Up Feature**, follow these steps: 1. Identify the methods that you want to pull up. 2. Create an abstract superclass for the class that contains the methods. 3. Copy the methods to the superclass
 - **Push Down Dependency** When you use Push Down Dependency, you make your current class abstract. Then you create a subclass that will be your new production class, and you push down all the problematic dependencies into that class. At that point you can subclass your original class to make its methods available for testing.
 - **Replace Function with Function Pointer** is a good way to break dependencies. One of the nice things about it is that it happens completely at compile time, so it has minimal impact on your build system.
 - **Replace Global Reference with Getter**, do the following:
 1. Identify the global reference that you want to replace.
 2. Write a getter for the global reference. Make sure that the access protection of the method is loose enough for you to be able to override the getter in a subclass.
 3. Replace references to the global with calls to the getter.
 4. Create a testing subclass and override the getter.
 - **Subclass and Override Method** The central idea of Subclass and Override Method is that you can use inheritance in the context of a test to nullify behavior that you don’t care about or get access to behavior that you do care about
 - **Supersede Instance Variable**, follow these steps:
 1. Identify the instance variable that you want to supersede.
 2. Create a method named supersedeXXX, where XXX is the name of the variable you want to supersede.
 3. In the method, write whatever code you need to so that you destroy the previous instance of the variable and set it to the new value. If the variable is a reference, verify that there aren’t any other references in the class to the object it points to. If there are, you might have additional work to do in the superceding method to make sure that replacing the object is safe and has the right effect.
 - **Text Redefinition** To use Text Redefinition in Ruby, follow these steps:
 1. Identify a class with definitions that you want to replace.
 2. Add a require clause with the name of the module that contains that class to the top of the test source file.
 3. Provide alternative definitions at the top of the test source file for each method that you want to replace.
