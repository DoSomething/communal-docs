# Puck spec

The "puck spec" is a set of fields that is automatically sent with every Puck event. It contains information that allows admins to query and follow an entire user journey.

## Definitions
This information is organized by parent/child relationship. The header represents the parent `key` in the json blob, and the bolded elements represent child properties.

### event
Data that explains what this event is and where it came from.

**name**: A title that describes why this event was fired. eg: `signup`. See the [events](./events.md) table for a list of event names.

**source**: The application where this event originated. eg: `phoenix-next`. See the [sources](./sources.md) table for a list of active event sources.

### meta
Data that is useful for organizing in a warehouse or processing in an ingestion worker.

**timestamp**: UNIX timestamp of when this event was fired.

**version**: A version number that is maintained as core spec elements are changed. Core spec elements are considered anything that doesn't fall under the custom `data` property. Checkout the [change log](./changelog.md) for more information.

### user
Data that is used for identifying this event with a specific user, whether authenticated or not.

**ip**: The users IP address, which is applied server side after the client sends the event. This is useful for tracking a user journey across multiple domains.

**deviceId**: A unique identifier generated the first time a user ever hits a Puck equipped DoSomething.org platform. This identifier sticks to the users browser regardless of authentication status. Some caveats with this identifier however,
- A shared computer (such as one at a public library, school) could have multiple Northstar id's associated with a single device id.
- A user might access DoSomething.org from multiple devices (Phone, laptop), and have multiple device id's associated with their Northstar id.

**northstarId**: The users northstar id, if they are currently authenticated.

### device
Data about the device which created this event.

**size**: The screen size of this device. Possible values are "small", "medium" and "large". Translated, they represent mobile phone, tablet and laptop+.

### page
Data about the web page where the event was fired and current users session.

**href**: The entire url of the page, including query params, hostname and the protocol.

**host**: The isolated hostname of the url.

**path**: The isolated path of the url, with a leading slash.

**query**: Each key/value pair in this object represents a query parameter in the url. This is where any relevant UTM parameters would be.

**landingTimestamp**: UNIX timestamp of when this session was started. Sessions expire after 30 minutes of inactivity.

**sessionId**: Unique identifier that represents a single session. Note: Sessions are specific to applications, the session id on phoenix next will not match the session id on Northstar.

#### referrer
Data about where the user came from prior to starting this session. Note: The referrer data is persistent throughout the entire user session, meaning, if you arrived to a campaign from a Google search, all subsequent events in the session will have Google as the referrer.

**href|host|path|query**: All keys match the definitions from **page** which were previously defined.

### data
The data section of the event is a free-form structure that individual events can populate with relevant data. See the [events](./events.md) table for a list of events and the data they send.

## Mock Spec

```json
{
  "event": {
    "name": "example-event",
    "source": "example-source"
  },
  "meta": {
    "timestamp": "1510241944177",
    "version": "1.0.0"
  },
  "user": {
    "ip": "::ffff:10.10.100.100",
    "deviceId": "151024188248196633",
    "northstarId": "553eabfc469c64ed7d8b45e9"
  },
  "page": {
    "href": "http://dosomething.org/us/campaigns/example?test=1",
    "host": "dosomething.org",
    "path": "/us/campaigns/example",
    "query": {
      "test": "1"
    },
    "landingTimestamp": "12324654765",
    "sessionId": "1510241882481966331510241882481",
    "referrer": {
      "href": "http://hello.world/blah/blah?puppet=sloth",
      "host": "hello.world",
      "path": "/blah/blah",
      "query": {
        "puppet": "sloth"
      }
    }
  },
  "device": {
    "size": "small"
  },
  "data": {
    "anything": {
      "we": {
        "want": "",
      }
    }
  },
}
```
