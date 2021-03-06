---
title: Configuration
---
thenping.me aims to be a zero-configuration tool for monitoring your application's scheduled tasks, however, does allow you to configure certain aspects using environment variables.

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

The most common configuration you will make in this file will be setting of the `release` value, if you would like to track your application release/version along with your pings.
