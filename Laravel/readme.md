# Laravel

We maintain several Laravel applications, which are hosted on Heroku:

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

### Memory Limit

Speaking of timeouts, if you run into a fatal error, e.g. `Allowed memory size of 536870912 bytes exhausted` - you can use the `-d memory_limit=512M` argument to the `php` command to bump the default memory allocation (since this is running as a CLI, and so doesn't need to save any memory for other requests).

```
heroku run:detached php -d memory_limit=512M artisan northstar:another-backfill-command --app dosomething-northstar-qa
```

If that runs out of memory again, you can try using the -`-size=standard-2x` Heroku argument and `-d memory_limit=1024M` to run the command on a bigger dyno:

```
heroku run:detached php -d memory_limit=1024M artisan northstar:another-backfill-command --app dosomething-northstar-qa --size=standard-2x
```
