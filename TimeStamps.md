---
layout: default
---
{% include links %}
* TOC
{:toc}

# Summary #
Most web apps display timestamps - when a record/item/comment... was created, modified or deleted. Good automation or QA should deal with timestamps between its scripts and the app even if

  * their clocks are not synchronized, or their synchronisation fluctuates
  * they run in different timezones
  * one or both of them run during daylight saving change
  * their performance or network latency varies, or the time difference between them varies

[Commands](Commands) allows you to do most of this robustly and easily. (Support for timezones depends on Mozilla implementing some of [ThirdPartyIssues](ThirdPartyIssues)).

# Why timestamps are not trivial

## Getting script timestamps
The [script] drives the application, which records a timestamp (in [app DB]). That timestamp will show up on the next or later page (or on the same page in an element updated via ajax). The script could parse that timestamp and store it in its DB - but that could prevent it from detecting app errors. The script, therefore, always generates its own timestamps. Then it uses them to validate timestamps parsed from the application.

## Issues with timestamps ##
The above situations cause

  * app and script timestamps to be different
  * two or more subsequent app timestamp, as displayed, to be the same
    * if those two or more operations took less than the smallest displayed unit of time (timestamp granularity)
    * thus the app can't be validated as correct

[Commands](Commands) solves it by introducing delays, where needed for robust comparison of parsed app timestamps and generated and stored script timestamps. The Selenese commands are `sleepUntilTimestampDistinctDownToMilliseconds, sleepUntilTimestampDistinctDownToSeconds` and `sleepUntilTimestampDistinctDownToMinutes`.

If you are testing performance/responsiveness of the web application, you could balance the negative effect of delays by throttling down application resources.

## Timezones ##
Will try to implement for Firefox 27 (Internationalisation API).

TODO time/speed reporting by scripts - stop/pause clock, which excludes the timestamp robustness wait time

# Configuration of time sync difference #
[Commands](Commands) adds field `maxTimeDifference` to [Settings](Settings) module `extensions.selite-settings.common`. It's  the max. time difference between the server and your script machine (in milliseconds).

Beware: If you run script(s) that record timestamps, and then you decrease `maxTimeDifference` later below a threshold needed by the already recorded timestamps, the recorded timestamps may cause false positive errors later. To prevent that either

  * restart Selenium IDE, or
  * wait until for a period equal to precision of those timestamps plus previous `maxTimeDifference`

after you decrease `maxTimeDifference`.