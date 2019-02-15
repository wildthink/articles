## iOS UDF in ~100 Lines of Code

It appears that few are aware of a "message bus" that is built into every iOS application; the **Responder Chain**. It's not just for IBActions and event processing; it can be quite useful in a number of scenarios. Utilizing this graph, we will implement a UDF framework in less than 100 lines of code! This framework, implemented with a single class and 2 extension methods, can support multiple `Stores` and can easily be extended with your own custom action functions. In addition, updates to the store can be applied in a single transaction as are all calls to update the UI based on the new state of the store.

#### What is Unidirectional Data Flow?

We will try to make this article self-contained but some knowledge of the UDF design pattern might be helpful to your understanding. If you need a more detailed explanation please see [Flux, Application Architecture for Building User Interfaces](https://facebook.github.io/flux/docs/in-depth-overview.html#content).

Briefly, the UDF pattern is designed to address issues related to the update of the an application's interface (views) that are driven by changes in the state of the underlying model.

> "We found that two-way data bindings led to cascading updates, where changing one object led to another object changing, which could also trigger more updates. As applications grew, these cascading updates made it very difficult to predict what would change as the result of one user interaction. When updates can only change data within a single round, the system as a whole becomes more predictable." ([FB](https://facebook.github.io/flux/docs/in-depth-overview.html#content))
>

In one direction, these bindings map model values to viewable elements; displaying a person's name in a text field, for example. In the other direction, editing the value in the text field updates the person's name in the model to a new value. This, in turn, may trigger other operations that update different model values, *ad infinitum*. In some cases, this can create feedback triggering multiple unnecessary display refreshes creating lags in application responsiveness.

UDF looks to reduce the complexity of wiring up the component views and view controllers and to break the feedback loop caused by the <u>automatic</u> triggering of display refreshes caused by interdependent observer chains watching for changes in the model state.

We eliminate the need for explicit dependency injection (passing references down through the view hierarchy during construction) by using the responder chain which is already in place as the message router. This makes it easy for any UIResponders in the hierarchy to opt-in to provide services or settings without the need to refactor any containing components.

In addition, we implement the dispatching mechanism such that if can gather all updates to the store into a single step so that updates to the views are also executed in a single step so that we can insure the store is immutable for the duration of the updates.

#### Design Overview

![data flow in Flux with data originating from user interactions](https://facebook.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png)

Each view refresh cycle is composed of the following functions:

​	Update Store -> Update Responders -> Update Controllers -> Update Views

#### A Simple Store

> Stores contain the application state and logic. Their role is somewhat similar to a model in a traditional MVC, but they manage the state of many objects — they do not represent a single record of data like ORM models do. … stores manage the application state for a particular **domain** within the application.
>
> ([FB](https://facebook.github.io/flux/docs/in-depth-overview.html#content))


Only two methods are needed to create the simplest `Store`. The addition of the `@objc protocol`  provides a bridging function for `UIResponders` to call during their `update(...)`.

```swift
protocol Store {
    associatedtype S = Self
    static var shared: S? { get }
    func read<T> (_ key: String) -> T?
    func update(f: @escaping (S) -> Void)
}

@objc protocol ReadableStore {
    func read(_ key: String) -> Any?
}
```

Any view or controller that <u>declares</u> that it is bound to store or state variable is <u>implicitly</u> subscribed to its updates. As views and controllers come and go at the whim of the user this becomes a significant advantage in reducing complexity and error.

#### Simplify the Dispatch

#### Eliminate Actions by bringing the target to the caller.

An `Action` in the UDF environment is really just a serialized form of a particular function identifier along with its parameters. This `action` is passed along to some target by the `Dispatcher`. Then the target must deserialize the `Action` and implement its intent. Not only does this incur CPU and memory costs but it also requires additional code and cognitive overhead to create, maintain, and utilize each explicitly specified `Action`.

How might this be avoided? If the target of our action method was in-hand we could just call the function directly. But explicitly wiring up the caller to the target introduces complexities that we are seeking to avoid with a central Dispatcher.

Suppose, instead, the Dispatcher's job is to facilitate a rendezvous between the caller and the callee dynamically at call time. All without pre-wiring or instantiating Actions. To effect this, the caller requests a reference to the target, a `Store` in our case, by specifying the Class or Protocol, with this strongly typed reference the caller can then invoke any exposed function it requires. The caller never retains the target but can always get it just-in-time for the call.

By creating a direct connection between the caller and target on-demand and only for the duration of the call we avoid any need to explicitly define much less create Action objects to "carry" values to the message receiver. The "Dispatcher" may, at its discretion, store the parameters or create a closure to delay the action's execution. Note, this logic is encapsulated in the Dispatcher so the caller need not concern itself with any explicit instantiation of an `Action` object.

In our example, user actions are translated into operations on the store at the earliest opportunity. Action code blocks are queued up executed in a single phase, callback free. If desired access to the store can be formally "locked" effectively wrapping all the updates in a single transaction.

So Simple. No `Actions` required!

#### Single Function Store Lookup

Message recipients can be identified by Class, Protocol, or by an arbitrary test.


```swift
extension UIResponder {

    func myStore<S: Store>(if f: ((S) -> Bool) = {_ in return true}) -> S? {
        var target: UIResponder? = self
        while target != nil {
            if let store = target as? S, f(store) {
                return store
            }
            target = target?.next
        }
        return S.shared as? S
    }
}
```

#### Update the Display in a single phase

UIViews and UIViewControllers need <u>not</u> explicitly `register()` or` unregister()` for `update(withStore:)` calls if they are part of the View Hierarchy. It is rare that you will want to update views or controllers not in the hierarchy and it is not recommended. Each update to the Store queues up a call the the `Store.updateAppliction()` function. Multiple queue entries are coalesced into a single `updateApplication()` call during each display cycle.

By definition, views that are not in the view hierarchy are not visible nor rendered. If they are not rendered, they need not be updated. Tapping into the native view graph means that no secondary or corresponding graph need be built nor maintained.



```swift
extension UIResponder {
    @objc func update (with store: ReadableStore) {}
}
```



#### Simple Store - Full Implementation

```Swift
class SimpleStore: UIResponder, Store, ReadableStore {

    typealias UpdateAction = (SimpleStore) -> Void
    static var shared: SimpleStore?

    public private(set) var values: [String: Any] = [:]
    var responders = Set<UIResponder>()
    var updates = [UpdateAction]()
    
    // MARK: ReadableStore Conformance
    func read(_ key: String) -> Any? {
        return values[key]
    }
    // MARK: Store Conformance
    func read<T> (_ key: String) -> T? {
        return values[key] as? T
    }
 
    func update(f: @escaping (SimpleStore) -> Void) {
        updates.append(f)
        // Peforming a "delayed" action allows up to coalesces multiple 
        // updates into a single call in the run loop cycle
        let cmd = #selector(updateApplication)
        NSObject.cancelPreviousPerformRequests(withTarget: self,
                                               selector: cmd, object: nil)
        perform(cmd, with: nil, afterDelay: 0)
    }

    func set(_ key: String, to value: Any) {
        values[key] = value
    }

    func register(responder: UIResponder) {
        responders.insert(responder)
    }

    func unregister(responder: UIResponder) {
        responders.remove(responder)
    }

    @IBAction func updateApplication() {
        updates.forEach { $0(self) }
        updates.removeAll()

        responders.forEach { $0.update (with: self) }

        update(controller: UIApplication.shared.keyWindow?.rootViewController)
        update(view: UIApplication.shared.keyWindow?.rootViewController?.viewIfLoaded)
    }

    func update (controller: UIViewController?) {
        guard let controller = controller else { return }
        controller.update(with: self)
        controller.childViewControllers.forEach { update(controller: $0) }
    }

    func update (view: UIView?) {
        guard let view = view else { return }
        view.update(with: self)
        view.subviews.forEach { update(view: $0) }
    }
}
```



##### Links

[Apple:  Understanding Event Handling](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/understanding_event_handling_responders_and_the_responder_chain)

[Medium: Understanding cocoa and cocoa touch responder chain](https://medium.com/ios-os-x-development/understanding-cocoa-and-cocoa-touch-responder-chain-12fe558ebe97)

[State Container Based Architecure](https://jobandtalent.engineering/ios-architecture-an-state-container-based-approach-4f1a9b00b82e)

[Real World Flux iOS](http://blog.benjamin-encz.de/post/real-world-flux-ios/)

[What is Flux](http://fluxxor.com/what-is-flux.html)

[VueFlux](https://github.com/ra1028/VueFlux)

[Transactional Memory](https://en.wikipedia.org/wiki/Transactional_memory)

[Unidirectional Data Flow? Yes. Flux? I Am Not So Sure.](https://medium.com/@AdamRNeary/unidirectional-data-flow-yes-flux-i-am-not-so-sure-b4acf988196c)

##### Alternatives

[Weaver](https://github.com/scribd/Weaver)

```
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Swift                           93           1678            946           6103
Markdown                         7            168              0            315
JSON                             1              0              0             93
Objective C                      1             26              7             64
make                             1              9              0             28
YAML                             3              0              0             27
Bourne Shell                     1              6              0             19
C/C++ Header                     3             14             20             11
-------------------------------------------------------------------------------
SUM:                           110           1901            973           6660
-------------------------------------------------------------------------------
```

[Swinject](https://github.com/Swinject)

```
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Swift                          142           2440           2920          12410
Markdown                        36           1830              0           4946
Objective C                     15            218            121            728
C/C++ Header                    10            173            171            634
C                                1            103             45            389
YAML                             6              4              2            224
ERB                              2             14              0            126
Ruby                             1             12              0             60
Bourne Shell                     1              2              0              6
-------------------------------------------------------------------------------
SUM:                           214           4796           3259          19523
-------------------------------------------------------------------------------
```

FB flux![unidirectional data flow in Flux](https://facebook.github.io/flux/img/flux-simple-f8-diagram-1300w.png)

![data flow in Flux with data originating from user interactions](https://facebook.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png)
