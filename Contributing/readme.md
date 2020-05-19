# Contributing

We keep our [code open-source at DoSomething.org](https://github.com/dosomething) when possible.

## Style Guide

Most of our major projects use StyleCI to lint each commit to the repo.

## PHP Code
Imports are organized in line-length order.

## JS Code
Imports are organized in ascending line-length order, with third-party package imports at the top of the file, followed by a line break and local project imports.

e.g.:

```js
import React from 'react';
import gql from 'graphql-tag';
import PropTypes from 'prop-types';

import Query from "../../../../Query";
import ReferralsListItem from "./ReferralsListItem";
import SectionHeader from "../../../../utilities/SectionHeader/SectionHeader";
```

### Line breaks

We prefer to add a new line before and/or after declaring a variable, to help with visibility:

```js
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

We also prefer to add a new line between HTML sibling elements to help visually separate elements and make it easier to comprehend the structure:

```jsx
// ✅ The following shows proper spacing between sibling elements.
<SomeComponent>
  <article>
    <header>
      <h1>Fusce Dolor</h1>

      <div>Etiam porta sem malesuada magna mollis euismod.</div>
    </header>

    <div>
      <p>
        Sociis natoque <strong>penatibus</strong> et magnis dis parturient
        montes. Praesent commodo cursus magna, vel scelerisque et.
      </p>

      <p>Nulla vitae elit libero, a pharetra augue.</p>
    </div>
  </article>

  <div>
    <p>Sed posuere consectetur est at lobortis.</p>
  </div>
</SomeComponent>
```

```jsx
// 🚫 The following shows improper spacing between sibling elements.
<SomeComponent>
  <article>
    <header>
      <h1>Fusce Dolor</h1>
      <div>Etiam porta sem malesuada magna mollis euismod.</div>
    </header>
    <div>
      <p>
        Sociis natoque <strong>penatibus</strong> et magnis dis parturient
        montes. Praesent commodo cursus magna, vel scelerisque et.
      </p>
      <p>Nulla vitae elit libero, a pharetra augue.</p>
    </div>
  </article>
  <div>
    <p>Sed posuere consectetur est at lobortis.</p>
  </div>
</SomeComponent>
```

### React Components
Initially when rapidly developing [Phoenix](https://github.com/DoSomething/phoenix-next), we made some decisions on code file locations/hierarchy. Overtime we've come to refine this structure a bit, and are now working to update the Phoenix codebase with the latest decisions.

We want to update our code directory structure within the `/components` directory to follow this pattern: 

```
- /components
  |_ /actions
  |_ /blocks
  |_ /pages
  |_ /utilities
```

Most of the standalone components residing as children of the `/components` should fall within one of the above children directories. 

- `/components/actions` contains dispatchers for different actions.
- `/components/blocks` contains interface components that may match the interface components available in Contentful, or other components that show up as distinct stacked rows that build up a page.
- `/components/pages` contains components related to specific page types available on Phoenix.
- `/components/utilities` contains reusable items; they don’t have a “container” component and are typically used inside other components.

### Tests

#### Data attributes

When writing tests, it is handy to check for the existence of specific HTML elements, but we want to avoid adding an id or class to an element if it is only used specifically for testing.

For those instances, we will use `data-testid` attributes.

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

**Note**: We have not been consistent about using a `data-testid` attribute, there is already a mix of other attributes like `data-test`, `data-id`, `data-ref` and `data-contentful-id` instances in the Phoenix codebase besides `data-testid`, that ideally should be refactored as `data-testid`.

## CSS Code

We use the [Tailwind CSS Framework](https://tailwindcss.com/) on our user and admin interfaces [to help simplify](https://github.com/DoSomething/rfcs/blob/master/005-tailwindcss-framework.md) front-end development across our team.

For translating spacing scale using Tailwind, there's a [handy reference](https://tailwindcss.com/docs/customizing-spacing/#default-spacing-scale).


## Assets

### SVG

When adding a SVG to the codebase, be sure to run it SVGs through [this optimizer](https://jakearchibald.github.io/svgomg).

It will help strip out a bunch of unnecessary junk that Adobe Illustrator, Sketch (insert your favorite vector program), etc add to the code. The default settings are usually just fine and you’ll usually see around like a 50% drop in file size for most SVGs!
