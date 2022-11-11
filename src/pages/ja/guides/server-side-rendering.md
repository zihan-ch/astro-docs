---
layout: ~/layouts/MainLayout.astro
title: サーバーサイドレンダリング
i18nReady: true
setup: |
  import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro'
---

Astroでは**サーバーサイドレンダリング**（別名：SSR）が利用できます。SSRを有効にすると、次のようなことができます。

- アプリのログイン状態のためにセッションを実装する。
- `fetch`を使って動的にAPIを呼び出しデータを表示する。
- *アダプター*を利用してサイトをホストにデプロイする。

## プロジェクトでSSRを有効にする

まずは設定オプション`output: server`を使って開発モードでのSSRを有効にします。

```js ins={5}
// astro.config.mjs
import { defineConfig } from 'astro/config';

export default defineConfig({
  output: 'server'
});
```

### アダプターの追加

SSRを有効にしたプロジェクトをデプロイするには、アダプターを追加する必要があります。SSRはサーバーサイドのコードを実行する環境であるサーバーランタイムが必要なためです。各アダプターによって、Astroは特定のランタイムでプロジェクトを実行するスクリプトを出力できます。

現在、次に示すアダプターが利用でき、今後追加されていきます。

- [Cloudflare](/ja/guides/integrations-guide/cloudflare/)
- [Deno](/ja/guides/integrations-guide/deno/)
- [Netlify](/ja/guides/integrations-guide/netlify/)
- [Node.js](/ja/guides/integrations-guide/node/)
- [Vercel](/ja/guides/integrations-guide/vercel/)

#### `astro add`を使ったインストール

公式のアダプターは`astro add`コマンドを使って追加できます。アダプターのインストールと`astro.config.mjs`ファイルへの適切な変更が一度に行われます。例えば、Netlifyアダプターをインストールするには次のコマンドを実行します。

<PackageManagerTabs>
  <Fragment slot="npm">
  ```shell
  npx astro add netlify
  ```
  </Fragment>
  <Fragment slot="pnpm">
  ```shell
  pnpx astro add netlify
  ```
  </Fragment>
  <Fragment slot="yarn">
  ```shell
  yarn astro add netlify
  ```
  </Fragment>
</PackageManagerTabs>

#### 手動でインストール

パッケージをインストールし、`astro.config.mjs`を更新して手動でアダプターを追加することもできます。（SSRを有効にするために必要な2つの手順については上記のリンクをご覧ください）`my-adapter`をプレースホルダーの例として、次のような手順になります。

1. 好みのパッケージマネージャーを利用してプロジェクトの依存関係にアダプターをインストールします。

   <PackageManagerTabs>
     <Fragment slot="npm">
     ```shell
     npm install @astrojs/my-adapter
     ```
     </Fragment>
     <Fragment slot="pnpm">
     ```shell
     pnpm install @astrojs/my-adapter
     ```
     </Fragment>
     <Fragment slot="yarn">
     ```shell
     yarn add @astrojs/my-adapter
     ```
     </Fragment>
   </PackageManagerTabs>

2. `astro.config.mjs`ファイルのimportとdefault exportに[アダプターを追加します](/ja/reference/configuration-reference/#アダブター)。

    ```js ins={3,6-7}
    // astro.config.mjs
    import { defineConfig } from 'astro/config';
    import myAdapter from '@astrojs/my-adapter';

    export default defineConfig({
      output: 'server',
      adapter: myAdapter(),
    });
    ```

## 機能

Astroのデフォルトは静的サイトジェネレーターのままです。しかし、サーバーサイドレンダリングのアダプターを有効にすると、**pagesディレクトリのすべてのルートがサーバーでレンダリングされるルートになり**、いくつかの新機能が利用できるようになります。

### `Astro.request.headers`

リクエストのヘッダーは、`Astro.request.headers`で取得できます。これは[Headers](https://developer.mozilla.org/ja/docs/Web/API/Headers)オブジェクトといい、Mapのようなオブジェクトで、Cookieなどのヘッダーを取得することができます。

```astro title="src/pages/index.astro" {2}
---
const cookie = Astro.request.headers.get('cookie');
// ...
---
<html>
  <!-- Page here... -->
</html>
```

:::caution
以下の機能はページレベルでのみ利用できます。（レイアウトコンポーネントを含むコンポーネントの内部では使用できません。）

これらの機能がブラウザに送信された後に変更することができない[レスポンスヘッダー](https://developer.mozilla.org/ja/docs/Glossary/Response_header)を変更することが理由です。SSRモードでは、AstroはHTMLストリーミングを使用して、各コンポーネントをレンダリングする際にブラウザに送信します。これによってユーザーはできるだけ早くHTMLを見ることができますが、Astroがコンポーネントのコードを実行する頃には、すでにレスポンスヘッダーが送信されていることになります。
:::

### `Astro.redirect`

`Astro`グローバルでは、このメソッドで他のページにリダイレクトすることができます。クッキーからセクションを取得して、ユーザーがログインしているかチェックした後にこれを実行します。

```astro title="src/pages/account.astro" {8}
---
import { isLoggedIn } from '../utils';

const cookie = Astro.request.headers.get('cookie');

// ユーザーがログインしていない場合、ログインページにリダイレクトします
if (!isLoggedIn(cookie)) {
  return Astro.redirect('/login');
}
---
<html>
  <!-- Page here... -->
</html>
```

### `Response`

どのページからでも[Response](https://developer.mozilla.org/ja/docs/Web/API/Response)を返すこともできます。データベースでidを検索した後、動的ページで404を返す際に利用することがあります。

```astro title="src/pages/[id].astro" {8-11}
---
import { getProduct } from '../api';

const product = await getProduct(Astro.params.id);

// 商品（Product）が見つからなかった
if (!product) {
  return new Response(null, {
    status: 404,
    statusText: 'Not found'
  });
}
---
<html>
  <!-- Page here... -->
</html>
```

### サーバーエンドポイント

**API route**とも知られているサーバーエンドポイントは、`src/pages`フォルダの中にある`.js`, `.ts`ファイルで[Request](https://developer.mozilla.org/ja/docs/Web/API/Request)を受け取って[Response](https://developer.mozilla.org/ja/docs/Web/API/Response)を返します。
SSRの強力な機能であるAPI routeはサーバーサイドでセキュアにコードを実行できます。詳しくは[エンドポイントガイド](/ja/core-concepts/endpoints/#サーバーエンドポイントapiルーティング)をご覧ください。
