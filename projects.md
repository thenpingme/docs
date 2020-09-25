# Projects

In the context of thenping.me, projects are synonymous with applications. Each application whose tasks need monitoring will require creating a new project.

Your thenping.me dashboard provides you with an overview of the health of all of your monitored projects.

The last update time reflects the last time one of your projects was deployed, a task ran, or the status of the project changed.

## Dashboard

Your project dashboard will give you a high-level overview of all of your configured tasks, so that you can see they're running as expected.

The total number of tasks, executions, and alerts as available for your team's plan are displayed at the top of your project dashboard.

Comparing the number of executions between the current and previous 30 day periods gives you quick insight into the consistency of task execution, whilst the total number of alerts gives you an indication of the overall health of your tasks between periods.

Below the period metrics, you'll also see a timeline of project alerts for the preceding 30 days, helping you to pinpoint problematic days.

Any current alerts are shown below the alert timeline, whilst recent alerts - closed alerts from the preceding 24 hours - are displayed below your task schedule.

Task schedules are displayed in the application timezone, however for convenience, the next due and last updated times are displayed in the browser's timezone.

## Release

You may [configure](/docs/configuration) your application to report the current release to us, which will allow you to connect deployments that caused scheduled tasks to start failing.

Set the `thenpingme.release` configuration parameter as needed, and each task ping or [project sync](/docs/artisan-commands#thenpingmesync) will keep the release value up to date.

## Status

A project can exist in one of three states:

<div class="flex flex-wrap space-y-2 md:space-y-3">
    <div class="flex w-full">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-gray-800 bg-gray-100 rounded-full leading-5">Pending</span>
        </div>
        <div class="w-5/6">
            Your application has not yet been setup, or has been setup and we are waiting for your tasks to start pinging.
        </div>
    </div>
    <div class="flex w-full pt-2 border-t border-gray-300 md:pt-3">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-green-800 bg-green-100 rounded-full leading-5">Healthy</span>
        </div>
        <div class="w-5/6">
            Your application has been setup and all tasks are either pending their first ping or healthy.
        </div>
    </div>
    <div class="flex w-full pt-2 border-t border-gray-300 md:pt-3">
        <div class="w-1/6">
            <span class="inline-flex px-2 text-xs font-semibold text-orange-800 bg-orange-100 rounded-full leading-5">Warning</span>
        </div>
        <div class="w-5/6">
            Your application has been setup and one or more tasks are missing, timed out, or have failed.
        </div>
    </div>
</div>
