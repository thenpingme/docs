---
title: Configuration
---
thenping.me aims to be a zero-configuration tool for starting to monitor your application's scheduled tasks. It does, however, allow you to configure certain aspects using either [environment variables](#environment-variables) or [task-specific configuration](#task-configuration).

<a name="environment-variables"></a>
## Environment variables

### `THENPINGME_ENABLED`

If you need to disable sending pings, for example staging or development environments, you may set this value to `false` in your environment to prevent pings being dispatched. 

Note that we will still expect your application to ping at configured intervals, so ensure you have at least one active environment sending pings to avoid false-positive alerts. 

### `THENPINGME_PROJECT_ID`

Identifies your application to thenping.me when it sends its ping payloads. This is automatically set when you run the [`thenpingme:setup`](/docs/installation) command for the first time.

Ensure that this is set in your environment wherever you run the `thenpingme:sync` command.

### `THENPINGME_SIGNING_KEY`

Is used to sign the payloads sent from your application to thenping.me, in order to verify the integrity of the data being transmitted. This is automatically set when you run the `thenpingme:setup` command for the first time.

Ensure that this is set in your environment wherever you run the `thenpingme:sync` command.

### `THENPINGME_QUEUE_PING`

By default, the package will dispatch all pings to the queue for asyncrhonous processing. If you would prefer the tasks be dispatched immediately, you may set this value to `false`.

### `THENPINGME_QUEUE_CONNECTION`

If you wish to send your ping jobs on a specific queue connection, you may specify a value from your queue configuration here. By default, we will use `config('queue.default')`.

### `THENPINGME_QUEUE_NAME`

If you want to specify a different queue to send your ping jobs to for processing, you may specify a value from your queue configuration here. By default, we will use the queue name from your configured queue connection.

> The job will be dispatched on the `default` queue, so ensure you have a queue worker for this queue running.

### `THENPINGME_API_URL`

This variable can be used to override the endpoint your application communicates with when sending pings.

You will not typically need to change this value.

## Publish config

If you would like to further customise any of thenping.me's configuration options, you may do so by first publishing the package configuration.

```
php artisan vendor:publish --tag=thenpingme-config
```

This will publish the configuration to `config/thenpingme.php`.

