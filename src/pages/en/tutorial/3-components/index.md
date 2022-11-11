---
layout: ~/layouts/TutorialLayout.astro
unitTitle: Build and design with Astro UI components
title: "Check in: Unit 3 - Components"
description: "Tutorial: Build your first Astro blog —\nBuild Astro components to reuse code for common elements across your website"
setup: |
  import Badge from '~/components/Badge.astro';
  import Box from '~/components/tutorial/Box.astro';
  import Checklist from '~/components/Checklist.astro';
  import MultipleChoice from '~/components/tutorial/MultipleChoice.astro';
  import Option from '~/components/tutorial/Option.astro';
  import StackblitzIntro from '~/components/tutorial/StackblitzIntro.astro';
---
Now that you have `.astro` and `.md` files generating entire pages on your website, let's make and reuse smaller bits of HTML with Astro components!

## Where are we now?

<StackblitzIntro tree="withastro/blog-tutorial-demo/tree/unit-3/start"/>

## Where are we going?

In this unit, you'll learn how to create **Astro components** to reuse code for common elements across your website. 

You'll build:
- A Navigation component that presents a menu of links to your pages 
- A Footer component to include on the bottom of each page
- A Social Media component, used in the Footer, that links to profile pages
- An interactive Hamburger component to toggle the Navigation on mobile

Along the way, you'll use CSS and JavaScript to build a responsive design that reacts to screen sizes and user input.



<Box icon="check-list">
## Checklist

<Checklist>
- [ ] I am ready to build some Astro components!
</Checklist>
</Box>
