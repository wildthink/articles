## Testing with Data Snapshots

Rarely are we testing functions that don't have side effects; mutable internal states. In testing components of an application we set up a mock application state around the component, invoke some high level functions and then check some "interesting" application state for some expected or unexpected values. 

The design of most testing frameworks pushes the developer toward verifying only a relative handful of values at a time. The developer typically fat-fingers in some "known" values which are generally restricted to ones they can easily calculate in their head. The reality is that there is likely to be many more values in the application state that would be appropriate to test but the shear number makes this an overwhelming task.

So if a particular function has the potential to effect more than one value, it certainly behoves us to check as many relevant values (or states) as possible. In our Object Oriented world these values can be found in many different points in an in-memory object graph.

Many of today's Obect-Oriented languages offer some form of reflection into the class structure which can enable a dynamic key path lookup off values deep into an object graph.



Is you're not familiar with FBSnapshotTesting you should definitey check it out. 

[Automated UI Tests in iOS Apps](https://jakubturek.com/automated-ui-tests-in-ios-apps/)

[Snapshot Testing](https://www.objc.io/issues/15-testing/snapshot-testing/)



