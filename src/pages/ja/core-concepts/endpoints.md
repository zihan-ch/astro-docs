---
layout: ~/layouts/MainLayout.astro
title: エンドポイント
description: あらゆる種類のデータを提供するエンドポイントの作成方法について説明します。
i18nReady: true
---
Astroでは、あらゆる種類のデータを提供するためのカスタムエンドポイントを作成できます。これを利用して、画像を生成したり、RSSを公開したり、またはAPIルーティングとして使用してサイトの完全なAPIを構築できます。

静的に生成されたサイトでは、カスタムエンドポイントは静的ファイルを生成するため、ビルド時に呼び出されます。[SSR](/ja/guides/server-side-rendering/)モードを選択した場合、カスタムエンドポイントはリクエストに応じて呼び出されるライブサーバーエンドポイントに変わります。静的エンドポイントとSSRエンドポイントは同じ様に定義されますが、SSRエンドポイントは追加機能をサポートします。


## 静的ファイルのエンドポイント

カスタムエンドポイントを作成するには、`.js`または`.ts`ファイルを`/pages`ディレクトリに追加してください。`.js`または`.ts`の拡張子はビルドプロセス中に削除されるので、ファイル名には作成したいデータの拡張子を含める必要があります。たとえば、`src/pages/data.json.ts`は、`/data.json`エンドポイントを構築します。

