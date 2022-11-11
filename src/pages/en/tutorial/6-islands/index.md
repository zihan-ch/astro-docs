---
layout: ~/layouts/TutorialLayout.astro
unitTitle: Set sail for Astro islands
title: "Check in: Unit 6 - Astro Islands"
description: "Tutorial: Build your first Astro blog —\nUse Astro islands to bring frontend framework components into your Astro site"
setup: |
  import Badge from '~/components/Badge.astro';
  import Box from '~/components/tutorial/Box.astro';
  import Checklist from '~/components/Checklist.astro';
  import MultipleChoice from '~/components/tutorial/MultipleChoice.astro';
  import Option from '~/components/tutorial/Option.astro';
  import StackblitzIntro from '~/components/tutorial/StackblitzIntro.astro';
---
Now that you have a fully functioning blog, let's add some interactive islands to your site!

## Where are we now?

<StackblitzIntro tree="withastro/blog-tutorial-demo/tree/unit-6/start"/>

## Where are we going?

In this unit, you'll use **Astro islands** to bring frontend framework components into your Astro site. 

You will:
- Add a UI framework, Preact, to your Astro project
- Use Preact to create an interactive greeting component
- Learn when you might _not_ choose islands for interactivity


<Box icon="check-list">

## Checklist

<Checklist key ="interactivity">
- [ ] I am ready to add some interactivity to my site, and start living that island life!
</Checklist>
</Box>
