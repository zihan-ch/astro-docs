---
layout: ~/layouts/MainLayout.astro
title: Images
description: Learn how to use images in Astro.
i18nReady: true
setup: |
  import Since from '../../../components/Since.astro';
---
Astro provides several ways for you to use images on your site, whether they are stored locally inside your project, linked to remotely, or stored in a CMS or CDN!

### In `.astro` files

Astro uses HTML `<img>` elements to display images, and all HTML image attributes are supported. 

The `src` attribute is required, and its format will depend on where your images are stored:

```astro title="src/pages/index.astro"
---
import rocket from '../images/rocket.svg';
---
<!-- Remote image on another server -->
<img src="https://astro.build/assets/logo.png" width="25" alt="Astro">

<!-- Local image stored at public/assets/stars.png -->
<img src="/assets/stars.png" alt="A starry night sky.">

<!-- Local image stored at src/images/rocket.svg -->
<img src={rocket} alt="A rocketship in space."/>
```

### In Markdown files

You can use standard Markdown `![]()` syntax or standard HTML `<img>` tags in your `.md` files for images located in your `public/` folder, or remote images on another server.

If you cannot keep your images in `public/`, we recommend using the `.mdx` file format, which allows you to combine imported components with Markdown-like syntax. Use the [MDX integration](/en/guides/integrations-guide/mdx/) to add support for MDX to Astro.

```md
// src/pages/post-1.md

# My Markdown Page

<!-- Local image stored at public/assets/stars.png -->
![A starry night sky.](/assets/stars.png)
<img src="/assets/stars.png" alt="A starry night sky.">

<!-- Remote image on another server -->
![Astro](https://astro.build/assets/logo.png)
<img src="https://astro.build/assets/logo.png" width="25" alt="Astro">
```

### In MDX files

You can use standard Markdown `![]()` syntax or  JSX `<img />` tags in your `.mdx` files to display images from your `public/` folder or remote servers. 

Additionally, you can import and use images located in your project's `src/` directory, as you would in Astro components.

```mdx title="src/pages/post-1.mdx"

import rocket from '../images/rocket.svg';

# My MDX Page

// Local image stored at src/images/rocket.svg
<img src={rocket} alt="A rocketship in space."/>

// Local image stored at public/assets/stars.png
![A starry night sky.](/assets/stars.png)
<img src="/assets/stars.png" alt="A starry night sky." />

// Remote image on another server
![Astro](https://astro.build/assets/logo.png)
<img src="https://astro.build/assets/logo.png" width="25" alt="Astro" />
```

### In UI Framework Components

When adding images in a [UI framework component](/en/core-concepts/framework-components/) (e.g React, Svelte), use the image syntax appropriate for that particular component's framework.

## Where to store images

### `src/`

Your images stored in `src/` can be used by components (`.astro`, `.mdx` and other UI frameworks) but not in Markdown files. 

