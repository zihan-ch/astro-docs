---
layout: ~/layouts/MainLayout.astro
title: Testes
setup: |
  import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro'
i18nReady: true
---

Fazer testes o ajuda a escrever e manter código Astro em funcionamento. O Astro suporta muitas ferramentas populares para testes de unidade, testes de componente e testes de ponta a ponta (e2e), incluindo Jest, Mocha, Jasmine, Cypress e Playwright. Você pode até mesmo instalar bibliotecas de testes específicos de frameworks como a React Testing Library, para testar seus componentes de frameworks de UI.

Frameworks de testes permitem que você declare **afirmações** ou **expectativas** sobre como seu código deve se comportar em situações específicas e, em seguida, as compare com o comportamento real do seu código atual.

## Playwright

Playwright é um framework de testes de ponta a ponta para aplicações web modernas. Use a API do Playwright com JavaScript ou TypeScript para testar seu código Astro em todos os motores de renderização modernos, incluindo Chromium, WebKit e Firefox.

### Instalação

Você pode começar e executar seus testes usando a [extensão do VS Code](https://playwright.dev/docs/getting-started-vscode).

Como alternativa, você pode instalar o Playwright dentro do seu projeto Astro usando o gerenciador de pacotes de sua escolha. Siga as etapas da CLI para escolher JavaScript/TypeScript, nomear sua pasta de testes e opcionalmente, adicionar um fluxo de trabalho do GitHub Actions.

<PackageManagerTabs>
  <Fragment slot="npm">
  ```shell
  npm init playwright@latest
  ```
  </Fragment>
  <Fragment slot="pnpm">
  ```shell
  pnpm dlx create-playwright
  ```
  </Fragment>
  <Fragment slot="yarn">
  ```shell
  yarn create playwright
  ```
  </Fragment>
</PackageManagerTabs>

### Crie seu primeiro teste com Playwright

1. Escolha uma página para testar. Usaremos a página `index.astro` abaixo como exemplo.

```html title="src/pages/index.astro"
---
---
<html lang="pt-BR">
  <head>
    <title>Astro é incrível!</title>
    <meta name='description' content="Extraia conteúdo de qualquer lugar e sirva-o rapidamente com a arquitetura em ilhas de última geração do Astro." />
  </head>
  <body></body>
</html>
```

2. Crie uma nova pasta e adicione o seguinte arquivo de teste em `src/test`. Copie e cole o seguinte teste no arquivo para verificar se os metadados da página estão corretos. Atualize o valor do `<title>` da página para corresponder à página que você está testando.

```jsx title="src/test/index.spec.ts" "Astro é incrível!"
test('metadados estão corretos', async ({ page }) => {
  await page.goto("http://localhost:3000/");
  await expect(page).toHaveTitle('Astro é incrível');
});
```

:::tip[Defina a URL base]
Você pode definir [`"baseURL": "http://localhost:3000"`](https://playwright.dev/docs/api/class-testoptions#test-options-base-url) no arquivo de configuração `playwright.config.ts` para usar `page.goto("/")` ao invés de `page.goto("http://localhost:3000/")` em prol de uma URL mais conveniente.
:::

### Executando seus testes com Playwright

Você pode executar um único teste ou vários de uma vez, testando em um ou em vários navegadores. Por padrão, os resultados dos seus testes serão mostrados no terminal. Como opção, você pode abrir o HTML Test Reporter para mostrar um relatório completo e filtrar os resultados dos testes.

1. Para executar nosso teste do exemplo anterior usando a linha de comando, use o comando `test`. Opcionalmente, inclua o nome do arquivo para executar um único teste:

```sh
npx playwright test index.spec.ts
```

2. Para ver o HTML Test Report completo, abra-o usando o seguinte comando:
```sh
npx playwright show-report
```

:::tip
Execute seus testes no seu código em produção para estar mais próximo ao seu site real após deploy.
:::

#### Avançado: Iniciando um servidor web de desenvolvimento durante os testes

Você também pode fazer com que o Playwright inicie seu servidor ao executar seu script de testes usando a opção de [`webServer`](https://playwright.dev/docs/test-advanced#launching-a-development-web-server-during-the-tests) no arquivo de configuração do Playwright.

Aqui está um exemplo da configuração e dos comandos necessários utilizando o Yarn:

1. Adicione um script de teste no arquivo `package.json` na raiz do projeto, como `"test:e2e": "yarn playwright"`.

2. Em `playwright.config.ts`, adicione o objeto `webServer` e atualize o valor de `command` para `yarn preview`.

```js title="playwright.config.ts" ins={3-8} "yarn preview"
import type { PlaywrightTestConfig } from '@playwright/test';
const config: PlaywrightTestConfig = {
  webServer: {
    command: 'yarn preview',
    url: 'http://localhost:3000/app/',
    timeout: 120 * 1000,
    reuseExistingServer: !process.env.CI,
  },
  use: {
    baseURL: 'http://localhost:3000/app/',
  },
};
export default config;
```

3. Execute `yarn build`, e em seguida execute `yarn test:e2e` para rodar os testes do Playwright.

Mais informações sobre o Playwright podem ser encontradas nos links abaixo:

- [Getting started with Playwright](https://playwright.dev/docs/intro)
- [Use a development server](https://playwright.dev/docs/test-advanced#launching-a-development-web-server-during-the-tests)
