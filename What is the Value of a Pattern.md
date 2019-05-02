## What is the Value of a Pattern?

What makes a programming pattern valuable? We must first identify what we value in a system before we can we adopt a pattern!

<u>We must consider why engineering is often easier than reverse-engineering.</u>

What principles influence your perspective and decision?

[The power of good design Dieter Rams’s ideology](https://www.vitsoe.com/us/about/good-design)

[Architecting UIs for Change](https://joreteg.com/blog/architecting-uis-for-change)

Simplicity - Cognitive Load, not complected

Ease - Learnability, "near at hand"

? Declarative vs Functioal ?

``` swift
var x = 0
for i in 1...5 {
    x += i
}
// vs
sum (1...5)
// vs
sum (from:1, to: 5)
// vs
sum (1, 2, 3, 4, 5)
// vs
1 + 2 + 3 + 4 + 5
// vs
(+ 1 2 3 4 5)
```





Parameter vs Constant (in Code) - When are parameters input? Compile, Build, Run, UX(interactive) time?

What is the expected life span/cycle of a given parameter?


Property vs Class (OOP)

States vs Event - which is easier to test / reason about?

Mock Data vs Mock Objects/Classes

Given the same amount of code coverage, running 1 test with a 1000 different inputs is preferable to 1000 tests with 1 input each.

[Ladders: Reinforce Values instead of Rules](https://www.theladders.com/career-advice/why-you-should-you-reinforce-values-instead-of-rules)

[Transactional Memory](https://en.wikipedia.org/wiki/Transactional_memory)

[MVC/MVP/MVVM/CLEAN/VIPER/REDUX/MVI/PRNSAASPFRUICC](https://proandroiddev.com/mvc-mvp-mvvm-clean-viper-redux-mvi-prnsaaspfruicc-building-abstractions-for-the-sake-of-building-18459ab89386)