We recommend that you keep your images in [`public/`](#public) or store them [remotely](#using-images-from-a-cms-or-cdn) if you must use Markdown files.

Import them from a **relative file path** or [import alias](/en/guides/aliases/) in any component file and then use the import as the image's `src` attribute. 


```astro
---
// src/pages/index.astro

// Access images in `src/images/`
import logo from '../images/logo.png';
---
<img src={logo} width="40" alt="Astro" />
```

### `public/`

Your images stored in `public/` can be used by components (`.astro`, `.mdx` and other UI frameworks) and also Markdown files.


However, files in the `/public` directory are always served or copied as-is, with no processing. If you are using images outside of Markdown files, we recommend that local images are kept in `src/` when possible so that Astro can transform, optimize and bundle them.

The `src` attribute is **relative to the public folder**. In Markdown, you can also use the `![]()` notation.

```astro title="src/pages/index.astro"
---
// Access images in `public/images/`
---
<img src="/images/logo.png" />
```

## Astro's Image Integration

Astro's official image integration provides two different Astro components for rendering optimized images, `<Image />` and `<Picture />`. It is supported for all static sites and for [some server-side rendering deploy hosts](/en/guides/integrations-guide/image/#installation).

After [installing `@astrojs/image`](/en/guides/integrations-guide/image/#installation), you can use these two components wherever you can use Astro components: in `.astro` and `.mdx` files.

### `<Image />`

Astro's [`<Image />` component](/en/guides/integrations-guide/image/#image-) allows you to optimize a single image and specify width, height, and/or aspect ratio. You can even transform your image to a particular output format.

This component is useful for images where you want to keep a consistent size across displays, or closely control the quality of an image (e.g. logos).

For responsive images, or art direction, use the `<Picture />` component instead.

#### Local Images in `src/`

(required attributes: [`src`](/en/guides/integrations-guide/image/#src), and [`alt`](/en/guides/integrations-guide/image/#alt))

Import your image in frontmatter and pass it directly to the `<Image />` component's `src` attribute.

`alt` is required, but [all other properties](/en/guides/integrations-guide/image/#image-) are optional and will default to the image file's original properties if not provided.

#### Remote Images

(required attributes: [`src`](/en/guides/integrations-guide/image/#src), [`alt`](/en/guides/integrations-guide/image/#alt), [`format`](/en/guides/integrations-guide/image/#format), and dimensions)

Pass a full URL to the `<Image />` component's `src` attribute and include a value for `alt`.

The `<Image />` component cannot determine the original file format of a remote image, so you must provide an output `format` (e.g. png, avif) to transform your remote image.

You must also either provide [`width`](/en/guides/integrations-guide/image/#width) and [`height`](/en/guides/integrations-guide/image/#height), or one of the dimensions plus an [`aspectRatio`](/en/guides/integrations-guide/image/#aspectratio) to avoid content layout shifts because the `<Image />` component does not know the dimensions of a remote image.

[All other properties](/en/guides/integrations-guide/image/#image-) are optional.

#### Local Images in `public/`

(required attributes: [`src`](/en/guides/integrations-guide/image/#src), [`alt`](/en/guides/integrations-guide/image/#alt), [`format`](/en/guides/integrations-guide/image/#format), and dimensions)

Pass the component's `src` attribute a path relative to the public folder and include a value for `alt`. 

It will be treated as a remote image, which requires either both [`width`](/en/guides/integrations-guide/image/#width) and [`height`](/en/guides/integrations-guide/image/#height), or one dimension and an [`aspectRatio`](/en/guides/integrations-guide/image/#aspectratio) attribute. 

A value for the `format` attribute (e.g. png, avif) to transform your image is required.

[All other properties](/en/guides/integrations-guide/image/#image-) are optional.

Your original image will be copied unprocessed to the build folder, like all files located in `public/`, and Astro's image integration will also return optimized versions of the image.

#### Examples

```astro
---
// src/pages/index.astro
import { Image } from '@astrojs/image/components';
import localImage from "../assets/logo.png";
const remoteImage = "https://picsum.photos/id/957/300/200.jpg";
const localAlt = "The Astro Logo";
const remoteAlt = "A low-angle view of a forest during the daytime";
---

<!--optimized local image, keeping the original width, height, and image format-->
<Image src={localImage} alt={localAlt} />

<!-- height will be recalculated to match the original aspect ratio-->
<Image src={localImage} width={300} alt={localAlt} />

<!--For remote images, the desired dimensions and format are required-->
<Image src={remoteImage} width={300} aspectRatio="1:1" format="png" alt={remoteAlt} />

<!-- cropping to a specific width and height -->
<Image src={localImage} width={300} height={600} alt={localAlt}/>
<Image src={remoteImage} width={544} height={184} format="png" alt={remoteAlt}/>

<!-- cropping to a specific aspect ratio and converting to an avif format-->
<Image src={localImage} aspectRatio="16:9" format="avif" alt={localAlt}/>
<Image src={remoteImage} height={200} aspectRatio="16:9" format="avif" alt={remoteAlt}/>

<!-- local image imports can also be inlined directly-->
<Image src={import('../assets/logo.png')} alt={localAlt}/>

<!-- If an image is stored in the `/public` folder, use its path relative to `/public`-->
<Image src="/penguin.jpg" width="300" aspectRatio={1} format="png" alt="A happy penguin"/>
```

### `<Picture /> `

Astro's [`<Picture />` component](/en/guides/integrations-guide/image/#picture-) can be used to provide responsive images on your site, including multiple image sizes, formats, and layouts. 

You can let the user's browser choose appropriate image sizes, resolutions, and file types based on factors like screen size and bandwidth. Or, you can specify rules that the browser must follow based on media queries. 

This component is useful to optimize what your user sees at various screen sizes, or for art direction.

:::tip
Check out MDN's guide for more information about [responsive images and art direction](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images#art_direction).
:::

#### Local Images

(required attributes: [`src`](/en/guides/integrations-guide/image/#src-1), [`widths`](/en/guides/integrations-guide/image/#widths), [`sizes`](/en/guides/integrations-guide/image/#sizes), [`alt`](/en/guides/integrations-guide/image/#alt-1))

Import your image in frontmatter and pass it directly to the `<Picture />` component's `src` attribute.

You must provide the component with guidance for image widths and screen sizes, but [all other properties](/en/guides/integrations-guide/image/#picture-) are optional.

By default, the `<Picture />` component's [`formats`](/en/guides/integrations-guide/image/#formats) will include `avif` and `webp` in addition to the image's original format if not specified.

#### Remote Images 

(required attributes: [`src`](/en/guides/integrations-guide/image/#src-1), [`widths`](/en/guides/integrations-guide/image/#widths), [`sizes`](/en/guides/integrations-guide/image/#sizes), [`alt`](/en/guides/integrations-guide/image/#alt-1), and [`aspectRatio`](/en/guides/integrations-guide/image/#aspectratio-1))

Pass a full URL to the `<Picture />` component's  `src` attribute.

A value for `aspectRatio` is also required to ensure the correct height can be calculated at build time for remote images.

You must provide the component with guidance for image widths and screen sizes, but [all other properties](/en/guides/integrations-guide/image/#picture-) are optional.

Although [`formats`](/en/guides/integrations-guide/image/#formats) is not required, the original image format of remote images is unknown and will not be included by default. If not provided, only `webp` and `avif` will be included.

#### Local Images in `public/`

(required attributes: [`src`](/en/guides/integrations-guide/image/#src-1), [`widths`](/en/guides/integrations-guide/image/#widths), [`sizes`](/en/guides/integrations-guide/image/#sizes), [`alt`](/en/guides/integrations-guide/image/#alt-1), and [`aspectRatio`](/en/guides/integrations-guide/image/#aspectratio-1))

Pass the component's `src` attribute a path relative to the public folder and include a value for `alt`. 

The image will be treated as a remote image, so a value for `aspectRatio` is also required to ensure the correct height can be calculated at build time.

You must provide the component with guidance for image widths and screen sizes, but [all other properties](/en/guides/integrations-guide/image/#picture-) are optional.

Although [`formats`](/en/guides/integrations-guide/image/#formats) is not required, the original image format of images in the `public/` folder is unknown and will not be included by default. If not provided, only `webp` and `avif` will be included.

Your original image will be copied unprocessed to the build folder, like all files located in `public/`, and Astro's image integration will also return optimized versions of the image.

#### Examples

```astro
---
import { Picture } from '@astrojs/image/components';
import localImage from '../assets/logo.png';
const remoteImage = 'https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png';
---

<!--Local image with multiple sizes and formats-->
<Picture src={localImage} widths={[200, 400, 800]} sizes="(max-width: 800px) 100vw, 800px" formats={['avif', 'jpeg', 'png', 'webp']} alt="The Astro logo" />

<!--Remote image (aspect ratio is required)-->
<Picture src={remoteImage} widths={[200, 400, 800]} aspectRatio="4:3" sizes="(max-width: 800px) 100vw, 800px" alt="The Google logo" />

<!--Images in /public work like remote images-->
<Picture src="/logo.png" widths={[200, 400, 800]} aspectRatio="4:3" sizes="(max-width: 800px) 100vw, 800px" alt="The Google logo" />

<!--Inlined imports are supported-->
<Picture src={import("../assets/logo.png")} widths={[200, 400, 800]} sizes="(max-width: 800px) 100vw, 800px" alt="The Astro logo" />

```


### Using in MDX

In `.mdx` files, `<Image />` and `<Picture />` can receive your image `src` through imports and exports.

```mdx
// src/pages/index.mdx

import { Image, Picture } from '@astrojs/image/components';
import rocket from '../assets/rocket.png';
export const galaxy = 'https://astro.build/assets/galaxy.jpg';

<Image src={import('../assets/logo.png')} alt="Astro"/>
<Image src={rocket} width={300} alt="Spaceship approaching the moon."/>
<Picture src={rocket} widths={[200, 400, 800]} sizes="(max-width: 800px) 100vw, 800px" alt="A rocket blasting off." />
<Picture src={galaxy} widths={[200, 400, 800]} aspectRatio={16/9} sizes="(max-width: 800px) 100vw, 800px" alt="Outer space." />
```

### Setting Default Values

Currently, there is no way to specify default values for all `<Image />` and `<Picture />` components. Required attributes should be set on each individual component.

As an alternative, you can wrap these components in another Astro component for reuse. For example, you could create a component for your blog post images:

```astro title="src/components/BlogPostImage.astro"
---
import { Picture } from '@astrojs/image/components';

const {src, ...attrs} = Astro.props;
---
<Picture src={src} widths={[400, 800, 1500]} sizes="(max-width: 767px) 100vw, 736px" {...attrs} />

<style>
  img, picture :global(img), svg {
    margin-block: 2.5rem;
    border-radius: 0.75rem;
  }
</style>
```

## Using Images from a CMS or CDN

Image CDNs work with Astro. Use an image's full URL as the `src` attribute in an `<img>` tag or Markdown notation. 

Alternatively, if the CDN provides a Node.js SDK, you can use that in your project. For example, [Cloudinary’s SDK](https://cloudinary.com/documentation/node_integration) can generate the `<img>` tag with the appropriate `src` for you.

To use [external images with the `<Image />`](#remote-images) and [`<Picture />`](#remote-images-1) components from Astro's image integration, you must specify the appropriate dimension and format values for working with remote images.

## Alt Text

Not all users can see images in the same way, so accessibility is an especially important concern when using images. Use the `alt` attribute to provide [descriptive alt text](https://www.w3.org/WAI/tutorials/images/) for images. 

This attribute is required for the image integration's `<Image />` and `<Picture />` components. These components will throw an error if no alt text is provided. 

If the image is merely decorative (i.e. doesn’t contribute to the understanding of the page), set `alt=""` so that screen readers know to ignore the image.

## Community Integrations

In addition to the official [`@astrojs/image`](/en/guides/integrations-guide/image/) integration, there are several third-party [community image integrations](https://astro.build/integrations/css+ui/?q=image) for optimizing and working with images in your Astro project.
