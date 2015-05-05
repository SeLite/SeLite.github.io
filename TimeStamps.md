---
title: Dealing with timestamps
layout: default
---


# Summary #
Most web apps display timestamps - when a record/item/comment... was created, modified or deleted. Good QA should test such timestamps between the app and the test even if
  * their clocks are not synchronized, or their synchronisation fluctuates
  * they run in different timezones
  * one or both of them run during daylight saving change
  * their performance or network latency varies, or the time difference between them varies
[ExtraCommands](ExtraCommands) allows you to do most of this robustly and easily. (Support for timezones depends on Mozilla implementing some of [ThirdPartyIssues](ThirdPartyIssues)).

# Why testing/validating timestamps is not trivial #
## Getting test timestamps ##
The test drives the application, which records a timestamp (in the app DB). That timestamp will show up on the next or later page (or on the same page in an element updated via ajax). The test could parse that timestamp and store it in test DB - but that could prevent it from detecting app errors. So, the test always generates its own timestamps and uses them to validate parsed app timestamps. Test should also store them in test DB for future validation of the app.

## Issues with timestamps ##
The above situations cause
  * app and test timestamps to be different between app and test
  * two or more subsequent app timestamp, as displayed, to be the same
    * if those two or more operations took less than the smallest displayed unit of time (timestamp granularity)
    * thus the app can't be validated as correct
[ExtraCommands](ExtraCommands) solves it by introducing delays, where needed for robust comparison of parsed app timestamps and generated and stored test timestamps. The Selenese commands are _sleepUntilTimestampDistinctDownToMilliseconds_, _sleepUntilTimestampDistinctDownToSeconds_ and _sleepUntilTimestampDistinctDownToMinutes_.

If you are testing performance/responsiveness of the web application, you could balance the negative effect of delays by throttling down application resources.

## Timezones ##
Will try to implement for Firefox 27 (Internationalisation API).

TODO time/speed reporting by test - stop/pause clock, which excludes the timestamp robustness wait time

# Configuration of time sync difference #
[ExtraCommands](ExtraCommands) adds field _maxTimeDifference_ to [Settings](Settings) module _extensions.selite-settings.common_. It's  the max. time difference between the server and your test machine (in milliseconds).

Beware: If you run some test(s) that record timestamps, and then you decrease maxTimeDifference later below a threshold needed by the already recorded timestamps, the recorded timestamps may cause false positive errors later. To prevent that either
  * restart Selenium IDE, or
  * wait until for a period equal to precision of those timestamps plus previous maxTimeDifference
after you decrease maxTimeDifference.