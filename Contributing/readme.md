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

## Assets

### SVG

When adding a SVG to the codebase, be sure to run it SVGs through this optimizer: https://jakearchibald.github.io/svgomg.

It will help strip out a bunch of unnecessary junk that Adobe Illustrator, Sketch (insert your favorite vector program), etc add to the code. The default settings are usually just fine and you’ll usually see around like a 50% drop in file size for most SVGs!

## Tests

### Data attributes

When writing tests, it's handy to check for the existence of specific HTML elements, but we want to avoid adding an id or class to an element if it's only used specifically for testing. For those instances, we'll use `data-test` (and if needed, `data-id`) attributes.

Example component:

```
  <MyComponent>
    {numberOfReferrals > 3 ? (
      <div
        data-test="additional-referrals-count"
        className="text-center md:text-left md:pt-16"
      >
        {`+ ${numberOfReferrals - 3} more`}
      </div>
    ) : null}
  </MyComponent>
```

Example test:

```
  it('Should display additional referrals count', () => {
    cy.get('[data-test=additional-referrals-count]').contains('+ 2 more');
  });
```

**Note**: We haven't been consistent about using a `data-test` attribute, there is already a mix of other attributes like `data-ref` and `data-contentful-id` instances in the Phoenix codebase besides `data-test`, that ideally should be refactored as `data-test` / `data-id`.
