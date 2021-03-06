This is our "playbook" to respond to incidents, such as [PagerDuty alerts](https://www.pagerduty.com).

First of all, **take a deep breath!** :relieved: It can be stressful to get an alert, but we're all in this together. As an on-call engineer, your first job is to [investigate the alert](#first-responder-checklist) to figure out whether this is a "real" emergency or not. If it is (or you're unsure), don't hesitate to [**Escalate** the incident](#escalate-an-incident) to summon help!

## When you get an alert...

As a first-responder, you'll run through the following "checklist" to investigate the alert, coordinate our response [during the incident](#during-the-incident) and document the issue [after the incident is resolved](#after-the-incident). You're the point person for our response, but the whole team is available to help out! We've got this!

### First-Responder Checklist

1. If you have time to troubleshoot (via phone or computer), **Acknowledge** the incident to let others know you're on it.
2. If the alert was triggered by an automated test failure (from [Ghost Inspector](https://ghostinspector.com) or [Runscope](https://www.runscope.com)), it will automatically retry. This might resolve the issue, in which case you're all set & can move on to [**After The Incident**](#after-the-incident) steps!

If this wasn't cleared up by an automatic retry, it's time to figure out the severity of the issue:

3. Have multiple incidents been created in different systems? If they seem at all related, [merge them](#merge-incidents).
4. What does the alert say? Can you reproduce the problem yourself? [Leave a note](#incident-notes) on the incident in PagerDuty.
5. Is this something that affects the "core user flow" (registering a new account, signing up for a campaign, and submitting a post)? Try re-running [this Ghost Inspector test](https://app.ghostinspector.com/tests/5c4a1efd638e692a23208132) or testing yourself via an incognito browser.
   1. If things truly are broken, keep going down this checklist. We'll figure it out!
   2. If not, [**Snooze** the incident](https://support.pagerduty.com/docs/editing-incidents#section-snooze-an-incident) and we can look into more on the next business day.

Alright, so we know things are a lil' wonky. Let's see if we can find out why:

6. Could this be an external issue? It may be something outside of our control. Check the status pages for our providers: [AWS Status](https://status.aws.amazon.com), [Heroku Status](https://status.heroku.com), [Apollo Status](http://status.apollographql.com), [Fastly Status](https://status.fastly.com), [Contentful Status](https://www.contentfulstatus.com) and [MongoDB Status](https://status.cloud.mongodb.com).
   1. If one of these services are unhealthy, note it on the incident & continue down the checklist.
7. Look at the [application logs](#application-logs) and [error analytics](#error-analytics) for this application – does anything seem unusual?
   1. We might have seen this error before & left ourselves some notes. Check the troubleshooting document for this application: [Phoenix](https://dosomething.github.io/communal-docs/Incidents/Phoenix), [GraphQL](https://dosomething.github.io/communal-docs/Incidents/GraphQL), [Northstar](https://dosomething.github.io/communal-docs/Incidents/Northstar), [Rogue](https://dosomething.github.io/communal-docs/Incidents/Rogue), [Gambit](https://dosomething.github.io/communal-docs/Incidents/Gambit), [Blink](https://dosomething.github.io/communal-docs/Incidents/Blink)
   2. If you see something spooky (like network timeouts, or something CPU or database related) try [restarting the application](#restarting-the-application). You probably don't want to do this if we're seeing issues across multiple applications because if there is a Heroku outage of sorts (which won't always be on their status page immediately) restarting things may stall and only make things worse.
8. Does this failure relate to a recent change or deploy?
   1. If we changed content on a campaign (such as a closed campaign or new copy) and that's causing the failure, update the test to reflect that & re-run the test. If that resolves the incident, you're all set! Follow the steps in [**After The Incident**](#after-the-incident) to document this.
   2. If this relates to a recent deploy, try [rolling back that deploy](#rolling-back-deploys) to the previous release. We can always fix the bug when we're back in the office.
9. If the site is still broken, it's time to bring in more help. Continue to [**During The Incident**](#incident-response), below.

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

## During the incident...

Now that you've confirmed this is a real issue, it's time to bring in some more help. [**Escalate** the incident](#escalate-an-incident) to bring in a technical lead, then [create the incident channel & notify staff](#create-the-incident-channel). This is where we'll coordinate our troubleshooting efforts.

As the first responder, your job is to organize our response – document troubleshooting steps we're taking & make sure everyone's on the same page. At this point, you may also want to bring in additional engineers with domain expertise on the failing system(s).

<br/>
<br/>
<br/>
<br/>
<p align="center">
:fire_extinguisher:
</p>
<br/>
<br/>
<br/>
<br/>

## After the incident...

After the incident is resolved, add a ticket or comment to our ['Incidents' Pivotal board](https://www.pivotaltracker.com/n/projects/2459675). Each card on this board is a "type" of problem that could cause an incident, like "Test failures due to 503 First Byte Timeout". We use this to track trends, which we'll then discuss at our Monthly Post-Mortem meeting.

<br/>
<br/>
<br/>
<br/>
<p align="center">
:ice_cube:
</p>
<br/>
<br/>
<br/>
<br/>

## Appendix

#### Merge Incidents

If multiple related incidents are triggered at the same time, you can merge them to consolidate all of the alerts & incident notes in one place. The newly merged incident will be resolved when all of the alerts within it resolve.

![Screen Shot 2020-05-01 at 11 19 07 AM](https://user-images.githubusercontent.com/583202/80816627-d8b3a680-8b9d-11ea-95d6-dab73711d134.png)

**Note:** You can only merge incidents while they're open (frustrating!), so do this when triaging an alert!

#### Incident Notes

You can leave a note on incidents, which will appear on the incident timeline:

![Screen Shot 2019-12-10 at 3 23 50 PM](https://user-images.githubusercontent.com/583202/70565983-300a9900-1b61-11ea-8571-670dd57eca29.png)

It will also appear as an update in `#deploys`:

<img width="624" alt="Screen Shot 2019-12-10 at 3 25 27 PM" src="https://user-images.githubusercontent.com/583202/70566139-78c25200-1b61-11ea-8b92-f6b9c878e4f6.png">

#### Application Logs

Our applications all publish logs to [Papertrail](https://papertrailapp.com/dashboard). You can filter by application (such as [`dosomething-phoenix`](https://my.papertrailapp.com/systems/dosomething-phoenix/events) or [`dosomething-graphql`](https://my.papertrailapp.com/systems/dosomething-graphql/events)) or by a saved search (such as [`Phoenix: Exceptions`](https://papertrailapp.com/searches/45812651) or [`GraphQL: Exceptions`](https://my.papertrailapp.com/groups/10447062/events?q=system%3Adosomething-graphql+program%3Alambda+%22Error%22)).

#### Error Analytics

If a problem is happening in Phoenix, Northstar, or Rogue, check [New Relic](https://newrelic.com)'s "Error Analytics" tab to see if there are any unusual errors in the past few hours. You can click on a specific error to see a stack trace:

![Screen Shot 2019-12-10 at 3 32 14 PM](https://user-images.githubusercontent.com/583202/70566555-48c77e80-1b62-11ea-843b-ebc9944758ab.png)

If the problem is happening in GraphQL, check Apollo Engine's [Metrics dashboard](https://engine.apollographql.com/graph/dosomething-graphql-lambda/metrics?range=lastDay&schemaTag=current&tab=errors) to see which queries are erroring out. Like in New Relic, you can click in to see more details on an error:

![Screen Shot 2019-12-10 at 3 37 33 PM](https://user-images.githubusercontent.com/583202/70566973-0a7e8f00-1b63-11ea-9982-1460fa45b3e6.png)

#### Restarting The Application

Sometimes, an issue might be solved by restarting the failing service. For applications on [Heroku](https://dashboard.heroku.com), just head to the application dashboard and choose "Restart all dynos" from the "More" menu:

![Screen Shot 2019-12-10 at 3 40 10 PM](https://user-images.githubusercontent.com/583202/70567189-72cd7080-1b63-11ea-8bb7-71a031d78626.png)

#### Rolling Back Deploys

Some problems may be due a bug in newly deployed code. If that seems like the case, the easiest solution might be to "roll back" to a previous deploy that we know was working. This is easy for applications on [Heroku](https://dashboard.heroku.com):

![Screen Shot 2019-12-10 at 3 41 26 PM](https://user-images.githubusercontent.com/583202/70567264-98f31080-1b63-11ea-92a9-38d53df7eed1.png)

#### Escalate an Incident

You can do this by "running a play" (hidden under the "more" menu on the mobile app):

<p align="center">
  <img width="708" style="border: 1px solid #eaecef" src="https://user-images.githubusercontent.com/583202/70751752-2159e880-1cff-11ea-809c-6567b180c9ed.png">
</p>

#### Create The Incident Channel

Once you've done that, **create a new `#incident-####` channel** to continue to debug & monitor the incident.

This can be done via PagerDuty, and should be named based on the number assigned to the incident:

![Screen Shot 2019-12-10 at 3 47 00 PM](https://user-images.githubusercontent.com/583202/70567666-639af280-1b64-11ea-9d1d-579dff70a5e6.png)

While we figure out next steps, we'll also write a message in `#announce` to let other staffers know that we're looking into the problem. The message should roughly follow this template:

> We're currently investigating issues with the website. [A brief sentence explaining the user impact in non-technical terms.] We're actively working on the problem in #incident-#### and will post updates here as we figure out more.
