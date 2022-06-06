# QA Refresh

We refresh our QA environment's databases & storage to match production every weekend. This ensures that our testing environment matches production as closely as possible (while not overwriting any data we write when testing too zealously).

## Automatic Refreshes

We automatically refresh data data every Sunday at midnight, using the following Cron schedule on the [QA Refresh](https://jenkins.dosomething.org/job/QA%20Refresh/configure) job:

```
H 0 * * 7
```

You can disable automatic refreshes by unchecking the "build periodically" trigger on this job. Don't forget to turn it back on later!

(See this [StackOverflow answer](https://stackoverflow.com/a/12472740) for more details on how this syntax works.)

## Manual Refreshes

Sometimes we want to manually refresh QA with production data during the week. To do so, head to [Jenkins](https://jenkins.dosomething.org/):

![Jenkins screenshot](https://user-images.githubusercontent.com/583202/122424338-a2311a80-cf5c-11eb-9437-7ff23b51f697.png)

You can click the <img height="18" src="https://user-images.githubusercontent.com/583202/122424461-bc6af880-cf5c-11eb-9cf9-1cf3f3d7298a.png" alt="run job button" /> icon to run any job on-demand:

- The **QA Refresh** job runs all of our refreshes. _(Recommended.)_
- The **QA Refresh - Contentful** job refreshes content (pages, etc.) for Phoenix.
- The **QA Refresh - Northstar MongoDB**, **QA Refresh - Northstar MariaDB**, and **QA Refresh - Northstar S3** jobs refresh the databases & file storage for our backend API. Running them independently may result in inconsistent data in our QA environment.

A full refresh takes about one hour. You may see errors on QA while the database is being rebuilt.

You can track in-progress jobs in the "Build Exector Status" section in the sidebar.
