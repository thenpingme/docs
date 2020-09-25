# Alerts

Alerts are a large part of why thenping.me exists; to communicate with you about when your application's scheduled tasks reached a state of alert.

There are two situations that will lead to an alert being raised:
* A task did not send a start ping i.e. is missing, taking into account its allowed start time grace period.
* A task did not send a finish ping i.e. timed out, taking into account its allowed run time.

Tasks can be [configured](/docs/tasks#settings) to allow multiple consecutive failures before sending a notification. In cases where the threshold is greater than 1, thenping.me will log a _suppressed_ alert internally. When your consecutive failure threshold is reached an _active_ alert will be raised.

> Example: If your task has a consecutive failures limit of 3, an active alert will be raised when your task fails for the third time.

Active alerts result in notifications being dispatched to users alerting them of the failure.

> There can only be one active alert for a task at any given time.

When a task subsequently runs on schedule, or when a timed-out task sends a finish ping, any active alerts for that task will be closed, and you will be notified of the recovery.
