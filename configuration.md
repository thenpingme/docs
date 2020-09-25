# Configuration

thenping.me aims to be a zero-configuration tool for monitoring your application's scheduled tasks, however, does allow you to configure certain aspects using environment variables.

## Environment variables

### `THENPINGME_PROJECT_ID`

Identifies your application to thenping.me when it sends its ping payloads. This is automatically set when you run the [`thenpingme:setup`](/docs/installation) command for the first time.

Ensure that this is set in your environment wherever you run the `thenpingme:sync` command.

### `THENPINGME_SIGNING_KEY`

Is used to sign the payloads sent from your application to thenping.me, in order to verify the integrity of the data being transmitted. This is automatically set when you run the `thenpingme:setup` command for the first time.

Ensure that this is set in your environment wherever you run the `thenpingme:sync` command.

### `THENPINGME_QUEUE_PING`

By default, the package will dispatch all pings to the queue for asyncrhonous processing. If you would prefer the tasks be dispatched immediately, you may set this value to `false`.

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
