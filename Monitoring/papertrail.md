# Papertrail

## What
[Papertrail](http://papertrailapp.com) is log management service that makes it easy to aggregate logs from all our applications, search and filter them, and trigger alerting based on trends. It's a valuable tool when debugging an issue in production.

## Why
We use a single Papertrail organization to make it easy for everyone on the team to access logs without having to configure their own custom infrastructure & to track issues that occur between multiple applications.

## How
### …via the web
If you don't have access to Papertrail, drop a note in [#announce-tech](https://dosomething.slack.com/messages/C782XBKDM/).

Papertrail's [Dashboard](https://papertrailapp.com/dashboard) makes it easy to jump to logs by environment or application:

![screen shot 2018-08-06 at 3 26 26 pm](https://user-images.githubusercontent.com/583202/43736945-8a67e30a-998d-11e8-9996-403b892bbdf1.png)

Papertrail's [Event Viewer](https://papertrailapp.com/events) lets you view & search logs by application or environment:

![screen shot 2018-08-06 at 3 31 23 pm](https://user-images.githubusercontent.com/583202/43737274-cd56efb6-998e-11e8-9cfe-e79d31787d1c.png)

Be sure to check out Papertrail's powerful [search syntax](https://help.papertrailapp.com/kb/how-it-works/search-syntax) to make the most of the search bar.

### …via the CLI
Papertrail has a helpful [command-line interface](https://help.papertrailapp.com/kb/how-it-works/command-line-client/) as well. Follow the instructions to [install & configure the `papertrail` gem](https://github.com/papertrail/papertrail-cli#quick-start). You can now access all our application logs directly from your terminal! For example:

```sh
# tail all logs from a specific environment:
papertrail -g production -f

# tail all logs from a specific app:
papertrail -s dosomething-northstar-qa -f

# tail all logs from a particular Heroku dyno:
papertrail -s dosomething-northstar "app/queue" -f

# or count recent production errors by application:
papertrail --min-time '1 hour ago' -S "All Errors" | cut -f 4 -d ' ' | sort | uniq -c
```

### Setup

To add a Heroku application to our Papertrail account, simply attach a [log drain](https://devcenter.heroku.com/articles/log-drains). First, grab the appropriate URL from the [Log Destinations](https://papertrailapp.com/account/destinations) tab in our account settings. Then, use the Heroku CLI to attach it to your app:

```sh
heroku drains:add syslog+tls://logsN.papertrailapp.com:NNNNN --app APP_NAME
```

You should see your app appear in [All Systems](https://papertrailapp.com/groups/4485452) shortly, with a randomly generated name like `d.6f2a3e41-b932-4dea-bd82-fab05d4e21ea`. Click the "settings" button to rename it to match your Heroku app. Finally, return to the [Dashboard](https://papertrailapp.com/dashboard) and hit the "edit" button on an environment (e.g. Production) to add it to the appropriate group (if not already there).

You can also add an EC2 instance with [rsyslog](https://help.papertrailapp.com/kb/configuration/advanced-unix-logging-tips/#aggregate-local-log-files-with-rsyslog), a Lambda function, or many [other sources](https://help.papertrailapp.com/#configuration).
