# Contributing

We keep our [code open-source at DoSomething.org](https://github.com/dosomething) when possible.

## Style Guide

Most of our major projects use StyleCI to lint each commit to the repo.

### Imports

We prefer to sort import statements by ascending line length, e.g.:

```
import React from 'react';
import gql from 'graphql-tag';
import PropTypes from 'prop-types';

import Query from '../../../../Query';
import ReferralsListItem from './ReferralsListItem';
import SectionHeader from '../../../../utilities/SectionHeader/SectionHeader';

```

### Line breaks

We also prefer to add a new line before and/or after declaring a variable, to help with visibility:

```
function getThings(numberOfThings) {
  const thingOne = new Thing();

  if (numberOfThings === 1) {
    return thingOne;
  }

  const thingTwo = new Thing();

  thingTwo.sing();
  // ....
}

```

## Assets

### SVG

When adding a SVG to the codebase, be sure to run it SVGs through this optimizer: https://jakearchibald.github.io/svgomg.

It will help strip out a bunch of unnecessary junk that Adobe Illustrator, Sketch (insert your favorite vector program), etc add to the code. The default settings are usually just fine and you’ll usually see around like a 50% drop in file size for most SVGs!

## Tests

### Data attributes

When writing tests, it's handy to check for the existence of specific HTML elements, but we want to avoid adding an id or class to an element if it is only used specifically for testing.

For those instances, we'll use `data-testid` attributes.

In some projects, like Phoenix, we include a package called [Testing Library](https://testing-library.com/docs/dom-testing-library/api-queries#bytestid) that provides a wonderful set of helpful utilities that can be used within our Jest and Cypress tests. It specifically includes a helper method that can grab elements by a specified `data-testid`.

Example component:

```jsx
<MyComponent>
  {numberOfReferrals > 3 ? (
    <div
      data-testid="additional-referrals-count"
      className="text-center md:text-left md:pt-16"
    >
      {`+ ${numberOfReferrals - 3} more`}
    </div>
  ) : null}
</MyComponent>
```

Example test utilizing testing-library utility:

```js
import { screen } from "@testing-library/react";

it("Should display additional referrals count", () => {
  cy.findByTestId("additional-referrals-count").contains("+ 2 more");
});
```

Example vanilla test:

```js
it("Should display additional referrals count", () => {
  cy.get("[data-testid=additional-referrals-count]").contains("+ 2 more");
});
```

**Note**: We haven't been consistent about using a `data-testid` attribute, there is already a mix of other attributes like `data-test`, `data-id`, `data-ref` and `data-contentful-id` instances in the Phoenix codebase besides `data-testid`, that ideally should be refactored as `data-testid`.
