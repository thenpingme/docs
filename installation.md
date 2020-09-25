# Installation

## Requirements

* Laravel 6.0 or higher
* PHP 7.2.5 or higher

## Getting started

Once you have <a href="/projects">created a new project</a> from your dashboard, there are just two steps to get started.

The first step is installing the `thenpingme/laravel` package into the application you wish to monitor.

```
composer require thenpingme/laravel
```

The second command is unique to your project, and is responsible for identifying your scheduled tasks and configuring your application to communicate with thenping.me.

```
php artisan thenpingme:setup <project-id>
```

This will configure a signing secret for your application and add it to your `.env` file along with other configuration parameters.

```env
THENPINGME_PROJECT_ID=<your-project-id>
THENPINGME_SIGNING_SECRET=<a-very-long-signing-secret-key>
THENGPINGME_QUEUE_PING=true
```

## Unique tasks

Because tasks are configured in code rather than a database, they are more difficult to uniquely identify in certain scenarios.

Any tasks that are not able to be uniquely identified will be flagged prior to the application setup continuing. You will not be able to sync your tasks with thenping.me until each has been resolved.

Job-based (`job`) and closure-based (`call`) tasks are the most likely to collide.

To avoid collision of job-based tasks, ensure they each specifiy a unique `description`, or run on a unique schedule.

To avoid collision of closure-based tasks, ensure they each specify a unique `description`.

Note that you can also verify the uniqueness of your application's tasks at any time with the [`thenpingme:verify`](/docs/artisan-commands#verify) command.
