# Artisan Commands

Once installed, the [`thenpingme/laravel`](https://github.com/thenpingme/thenpingme-laravel) package provides four Artisan commands to your application.

## `thenpingme:setup`

The `setup` command is responsible for configuring your application and reporting your scheduled tasks to thenping.me. This command only has to be run a single time and can be run locally or in production.

It gathers all the tasks configured in your `app/Console/Kernel.php` file, validates them for uniqueness, and configures them for monitoring.

This command will also create a random signing key and add it to your `.env` file. This key must be present in your production environment.

Once setup is complete, you will receive an email to verify that your application's tasks are ready to be monitored. Each of your tasks will remain in a pending state until they are triggered by your scheduler for the first time.

> Any tasks that are defined using the `environments()` constraint will only be detected in that environment.

## `thenpingme:sync`

As the name suggests, the `sync` command is responsible for ensuring that your application tasks are kept in sync with thenping.me.

<div class="rounded shadow bg-indigo-50 px-4 py-1">
    <p class="text-indigo-700">You should include this command as part of your deployment process, to ensure your tasks and their schedules are correctly monitored.</p>
</div>

In order for all relevant tasks to be monitored, this command should be run in the same environment that your tasks are reporting from.

That is to say, if you have tasks constrained to only run in production environments &ndash; `environments(['production'])` &ndash; you should run `thenpingme:sync` *in* the production environment.

## `thenpingme:schedule`

The `schedule` command is useful to get a quick overview of all of your application's scheduled tasks, their interval, expected previous and next runs.

```
+--+--------------------------+----------------------+-------------+---------------------+---------------------+
|  | Command                  | Interval             | Description | Last Run            | Next Due            |
+--+--------------------------+----------------------+-------------+---------------------+---------------------+
|  | check:missing-tasks      | Every minute         |             | 2020-09-05 05:03:00 | 2020-09-05 05:05:00 |
|  | check:missing-executions | Every minute         |             | 2020-09-05 05:03:00 | 2020-09-05 05:05:00 |
|  | metrics:gather           | Every day at 12:07am |             | 2020-09-05 00:07:00 | 2020-09-06 00:07:00 |
+--+--------------------------+----------------------+-------------+---------------------+---------------------+
```

Any tasks that are identified as being not uniquely identifiable will be flagged as such.

```
+-----+--------------------------+----------------------+-------------+---------------------+---------------------+
|     | Command                  | Interval             | Description | Last Run            | Next Due            |
+-----+--------------------------+----------------------+-------------+---------------------+---------------------+
|  !  | check:missing-tasks      | Every minute         |             | 2020-09-05 05:03:00 | 2020-09-05 05:05:00 |
|  !  | check:missing-tasks      | Every minute         |             | 2020-09-05 05:03:00 | 2020-09-05 05:05:00 |
|     | check:missing-executions | Every minute         |             | 2020-09-05 05:03:00 | 2020-09-05 05:05:00 |
|     | metrics:gather           | Every day at 12:07am |             | 2020-09-05 00:07:00 | 2020-09-06 00:07:00 |
+-----+--------------------------+----------------------+-------------+---------------------+---------------------+
Tasks have been identified that are not uniquely distinguishable!
```

## `thenpingme:verify`

The `verify` command is used to identify tasks that are not uniquely distinguishable.

This command will return a non-zero exit code, so it can be used as part of your continuous integration's linting process to halt deployment, in the event you have collisions.

```
+---------+---------------------+------------+--------------+-------------+-------+
| Type    | Command             | Expression | Interval     | Description | Extra |
+---------+---------------------+------------+--------------+-------------+-------+
| command | check:missing-tasks | * * * * *  | Every minute |             |       |
| command | check:missing-tasks | * * * * *  | Every minute |             |       |
+---------+---------------------+------------+--------------+-------------+-------+
Tasks have been identified that are not uniquely distinguishable!
```

> Unlike the `sync` command, `verify` will return only those tasks that collide.
