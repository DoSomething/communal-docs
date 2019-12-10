# Incident Playbook

This is our "playbook" to respond to incidents, such as [PagerDuty alerts](https://www.pagerduty.com).

First of all, **take a deep breath!** :relieved: It can be stressful to get an alert, but we're all in this together. As a first responder, your job is to figure out whether this is a "real" emergency or not. If it is (or you're unsure), don't hesitate to hit the **Escalate** button to summon help! We've got this!

### First-Responder Checklist

1. If you have access to a computer to troubleshoot with, **acknowledge** the incident to let others know you're on it.
2. If the alert was triggered by an automated test failure (from [Ghost Inspector](https://ghostinspector.com) or [Runscope](https://www.runscope.com)), it will automatically retry. This might resolve the issue, in which case you're all set & no further action is necessary!

If this wasn't cleared up by an automatic retry, it's time to figure out the severity of the issue:

3. What does the alert say? Can you reproduce the problem yourself? [Leave a note](#incident-notes) on the incident in PagerDuty.
4. Is this something that affects the "core user flow" (registering a new account, signing up for a campaign, and submitting a post)? Try re-running [this Ghost Inspector test](https://app.ghostinspector.com/tests/5c4a1efd638e692a23208132) or testing yourself via an incognito browser.
    1. If things truly are broken, keep going down this checklist. We'll figure it out!
    2. If not, manually **resolve** the incident & we can look into this more on the next business day.

Alright, so we know things are a lil' wonky. Let's see if we can find out why:

5. Could this be an external issue? It may be something outside of our control. Check the status pages for our providers: [AWS Status](https://status.aws.amazon.com), [Heroku Status](https://status.heroku.com), [Apollo Status](http://status.apollographql.com), and [MongoDB Status](https://status.cloud.mongodb.com).
    1. If one of these services are unhealthy, note it on the incident & continue down the checklist.
6. Look at the [application logs](#application-logs) and [error analytics](#error-analytics) for this application – does anything seem unusual? If you see something spooky, like network timeouts, try [restarting the application](#restarting-the-application).
7. Does this failure relate to a recent change or deploy?
    1. If we changed content on a campaign (such as a closed campaign or new copy) and that's causing the failure, update the test to reflect that & re-run the test. If that resolves the incident, you're all set!
    2. If this relates to a recent deploy, try [rolling back that deploy](#rolling-back-deploys) to the previous release. We can always fix the bug when we're back in the office.
8. If the site is still broken, escalate the incident to bring in a technical lead.

We'll [create a new `#incident-####` channel](#incident-channels) to continue to debug & monitor the incident. While we figure out next steps, we'll also write a message in `#announce` to let other staffers know that we're looking into the problem. The message should roughly follow this template:

> We're currently investigating issues with the website. [A brief sentence explaining the user impact in non-technical terms.] We're actively working on the problem in #incident-#### and will post updates here as we figure out more.

<br/>
<br/>
<br/>
<br/>
<p align="center">
:fire_engine:
</p>
<br/>
<br/>
<br/>
<br/>

## Incident Notes

You can leave a note on incidents, which will appear on the incident timeline:

![Screen Shot 2019-12-10 at 3 23 50 PM](https://user-images.githubusercontent.com/583202/70565983-300a9900-1b61-11ea-8571-670dd57eca29.png)

It will also appear as an update in `#deploys`:

<img width="624" alt="Screen Shot 2019-12-10 at 3 25 27 PM" src="https://user-images.githubusercontent.com/583202/70566139-78c25200-1b61-11ea-8b92-f6b9c878e4f6.png">

## Application Logs

Our applications all publish logs to [Papertrail](https://papertrailapp.com/dashboard). You can filter by application (such as `dosomething-phoenix` or `dosomething-graphql`) or by a saved search (such as [`Phoenix: Exceptions and 5xx Server Errors`](https://papertrailapp.com/searches/45812651) or [`GraphQL: Exceptions`](https://my.papertrailapp.com/groups/10447062/events?q=system%3Adosomething-graphql+program%3Alambda+%22Error%22)).

## Error Analytics

If a problem is happening in Phoenix, Northstar, or Rogue, check [New Relic](https://newrelic.com)'s "Error Analytics" tab to see if there are any unusual errors in the past few hours. You can click on a specific error to see a stack trace:

![Screen Shot 2019-12-10 at 3 32 14 PM](https://user-images.githubusercontent.com/583202/70566555-48c77e80-1b62-11ea-843b-ebc9944758ab.png)

If the problem is happening in GraphQL, check Apollo Engine's [Metrics dashboard](https://engine.apollographql.com/graph/dosomething-graphql-lambda/metrics?range=lastDay&schemaTag=current&tab=errors) to see which queries are erroring out. Like in New Relic, you can click in to see more details on an error:

![Screen Shot 2019-12-10 at 3 37 33 PM](https://user-images.githubusercontent.com/583202/70566973-0a7e8f00-1b63-11ea-9982-1460fa45b3e6.png)

## Restarting The Application

Sometimes, an issue might be solved by restarting the failing service. For applications on [Heroku](https://dashboard.heroku.com), just head to the application dashboard and choose "Restart all dynos" from the "More" menu:

![Screen Shot 2019-12-10 at 3 40 10 PM](https://user-images.githubusercontent.com/583202/70567189-72cd7080-1b63-11ea-8bb7-71a031d78626.png)


## Rolling Back Deploys

Some problems may be due a bug in newly deployed code. If that seems like the case, the easiest solution might be to "roll back" to a previous deploy that we know was working. This is easy for applications on [Heroku](https://dashboard.heroku.com):

![Screen Shot 2019-12-10 at 3 41 26 PM](https://user-images.githubusercontent.com/583202/70567264-98f31080-1b63-11ea-92a9-38d53df7eed1.png)

## Incident Channels

If we have multiple engineers responding to an incident, we'll create an `#incident-####` channel. This can be done via PagerDuty, and should be named based on the number assigned to the incident:

![Screen Shot 2019-12-10 at 3 47 00 PM](https://user-images.githubusercontent.com/583202/70567666-639af280-1b64-11ea-9d1d-579dff70a5e6.png)
