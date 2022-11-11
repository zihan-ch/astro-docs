---
layout: ~/layouts/TutorialLayout.astro
unitTitle: Add, style and link to pages on your site
title: "Check in: Unit 2 - Pages"
description: "Tutorial: Build your first Astro blog —\nCreate, style and link to pages posts on your site"
setup: |
  import Checklist from '~/components/Checklist.astro';
  import Badge from '~/components/Badge.astro';
  import Box from '~/components/tutorial/Box.astro';
  import StackblitzIntro from '~/components/tutorial/StackblitzIntro.astro';
---
Now that you have a working site on the web, let's add pages and posts!

## Where are we now?

<StackblitzIntro tree="withastro/blog-tutorial-demo/tree/unit-2/start"/>

## Where are we going?

In this unit, you'll
- Create your first Astro pages with the `.astro` syntax
- Add blog posts with Markdown (`.md`) files
- Style an individual page with `<style>` 
- Apply global styles across pages

 Along the way, you'll learn how the **two sections of a `.astro` file**  work together to create a page, and how to use variables and conditional rendering on your pages.
 

<!-- ### Anatomy of an Astro file

astro title="src/pages/a-typical-astro-file.astro"
--- 
// Astro Script (frontmatter) 
// Written in JavaScript/TypeScript
// used for imports, variables, functions…
--- -->
<!-- Astro Template (body) -->  
<!-- Written in Astro (HTML with additional JSX-like features) -->
<!-- contains HTML elements, components, JX/JSX espressions -->

<!--
[.astro file example image, annotated - CAN WE GET A HIPPO SAMPLE WITH THE NEW CODE COMMENTS??]
-->



<Box icon="check-list">
## Checklist

<Checklist>
- [ ] I am ready to make some new pages for my Astro website!
</Checklist>
</Box>
