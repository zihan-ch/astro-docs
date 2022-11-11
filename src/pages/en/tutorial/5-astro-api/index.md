---
layout: ~/layouts/TutorialLayout.astro
unitTitle: Beef up your blog
title: "Check in: Unit 5 - Astro API"
description: "Tutorial: Build your first Astro blog —\nFetching and using data from project files to dynamically generate page content and routes"
setup: |
  import Badge from '~/components/Badge.astro';
  import Box from '~/components/tutorial/Box.astro';
  import Checklist from '~/components/Checklist.astro';
  import MultipleChoice from '~/components/tutorial/MultipleChoice.astro';
  import Option from '~/components/tutorial/Option.astro';
  import StackblitzIntro from '~/components/tutorial/StackblitzIntro.astro';
---

Now that you have some blog posts, let's use Astro's API to work with your files!

## Where are we now?

<StackblitzIntro tree="withastro/blog-tutorial-demo/tree/unit-5/start"/>

## Where are we going?

In this unit, you'll supercharge your blog with an index page, tag pages, and an RSS feed. 

Along the way, you'll learn how to use:
- `Astro.glob()` to access data from files in your project
- `getStaticPaths()` to create multiple pages (routes) at once
- The Astro RSS package to create an RSS feed



<Box icon="check-list">

## Checklist

<Checklist>
- [ ] I am ready to add some blog features to my Astro project!
</Checklist>
</Box>