エンドポイントは、`Astro`グローバルと同様のプロパティを持つ[コンテキストオブジェクト](/ja/reference/api-reference/#endpoint-context)を受け取る`get`関数（オプションで`async`）をエクスポートします。これは`body`を持つオブジェクトを返し、Astroはビルド時にこれを呼び出し、bodyの内容を使ってファイルを生成します。

```ts
// 例: src/pages/builtwith.json.ts
// 出力: /builtwith.json
export async function get({params, request}) {
  return {
    body: JSON.stringify({
      name: 'Astro',
      url: 'https://astro.build/',
    }),
  };
}
```

戻り値のオブジェクトは、`encoding`プロパティを持つことができます。これは、Node.jsの`fs.writeFile`メソッドで受け入れられる有効な[`BufferEncoding`](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/bdd02508ddb5eebcf701fdb8ffd6e84eabf47885/types/node/buffer.d.ts#L169)であれば何でもかまいません。たとえば、バイナリのpngイメージを生成する場合は次のようになります。

```ts title="src/pages/astro-logo.png.ts" {6}
export async function get({ params, request }) {
  const response = await fetch("https://astro.build/assets/press/full-logo-light.png");
  const buffer = Buffer.from(await response.arrayBuffer());
  return {
    body: buffer,
    encoding: 'binary',
  };
}
```

また、`APIRoute`型を使用してエンドポイント関数に型付けもできます。

```ts
import type { APIRoute } from 'astro';

export const get: APIRoute = async function get ({params, request}) {
...
```

### `params`と動的ルーティング

エンドポイントは、ページと同じ[動的ルーティング](/ja/core-concepts/routing/#%E5%8B%95%E7%9A%84%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0)機能をサポートしています。ファイル名を括弧付きのパラメーター名とし、[`getStaticPaths()`関数](/ja/reference/api-reference/#getstaticpaths)をエクスポートしてください。そして、エンドポイント関数に渡された`params`プロパティを使用して、パラメーターにアクセスします。


```ts title="src/pages/[id].json.ts"
import type { APIRoute } from 'astro';

const usernames = ["Sarah", "Chris", "Dan"]

export const get: APIRoute = ({ params, request }) => {
  const id = params.id;
  return {
    body: JSON.stringify({
      name: usernames[id]
    })
  }
};

export function getStaticPaths () {
  return [ 
    { params: { id: "0"} },
    { params: { id: "1"} },
    { params: { id: "2"} },
  ]
}
```

これにより、ビルド時に`/api/1.json`、`/api/2.json`、`/api/3.json`という3つのJSONエンドポイントが生成されます。エンドポイントによる動的ルーティングはページと同じように動作しますが、エンドポイントはコンポーネントではなく関数であるため、[props](/ja/reference/api-reference/#data-passing-with-props)はサポートされていません。

### `request`

すべてのエンドポイントは`request`プロパティを受け取りますが、静的モードでは`request.url`にのみアクセスが可能です。これは、現在のエンドポイントの完全なURLを返し、[Astro.request.url](/ja/reference/api-reference/#astrorequest)がページに対して行うのと同じように動作します。

```ts title="src/pages/request-path.json.ts"
import type { APIRoute } from 'astro';

export const get: APIRoute = ({ params, request }) => {
  return {
    body: JSON.stringify({
      path: new URL(request.url).pathname
    })
  };
}
```


## サーバーエンドポイント（APIルーティング）

静的ファイルエンドポイントのセクションで説明したものはすべて、SSRモードでも使用できます。ファイルは、`Astro`グローバルと同様のプロパティを持つ[コンテキストオブジェクト](/ja/reference/api-reference/#endpoint-context)を受け取る`get`関数をエクスポートできます。

しかし、`static`モードとは異なり、`server`モードを設定すると、エンドポイントはリクエストされた時点で構築されます。これにより、ビルド時には利用できない新しい機能がアンロックされ、リクエストをリッスンするAPIルートを構築したり、実行時にサーバー上で安全にコードを実行できるようになります。

:::note
これらの例を試す前に、必ず[SSRを有効](/ja/guides/server-side-rendering/#プロジェクトでssrを有効にする)にしてください。
:::

サーバーエンドポイントは、`getStaticPaths`をエクスポートせず`params`にアクセスでき、[`Response`](https://developer.mozilla.org/ja/docs/Web/API/Response)オブジェクトを返せるので、ステータスコードやヘッダーを設定できます。


```js title="src/pages/[id].json.js"
import { getProduct } from '../db';

export async function get({ params }) {
  const id = params.id;
  const product = await getProduct(id);

  if (!product) {
    return new Response(null, {
      status: 404,
      statusText: 'Not found'
    });
  }

  return new Response(JSON.stringify(product), {
    status: 200,
    headers: {
      "Content-Type": "application/json"
    }
  });
}
```

これは、動的ルーティングにマッチするすべてのリクエストに応答します。たとえば、`/helmet.json`に移動した場合、`params.id`は`helmet`に設定されます。モックの商品データベースに`helmet`が存在すれば、エンドポイントは`Response`オブジェクトを作成してJSONで応答し、[HTTPステータスコード](https://developer.mozilla.org/ja/docs/Web/API/Response/status)として成功を返します。存在しない場合は、`Response`オブジェクトを使用して`404`で応答します。

### HTTPメソッド

`get`関数に加え、任意の[HTTPメソッド](https://developer.mozilla.org/ja/docs/Web/HTTP/Methods)名を持つ関数をエクスポートできます。リクエストが来ると、Astroはそのメソッドをチェックして、対応する関数を呼び出します。

また、対応するエクスポートされた関数がないメソッドにマッチするように、`all`関数をエクスポートできます。一致するメソッドがないリクエストの場合、サイトの[404ページ](/ja/core-concepts/astro-pages/#%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%A0404%E3%82%A8%E3%83%A9%E3%83%BC%E3%83%9A%E3%83%BC%E3%82%B8)にリダイレクトされます。

:::note
JavaScriptでは`delete`は予約語なので、代わりに`del`関数をエクスポートするとdeleteメソッドにマッチします。
:::

```ts title="src/pages/methods.json.ts"
export const get: APIRoute = ({ params, request }) => {
  return {
    body: JSON.stringify({
      message: "This was a GET!"
    })
  }
};

export const post: APIRoute = ({ request }) => {
  return {
    body: JSON.stringify({
      message: "This was a POST!"
    })
  }
}

export const del: APIRoute = ({ request }) => {
  return {
    body: JSON.stringify({
      message: "This was a DELETE!"
    })
  }
}

export const all: APIRoute = ({ request }) => {
  return {
    body: JSON.stringify({
      message: `This was a ${request.method}!`
    })
  }
}
```

### `request` 

SSRモードでは、`request`プロパティは、現在のリクエストを参照する完全に使用可能な[`Request`](https://developer.mozilla.org/ja/docs/Web/API/Request)オブジェクトを返します。これにより、データの受け入れやヘッダーのチェックができます。

```ts title="src/pages/test-post.json.ts"
export const post: APIRoute = async ({ request }) => {
  if (request.headers.get("Content-Type") === "application/json") {
    const body = await request.json();
    const name = body.name;
    return new Response(JSON.stringify({
      message: "名前: " + name
    }), {
      status: 200
    })
  }
  return new Response(null, { status: 400 });
}
```

### リダイレクト

エンドポイントコンテキストは、`Astro.redirect`に似た`redirect()`ユーティリティをエクスポートします。

```js title="src/pages/links/[id].js" {14}
import { getLinkUrl } from '../db';

export async function get({ params, redirect }) {
  const { id } = params;
  const link = await getLinkUrl(id);

  if (!link) {
    return new Response(null, {
      status: 404,
      statusText: 'Not found'
    });
  }

  return redirect(link, 307);
}
```

### 例：CAPTCHA（キャプチャ）の検証

サーバーエンドポイントをREST APIのエンドポイントとして使用することで、機密データをクライアントに公開することなく、認証、データベースアクセス、検証などの機能を実行できます。

以下の例では、Google reCAPTCHA v3を検証するためにAPIルートを使用していますが、これはクライアントにシークレットを公開しません。

サーバー上では、recaptchaのデータを受け入れるpostメソッドを定義し、reCAPTCHAのAPIを使って検証を行います。ここで、secret値を安全に定義したり、環境変数を読み込んだりできます。

```js title="src/pages/recaptcha.js"
export async function post({ request }) {
  const data = await request.json();

  const recaptchaURL = 'https://www.google.com/recaptcha/api/siteverify';
  const requestBody = {
    secret: "YOUR_SITE_SECRET_KEY",   // 環境変数にできます
    response: data.recaptcha          // クライアントから渡されたトークン
  };

  const response = await fetch(recaptchaURL, {
    method: "POST",
    body: JSON.stringify(requestBody)
  });

  const responseData = await response.json();

  return new Response(JSON.stringify(responseData), { status: 200 });
}
```

そして、クライアントスクリプトから`fetch`を使ってエンドポイントにアクセスします。

```astro title="src/pages/index.astro"
<html>
  <head>
    <script src="https://www.google.com/recaptcha/api.js"></script>
  </head>

  <body>
    <button class="g-recaptcha" 
      data-sitekey="PUBLIC_SITE_KEY" 
      data-callback="onSubmit" 
      data-action="submit">CAPTCHA認証のためクリックしてください</button>

    <script is:inline>
      function onSubmit(token) {
        fetch("/recaptcha", {
          method: "POST",
          body: JSON.stringify({ recaptcha: token })
        })
        .then((response) => response.json())
        .then((gResponse) => {
          if (gResponse.success) {
            // CAPTCHA認証が成功した場合
          } else {
            // CAPTCHA認証が失敗
          }
        })
      }
    </script>
  </body>
</html>
```
