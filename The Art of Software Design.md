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

