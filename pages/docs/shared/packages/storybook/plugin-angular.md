---
title: Set up Storybook for Angular Projects
description: This guide explains how to set up Storybook for Angular projects in your Nx workspace.
---

# Set up Storybook for Angular Projects

This guide will walk you through setting up [Storybook](https://storybook.js.org) for Angular projects in your Nx workspace.

{% callout type="warning" title="Set up Storybook in your workspace" %}
You first need to set up Storybook for your Nx workspace, if you haven't already. You can read the [Storybook plugin overview guide](/packages/storybook) to get started.
{% /callout %}

## Generate Storybook Configuration for an Angular project

You can generate Storybook configuration for an individual Angular project by using the [`@nx/angular:storybook-configuration` generator](/packages/angular/generators/storybook-configuration), like this:

```shell
nx g @nx/angular:storybook-configuration project-name
```

## Auto-generate Stories

The [`@nx/angular:storybook-configuration` generator](/packages/angular/generators/storybook-configuration) has the option to automatically generate `*.stories.ts` files for each component declared in the library. The stories will be generated using [Component Story Format 3 (CSF3)](https://storybook.js.org/blog/storybook-csf3-is-here/).

```text
<some-folder>/
├── my.component.ts
└── my.component.stories.ts
```

If you add more components to your project, and want to generate stories for all your (new) components at any point, you can use the [`@nx/angular:stories` generator](/packages/angular/generators/stories):

```shell
nx g @nx/angular:stories --project=<project-name>
```

{% callout type="note" title="Example" %}
Let's take for a example a library in your workspace, under `libs/feature/ui`, called `feature-ui`. This library contains a component, called `my-button`.

The command to generate stories for that library would be:

```shell
nx g @nx/angular:stories --project=feature-ui
```

and the result would be the following:

```text
<workspace name>/
├── apps/
├── libs/
│   ├── feature/
│   │   ├── ui/
|   |   |   ├── .storybook/
|   |   |   ├── src/
|   |   |   |   ├──lib
|   |   |   |   |   ├──my-button
|   |   |   |   |   |   ├── my-button.component.ts
|   |   |   |   |   |   ├── my-button.component.stories.ts
|   |   |   |   |   |   └── etc...
|   |   |   |   |   └── etc...
|   |   |   ├── README.md
|   |   |   ├── tsconfig.json
|   |   |   └── etc...
|   |   └── etc...
|   └── etc...
├── nx.json
├── package.json
├── README.md
└── etc...
```

{% /callout %}

## Cypress tests for Stories

The [`@nx/angular:storybook-configuration` generator](/packages/angular/generators/storybook-configuration) gives the option to set up an e2e Cypress app that is configured to run against the project's Storybook instance.

To launch Storybook and run the Cypress tests against the iframe inside of Storybook:

```shell
nx run project-name-e2e:e2e
```

The url that Cypress points to should look like this:

`'/iframe.html?id=buttoncomponent--primary&args=text:Click+me!;padding;style:default'`

- `buttoncomponent` is a lowercase version of the `Title` in the `*.stories.ts` file.
- `primary` is the name of an individual story.
- `style=default` sets the `style` arg to a value of `default`.

Changing args in the url query parameters allows your Cypress tests to test different configurations of your component. You can [read the documentation](https://storybook.js.org/docs/angular/writing-stories/args#setting-args-through-the-url) for more information.

## Example Files

Let's take for a example a library in your workspace, under `libs/feature/ui`, called `feature-ui` with a component, called `my-button`.

### Story file

The [`@nx/angular:storybook-configuration` generator](/packages/angular/generators/storybook-configuration) would generate a Story file that looks like this:

```typescript {% fileName="libs/feature/ui/src/lib/my-button/my-button.component.stories.ts" %}
import { Meta } from '@storybook/angular';
import { MyButtonComponent } from './my-button.component';

export default {
  title: 'MyButtonComponent',
  component: MyButtonComponent,
} as Meta<MyButtonComponent>;

export const Primary = {
  render: (args: MyButtonComponent) => ({
    props: args,
  }),
  args: {
    text: 'Click me!',
    padding: 10,
    disabled: true,
  },
};
```

### Cypress test file

For the library described above, Nx would generate an E2E project called `feature-ui-e2e` with a Cypress test file that looks like this:

```typescript {% fileName="apps/feature-ui-e2e/src/e2e/my-button/my-button.component.cy.ts" %}
describe('feature-ui', () => {
  beforeEach(() =>
    cy.visit(
      '/iframe.html?id=mybuttoncomponent--primary&args=text:Click+me!;padding:10;disabled:true;'
    )
  );

  it('should contain the right text', () => {
    cy.get('button').should('contain', 'Click me!');
  });
});
```

Depending on your Cypress version, the file will end with `.spec.ts` or `.cy.ts`.

## More Documentation

- [Set up Compodoc for Storybook on Nx](/packages/storybook/documents/angular-storybook-compodoc)
- [Information about the `storybook` targets](/deprecated/storybook/angular-storybook-targets)
- [Configuring styles and preprocessor options](/packages/storybook/documents/angular-configuring-styles)
- [The `browserTarget`](/deprecated/storybook/angular-browser-target)

You can find all Storybook-related Nx topics [here](/packages#storybook).

For more on using Storybook, see the [official Storybook documentation](https://storybook.js.org/docs/angular/get-started/introduction).

### Migration Scenarios

Here's more information on common migration scenarios for Storybook with Nx. For Storybook specific migrations that are not automatically handled by Nx please refer to the [official Storybook page](https://storybook.js.org/)

- [Set up Storybook version 7](/packages/storybook/documents/storybook-7-setup)
- [Migrate to Storybook version 7](/packages/storybook/generators/migrate-7)

#### Older migration scenarios

- [Upgrading to Storybook 6](/deprecated/storybook/upgrade-storybook-v6-angular)
- [Migrate to the new Storybook `webpackFinal` config](/deprecated/storybook/migrate-webpack-final-angular)
