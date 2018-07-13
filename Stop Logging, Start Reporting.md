## Stop Logging. Measure or Call to Action!

Target your audience and tell them what they can do.

Replacing standard log levels with a target audience and a clear call-to-action can make



The fundamental purpose of reporting is to either gather metrics or to elicit a "fix" or action that the application can't handle itself.

#### Audience - Category - Cost (CPU/log size)

User - Support - Developer

Milestones - marks the progress of a process or activity, a good point to gather metrics

Trace - Small, incremental steps in execution

Limiting or Blocking - security, permissions, resource scarcity (file space, bandwidth)

Deadlock

Unexpected Value

Data Corruption - purge corrupted data, reload if possible

Application Corruption - > restart the application

Crash

The parameters of the report should identify data key to effecting a solution

=========

[To Log or Not to Log](https://getpocket.com/a/read/2187810674)

[IBM: Understanding Log Levels](https://www.ibm.com/support/knowledgecenter/en/SSEP7J_10.2.2/com.ibm.swg.ba.cognos.ug_rtm_wb.10.2.2.doc/c_n30e74.html)

#### Info

These messages are lifecycle events. They highlight the progress of an application.

`Example:` *application start/stop*; *order completion in OMS*

#### Debug

These messages help in figuring out weird stuff. Logging at *debug* level means tracking state changes at every step of an application.

`Example:` Let’s say a driver accepts order abc, but OMS assigns order def. To find root cause of issues, log all events: *creation of order*, *list of eligible drivers for an order*, *acceptance of booking by a driver*, *saving state in OMS.*

#### Warn

These messages are potentially harmful situations. These issues need to be fixed, but may not require immediate intervention.

`Example:` Let’s say OMS talks to a payment service for paying drivers. On breach of SLA with payment service, OMS will timeout and retry the request. Log at warning level: *payment service SLA breached. Retrying.*

#### Error

These messages are of considerable importance. Log at *error* level when normal flow of execution is blocked requires human intervention. You should keep an eye out for error logs in order to fix issues manually in order to maintain consistency in the system.

`Example:` If OMS has exhausted, driver payment retries to payment service, Log *Error paying driver XYZ for order abc.* Later, pay the driver manually.

#### Fatal

These messages are severe events that might cause the application to terminate. There should be an immediate action when application logs at fatal level.

`Example:` *Running out of disk space*, *total loss of network connectivity*

#### Panic

These messages are exceptional scenarios which should be fixed immediately. Log this when an application panics and terminates, or recovers.

`Example:` *database wasn’t configured*, *number divided by by 0*

 