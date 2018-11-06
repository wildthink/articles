## Application Design

This document offers a collection of concepts, patterns, notes and resources for the development of UI/UX applications. These concepts and patterns reflect my approach to application design in general but the resources and examples are specific to the Apple ecosystem (iOS/OSX) unless otherwise indicated.

What exactly are the parameters of the application?

Do we need to "hot reload" code or just parameters? Perhaps both but one in certainly easier than the other. And more often than not its the parameters that we end up tweaking while running; styles, layouts, navigation, data.

Partition the application parameters.

#### Linchpins

Doman Model Schema

##### Named UI Contexts and Components

Names/Identifiers instead of references/pointers. Fully qualified by scene, group, name, property

#### Dimensions

By definition "architectural dimensions" are orthogonal; changes in one do not influence nor depend on changes in the others.

- Styling - Colors, fonts, shadows, rounded corners
- Localization / i18n
- Data Formatting - combines style-forms and Localization
  - Dates
  - Time Intervals
  - Numbers
  - Units/Measurements
  - Location
  - Phone Number
  - Inflector (Pluralization)
  - Currency
  - Person Names
  - Organization Names
  - Addresses
- Accessibility - 508 Compliance
- Scene - view hierarchy, logical composition, layout independent
- Layout - spacing, sizing, etc. includes font sizes
- Navigation Controllers
  - Routes between scenes (Segues)
  - Data handoff
  - Visual Transitions
- Data Bindings - Domain values to renderings
  - Data -> (Formatter) -> String -> (Label | TextField) 
  - Data -> Image -> ImageView
  - Data -> (Transformer) -> Widget.property
- User Input
  - Controls
  - Gestures
  - Domain Commands / @IBAction
- Network Services
  - Authentication
  - Requests (eg. GET, POST)
  - Object marshaling (see Schema.Mappings)
- Schema
  - Types
  - Relationships
  - Default Values
  - ACL
  - Validation Rules
  - Default Formatters
  - Mappings - Relational|JSON|REST|GraphQL <-> DomainModel
- Domain Logic
- Application Reporting (Logging and Metrics) - Calls to Action

- Application Data (base)
  - Security / Access Control / Encryption
  - User preferences
  - Persistance
  - Caching
  - Synchronization
- Application State
  - Navigation - "route" to current scene
  - Layout - any parts hidden?
  - Domain value(s) for each scene in the route
  - Recall `autosaveName` 
  - Easy to archive the `query` instead of the `object` !!

#### States

- Domain model values
- UI parameters
  - layout
  - style
  - Visible/hidden
- Navigation State
  - Current scene
  - Transition status
  - Next scene

#### Input Events

- Touch/mouse gestures
- Keyboard
- Network
- Timer
- Microphone
- Other Sensors

#### Output Events

Output is, by definition, ephemeral. Thus, distinctions in display should distinguish between those that are "events" versus a direct reflection of state. The emission event may be documented but otherwise leaves no lasting state within the system.

- Video / Animation
- Sound / Voice
- Network

#### TeXInfo

Node Specification

- Title
- siblings
- parent
- actions

Node Navigation

- Up
- Down / Into
- Siblings Next / Previous

##### Resources

[GNU Texinfo](https://www.gnu.org/software/texinfo/)

[Wiki Texinfo](https://en.wikipedia.org/wiki/Texinfo)

[Siesta](https://bustoutsolutions.github.io/siesta/) - The elegant way to write iOS / macOS REST clients

[Wikipedia: Localization](https://en.wikipedia.org/wiki/Internationalization_and_localization)

[ObjC FormatterKit](https://github.com/mattt/FormatterKit)

[Stylist](https://github.com/yonaskolb/Stylist)

##### Additional Reading

- [Weaver: A Painless Dependency Injection Framework For Swift](https://medium.com/scribd-data-science-engineering/weaver-a-painless-dependency-injection-framework-for-swift-7c4afad5ef6a)
- [Dependency Injection Demisifyied, James Shore, 03/22/2006](http://www.jamesshore.com/Blog/Dependency-Injection-Demystified.html) ¹
- [Swift-AST](https://github.com/yanagiba/swift-ast)
- [AirBnB: Visual Design Language](https://airbnb.design/building-a-visual-language/)
- [Transactional Memory](https://en.wikipedia.org/wiki/Transactional_memory)
- [ObjC.io: Separate UI & Model](https://www.objc.io/blog/2018/06/19/separating-ui-and-model-code/)



 