The most common configuration you will make in this file will be setting of the `release` value, if you would like to [track your application release/version](/docs/faq#track-releases-with-tasks) along with your pings.

<a name="task-configuration"></a>
## Task configuration <dl class="ml-3 mt-1.5 align-top inline-flex items-center px-3 py-1 rounded-full text-sm font-medium leading-4 bg-indigo-100 text-indigo-900 tracking-tight"><dt class="sr-only">Tailwind CSS version</dt><dd>v3.0+</dd></dl>

Settings may be configured on a per-task basis directly within your scheduled task configuration in your application code.

This puts you in control of how your tasks are monitored right alongside where you define the task schedule.

```php
// app/Console/Kernel.php

protected function schedule(Schedule $schedule)
{
    $schedule
        ->command(CheckForMissingTasks::class)
        ->everyMinute()
        ->thenpingme(
            grace_period: 2,     // In minutes
            allowed_run_time: 1, // In minutes
            notify_after_consecutive_alerts: 2,
        );
}
```

### Project defaults <dl class="ml-3 mt-1.5 align-top inline-flex items-center px-3 py-1 rounded-full text-sm font-medium leading-4 bg-indigo-100 text-indigo-900 tracking-tight"><dt class="sr-only">Tailwind CSS version</dt><dd>v3.1+</dd></dl>

Relatedly, you may also configure project-wide setting defaults directly within your application.

These values, when set, will be the default for all tasks in your project and take precedence over the default values used by thenping.me.

Task-specific settings will take precedence over any project defaults.

```php
// config/thenpingme.php
return [
    'settings' => [
        // How much time, in minutes, should be allowed to pass before a task is considered late
        'grace_period' => env('THENPINGME_SETTING_GRACE_PERIOD', 1),

        // How much time, in minutes, should a task be allowed to run before it is considered timed out
        'allowed_run_time' => env('THENPINGME_SETTING_ALLOWED_RUN_TIME', 1),

        // How many consecutive alerts should occur before you wish to be notified
        'notify_after_consecutive_alerts' => env('THENPINGME_SETTING_NOTIFY_AFTER_CONSECUTIVE_ALERTS', 1),
    ],
]
```

<a name="skipping-tasks"></a>
### Skipping tasks <dl class="ml-3 mt-1.5 align-top inline-flex items-center px-3 py-1 rounded-full text-sm font-medium leading-4 bg-indigo-100 text-indigo-900 tracking-tight"><dt class="sr-only">Tailwind CSS version</dt><dd>v3.1+</dd></dl>

If there are any tasks you do not wish to monitor at all, you may use the `skip` configuration option.

When skipped, the task will not be sent as part of the `thenpingme:setup` or `thenpingme:sync` commands, nor will it appear in the `thenpingme:schedule` or `thenpingme:verify` output.

```php
// app/Console/Kernel.php

protected function schedule(Schedule $schedule)
{
    $schedule
        ->command(SomeCommandThatShouldNotBeMonitored::class)
        ->hourly()
        ->thenpingme(
            skip: true
        );
}
```

<a name="output-logging"></a>
### Output logging <dl class="ml-3 mt-1.5 align-top inline-flex items-center px-3 py-1 rounded-full text-sm font-medium leading-4 bg-indigo-100 text-indigo-900 tracking-tight"><dt class="sr-only">Tailwind CSS version</dt><dd>v3.2+</dd></dl>

<div class="p-4 shadow sm:rounded-md bg-blue-50">
    <span class="text-md text-blue-700">
        <b>Note</b>: Output logging may only be configured in your application's task schedule as command output is pushed to thenping.me.
    </span>
</div>

On [supported plans](https://thenping.me/#pricing), you may enable output logging on a per-task basis.

Of course, this is useful for monitoring the output of your scheduled tasks in general, but can be configured to capture output only on failure. 

When a task failure raises an alert, the output will be included with the alert notification giving you clear context surrounding the task failure.

| Configuration Level      | Description                     |
| :----------------------- | :------------------------------ |
| `Thenpingme::STORE_OUTPUT` | Store all scheduled task output |
| `Thenpingme::STORE_OUTPUT_ON_SUCCESS` | Store scheduled task output only on successful task finish or skip |
| `Thenpingme::STORE_OUTPUT_ON_FAILURE` | Store scheduled task output only on non-zero exit code, or scheduled task failure |
| `Thenpingme::STORE_OUTPUT_IF_PRESENT` | When used <a href="#conditional-output-logging">in combination with</a> the any of the above options, output is logged only when it is not empty |

```php
// app/Console/Kernel.php
use Thenpingme\Thenpingme;

protected function schedule(Schedule $schedule)
{
    $schedule
        ->command(SomeCommandThatShouldNotBeMonitored::class)
        ->hourly()
        ->thenpingme(
            output: Thenpingme::STORE_OUTPUT,
        );
}
```

**Note**: Whilst you may configure tasks to send output at any time, only plans that [support output logging](/#pricing) will display the output and include it in alert notifications.

<a name="conditional-output-logging"></a>
### Conditional output logging <dl class="ml-3 mt-1.5 align-top inline-flex items-center px-3 py-1 rounded-full text-sm font-medium leading-4 bg-indigo-100 text-indigo-900 tracking-tight"><dt class="sr-only">Tailwind CSS version</dt><dd>v3.2+</dd></dl>

By leveraging [bitwise operations](https://www.php.net/manual/en/language.operators.bitwise.php), you may choose to log task output only when it is present. 

This is useful in times when you have a known-failure scenario that produces a non-zero exit code but does not render any output. 

Receiving blank task output in this scenario may lead to confusing debugging scenarios.

```php
// app/Console/Kernel.php
use Thenpingme\Thenpingme;

protected function schedule(Schedule $schedule)
{
    $schedule
        ->command(CommandThatSometimesOutputs::class)
        ->hourly()
        ->thenpingme(
            output: Thenpingme::STORE_OUTPUT | Thenpingme::STORE_OUTPUT_IF_PRESENT,
        );
}
```
