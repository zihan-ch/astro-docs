---
layout: ~/layouts/MainLayout.astro
title: RSS
description: AstroのRSS入門
i18nReady: true
setup: import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro'
---

Astroはブログやその他のコンテンツウェブサイト向けに、RSSフィードの高速な自動生成をサポートしています。一般的なRSSフィードに関する情報は[aboutfeeds.com](https://aboutfeeds.com/)をご覧ください。

## `@astrojs/rss`の準備

`@astrojs/rss`パッケージは、[APIエンドポイント](/ja/core-concepts/endpoints/#静的ファイルのエンドポイント)を利用したRSS生成のヘルパーを提供します。静的ビルドと[SSRアダプター](/ja/guides/server-side-rendering/#アダプターの追加)を利用したオンデマンド生成の両方に対応しています。

はじめに、お好きなパッケージマネージャーで`@astrojs/rss`をインストールします。

<PackageManagerTabs>
  <Fragment slot="npm">
  ```shell
  npm install @astrojs/rss
  ```
  </Fragment>
  <Fragment slot="pnpm">
  ```shell
  pnpm install @astrojs/rss
  ```
  </Fragment>
  <Fragment slot="yarn">
  ```shell
  yarn add @astrojs/rss
  ```
  </Fragment>
</PackageManagerTabs>

次に、プロジェクトの`astro.config`に[`site`を設定](/ja/reference/configuration-reference/#site)していることを確認します。これを利用して、[`SITE`環境変数経由](/ja/guides/environment-variables/#デフォルト環境変数)でRSSフィード内のリンクを生成することになります。

:::note[Astro v1が必要です]
`SITE`環境変数はAstro v1ベータ版でのみ利用できます。Astroの最新バージョン（`astro@latest`）にアップグレードするか、不可能な場合、手動で`site`を書いてください。（次の例を参照してください）
:::

それでは、はじめてのRSSフィードを生成しましょう。`src/pages`ディレクトリに`rss.xml.js`を作ります。`rss.xml`は出力されるURLになりますので、必要であれば名前を変更しても構いません。

次に、`@astrojs/rss`パッケージから`rss`ヘルパーをインポートし、次のパラメーターで呼び出します。

```js
// src/pages/rss.xml.js
import rss from '@astrojs/rss';

export const get = () => rss({
  // 出力されるXMLの`<title>`フィールド
  title: 'Buzz’s Blog',
  // 出力されるXMLの`<description>`フィールド
  description: 'A humble Astronaut’s guide to the stars',
  // RSS内<item>リンクのベースURL
  // SITEはプロジェクトのastro.configにあるsiteの値が使用されます。
  site: import.meta.env.SITE,
  // 出力されるXMLの<item>のリスト
  // 簡単な例: src/pagesにあるマークダウンファイルからそれぞれitemsを生成する
  // 必要なfrontmatterや複雑なユースケースに関しては「`items`の生成」セクションをご覧ください。
  items: import.meta.glob('./**/*.md'),
  // (任意) カスタムxmlを利用する
  customData: `<language>en-us</language>`,
});
```

## `items`の生成

`items`フィールドはこのどちらかを受け付けます。
1. [`import.meta.glob(...)`の結果](#1-importmetaglobの結果) **（`src/pages`ディレクトリに含まれる`.md`ファイルのみ利用します）**
2. [RSSフィードオブジェクトのリスト](#2-rssフィードオブジェクトのリスト)。各オブジェクトは`link`, `title`, `pubDate`、オプションで`description`, `customData`フィールドを持ちます。

### 1. `import.meta.glob`の結果

`src/pages`にある`.md`ファイルの便利な簡易記法としておすすめの選択肢です。各投稿のfrontmatterに`title`と`pubDate`、任意で`description`と`customData`フィールドを持たせてください。これが不可能な場合やコードでfrontmatterを生成したい場合は[2の選択肢](#2-rssフィードオブジェクトのリスト)をご覧ください。

ブログ投稿を`src/pages/blog`ディレクトリに保存しているとしましょう。次のようにRSSフィードを生成できます。

```js
// src/pages/rss.xml.js
import rss from '@astrojs/rss';

export const get = () => rss({
  title: 'Buzz’s Blog',
  description: 'A humble Astronaut’s guide to the stars',
  site: import.meta.env.SITE,
  items: import.meta.glob('./blog/**/*.md'),
});
```

このインポートの構文は[Viteのglob importのドキュメント](https://vitejs.dev/guide/features.html#glob-import)を参照してください。

### 2. RSSフィードオブジェクトのリスト

`pages`ディレクトリの外にある`.md`ファイルにおすすめな選択肢です。[`getStaticPaths`で](/ja/reference/api-reference/#getstaticpaths)ルーティングを生成する場合によく使います。

たとえば`src/posts`ディレクトリに`.md`投稿を保存しているとします。各投稿は`title`, `pubDate`と`slug`をfrontmatterに持ち、`slug`はサイト上の出力されるURLに対応しています。[Viteの`import.meta.glob`ヘルパー](https://vitejs.dev/guide/features.html#glob-import)を使って、次のようにRSSフィードを生成できます。

```js
// src/pages/rss.xml.js
import rss from '@astrojs/rss';

const postImportResult = import.meta.glob('../posts/**/*.md', { eager: true });
const posts = Object.values(postImportResult);

export const get = () => rss({
  title: 'Buzz’s Blog',
  description: 'A humble Astronaut’s guide to the stars',
  site: import.meta.env.SITE,
  items: posts.map((post) => ({
    link: post.url,
    title: post.frontmatter.title,
    pubDate: post.frontmatter.pubDate,
  }))
});
```

## スタイルシートの追加

ブラウザでファイルを見たときのユーザー体験をよくするために、RSSフィードにスタイルを加えられます。

`rss`関数の`stylesheet`オプションにスタイルシートの絶対パスを指定してください。

```js
rss({
  // 例. "public/rss/styles.xsl"からスタイルシートを利用します
  stylesheet: '/rss/styles.xsl',
  // ...
});
```

RSSのスタイルシートを特に気にしない場合、[Pretty Feed v3 default stylesheet](https://github.com/genmon/aboutfeeds/blob/main/tools/pretty-feed-v3.xsl)がおすすめです。これはGitHubからダウンロードでき、プロジェクトの`public`ディレクトリに保存します。
