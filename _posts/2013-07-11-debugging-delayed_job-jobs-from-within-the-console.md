---
title: "Debugging delayed_job jobs from within the console"
date: "2013-07-11"
---

Did you ever want to step in with the Ruby debugger to fix or inspect a background job? Here’s how:

**DISCLAIMER**: Tested with `delayed_job` 2.14. Should work with recent versions, too.

Open a rails console within your project

```
$ rails c
Loading development environment (Rails 3.2.13)

```

Initialise a delayed\_job worker, and make it output to console by passing `{quiet:  false}` to its initialiser.

```
1.9.2p325 :001 > worker = Delayed::Worker.new({quiet: false})
 => #

```

Start the worker

```
1.9.2p325 :002 > worker.start

```

Now you can put a `debugger` statement into your jobs and debug nicely without using loggers.
