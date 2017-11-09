# Puck events
Puck sends a multitude of events that describe the user journey.

## Enforced events
By default, the Puck Client sends events related to the users session and page views.

**visit**: This event is fired once per session, and marks the start of a new session.

**view**: This event is fired every time a user visits a page.

## Custom events
Applications using Puck can additionally send custom events. These are the following custom events in use, grouped by application source.

### phoenix-next

#### clicked facebook share
This event is fired anytime a user clicks the Facebook share button on Phoenix Next. Includes the following custom data,

**quote**: Custom text injected into the Facebook share, likely null in most cases as it's not being used in the product currently.

**parentSource**: This explains where on the page the Facebook share button was. The following is a list of possible parent sources ([Github query](https://github.com/DoSomething/phoenix-next/search?utf8=%E2%9C%93&q=parentSource&type=)),
- affirmation
- campaignUpdate
- dashboard
- quiz

**link**: The full link that is being shared in this event.

**variant**: The visual style of the button. This can either be `black` or `icon`.

Example data:
```json
{
  "quote": null,
  "parentSource": "campaignUpdate",
  "link": "https://www.dosomething.org/us/campaigns/defend-dreamers/blocks/usEJ6X1rNeC04iOGSK28Q",
  "variant": "icon"
}
```

#### facebook share posted
This event is fired if a user successfully posts to Facebook after clicking the Facebook share button. It inherits the same data from the `clicked facebook share` event.

#### facebook share cancelled
This event is fired if a user closes the Facebook share dialog after clicking the Facebook share button. It inherits the same data from the `clicked facebook share` event.

#### open modal
This event is fired whenever a user is presented with a modal.

**modalType**: The type of the modal that was presented. Can be one of the following types,
- POST_SIGNUP_MODAL
- CONTENT_MODAL
- REPORTBACK_UPLOADER_MODAL

Example data:
```json
{
  "modalType": "CONTENT_MODAL"
}
```

#### close modal
This event is fired when a modal is closed by the user. Includes the same data as the `open modal` event.

#### signup
This event is fired when a user clicks a Signup button. It does not verify the user is authenticated, sucessfuly authenticated if not already, and hit Rogue without error. Again, it only tracks the button was pressed. Any futher validation should be done within Quasar.

**source**: The place on the page where the signup occurred. Possible values are ([Github search](https://github.com/DoSomething/phoenix-next/search?utf8=%E2%9C%93&q=SignupButtonFactory&type=)),
- lede banner
- legacy lede banner
- call to action
- tabbed navigation

**sourceData**: Optional object containing additional information about the source.

**sourceText**: The text in the button when it was pressed.

**template**: The campaign template the user is currently looking at. Possible values are
- mosaic
- legacy

If null, assume the default template is mosaic.

**campaignId**: The campaign id of the campaign which the user signed up for.

Example Data:
```json
{
  "source": "legacy lede banner",
  "sourceData": {
    "text": "sign up"
  },
  "template": "legacy",
  "campaignId": "7992"
}
```

### northstar

#### clicked facebook auth
This event is fired when a user clicks the Facebook authentication button.

#### has validation errors
This event is fired if a user attempts to login with Northstar but encounters a validation error when they submit a form.

**invalidFields**: An array of Northstar fields the user had an error submitting (eg: `mobile`, `email`)

**validationMessages**: An array of error messages that was presented to the user about the fields.

Assume the indexes in both arrays match each other.

Example data
```json
{
  "invalidFields": ["email"],
  "validationMessages": ["The email has already been taken."],
}
```
