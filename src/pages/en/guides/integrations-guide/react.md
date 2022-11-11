---
# NOTE: This file is auto-generated from 'scripts/generate-integration-pages.ts'
#       and pulls content directly from the package’s README.
#       DO NOT MAKE EDITS TO THIS FILE DIRECTLY, THEY WILL BE OVERWRITTEN!
#       For corrections, please edit the package README at
#       https://github.com/withastro/astro/tree/main/packages/integrations/react/
#
# TRANSLATORS: please remove this note and the <DontEditWarning/> component.

layout: ~/layouts/IntegrationLayout.astro
title: '@astrojs/react'
description: Learn how to use the @astrojs/react framework integration to extend component support in your Astro project.
githubURL: 'https://github.com/withastro/astro/tree/main/packages/integrations/react/'
hasREADME: true
category: renderer
i18nReady: false
setup: |
  import Video from '~/components/Video.astro';
  import DontEditWarning from '../../../../components/DontEditWarning.astro';
---

<DontEditWarning/>

This **[Astro integration][astro-integration]** enables server-side rendering and client-side hydration for your [React](https://reactjs.org/) components.

## Installation

There are two ways to add integrations to your project. Let's try the most convenient option first!

### `astro add` command

Astro includes a CLI tool for adding first party integrations: `astro add`. This command will:

1.  (Optionally) Install all necessary dependencies and peer dependencies
2.  (Also optionally) Update your `astro.config.*` file to apply this integration

To install `@astrojs/react`, run the following from your project directory and follow the prompts:

```sh
# Using NPM
npx astro add react
# Using Yarn
yarn astro add react
# Using PNPM
pnpm astro add react
```

If you run into any issues, [feel free to report them to us on GitHub](https://github.com/withastro/astro/issues) and try the manual installation steps below.

### Install dependencies manually

First, install the `@astrojs/react` integration like so:

```sh
npm install @astrojs/react
```

Most package managers will install associated peer dependencies as well. Still, if you see a "Cannot find package 'react'" (or similar) warning when you start up Astro, you'll need to install `react` and `react-dom`:

```sh
npm install react react-dom
```

Now, apply this integration to your `astro.config.*` file using the `integrations` property:

**`astro.config.mjs`**

```js
import react from '@astrojs/react';

export default {
  // ...
  integrations: [react()],
}
```

## Getting started

To use your first React component in Astro, head to our [UI framework documentation][astro-ui-frameworks]. You'll explore:

*   📦 how framework components are loaded,
*   💧 client-side hydration options, and
*   🤝 opportunities to mix and nest frameworks together

## Troubleshooting

For help, check out the `#support` channel on [Discord](https://astro.build/chat). Our friendly Support Squad members are here to help!

You can also check our [Astro Integration Documentation][astro-integration] for more on integrations.

## Contributing

This package is maintained by Astro's Core team. You're welcome to submit an issue or PR!

[astro-integration]: /en/guides/integrations-guide/

[astro-ui-frameworks]: /en/core-concepts/framework-components/#using-framework-components
