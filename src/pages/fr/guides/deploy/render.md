---
title: Déployer votre site Astro sur Render
description: Comment déployer votre site Astro sur le Web avec Render.
layout: ~/layouts/DeployGuideLayout.astro
i18nReady: true
---

Vous pouvez déployer votre projet Astro sur [Render](https://render.com/), un service pour build des sites web avec des certificats libres TLS, un CDN global, une protection DDoS, des réseaux privés et des déploiements automatiques depuis Git.

## Comment déployer

1. Créer un [compte render.com](https://dashboard.render.com/) et se connecter
2. Cliquer sur le bouton **Nouveau +** du tableau de bord et sélectionner **Site Statique**
3. Connecter votre dépôt [GitHub](https://github.com/) ou [GitLab](https://about.gitlab.com/) ou, alternativement saisir l'adresse publique d'un dépôt publique
4. Donner à votre site web un nom, sélectionner la branche et spécifier la commande de génération et le dossier de publication
   - **commande de génération:** `npm run build`
   - **dossier de publication:** `dist`
5. Cliquer sur le bouton **Créer un Site Statique**
