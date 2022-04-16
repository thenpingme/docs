---
title: Executions
---
Executions track a single run of your monitored tasks.

Each time your application's task scheduler kicks off a task, a new execution record will be created.

A healthy execution lifecycle will start by the tasks next scheduled start time and finish within the tasks allowed runtime.

> Note that a configurable grace period is added to the scheduled task's next start time to prevent false alerts due to network latency or simultaneous job delays.

Execution lifecycles that exceed the allowed execution run time will be considered to have timed out.

Tasks that are scheduled in your `app/Console/Kernel.php` using `when()` or `unless()` truth test constraints may finish in a healthy, but skipped state.

## Status

An execution can exist in one of five statuses:

<div class="w-full overflow-hidden bg-white rounded-md shadow-md divide-y">
    <div class="grid grid-cols-6 gap-4 p-3">
        <div class="col-span-1 flex items-start">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-blue-800 bg-blue-100 rounded-full leading-5">Running</span>
            </div>
        </div>
        <div class="col-span-5">
            We have received a start ping, indicating that the execution is currently in progress.
        </div>
        <div class="col-span-1 flex items-start">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-green-800 bg-green-100 rounded-full leading-5">Finished</span>
            </div>
        </div>
        <div class="col-span-5">
            We received a finish ping, indicating that the execution completed without failure.
        </div>
        <div class="col-span-1 flex items-start">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-purple-800 bg-purple-100 rounded-full leading-5">Skipped</span>
            </div>
        </div>
        <div class="col-span-5">
            We received a skipped ping, indicating that your task was evaluated for execution but did not run based on the result of a truth test constraint in your scheduler.
        </div>
        <div class="col-span-1 flex items-start">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-red-800 bg-red-100 rounded-full leading-5">Timedout</span>
            </div>
        </div>
        <div class="col-span-5">
            A finish ping was either not received, or the finish ping was not received within the allowed runtime. Note that an execution that reached the timedout status is not final. For a long-running task, the finish ping may arrive later.
        </div>
        <div class="col-span-1 flex items-start">
            <div>
                <span class="inline-flex px-2 text-xs font-semibold text-red-800 bg-red-100 rounded-full leading-5">Failed</span>
            </div>
        </div>
        <div class="col-span-5">
            An exception was thrown during execution of your task.
        </div>
    </div>
</div>
