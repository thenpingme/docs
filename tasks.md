---
title: Tasks
---
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

## Legend

For each task, there are a series of icons that identify different components of its configuration.

<div class="w-full overflow-hidden bg-white rounded-md shadow-md divide-y">
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <svg class="w-6 h-6 text-indigo-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path d="M7 9a2 2 0 012-2h6a2 2 0 012 2v6a2 2 0 01-2 2H9a2 2 0 01-2-2V9z"></path>
                <path d="M5 3a2 2 0 00-2 2v6a2 2 0 002 2V5h8a2 2 0 00-2-2H5z"></path>
            </svg>
        </div>
        <div class="ml-2 w-5/6 flex flex-col">
            <span class="font-medium">May overlap</span>
            <span class="text-gray-600 text-sm">Should this task run longer than it's configured interval, it is possible that it can be running multiple times simultaneously.</span>
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <svg class="w-6 h-6 text-indigo-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M9 3a1 1 0 012 0v5.5a.5.5 0 001 0V4a1 1 0 112 0v4.5a.5.5 0 001 0V6a1 1 0 112 0v5a7 7 0 11-14 0V9a1 1 0 012 0v2.5a.5.5 0 001 0V4a1 1 0 012 0v4.5a.5.5 0 001 0V3z" clip-rule="evenodd"></path>
            </svg>
        </div>
        <div class="ml-2 w-5/6 flex flex-col">
            <span class="font-medium">Not in maintenance mode</span>
            <span class="text-gray-600 text-sm">This task will not run if you have run <code>artisan down</code> in your application.</span>
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <svg class="w-6 h-6 text-indigo-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M3 5a2 2 0 012-2h10a2 2 0 012 2v8a2 2 0 01-2 2h-2.22l.123.489.804.804A1 1 0 0113 18H7a1 1 0 01-.707-1.707l.804-.804L7.22 15H5a2 2 0 01-2-2V5zm5.771 7H5V5h10v7H8.771z" clip-rule="evenodd"></path>
            </svg>
        </div>
        <div class="ml-2 w-5/6 flex flex-col">
            <span class="font-medium">On one server</span>
            <span class="text-gray-600 text-sm">This task has been configured to run on one server only.</span>
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <svg class="w-6 h-6 text-indigo-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path d="M7 3a1 1 0 000 2h6a1 1 0 100-2H7zM4 7a1 1 0 011-1h10a1 1 0 110 2H5a1 1 0 01-1-1zM2 11a2 2 0 012-2h12a2 2 0 012 2v4a2 2 0 01-2 2H4a2 2 0 01-2-2v-4z"></path>
            </svg>
        </div>
        <div class="ml-2 w-5/6 flex flex-col">
            <span class="font-medium">Runs in background</span>
            <span class="text-gray-600 text-sm">This task has been configured to run in the background, so it will not delay the start of other tasks on the same execution schedule.</span>
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <svg class="w-6 h-6 text-indigo-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M3 3a1 1 0 011-1h12a1 1 0 011 1v3a1 1 0 01-.293.707L12 11.414V15a1 1 0 01-.293.707l-2 2A1 1 0 018 17v-5.586L3.293 6.707A1 1 0 013 6V3z" clip-rule="evenodd"></path>
            </svg>
        </div>
        <div class="ml-2 w-5/6 flex flex-col">
            <span class="font-medium">Task is filtered</span>
            <span class="text-gray-600 text-sm">This task may be skipped, subject to the result of a <a href="https://laravel.com/docs/8.x/scheduling#truth-test-constraints" target="_blank">truth-test constraint</a> at runtime.</span>
        </div>
    </div>
</div>

## Status

A task can exist in one of six states:

<div class="w-full overflow-hidden bg-white rounded-md shadow-md divide-y">
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-gray-800 bg-gray-100 rounded-full leading-5">Pending</span>
            </div>
        </div>
        <div class="ml-2 w-5/6">
            We are waiting for your task to ping for the first time.
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-blue-800 bg-blue-100 rounded-full leading-5">Running</span>
            </div>
        </div>
        <div class="ml-2 w-5/6">
            We have received a start ping, indicating that your task is currently running.
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-green-800 bg-green-100 rounded-full leading-5">Passing</span>
            </div>
        </div>
        <div class="ml-2 w-5/6">
            Your task is currently running on schedule.
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-red-800 bg-red-100 rounded-full leading-5">Missing</span>
            </div>
        </div>
        <div class="ml-2 w-5/6">
            Your task was expected to send a start ping, but it has not been received within the allowed grace period.
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-red-800 bg-red-100 rounded-full leading-5">Failed</span>
            </div>
        </div>
        <div class="ml-2 w-5/6">
            We received a start ping on schedule, however, your task threw an exception and reported that it had failed.
        </div>
    </div>
    <div class="flex w-full p-4">
        <div class="w-16 sm:w-20 flex justify-center">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-orange-800 bg-orange-100 rounded-full leading-5">Failing</span>
            </div>
        </div>
        <div class="ml-2 w-5/6">
            We received a start ping on schedule, however, your task has not sent the finish ping within the runtime allowance.
        </div>
    </div>
</div>
