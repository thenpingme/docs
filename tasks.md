# Tasks

Tasks are a one-to-one mapping of the scheduled tasks configured in your application.

For each task, we will track the
* Command; whether that be an Artisan command, job, shell command, or closure,
* Configured schedule, translated for convenience
* When the task is next due
* When the task was last updated
* Task-specific configuration such as whether the task is allowed to overlap, run in maintenance mode, on one server, in the background, or has a truth test constraint.

## Dashboard

The current time - in the application's timezone - next run time, and run late time - in browser-local timezone - as well as the configured cron schedule are displayed.

Like the project dashboard, the task dashboard will isplay metrics and insights for an individual task. This will show the total number of executions, average runtime, memory usage, and raised alerts over the preceding 30 days.

The execution history is time-limited based on your team's plan, however the data presented is identical.

For each execution, you will be able to see:
* The name of the server
* The local IP address of the server
* The environment that the task ran in
* The exit status of the task
* The started and finished time
* The memory consumed
* The total runtime
* The execution status

> Tasks that were skipped due to the presence of a truth test constraint will show as such, and will not have any start or finish time, memory, or runtime data associated.


## Settings

Not all tasks are created equal.

Tasks that run once a week, or once per month are likely to take more time to run than those that are scheduled to run daily, hourly, or every five minutes.

To that end, thenping.me provides some configuration options for runtime.

### Warn if late
A number of minutes that we allow as a grace period after which we consider your task to be missing.

### Allowed run time
Reporting tasks that run infrequently are typically responsible for running large queries, and aggregating lots of data. This, naturally, leads to tasks that can take longer to run and is indeed expected behaviour.

Configure the number of minutes we should wait before considering the task to be failing.

### Consecutive failures threshold
For tasks that run more frequently, it may not be as critical that each and every execution is on time, completes successfully, or within a given runtime allowance.

To account for this, you may specify a threshold for consecutive failures that must breached before we raise an alert that it is missing.

For example, if you were to configure a consecutive failure threshold of 3, we will raise an alert _on_ the third failure.

## Status

A task can exist in one of six states:

<div class="flex flex-wrap space-y-2 md:space-y-3">
    <div class="flex w-full">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-gray-800 bg-gray-100 rounded-full leading-5">Pending</span>
        </div>
        <div class="w-5/6">
            We are waiting for your task to ping for the first time.
        </div>
    </div>
    <div class="flex w-full pt-2 border-t border-gray-300 md:pt-3">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-blue-800 bg-blue-100 rounded-full leading-5">Running</span>
        </div>
        <div class="w-5/6">
            We have received a start ping, indicating that your task is currently running.
        </div>
    </div>
    <div class="flex w-full pt-2 border-t border-gray-300 md:pt-3">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-green-800 bg-green-100 rounded-full leading-5">Passing</span>
        </div>
        <div class="w-5/6">
            Your task is currently running on schedule.
        </div>
    </div>
    <div class="flex w-full pt-2 border-t border-gray-300 md:pt-3">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-red-800 bg-red-100 rounded-full leading-5">Missing</span>
        </div>
        <div class="w-5/6">
            Your task was expected to send a start ping, but it has not been received within the allowed grace period.
        </div>
    </div>
    <div class="flex w-full pt-2 border-t border-gray-300 md:pt-3">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-red-800 bg-red-100 rounded-full leading-5">Failed</span>
        </div>
        <div class="w-5/6">
            We received a start ping on schedule, however, your task threw an exception and reported that it had failed.
        </div>
    </div>
    <div class="flex w-full pt-2 border-t border-gray-300 md:pt-3">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-orange-800 bg-orange-100 rounded-full leading-5">Failing</span>
        </div>
        <div class="w-5/6">
            We received a start ping on schedule, however, your task has not sent the finish ping within the runtime allowance.
        </div>
    </div>
</div>
