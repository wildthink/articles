## The Art of Software Design

"The art and craft of software design"



A simple system is...

- Easier to understand
- Easier to maintain
- Easier to improve - large improvements can be implemented with little effort



Good Design

- Reduces the amount of code that is affected by a design decision



"Complexity is anything related to the structure of a software system that makes it hard to understand and modify the system." - Ousterhout (PoSD)



- "It is better to have 100 functions operate on one data structure than to have 10 functions operate on 10 data structures." - Alan J. Perlis



- Mutable stateful objects are the new spaghetti code
  - Hard to understand, test, reason about
  - Concurrency disaster



- Many functions defined on few primary data structures (seq, map, vector, set).



PoSD - A Philosophy of Software Design by John Ousterhout

### The True Cost of Complexity

Change Amplification - Blase Radius! (“Close the blast doors”), “Negative/Extended Cascade”
Linear/Quadratic cascade

Blast radius (total area and “distance”) vs Blast vector (the actual path including furcations)
Types of “blast” impact include:
- Insert/Delete Type
- Extending type properties
- Refactoring type properties (e.g. Int -> Float)
- Insert/Delete function/method
- Refactor function/method
- Files touched
- Fan-in, fan-out

Of late, the term is being used for impact analysis in technology parlance. For example, when a particular IT service fails, the users, customers, other dependent services that are affected, fall into Blast Radius.

Concretions, warts, barnacles on the hull.


One-time Complexity
- Initial Learning curve
- Refactoring

Ongoing Complexity
- Ongoing learning
- Not all code generation is compilation
- Affect on Understanding (see below)
- Affect on mastery
- Incidental - Boilerplate
- Accidental
- Essential
- Cognitive friction to Comprehension (understanding what you see)

How does the nature of complexity impede mastery?

Mastery - is a measure combining the following: Smaller being better.
- The time to “realize” (bring to mind) the solution
  - Approach
  - Abstraction (relevant parameters)
  - Specifics
- The time to execute on the solution
- The optimality of the solution
  - expenditure of resources in creating the solution
  - Efficiency of the solution itself

Learning is a journey toward mastery of behaviors (skills)

What should we learn? Why should we learn?
Why is mastery of a particular skill valuable?


http://julien.gunnm.org/geek/misc/2015/07/29/what-is-the-real-cost-of-software-complexity/

http://www.pitt.edu/~ckemerer/CK%20research%20papers/SwComplexityAndMaintenanceCost_BankerDatar93.pdf

https://medium.com/grand-parade/the-cost-of-software-development-and-tips-how-to-reduce-it-60ba44e85948

https://www.coherentsolutions.com/blog/determining-the-real-cost-of-software-development/




Decision Costs - note: 1st, 2nd, 3rd? Order consequences
- Uncertainty
- Degree of Risk - Mistakes and accidents cost resources/money (in various forms)
  - Mistakes - misinterpreted intention (answering the wrong question)
  - Accidents - unintended consequences - aka “bugs”
- Time to implement
- Time to verify (review, test, merge, test)
- Time to troubleshoot - find root causes of identified risk
- Switching Cost (Pivot) - The Resources/Time/Risk (blast radius) required to respond to change (pivot when specifications change) 

Understanding
- State vs Flow vs Process vs Loops/Iteration/Recursion
- Pipes, DAGs (furcations), and Graphs (joins, and cycles)
- Declarative vs Imperative
- Command/Function vs Observer
- Observer’s a level of indirection requiring an analysis of the Observers in order to understand the effect of an event or action
- Adopting the Subject-Observer pattern also opens up the possibility of having multiple observers which can lead to a wider blast vector and possible race conditions from downstream interactions
- Corresponding code/parameters (the implementation A depends on the implementation B)
- Language and Symbology
- Verbosity vs succinctness (Bret Victor math symbology)
- Cognitive friction to Comprehension (understanding what you see, symbology and verbosity)
- Comprehension of intent (e.g. what does it do?)
- Comprehension of strategy/technique (e.g. how does it work?)
- Explicit vs implicit intent
  - average([1 2 3]) vs (1 + 2 + 3)/3


Examples
Dependency injection pushing the context down through.


“Technologies both new and old provide us with a wide range of cognitive artifacts that change the structure of our cognitive tasks. After a brief analysis of past classifications of these artifacts, I shall elaborate a new way of classifying them developed by focusing on an aspect that has been previously overlooked, namely the possible relationships between these objects and the cognitive processes they involve. Cognitive artifacts are often considered as objects that simply complement our cognitive capabilities, but this “complementary view” seems to be an oversimplification. Assuming an “interaction-centered approach”, this article identifies three essential ways in which cognitive artifacts carry out their function: complementing, constituting and substituting our cognitive processes, and builds a taxonomy of these objects that is grounded on these relations. In so doing, it also addresses the chaotic set of different micro-functions carried out by cognitive artifacts, which have not thus far been dealt with, sorting these functions into three corresponding categories. The second part of the article analyzes in greater detail how cognitive artifacts work in our cognitive life, identifying a new kind of functions, called semi-proper functions, and providing a new definition of cognitive artifact based on the previous analysis of these objects.”

"What is the square which when taken with ten of its roots will give a sum total of thirty- nine? Now the roots in the problem before us are ten. Therefore take five, which multiplied by itself gives twenty-five, an amount you add to thirty-nine to give sixty-four. Having taken the square root of this which is eight, subtract from this half the roots, five leaving three. The number three represents one root of this square, which itself, of course is nine. Nine therefore gives the square."

Solve the following quadratic equations by the Arabic method of completing the square.”

Show your work. Note that the questions ask for the square," or x as the solution.

  1. What is the square which when taken with eight of its roots will give a sum total of nine?
  2. What is the square which when taken with ten of its roots will give a sum total of one hundred forty- four?
  3. What is the square which when taken with twelve of its roots gives the sum total of sixty-four? Solve 3x2 + 10x = 32 using a variable substitution and then the Arabic method of completing the square.

Again, state the solution in terms of x?

What is the square which when taken with ten of its roots will give a sum total of thirty- nine? 

x2 + 10x = 39

x2 + 10x - 39 = 0

(x - 3)(x + 13) = 0

X = 3, x = -13
