---
title: Faça o deploy do seu site Astro para o Google Firebase Hosting
description: Como fazer o deploy do seu site Astro usando o Google Firebase Hosting.
layout: ~/layouts/DeployGuideLayout.astro
i18nReady: true
---

[Firebase Hosting](https://firebase.google.com/products/hosting) é uma serviço provido pela plataforma de desenvolvimento de aplicativos da Google, chamado [Firebase](https://firebase.google.com/), que pode ser usado para fazer o deploy de um site Astro.

## Como fazer o deploy

1. Verifique se o [firebase-tools](https://www.npmjs.com/package/firebase-tools) está instalado.

2. Crie os arquivos `firebase.json` e `.firebaserc` na raiz do seu projeto com os seguintes conteúdos:

   `firebase.json`:

   ```json
   {
     "hosting": {
       "public": "dist",
       "ignore": []
     }
   }
   ```

   `.firebaserc`:

   ```json
   {
     "projects": {
       "default": "<YOUR_FIREBASE_ID>"
     }
   }
   ```
   
3. Depois de executar `npm run build`, faça o deploy usando o comando `firebase deploy`.
