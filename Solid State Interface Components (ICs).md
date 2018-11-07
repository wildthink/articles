## Solid State Interface Components (ICs)

Even for non-game applications maintaining a consistent presentation (UI) amidst the juggling of user interactions, network events, timers, run loops, and system display cycles in an application's multithreaded concurrent environment is an increasingly complex problem. Today's application developers are typically left to fend for themselves, rolling their own solutions.

This generally entails manually calling for view refreshes based on hand-crafted combinations of connecting and disconecting object pointers, a multitude of registering, unregistering, and posting of notifications, and/or making asynchronous data requests each and every time a view or viewController moves into and out of a scene.

Our user, blissfully unaware, flips to a new view and back again impatient for results. But, more often than not, this only serves to jerk the rug out from under us, causing requests to be cancelled if we're lucky, data to be thrown away and/or multiple redundent redisplay events to be triggered resulting in visual stuttering and lags.



![Manual Swithboard](http://www.clipartmasters.com/clip-arts/1088/telephone-switchboard-operator-1088564.jpg)

![switchboard.jpg](http://www.lesstroud.com/content/images/2014/Aug/switchboard.jpg)

Yet, can not even the most complex UI applications be distilled to a series of simple steps repeated over and over again?

1. Start with some Initial State

2. Present current State to the user

3. Accept some user input

4. Compute a new State based on the previous state and the input parameters

5. Goto #2

The challenge stems from combining a multitude of various components all twiddling parts of the state. Issues (aka "bugs") are created because the parts are NOT well coordinated; some are reading while others are writing or one part of the UI didn't get notified in time causing an inconsistent state being presented to the user. Multiple reloads and refreshes are not uncommon when the left hand is unaware what the right is doing.

And then a new question percolated up into my consciousness. "How is it that games are able to render tens of thousands of polygons with lighting and textures galore all near 60 frame-per-second with nary a hiccup?" We must be missing something. For reference, 60 fps is considered optimal with 30 fps generally considered the baseline for playability.

Many of these issues can be distilled down to a single problem; how you coordinate changes to and propagate your application state. Given the usual collection of components asynchronously interacting, most settle for the "eventually consistent" model of state managment and only worry about redundant refreshes if results in a noticable performance degradation. A major challenge in use any Observer pattern lies in the fact that UI updates get interleaved with state changes so its very easy to create race conditions and contention for data being rendered. This is especially true when dealing with collections; some members of the collection are inserted, deleted, and/or updated while trying to display the collection as a whole.

Exactly what all is going on? Any given state change typically results in any or all of the following:

- "Telling" other components resulting in...

- Asynchronously messaging your server

- Saving to a database

- Updating your display which can entail
  - Switching scenes (View Controllers)
  - Updating the content of your views
  - Updating your graphics context, colors, fonts, etc.
  - Calculating your layout (Contraints)
  - Updating your view frames
  - And finally Rendering to the display

  Shake vigoursly by asynchronously triggering tens, hundreds, if not thousands of different state changes per second in no particular order to create a cacaphony of redundant refreshes. We have thus created an environment ripe for contentous data manipulations and various concurrency-related errors.

  #### Over React

  While React utilzies the unidirectional data flow...

  #### REPL - Redux

  > [Read-eval-print loop](https://en.wikipedia.org/wiki/Read-eval-print_loop)
  >
  > A Read–Eval–Print Loop, also known as an interactive toplevel or language shell, is a simple, interactive computer programming environment that takes single user inputs, evaluates them, and returns the result to the user; a program written in a REPL environment is executed piecewise.
  >
  > [Wikipedia](https://en.wikipedia.org/wiki/Read-eval-print_loop)

  And isn't this ultimately the same cycle for today's applications? Just at a higher frame rate. A significant point here is to model and reason about the system in terms of discrete state transitions over time and then interleave user input, timer and network events, and, germane to our discussion, computation and rendering with these transitions. All with the intent to maximize the responsiveness of the user experience while minimizing the impact on computation and energy utilization.

  Game developers are very familiar with some form of this as we see illustrated in the figure from a fantastic  [tutorial from RayWenderlich.com](https://www.raywenderlich.com/128728/scene-kit-tutorial-swift-part-4-render-loop).

  ![SceneKit Render Loop](https://koenig-media.raywenderlich.com/uploads/2016/03/RenderLoop.jpg)

In researching for this article I discovered the "Temporal Logic of Actions", [TLA+](https://en.wikipedia.org/wiki/TLA%2B), a fascinating subject in its own right.  I highly recommend you take a look.

> **TLA+** ([pronounced](https://en.wikipedia.org/wiki/English_alphabet#Letter_names) as *tee ell a plus*, [/ˈtiː ɛl eɪ plʌs/](https://en.wikipedia.org/wiki/Help:IPA/English)) is a [formal specification](https://en.wikipedia.org/wiki/Formal_specification) language developed by [Leslie Lamport](https://en.wikipedia.org/wiki/Leslie_Lamport). It is used to design, model, document, and verify [concurrent systems](https://en.wikipedia.org/wiki/Concurrent_systems). TLA+has been described as exhaustively-testable [pseudocode](https://en.wikipedia.org/wiki/Pseudocode)[[4\]](https://en.wikipedia.org/wiki/TLA%2B#cite_note-awsorig-4) and [blueprints](https://en.wikipedia.org/wiki/Blueprints) for software systems;[[5\]](https://en.wikipedia.org/wiki/TLA%2B#cite_note-wired-5) the TLA stands for "Temporal Logic of Actions."
>

[TLA+](https://en.wikipedia.org/wiki/TLA%2B) has a lot to offer but I am going to only lift out a small portion of some key terms and definitions for our discussion. I do this because I think there is a useful correspondence and picking different but similar terminology would confuse any relationships between it and what we are trying to accomplish here.

- *State* - an assignment of values to variables
- *Behaviour* - a sequence of states
- *Step* - a pair of successive states in a behavior
- *Stuttering step* - a step during which variables are unchanged
- *Next-state relation* - a relation describing how variables can change in any step
- *State function* - an expression containing variables and constants that is not a next-state relation
- *State predicate* - a Boolean-valued state function
- *Invariant* - a state predicate true in all reachable states
- *Temporal formula* - an expression containing statements in temporal logic



![meta-chart](/Users/jason/Downloads/meta-chart.svg)

------

This is all facilitated by liberally intermixing the following patterns:

- Delegation
- Key-Value-Observing
- Notification (aka Publish/Subscribe)
- Callbacks
- Timers

Swirl this around with your choice of MVC, MVP, MVVM, or VIPER; respectively, Model-View-Controller, Model-View-Presenter, Model-View-ViewMode, and View-Interactor-Presenter-Entity-Routing.

Or if your a modern develper, FLUX, React, MOVVER, or SOLID. 

Really! I'm not making this stuff up!

Go read up. I'll wait. The references are below.

So what's a guy to do? Throw another log on the fire of course. :-)

### Before we begin

To be sure, I want to clarify that these are complex problems and many smart folks have and continue to provide solutions to this problem. And I have learned a great deal from their work and respect and appreciate the sharing of code, articles, presentations and ideas. Indeed the ideas I present herein are not necessarily new; rather I hope to contribute by building upon and refining on these ideas.

Indeed conceptual elements of FLUX 

### Let's Begin - Step Back

The thing that underpins every application is its **state**. To be more specific, we need to be careful not to conflate the model or semantic state with the presentation state. The model is the domain content whereas the presentation state simply specifies a set of display attributes including (but not limited to), colors, fonts, frames, coordinate translations, etc. applied to a collection of views. Also part of the presentation state is a set of bindings which specifies what parts of the domain model are displayed in particular views. We are free to change each independently. The presentation state can depend on the model but not the other way around.

An essential concept for any application is to display a value from the model/state in some view. These renderings can be grpahical but for non-games it usually some text or formatted number to be rendered by a Lable or TextField or a graphic of some sort. These should serve as good examples to talk to.



Using KeyValueObserving (KVO) or Notifications (Publish/Subscribe) require interested objects to register/subsribe to be notified and correspondingly unregister/unsubscribe when they are not. But there is no way to specifiy an order or priority of notification nor to know when all objects have received such notice. Consequently, one may trigger undesirable and/or redundant notifications, interface refreshes, or other calls due to multiple broadcasts, cycles or cascades. Aside from the inefficiencies, this othen leads to concurrency-type race conditions, corrupted data, deadlocks, and even memory leaks.

Other asynchronous events such as network request reciepts, user actions, timers, and background operations can likewise contribute to notification thrashing or overload triggering the same type of errors.

We leverage the application run loop to coordinate reads and writes to the application state in order to minimize required UI refreshes. By controlling when the state changes we can guarantee a period of state immutability during layout and rendering of the active UI.

Stealing a technique from real-time game engines, we propose that that different types of operations be partitioned and sequenced in a manner to minimize cross partition activation thus reducing the number of required graphic layout and rendering operations during each refresh.

Are you tired of writing boiler plate code to glue your interface to your model?

Are users dismissing your ViewControllers before they can handle some incoming data?

Seeing a lot of redundant reloadData() calls?

Are your views not always getting refreshed?

Think unidirectional data flow is a good idea?

Are notifications flying about all which-a-way? Getting caught

Model/Semantic State vs Presentation State

S-Frame - Semantic/State Frame in a tree/stack

Run Loop - Model Mutation

- Initial State
- Apply User State Mutations
- Apply System / Network Mutations
- Dependent State Calculations (? spreadsheet)

### F.A.B.

Display state instead of reacting to events.

If all state transitions are expressed as `x' = f(S)` and all temporal formulas

For any set of temporal formulas, any subset whose count is > 1 that affect the same variable in the next state represent a conflict. i.e. the order of their execution could result in different end states.

### Included in the Box

- Caching - have your services store their content in the DB
- Common Interface
- Transactions
- Snaphots - insures an atomic display update
- Complex Queries and Sorting
- Never miss a Notification because you weren't "observing" at the time
- Rx Compatible
- Testing - Multiple states and complex queries easily available
- Reduces Memory footprint by passing around ids instead of structs or references

### Typical Application Data Domains

- Application wide - "Global"
- User / Session
- Document
- Section
- Collections
- Implicit (by Context) vs Explicit (Hard coded wiring) Injection

"Injection" is nothing more than letting one object twiddle the components or settings of another.

### References

https://awesomeswift.com/2016/12/20/real-world-flux-architecture-on-ios.swift

https://cocoacasts.com/key-value-observing-kvo-and-swift-3/

https://themindstudios.com/blog/mvp-vs-mvc-vs-mvvm-vs-viper/

http://www.develapps.com/es/noticias/movver-ios-architecture

https://speakerdeck.com/serkrapiv/being-solid-2

https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)

[RxFlow Part 1](https://twittemb.github.io/swift/coordinator/rxswift/rxflow/reactive%20programming/2017/11/08/rxflow-part-1-in-theory/)

## Switchboard

vs Flux / React / 

[Dispatch](https://github.com/alexdrone/Dispatch)



## Adaptive Object Interfaces

Protocol-Oriented-Programming with Swift is the latest rage, or at least pitch, from Apple these days. I seems all the cool kids are doing it.

The hoopla is not unjustified. Protocols offer multiple advantages; they ...

- allow us to side-step the problems with multiple inheritance, 
- separate interface from implementation,
- and ...

If adapters are such nice light weight encapsulators then why don't we use them?

My theory is that it creates a significant cognitive overload for the developer when it requires her to know the class of the Adaptor to use for the particular class in hand; adapters, as typically implemented, create a combinatorial explosion of pairings between wrappers and wrapp-ees. And its way too much hassle to unwrap them to pass them back or out to other consumers. 

Always ask an object for an adaptor to it with a given Protocol or Class. The caller need to worry about implementation details (sort of like ObjC class clusters from the 'old days'). The object "knows" best how to provide the requested API and can transparently provide a subclass or particular Protocol conforming object.

What hierarchy would you create to model a politician/body-builder/actor?

The object may return

- Itself
- A component
- A wrapper of itself providing the interface

Related to

- Entity/Component - Seen in Game Frameworks including Apple's [GameplayKit](https://developer.apple.com/library/content/documentation/General/Conceptual/GameplayKit_Guide/EntityComponent.html)
- Protocol-oriented Programming


```swift
public protocol Adaptable {
    func canAdapt<T> (to: T.Type) -> Bool
    func adapt<T> (to: T.Type) -> T?
    func adapt<T> () -> T?
}
```

```swift
public protocol AdaptorProvider {
    func canAdapt<T> (to: T.Type) -> Bool
    func adapt<T> (to: T.Type) -> T?
    func adapt<T> () -> T?
}
```



#### Resources

[Protocol Oriented Programming](https://www.raywenderlich.com/148448/introducing-protocol-oriented-programming)

[Wikipedia: POP](https://en.wikipedia.org/wiki/Protocol_(object-oriented_programming))

[Transactional Memory](https://en.wikipedia.org/wiki/Transactional_memory)

#### Use Case

[Moving Safety into Types](http://www.figure.ink/blog/2017/10/15/moving-safety-into-types?utm_campaign=Swiftly&utm_medium=web&utm_source=Swiftly_13)



#### Extra Notes

![Scene Kit Loop](https://docs-assets.developer.apple.com/published/beef028d16/1b2e406d-c49e-4d11-9888-850579f5f8ab.png)