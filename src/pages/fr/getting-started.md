---
setup: |
    import Button from '../../components/Button.astro'
    import ContributorList from '../../components/ContributorList.astro'
    import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro'
layout: ~/layouts/MainLayout.astro
title: Bien démarrer
description: Une intro basique à Astro.
---

## Astro, qu'est-ce que c'est ?

Astro est un **framework web** **tout-en-un** qui permet de construire des sites web **rapides** et **axés sur le contenu**.

## Principales caractéristiques

- **Composants Islands:** Une nouvelle architecture web pour élaborer des sites web plus rapides.
- **Design d'API Server-first:** Ne pas laisser aux périphériques utilisateurs le soin d'exécuter une hydratation coûteuse.
- **Aucun JS, par défaut:** Aucune surcharge d'exécution JavaScript pour vous ralentir.
- **Edge-ready:** Déployer n'importe où, même un environnement d'exécution global comme Deno ou Cloudflare.
- **Personnalisable:** Tailwind, MDX, et plus de 100 autres integrations à choisir.
- **UI-agnostic:** Support de React, Preact, Svelte, Vue, Solid, Lit et plus.

<!-- - **`client:visible` component loading:** If your user never sees it, it never loads. -->
<!-- - **Image optimizations:** Astro's very own `<Image />` component. -->
<!-- - **TypeScript support**  -->
<!-- - **File-based routing:** Every file in the pages directory becomes a route. -->

Consultez l'article détaillé [Pourquoi Astro](/fr/concepts/why-astro/) pour en apprendre plus sur ce qui rend Astro spécial. ✨

## Essayez Astro dans votre navigateur

Visitez [astro.new](https://astro.new/) et choisissez parmi une grandre variété de modèles pour débuter. Testez une version complète et fonctionnelle d'Astro directement dans votre navigateur !

<div style="display: flex; flex-wrap: wrap; gap: 0.5rem;">
  <Button href="https://astro.new/basics?on=stackblitz">Lancer le modèle basique</Button>
  <Button variant="outline" href="https://astro.new/">Voir tous les modèles →</Button>
</div>

## Démarrez votre premier projet

Obtenez un nouveau projet Astro opérationnel localement avec notre assistant utile en ligne de commande `create-astro` !

<PackageManagerTabs>
  <Fragment slot="npm">
  ```shell
  # créer un nouveau projet avec npm
  npm create astro@latest
  ```
  </Fragment>
  <Fragment slot="pnpm">
  ```shell
  # créer un nouveau projet avec pnpm
  pnpm create astro@latest
  ```
  </Fragment>
  <Fragment slot="yarn">
  ```shell
  # créer un nouveau projet avec yarn
  yarn create astro
  ```
  </Fragment>
</PackageManagerTabs>

Notre [Guide d'installation](/fr/install/auto/) contient des instructions complètes, étape par étape, pour installer Astro à l'aide de votre gestionnaire de packages préféré.

## Apprendre Astro

Voir des exemples de certains des concepts et modèles clés d'un site Astro !

📚 [Ajouter une première page](/fr/core-concepts/astro-pages/) à votre site.

📚 En savoir plus sur la [structure de projet](/fr/core-concepts/project-structure/) d'Astro.

📚 En savoir plus sur le [routage basé sur les fichiers](/fr/core-concepts/routing/) d'Astro.

*... retrouvez notre documentation d'API complète sous l'onglet **Référence**.*

## Etendre Astro

🧰 Démarrez votre prochain projet avec un [thème prédéfini](https://astro.build/themes/)

🧰 Personnalisez votre site avec les [plugins et composants](https://astro.build/integrations/) officiels et ceux de la communauté.

🧰 Soyez inspiré en visitant le [showcase de sites](https://astro.build/showcase/).

*... voir notre [guide sur l'utilisation des intégrations](/fr/guides/integrations-guide/)*.

## Rejoignez notre Communauté

Rejoignez-nous dans le [Discord Astro](https://astro.build/chat/) afin de partager et obtenir de l'aide de la part d'une communauté active et accueillante !

💬 Dîtes bonjour dans notre channel `#introduce-yourself` !

💬 Posez une question à notre équipe d'aide dans notre channel `#support-threads` !

💬 Partagez ce sur quoi vous avez travaillé dans notre channel `#showcase` !

## En savoir plus

[Blog Astro](https://astro.build/blog/)

[Changelog Astro](https://github.com/withastro/astro/blob/main/packages/astro/CHANGELOG.md)

[Guide de migration Astro](/fr/migrate/)

## Contribuer

Ces docs vous sont proposées par toutes ces personnes. [Rejoignez-nous sur GitHub !](https://github.com/withastro/docs)

<ContributorList githubRepo="withastro/docs" />
