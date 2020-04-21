# Laravel

We maintain several Laravel applications:

* [Northstar](https://github.com/DoSomething/northstar)
* [Rogue](https://github.com/DoSomething/rogue)
* [Phoenix](https://github.com/DoSomething/phoenix-next)
* [Aurora](https://github.com/DoSomething/aurora)
* [Chompy](https://github.com/DoSomething/chompy)

## Installation

See [Homestead](https://github.com/DoSomething/communal-docs/tree/master/Homestead) for installing locally.

## Commands

When running a console command in QA or Production, run the command as a "detached" one-off dyno (via heroku run:detached) in order to see the console output in [Papertrail](https://github.com/DoSomething/communal-docs/tree/master/Monitoring).

Example:

```
heroku run:detached php artisan northstar:another-backfill-command --app dosomething-northstar-qa
```

Heroku doesn't print to the logs for normal `heroku run` since they're in an interactive terminal. It's useful to see these logs to know when a command has completed (or if it timed out).
