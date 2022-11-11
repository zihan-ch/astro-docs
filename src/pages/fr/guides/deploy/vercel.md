---
title: Déployer votre Site Astro vers Vercel
description: Comment déployer votre site Astro vers le web sur Vercel.
layout: ~/layouts/DeployGuideLayout.astro
i18nReady: true
---

Vous pouvez utiliser [Vercel](http://vercel.com/) pour déployer un site Astro vers son global edge network sans aucune configuration.

Ce guide inclut des instructions pour déployer vers Vercel via l'IU du site web ou l'ILC de Vercel.

## Configuration du projet

Votre projet Astro peut être déployé vers Vercel en tant que site statique, ou en tant que site rendu côté serveur (SSR).

### Site Statique

Votre projet Astro est un site statique par défaut. Vous n'avez pas besoin de configuration supplémentaire pour déployer un site statique Astro vers Vercel. 

:::note
Il y a actuellement un problème Vercel affichant une page 404 sur les sites web Astro. Jusqu'à ce que ça soit corrigé, vous pouvez ajouter le fichier de configuration suivant à la racine de votre projet :

```json title="vercel.json"
{
  "cleanUrls": true
}
```
:::

### Adapteur pour SSR

Pour activer le SSR dans votre projet Astro et déployer sur Vercel:

Ajoutez [l'adapteur Vercel](/fr/guides/integrations-guide/vercel/) pour activer le SSR dans votre projet Astro avec la commande `astro add`. Ceci installera l'adapteur et fera les changements appropriés dans votre fichier `astro.config.mjs` en une étape.

```bash
npx astro add vercel
```

Si vous préférez installer l'adapteur manuallement à la place, complétez les deux étapes suivantes :

1. Installez [l'adapteur `@astrojs/vercel`](/fr/guides/integrations-guide/vercel/) dans les dépendances de votre projet en utilisant votre gestionnaire de package préféré. Si vous utilisez npm ou que vous n'êtes pas sûr, exécutez ceci dans le terminal:

    ```bash
      npm install @astrojs/vercel
    ```

1. Ajoutez deux nouvelles lignes à votre fichier de configuration de projet `astro.config.mjs`.

    ```js title="astro.config.mjs" ins={2, 5-6}
    import { defineConfig } from 'astro/config';
    import vercel from '@astrojs/vercel/serverless';

    export default defineConfig({
      output: 'server',
      adapter: vercel(),
    });
    ```

## Comment déployer

Vous pouvez déployer vers Vercel via l'interface utilisateur du site web ou utiliser l'ILC (interface en ligne de commande) de Vercel. Le processus est le même pour les sites statiques et SSR Astro.

### Déploiement via l'interface utilisateur du site web

1. Poussez votre code vers votre dépôt Git en ligne (GitHub, GitLab, BitBucket).
2. [Importez votre projet](https://vercel.com/new) dans Vercel.
3. Vercel détectera automatiquement Astro et configurera les bons paramètres.
4. Votre application est déployée ! (ex : [astro.vercel.app](https://astro.vercel.app/))

Après que le projet a été importé et déployé, tous les pushes subséquents vers des bracnhes généreront des [Aperçus de déploiements](https://vercel.com/docs/concepts/deployments/environments#preview), et tous les changements effectués sur la Branche de Production (“main” habituellement) produiront un [Déploiement de Production](https://vercel.com/docs/concepts/deployments/environments#production).

📚 Apprenez-en plus sur l'[Intégration Git](https://vercel.com/docs/concepts/git) de Vercel.


### Déploiement via ILC

1. Installez l'[ILC Vercel](https://vercel.com/cli) et exécutez `vercel` pour déployer.

    ```bash
    npm install -g vercel
    vercel
    ```

2. Vercel détectera automatiquement Astro et configurera les bons paramètres.
3. Lorsqu'il sera demandé `Want to override the settings? [y/N]`, sélectionner `N`.
4. Votre application est déployée ! (ex : [astro.vercel.app](https://astro.vercel.app/))

