---
title: Test methods
layout: default
---


# About the diagrams and descriptions #
Time axis is vertical, from up downwards. Steps are grouped in runs, which are separated by a vertical dotted line.

A 'step' is an operation, either
  * data flow
  * logic (decision - can be random-based)
  * control (including generating contents, input values, page navigation)
  * comparison (of the actual and expected contents)

The focus is on problems and complications in the testing process. Therefore diagrams omit any successful steps that are not relevant to demonstrating a problem.

Diagrams are as cut down as possible. There can be many variations of the same situation, all with the same idea. For example, there may be more/less buggy steps, but the overall pattern is the same. If a diagram doesn't indicate separate test/app sessions between runs, it's likely that a similar pattern applies, even if some of the runs happen to be in separate test/application sessions.

# Perspective of correctness and side effects #
When demonstrating a feature/pattern of the QA process, it's often irrelevant whether a particular step is fully correct. So, if a step is 'correct enough' for the purpose of the example, it's drawn in black (or blue or yellow).
Sometimes it's not obvious whether a step should be presented as correct. In such cases I deem the correctness in the local context. An example is the first diagram with its second test run. Application data has been affected by the first test run and it is correct from the viewpoint of the app. The app data may seem to be incorrect for the test, but that's because the demonstrated test framework is incapable of tracking app data (as opposed to SeLite). So I colour the app data as correct, and the test comparison as incorrect (false negative).

Referring to steps and their effects/side effects
  * if effects of a step are important and I refer to them later on, I colour the step in blue or yellow
  * a correct step can have  side effects/knock on effects that are not relevant to the feature/pattern in question

# Legend #
The following legend applies to all diagrams here ![Basic legend](https://raw.githubusercontent.com/selite/selite/master/diagrams/legend_basic.png).

# Test has no DB #

## Separate test runs don't account for previous actions ##
Since the test has no DB, it 'loses the track' when a testing session ends. Successive separate test sessions will not know what will have been applied to the tested DB by earlier runs.

![App with no bug fails on further runs](https://raw.githubusercontent.com/selite/selite/master/diagrams/test_has_no_data/app_no_bug_fails_further_runs.png)

## A test failure confuses further tests ##
If a test run fails, it may confuse successive test runs which would succeed otherwise (false negative).

![App with a bug fails further tests](https://raw.githubusercontent.com/selite/selite/master/diagrams/test_has_no_data/app_bug_fails_all_runs.png)

# Test has backdoor access to app DB #
If a test doesn't detect a bug straight away, it may not be discovered even though it made app data incorrect. The test framework depends on the backdoor data from the app DB. That shared data fools it - the test can't detect the incorrect data, because it has no other dataset to compare against. This is the main reason for SeLite.
![A bug goes undetected](https://raw.githubusercontent.com/selite/selite/master/diagrams/test_backdoor_data/app_bug_goes_undetected.png)

# Test keeps a separate replica of app DB (SeLite) #
Just as with other test frameworks, if a test fails, it may affect further tests (which would succeed otherwise).

![App bug fails further tests](https://raw.githubusercontent.com/selite/selite/master/diagrams/test_has_data/app_bug_fails_all_runs.png)

## Knock-on effect of hidden errors ##
This is a type of bugs that can't be generally and easily detected without test having its own DB. SeLite is beneficial here.
![App bug fails further tests](https://raw.githubusercontent.com/selite/selite/master/diagrams/test_has_data/app_bug_fails_further_runs.png)