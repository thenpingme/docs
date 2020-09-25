# Introduction

thenping.me is a monitoring solution built specifically for your Laravel application's scheduled tasks.

Whilst error tracking is great at telling you when something broke, monitoring helps you keep an eye on your applications scheduled tasks to ensure they run when you expect them.

How many times have you been caught out by tasks not running when they should, held up by other tasks, or simply timing out?

thenping.me installs as a package into your application and reports back any time your scheduled tasks run - automatically.

There is no need for you to configure individual monitors, send HTTP requests, or change your application code. Just include the `thenpingme:sync` command in your deploy process and we'll do the rest.
