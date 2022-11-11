---
title: Installation d'Astro avec l'ILC automatique
description: Comment installer Astro avec NPM, PNPM, ou Yarn via l'outil de création create-astro inclus dans l'ILC.
layout: ~/layouts/MainLayout.astro
setup: | 
  import InstallGuideTabGroup from '~/components/TabGroup/InstallGuideTabGroup.astro';
  import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro'
---

Prêt à installer Astro ? Suivez notre guide d'installation automatique ou manuel pour commencer.

#### Prérequis

- **Node.js** - version `14.18.0`, `v16.12.0`, ou supérieure.
- **Éditeur de code** - Nous recommandons [VS Code](https://code.visualstudio.com/) avec notre [extension officielle Astro](https://marketplace.visualstudio.com/items?itemName=astro-build.astro-vscode).
- **Console de terminal** - Astro est accessible via son interface par ligne de commande (ILC).

<InstallGuideTabGroup />

#### Installation

`create-astro` est le moyen le plus rapide pour démarrer un nouveau projet Astro à partir de zéro. Il vous guidera lors de chaque étape d'installation de votre nouveau projet Astro. Il vous permet de choisir entre différents templates de démarrage ou fournir le votre  en utilisant l'argument `--template`.

:::tip[Aperçus en ligne]
Vous préférez essayer Astro dans votre navigateur ? Visitez [astro.new](https://astro.new/) pour naviguer entre nos différents templates de démarrage et démarrer un nouveau projet Astro sans jamais quitter votre navigateur.
:::

## 1. Utiliser l'assistant d'installation

Lancez la commande suivante dans votre terminal pour démarrer notre assistant d'installation, `create-astro`. Vous serez guidé dans la création de votre premier projet Astro à l'intérieur du répertoire où vous l'avez lancé.

Pas besoin de créer un répertoire ! L'assistant d'installation créera automatiquement un répertoire pour vous.

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

Vous pouvez exécuter `create-astro` n'importe où sur votre machine, il n'y a donc pas besoin de créer un nouveau répertoire vide  pour votre projet avant de commencer. Si vous n'avez encore pas de répertoire vide pour votre nouveau projet, l'assistant aidera à en créer un automatiquement.

Si tout se passe bien, vous devriez voir un message "Ready for liftoff!" suivi de quelques "Étapes suivantes". Exécutez `cd` vers votre nouveau répertoire pour commencer à utiliser Astro.

Si vous avez manqué l'étape `npm install` durant l'assistant `create-astro`, veuillez vous assurer d'installer les dépendances avant de continuer.

## 2. Lancer Astro ✨

Vous pouvez vous attendre à utiliser le serveur de développement intégré d'Astro pour la plupart de vos projets. C'est de cette manière que vous lancerez votre projet localement pendant le développement.

Pour commencer, utilisez votre gestionnaire de Packages pour lancer le script de démarrage par défaut :

<PackageManagerTabs>
  <Fragment slot="npm">
  ```shell
  npm run dev
  ```
  </Fragment>
  <Fragment slot="pnpm">
  ```shell
  pnpm run dev
  ```
  </Fragment>
  <Fragment slot="yarn">
  ```shell
  yarn run dev
  ```
  </Fragment>
</PackageManagerTabs>

Si tout se passe comme prévu, Astro devrait maintenant être en train de faire tourner votre projet à l'addresse [http://localhost:3000/](http://localhost:3000/) !

Astro va également suivre les modifications de fichiers dans le répertoire `src/`, vous n'aurez donc pas besoin de redémarrer le serveur à chaque fois que vous apporterez des modifications au cours du développement.

Si vous n'arrivez pas à ouvrir votre projet dans le navigateur, revenez au terminal où vous avez lancé le script `dev` pour voir ce qui a mal tourné, ou si votre projet est servi sur une url différente que celle mentionnée ci-dessus.

## Étapes Suivantes

Bravo ! Vous êtes maintenant prêt à développer avec Astro !

Voici quelques sujets que nous vous recommendons de suivre. Vous pouvez les suivre dans l'ordre. Vous pouvez même laisser la documentation de côté pendant un temps et aller jouer dans le code de votre nouveau projet Astro, en revenant ici si vous avez des problèmes ou une question.

📚 **Ajouter un framework:** Apprenez comment étendre Astro avec le support de React, Svelte, Tailwind et plus en utilisant `npx astro add` dans notre [guide d'Integrations](/fr/guides/integrations-guide/).

📚 **Déployer votre site:** Apprenez-en plus sur la façon de builder et déployer un projet Astro sur le web dans notre [guide de Déploiement](/fr/guides/deploy/).

📚 **Comprendre la structure de projet:** En apprendre plus sur la structure de votre projet Astro dans notre [Guide de structure de projet](/fr/core-concepts/project-structure/).
