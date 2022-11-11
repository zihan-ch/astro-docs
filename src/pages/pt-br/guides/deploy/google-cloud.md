---
title: Faça deploy do seu Site Astro no Google Cloud
description: Como fazer deploy do seu site Astro usando Google Cloud.
layout: ~/layouts/DeployGuideLayout.astro
i18nReady: true
---

[Google Cloud](https://cloud.google.com/) é uma plataforma de hospedagem de aplicativos web completa que pode ser usada para fazer deploy do seu site Astro.

## Como fazer o deploy

Fazer deploy de um projeto para o Google Cloud requer apenas alguns cliques. (A maioria das ações também podem ser feitas usando a [CLI gcloud](https://cloud.google.com/sdk/gcloud/)).

### Cloud Run

1. Crie um novo projeto GCP, ou selecione um que você já tenha.

2. Verifique se a API Cloud Run está ativada.

3. Crie um novo serviço.

4. Use um container do Docker ou crie o seu usando [Cloud Build](https://cloud.google.com/build).

5. Configure uma porta a partir da qual os arquivos são servidos.

6. Habilite o acesso público adicionando uma nova permissão, chamada `Cloud Run Invoker`, ao `allUsers`.

### Cloud Storage

1. Crie um novo projeto GCP, ou selecione um que você já tenha.

2. Crie um novo bucket em [Cloud Storage](https://cloud.google.com/storage).

3. Dê um nome e outras configurações necessárias.

4. Faça upload de sua pasta `dist` para ele ou faça upload usando o [Cloud Build](https://cloud.google.com/build).

5. Habilite o acesso público adicionando uma nova permissão, chamada `Storage Object Viewer`, ao `allUsers`.

6. Edite a configuração do site e adicione `ìndex.html` como entrypoint e `404.html` como página de erro.
