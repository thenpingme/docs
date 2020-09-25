# Frequently Asked Questions

## What is the difference between error tracking and task monitoring?
An error tracker - such as [Flare](https://flareapp.io), [Sentry](https://sentry.io), or [Bugsnag](https://www.bugsnag.com) - is the perfect tool to tell you when your application throws an exception, a database goes away, or a connection to a mail server times out.

Any of the above situations will be handled in your error tracking service and will inform you when a scheduled task fails mid-execution. What an error tracking service will _not_ tell you is when your scheduled task fails to run at all.

They certainly won't communicate that you forgot to add `schedule:run` to your latest project's crontab or give you any indication that a task is taking too long to finish.

## How much do you know about my app?
We capture a lot of telemetry from your application's scheduled tasks but capture nothing about the contents of the jobs or commands themselves. We will not inspect your jobs, what they're doing, or what they're operating on. It's not important to us what you're tasks are accomplishing, just that they are running on schedule or running at all.

For each task, we capture:

* The current release (if configured)
* The type of task (command, job, shell command, or closure)
* The cron expression
* The command being run
* Whether or not the task runs in maintenance mode
* Whether or not the task can run on multiple servers
* Whether or not the task can ovelap
* Whether or not the task runs in the background
* Whether or not the task has a truth test constraint
* The task description (if configured)
* The fingerprint for a task's execution
* The file and line number closure tasks are defined on (this is almost always `app/Console/Kernel.php`). The line numbers are used to aid in uniquely identifying closure-based tasks.

We also know your application's name, read from the `app.name` config value.

In addition, we capture:

* The hostname of the server running the task
* The local IP address of the server running the task
* The environment the task runs on, read from the `app.env` config value
* The timezone your app is running in

The thenping.me package is open source and available on [GitHub](https://github.com/thenpingme/thenpingme-laravel).

## My error tracker didn't notify me of failure, why is my task missing?
Without monitoring, it's difficult to tell why this might be the case, though. Laravel's task scheduler runs each configured task asyncrhonously; that is to say that each task must finish before the next one begins.

If you have two tasks scheduled to run hourly, both will fall into the same scheduled time slot and will be queued to run one after the other.

Should the first task take a long time to run, the second task may not start when it is expected to. In larger systems with many overlapping tasks, this can quickly get out of hand.

thenping.me helps to identify and resolve these overlapping tasks which can cause intermittent failures with no error tracking notifications.

## My error tracker didn't notify me of failure, why is my task failing?
By default, your scheduled tasks are configured to allow one minute of run time. Any tasks taking longer than this to run will be marked as `timedout`, and will raise an alert.

If you have long-running tasks, you can [configure](/docs/tasks#settings) a longer runtime threshold to avoid false-positive alerts. 

Be mindful of multiple tasks that fall onto the same schedule. As discussed in the previous FAQ, long running tasks that share a scheduled time slot may raise alerts due to long-running tasks.

## I expect my task to fail every now and then, how can I stop alerts?
[Configure](/docs/tasks#settings) a consecutive failure threshold for your task.


Setting this threshold will allow your scheduled task to fail a designated number of times before the failure is brought to your attention through a notification.

Set a threshold that allows for expected failures but doesn't hide any real problems with your executions.

## I added a new task to my application, but it's not being monitored!
Make sure that the [`thenpingme:sync`](/docs/artisan-commands#thenpingmesync) command has been added to your deploy process.

The sync command is responsible for making sure that we are monitoring tasks consistent with how they are configured in your `app/Console/Kernel.php` file.

## I changed the schedule for my task and lost all the history. What do I do?
Monitoring changes to task schedules is difficult because they are configured in code. We can't be sure if you've changed the task's schedule or added the same task on a different schedule.

Rather than guessing and making assumptions about your configuration, we play it safe and create a new task configuration. For the time being, if you need to link a changed task, [contact us](mailto:support@thenping.me).

## Not all of my scheduled tasks are showing up. What do I do?
Chances are you have some tasks that are configured to run only in specific environments.

If you have a task that only runs in production, but you ran the [`thenpingme:setup`](/docs/artisan-commands#thenpingmesetup) or [`thenpingme:sync`](/docs/artisan-commands#thenpingmesync) command in your local environment, the tasks will not have been picked up.

Always make sure you run the `thenpingme:sync` command from the environment you are monitoring wherever possible.

## How can I track releases with my tasks?
You can [configure](/docs/configuration) your application to send the release to us on each ping using the `thenpingme.release` key.

We provide a sample for this in the published `config/thenpingme.php` file:

```php
trim(exec('git --git-dir ' . base_path('.git') . ' log --pretty="%h" -n1 HEAD')),
```

This assumes that you are deploying your application to production using some variation of a git-based strategy.

If you are using an environment where you do not deploy via git, you should consider committing a `.release` or `.version` file or into production. On deploy, you may read the contents of this file as the release value:

```php
is_file($version = base_path('.version')) ? trim(file_get_contents($version)) : null,
```

For those of you deploying to [Vapor](https://vapor.laravel.com), you can use the `VAPOR_ARTIFACT_NAME` environment variable:

```php
env('VAPOR_ARTIFACT_NAME'),
```

## How do you know that the 🤩 plan is the most popular?
This plan was built around the inclusions most of the people we spoke with about thenping.me very early on were after.

## Can I actually pay in 🤔, 🤩, and 🥰?
When fully launched, we will not be accepting payment in the form of emoji. Sorry!
