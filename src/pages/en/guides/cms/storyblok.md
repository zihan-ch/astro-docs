---
title: Storyblok & Astro
description: Add content to your Astro project using Storyblok as a CMS
layout: ~/layouts/CMSLayout.astro
service: Storyblok
setup: |
    import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro'
stub: false
---

[Storyblok](https://www.storyblok.com/) is a component based headless CMS that allows you to manage your content by creating reusable components called Bloks.

## Integrating with Astro

In this section, we will use the [Storyblok integration](https://github.com/storyblok/storyblok-astro) to connect Storyblok to Astro.

### Prerequisites

To get started, you will need to have the following:

1. **An Astro project** - If you don't have an Astro project yet, our [Installation guide](/en/install/auto/) will get you up and running in no time.

2. **A Storyblok account and space** - If you don't have an account yet, [sign up for free](https://app.storyblok.com/#/signup) and create a new space.

3. **Storyblok API tokens** - You can find and generate your API tokens in the Access Tokens tab of your Storyblok space settings. 
    - **Preview token** - This token will be used in development to fetch drafts or unpublished versions of your content.
    - **Public token** - This token will be used in production builds to fetch only published content.

### Setting up credentials

To add your Storyblok credentials to Astro, create a `.env` file in the root of your project with the following variables:

```ini title=".env"
STORYBLOK_PREVIEW_TOKEN=YOUR_PREVIEW_TOKEN
STORYBLOK_PUBLIC_TOKEN=YOUR_PUBLIC_TOKEN
```

Now, you should be able to use these environment variables in your project.

Your root directory should now include this new file:

```ini title="Project Structure" ins={2}
├── src/
├── .env
├── astro.config.mjs
└── package.json
```

### Installing dependencies

To connect Astro with your Storyblok space, install the official [Storyblok integration](https://github.com/storyblok/storyblok-astro) using the single command below for your preferred package manager:

<PackageManagerTabs>
  <Fragment slot="npm">
  ```shell
  npm install @storyblok/astro 
  ```
  </Fragment>
  <Fragment slot="pnpm">
  ```shell
  pnpm install @storyblok/astro
  ```
  </Fragment>
  <Fragment slot="yarn">
  ```shell
  yarn add @storyblok/astro 
  ```
  </Fragment>
</PackageManagerTabs>

### Configuring Storyblok

Modify your Astro config file to include the Storyblok integration:

```js title="astro.config.mjs"
import { defineConfig } from 'astro/config';
import storyblok from '@storyblok/astro';
import { loadEnv } from 'vite';

const env = loadEnv(import.meta.env.MODE, process.cwd(), 'STORYBLOK');

export default defineConfig({
  integrations: [
    storyblok({
      accessToken:
        import.meta.env.MODE === 'development'
          ? env.STORYBLOK_PREVIEW_TOKEN
          : env.STORYBLOK_PUBLIC_TOKEN,
      components: {
        // Add your components here
      },
      apiOptions: {
        // Choose your Storyblok space region 
        region: 'us',
      },
    })
  ],
});
```

The Storyblok integration requires an object with the following properties:

1. `accessToken` - This references the Storyblok API tokens that we added in the previous step. We will use the preview token in development and the public token in production.

    :::tip
    Since the Astro config file does not normally support environment variables, use the `loadEnv` function from Vite to load them.
    :::

2. `components` - An object that maps Storyblok component names to paths to your local components. This is required to render your Storyblok components in Astro.

    :::note
    The component paths are relative to the `src` directory.
    :::

3. `apiOptions` - An object containing [Storyblok API options](https://github.com/storyblok/storyblok-astro#options). 

    :::caution
    The default region is `eu`. If your Storyblok space was created in the US region, you will need to set the `region: 'us'`.
    :::

### Connecting Bloks to Astro components

To connect your Bloks to Astro, create a new folder named `storyblok` in the `src` directory. This folder will contain all the Astro components that will match your Bloks in your Storyblok Blok library.

In this example, we have a `blogPost` Blok content type in our Storyblok library with the following fields:

- `title` - A text field
- `description` - A text field
- `content` - A rich text field

Our goal is to create the equivalent Astro component that will use these fields to render its content. To do this, create a new file named `BlogPost.astro` inside `src/storyblok` with the following content:

```astro title="src/storyblok/BlogPost.astro"
---
import { storyblokEditable, renderRichText } from '@storyblok/astro'

const { blok } = Astro.props
const content = renderRichText(blok.content)
---

<article {...storyblokEditable(blok)}>
  <h1>{blok.title}</h1>
  <p>{blok.description}</p>
  <Fragment set:html={content} />
</article>
```

The `blok` property contains the data that we will receive from Storyblok. It contains the values of the fields that we defined in the `blogPost` Blok in Storyblok.

To render our content, the integration provides utility functions such as:

- `storyblokEditable` - it adds the necessary attributes to the elements so that you can edit them in Storyblok.
- `renderRichText` - it transforms the rich text field into HTML.

Your root directory should include this new file:

```ini title="Project Structure" ins={3}
├── src/
│   └── storyblok/
│       └── BlogPost.astro
├── .env
├── astro.config.mjs
└── package.json
```

Finally, to connect the `blogPost` Blok to the `BlogPost` component, add a new property to your components object in your Astro config file. The key is the name of the Blok and the value is the path to the component.

```js title="astro.config.mjs" ins={15}
import { defineConfig } from 'astro/config';
import storyblok from '@storyblok/astro';
import { loadEnv } from 'vite';

const env = loadEnv(import.meta.env.MODE, process.cwd(), 'STORYBLOK');

export default defineConfig({
  integrations: [
    storyblok({
      accessToken:
        import.meta.env.MODE === 'development'
          ? env.STORYBLOK_PREVIEW_TOKEN
          : env.STORYBLOK_PUBLIC_TOKEN,
      components: {
        blogPost: 'storyblok/BlogPost',
      },
      apiOptions: { 
        region: 'us',
      },
    })
  ],
});
```
### Fetching data

To test the setup, in Storyblok create a new story with the `blogPost` content type named `test-post`. 

In Astro, create a new page in the `src/pages` directory named `test-post.astro` with the following content:

```astro title="pages/test-post.astro"
---
import { useStoryblokApi } from '@storyblok/astro'
import StoryblokComponent from '@storyblok/astro/StoryblokComponent.astro'

const storyblokApi = useStoryblokApi()

const { data } = await storyblokApi.get("cdn/stories/test-post", {
  version: import.meta.env.DEV ? "draft" : "published",
});

const content = data.story.content;
---
<StoryblokComponent blok={content} />
```

To query our data, we will use the `useStoryblokApi` hook. This will initialize a new client instance using our integration configuration.

To render our content, pass the `content` property of the Story to the `StoryblokComponent` as `blok`. This component will render the Bloks that are defined inside the `content` property. In our case, it will render the `BlogPost` component.

## Making a blog with Astro and Storyblok

With the integration set up, we can now create a blog with Astro and Storyblok.

### Prerequisites

1. **A Storyblok space** - For this tutorial, we recommend using a new space. If you already have a space with Bloks, feel free to use them, but you will need to modify the code to match the Blok names and content types.

2. **An Astro project integraded with Storyblok** - See [integrating with Astro](#integrating-with-astro) for instructions on how to set up the integration.

### Creating a blok library

To create Bloks, go to the Storyblok app and click on the **Block Library** tab. Click on the <kbd>+ New blok</kbd> button and create the following Bloks:

1. `blogPost` - A content type Blok with the following fields:
    - `title` - A text field
    - `description` - A text field
    - `content` - A rich text field

2. `blogPostList` - An empty nestable Blok

3. `page` - A content type Blok with the following fields:
    - `body` - A nestable Blok

### Creating content

To add content, go to the content section by clicking the **Content** tab. Using the Blok library that we created in the previous step, create the following stories:

1. `home` - A content type story with the `page` Blok. Inside the `body` field, add a `blogPostList` Blok.

2. `blog/no-javascript` - A story with the `blogPost` content type inside the blog folder.
    ```yaml
    title: No JavaScript
    description: A sample blog post
    content: Hi there! This blog post doesn't use JavaScript.
    ```
3. `blog/astro-is-amazing` - A story with the `blogPost` content type inside the blog folder.
    ```yaml
    title: Astro is amazing
    description: We love Astro
    content: Hi there! This blog post was build with Astro.
    ```

Now that we have our content ready, we can switch to our Astro project and start building our blog.

### Connecting Bloks to components

To connect your newly created Bloks to Astro components, create a new folder named `storyblok` in your `src` directory and add the following files:

`Page.astro` is a nestable Block content type component that will recursively render all the Bloks inside the `body` property of the `page` Blok. It also adds the `storyblokEditable` attributes to the parent element which will allow us to edit the page in Storyblok.

```astro title="src/storyblok/Page.astro"
---
import { storyblokEditable } from '@storyblok/astro'
import StoryblokComponent from '@storyblok/astro/StoryblokComponent.astro'
const { blok } = Astro.props
---

<main {...storyblokEditable(blok)}>
  {
    blok.body?.map((blok) => {
      return <StoryblokComponent blok={blok} />
    })
  }
</main>
```

`BlogPost.astro` will render the `title`, `description` and `content` properties of the `blogPost` Blok.

To transform the `content` property from a rich text field to HTML, we use the `renderRichText` function.

```astro title="src/storyblok/BlogPost.astro"
---
import { storyblokEditable, renderRichText } from '@storyblok/astro'
const { blok } = Astro.props
const content = renderRichText(blok.content)
---
<article {...storyblokEditable(blok)}>
  <h1>{blok.title}</h1>
  <p>{blok.description}</p>
  <Fragment set:html={content} />
</article>
```

`BlogPostList.astro` is a nestable Blok content type component that will render a list of blog posts previews. 

It uses the `useStoryblokApi` hook to fetch all the stories with the content type of `blogPost`. It uses the `version` query parameter to fetch the draft versions of the stories when in development mode and the published versions when building for production.

```astro title="src/pages/blogPostList.astro"
---
import { useStoryblokApi } from '@storyblok/astro'

const storyblokApi = useStoryblokApi();
const { data } = await storyblokApi.get('cdn/stories', {
  version: import.meta.env.DEV ? "draft" : "published",
  content_type: 'blogPost',
})
const posts = data.stories.map(story => {
  return {
    title: story.content.title,
    date: new Date(story.published_at).toLocaleDateString("en-US", {dateStyle: "full"}),
    description: story.content.description,
    slug: story.full_slug,
  }
})
---
<h1>My blog</h1>
<ul>
  {posts.map(post => (
    <li>
      <time>{post.date}</time>
      <a href={post.slug}>{post.title}</a>
      <p>{post.description}</p>
    </li>
  ))}
</ul>
```

Finally, add your components to the `components` property of the `astro.config.mjs` file. Each key is the name of the Blok in Storyblok, and the value is the path to the component relative to `src`.

```js title="astro.config.mjs" ins={15-17}
import { defineConfig } from 'astro/config';
import storyblok from '@storyblok/astro';
import { loadEnv } from 'vite';

const env = loadEnv(import.meta.env.MODE, process.cwd(), 'STORYBLOK');

export default defineConfig({
  integrations: [
    storyblok({
      accessToken:
        import.meta.env.MODE === 'development'
          ? env.STORYBLOK_PREVIEW_TOKEN
          : env.STORYBLOK_PUBLIC_TOKEN,
      components: {
        blogPost: 'storyblok/BlogPost',
        blogPostList: 'storyblok/BlogPostList',
        page: 'storyblok/Page',
      },
      apiOptions: { 
        region: 'us',
      },
    })
  ],
});
```

### Generating pages

#### Static site generation

If you are using Astro's default static site generation, you will use [dynamic routes](/en/core-concepts/routing/#dynamic-routes) and the `getStaticPaths` function to generate your project pages.

Create a new file in the `src/pages` directory called `[...slug].astro` and add the following code:

```astro title="src/pages/[...slug].astro"
---
import { useStoryblokApi } from '@storyblok/astro'
import StoryblokComponent from '@storyblok/astro/StoryblokComponent.astro'

export async function getStaticPaths() {
  const storyblokApi = useStoryblokApi()
  const { data } = await storyblokApi.get("cdn/stories", {
    version: import.meta.env.DEV ? "draft" : "published",
  });
  const pages = data.stories.map(story => {
    return {
      params: {
        slug: story.full_slug === 'home' ? undefined : story.full_slug
      },
      props: {
        content: story.content
      }
    }
  })
  return pages
}
const { content } = Astro.props
---
<html lang="en">
  <head>
    <title>Storyblok & Astro</title>
  </head>
  <body>
    <StoryblokComponent blok={content} />
  </body>
</html>
```

This file will generate a page for each story, with the slug and content fetched from the Storyblok API. If the story's slug is `home`, we return an undefined slug, which generates the `/` route.

#### Server-side rendering

If you’ve [opted in to SSR mode](/en/guides/server-side-rendering/#enabling-ssr-in-your-project), you will use a dynamic route that uses a slug parameter to fetch the page data from Storyblok.

```astro title="src/pages/[...slug].astro"
---
import { useStoryblokApi } from '@storyblok/astro'
import StoryblokComponent from '@storyblok/astro/StoryblokComponent.astro'
const storyblokApi = useStoryblokApi()
const slug = Astro.params.slug === undefined ? "home" : Astro.params.slug
let content;
try {
  const { data } = await storyblokApi.get(`cdn/stories/${slug}`, {
    version: import.meta.env.DEV ? "draft" : "published",
  });
  content = data.story.content
} catch (error) {
  return Astro.redirect('/404')
}
---
<html lang="en">
  <head>
    <title>Storyblok & Astro</title>
  </head>
  <body>
    <StoryblokComponent blok={content} />
  </body>
</html>
```

This file will fetch and render the page data from Storyblok that matches the dynamic `slug` parameter.

If the story is not found, it will redirect to the 404 page.

### Publishing your site

To deploy your website, visit our [deployment guides](/en/guides/deploy/) and follow the instructions for your preferred hosting provider. 

#### Rebuild on Storyblok changes

If your project is using Astro's default static mode, you will need to set up a webhook to trigger a new build when your content changes. If you are using Netlify or Vercel as your hosting provider, you can use its webhook feature to trigger a new build from Storyblok events. 

##### Netlify

To set up a webhook in Netlify:

1. Go to your site dashboard and click on **Build & deploy**. 

2. Under the **Continuous Deployment** tab, find the **Build hooks** section and click on **Add build hook**. 

3. Provide a name for your webhook and select the branch you want to trigger the build on. Click on **Save** and copy the generated URL.

##### Vercel

To set up a webhook in Vercel:

1. Go to your project dashboard and click on **Settings**. 

2. Under the **Git** tab, find the **Deploy Hooks** section. 

3. Provide a name for your webhook and the branch you want to trigger the build on. Click **Add** and copy the generated URL.

##### Adding a webhook to Storyblok

In your Storyblok space **Settings**, click on the **Webhooks** tab. Paste the webhook URL you copied in the **Story published & unpublished** box. Finally, hit <kbd>Save</kbd> to create the webhook.

Now, whenever you publish a new story, a new build will be triggered and your blog will be updated.

## Official Resources

- Storyblok provides an [Astro Integration](https://www.storyblok.com/mp/announcing-storyblok-astro) to add Storyblok to your project.

## Community Resources 

- Add yours!
