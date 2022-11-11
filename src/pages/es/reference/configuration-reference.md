---
layout: ~/layouts/MainLayout.astro
title: Referencia de configuración
i18nReady: true
setup: |
  import Since from '../../../components/Since.astro';
---

La siguiente referencia cubre todas las opciones de configuración compatibles en Astro. Para obtener más información sobre la configuración de Astro, lea nuestra guía sobre [configuración de Astro](/es/guides/configuring-astro/).

```js
// astro.config.mjs
import { defineConfig } from 'astro/config'

export default defineConfig({
  // tus opciones de configuración aquí...
})
```
## Opciones de nivel superior

### root

<p>

**Tipo:** `string`<br>
**CLI:** `--root`<br>
**Por defecto:** `"."` (carpeta de trabajo actual)
</p>

Solo debes proporcionar esta opción si ejecutas los comandos CLI `astro` en una carpeta que no sea la carpeta raíz del proyecto. Por lo general, esta opción se proporciona a través de la CLI en lugar del [archivo de configuración de Astro](/es/guides/configuring-astro/#tipos-de-archivo-de-configuración-compatibles), ya que Astro necesita conocer la raíz de tu proyecto antes de que pueda localizar su archivo de configuración.

Si proporcionas una ruta relativa (p. ej., `--root: './my-project'`), Astro la resolverá en tu directorio de trabajo actual.

#### Ejemplos

```js
{
  root: './my-project-directory',
}
```
```bash
$ astro build --root ./my-project-directory
```


### srcDir

<p>

**Tipo:** `string`<br>
**Por defecto:** `"./src"`
</p>

Establece la carpeta desde el cual Astro leerá tu proyecto.

El valor puede ser una ruta absoluta del sistema de archivos o una ruta relativa a la raíz del proyecto.

```js
{
  srcDir: './www',
}
```


### publicDir

<p>

**Tipo:** `string`<br>
**Por defecto:** `"./public"`
</p>

Establece la carpeta para los activos estáticos. Los archivos en esta carpeta se sirven desde `/` durante el desarrollo y se copian en la carpeta de compilación durante la compilación. Estos archivos siempre se sirven o se copian tal cual, sin transformación ni empaquetamiento.

El valor puede ser una ruta absoluta del sistema de archivos o una ruta relativa a la raíz del proyecto.

```js
{
  publicDir: './my-custom-publicDir-directory',
}
```


### outDir

<p>

**Tipo:** `string`<br>
**Por defecto:** `"./dist"`
</p>

Establece la carpeta en la que `astro build` escribe la compilación final.

El valor puede ser una ruta absoluta del sistema de archivos o una ruta relativa a la raíz del proyecto.

```js
{
  outDir: './my-custom-build-directory',
}
```
**Ver también:**
- build.server


### site

<p>

**Tipo:** `string`
</p>

La URL final donde se desplegará. Astro usa esta URL completa para generar el sitemap y las URL canónicas en la compilación final. Se recomienda establecer esta configuración para aprovechar al máximo Astro.

```js
{
  site: 'https://www.my-site.dev',
}
```


### base

<p>

**Tipo:** `string`
</p>

La ruta base en la que se desplegará. Astro compilará tus páginas y recursos usando esta ruta como la raíz. En este momento, esta configuración no tiene efecto durante el modo de desarrollo.

```js
{
  base: '/docs',
}
```


### trailingSlash

<p>

**Tipo:** `'always' | 'never' | 'ignore'`<br>
**Por defecto:** `'ignore'`
</p>

Establece el comportamiento de coincidencia de rutas del servidor de desarrollo. Elige entre las siguientes opciones:
   - `'always'`: solo coincide con las URL que incluyen una barra inclinada al final (por ejemplo: "/foo/")
   - `'never'`: nunca haga coincidir las URL que incluyen una barra inclinada al final (por ejemplo: "/foo")
   - `'ignore'`: coincide con las URL independientemente de si existe un "/" final

Utiliza esta opción de configuración si tu host de producción tiene un manejo estricto de cómo funcionan o no las barras inclinadas finales.

También puedes configurar esto si prefieres ser más estricto, de modo que las URL con o sin barras diagonales finales no funcionen durante el desarrollo.

```js
{
  // Ejemplo: Requiera una barra inclinada final durante el desarrollo
  trailingSlash: 'always',
}
```
**Vea también:**
- build.format


### adapter

<p>

**Tipo:** `AstroIntegration`
</p>

Despliega a tu servidor favorito, serverless o edge host con adaptadores de compilación. Importa uno de nuestros adaptadores propios para [Netlify](/es/guides/deploy/netlify/#adaptador-para-ssredge), [Vercel](/es/guides/deploy/vercel/#adaptador-para-ssr), y más para incluir a Astro SSR.

[Consulta nuestra guía de renderizado en el servidor](/es/guides/server-side-rendering/) para obtener más información sobre SSR, y [nuestras guías de despliegue](/es/guides/deploy/) para obtener una lista completa de hosts.

```js
import netlify from '@astrojs/netlify/functions';
{
  // Ejemplo: Compila para desplegar en Netlify serverless
  adapter: netlify(),
}
```

**Vea también:**
- output


### output

<p>

**Tipo:** `'static' | 'server'`<br>
**Default:** `'static'`
</p>

Especifica el tipo de la compilación.

- 'static': construye un sitio estático para implementarlo en cualquier host estático.
- 'server': construye una aplicación que se implementará en un host compatible con SSR (renderizado en el servidor).

```js
import { defineConfig } from 'astro/config';

export default defineConfig({
  output: 'static',
})
```
**Vea también:**
- adapter


## Opciones de Build

### build.format

<p>

**Tipo:** `('file' | 'directory')`<br>
**Por defecto:** `'directory'`
</p>

Controla el formato del archivo compilado de cada página.
  - Si es 'file', Astro generará un archivo HTML (por ejemplo: "/foo.html") para cada página.
  - Si es 'directory', Astro generará un directorio con un archivo `index.html` anidado (por ejemplo: "/foo/index.html") para cada página.

```js
{
  build: {
    // Ejemplo: Genera `page.html` en lugar de `page/index.html` durante la compilación.
    format: 'file',
  }
}
```

### Efecto en Astro.url
La opción `build.format` indica el valor que `Astro.url` obtendrá durante la compilación. Si es:
- `directory` - `Astro.url.pathname` incluirá una barra final para imitar el comportamiento de carpetas. Ej.: `/foo/`.
- `file` - `Astro.url.pathname` incluirá `.hmtl`. Ej.: `/foo.html`.

Esto significa que cuando crees URLs relativas usando `new URL('./relativa', Astro.url)`, tendrás un comportamiento consistente entre desarrollo y compilación.


### build.client

<p>

**Type:** `string`<br>
**Por defecto:** `'./dist/client'`
</p>

Controla el directorio de salida del CSS y JavaScript del lado del cliente solamente cuando esté configurado `output: 'server'`.
`outDir` controla dónde se compila el código.

Este valor es relativo al `outDir`.

```js
{
  output: 'server',
  build: {
    client: './client'
  }
}
```


### build.server

<p>

**Type:** `string`<br>
**Por defecto:** `'./dist/server'`
</p>

Controla el directorio de salida del JavaScript del servidor cuando compila a SSR.

Este valor es relativo al `outDir`.

```js
{
  build: {
    server: './server'
  }
}
```


### build.serverEntry

<p>

**Type:** `string`<br>
**Por defecto:** `'entry.mjs'`
</p>

Especifica el nombre de archivo del punto de entrada al servidor cuando compila a SSR.
Este punto de entrada suele depender del host al que estés desplegando y
será configurado por el adaptador que estés utilizando.

Ten en cuenta que se recomienda que este archivo tenga la extensión `.mjs` así el runtime
detecta que el archivo es un módulo de JavaScript.

```js
{
  build: {
    serverEntry: 'main.mjs'
  }
}
```


## Opciones del Servidor

Personaliza el entorno de desarrollo de Astro, utilizado por `astro dev` y `astro preview`.

```js
{
  server: { port: 1234, host: true},
}
```

Para establecer una configuración diferente basada en el comando ejecutar ("dev", "preview"), también puedes pasar una función a esta opción de configuración.

```js
{
  // Ejemplo: Usa una función para personalizar según el comando
  server: (command) => ({ port: command === 'dev' ? 3000 : 4000 }),
}
```

### server.host

<p>

**Tipo:** `string | boolean`<br>
**Por defecto:** `false`<br>
<Since v="0.24.0" />
</p>

Establece en qué direcciones de IP el servidor debe escuchar (es decir, direcciones IP no locales).
- `false` - no exponer una dirección IP
- `true` - escuchar todas las direcciones, incluidas LAN y direcciones públicas
- `[dirección personalizada]` - exponer una dirección IP en `[dirección personalizada]` (por ejemplo, `192.168.0.1`)


### server.port

<p>

**Tipo:** `number`<br>
**Por defecto:** `3000`
</p>

Establece en qué puerto debe escuchar el servidor.

Si el puerto dado ya está en uso, Astro probará automáticamente el siguiente puerto disponible.

```js
{
  server: { port: 8080 },
}
```


## Opciones de Markdown

### markdown.drafts

<p>

**Tipo:** `boolean`<br>
**Por defecto:** `false`
</p>

Controla si las páginas Markdown de borrador deben incluirse en la compilación.

Una página de Markdown se considera un borrador si incluye `draft: true` en el frontmatter. Las páginas de borrador siempre se incluyen y son visibles durante el desarrollo (`astro dev`), pero de forma predeterminada no se incluirán en la compilación final.

```js
{
  markdown: {
    // Ejemplo: Incluye todos los borradores en la compilación final
    drafts: true,
  }
}
```


### markdown.shikiConfig

<p>

**Tipo:** `Partial<ShikiConfig>`
</p>

Opciones de configuración de Shiki. Consulta [la documentación de configuración de Markdown](/es/guides/markdown-content/#configuración-de-shiki) para conocer su uso.


### markdown.syntaxHighlight

<p>

**Tipo:** `'shiki' | 'prism' | false`<br>
**Por defecto:** `shiki`
</p>

Qué resaltador de sintaxis usar, si lo hay.
- `shiki` - usa el resaltador [Shiki](https://github.com/shikijs/shiki)
- `prism` - usa el resaltador [Prism](https://prismjs.com/)
- `false` - no aplicar resaltado de sintaxis.

```js
{
  markdown: {
    // Ejemplo: Cambia el resaltado de sintaxis a prism en Markdown
    syntaxHighlight: 'prism',
  }
}
```

### markdown.remarkPlugins

<p>

**Tipo:** `RemarkPlugins`
</p>

Pasa [plugins de remark](https://github.com/remarkjs/remark) para personalizar la construcción del Markdown. Puedes importar y aplicar la función del plugin (recomendado), o pasar el nombre del plugin como string.

:::caution
Proveer una lista de plugins **removerá** nuestros plugins por defecto. Para mantener los incluidos por defecto, lee sobre la flag [`extendDefaultPlugins`](#markdownextenddefaultplugins).
:::

```js
import remarkToc from 'remark-toc';
{
  markdown: {
    remarkPlugins: [remarkToc],
  }
}
```


### markdown.rehypePlugins

<p>

**Tipo:** `RehypePlugins`
</p>

Pasa [plugins de rehype](https://github.com/remarkjs/remark-rehype) para personalizar cómo el HTML generado en compilación es procesado. Puedes importar y aplicar la función del plugin (recomendado), o pasar el nombre del plugin como string.

:::caution
Proveer una lista de plugins **removerá** nuestros plugins por defecto. Para mantener los incluidos por defecto, lee sobre la flag [`extendDefaultPlugins`](#markdownextenddefaultplugins).
:::

```js
import rehypeMinifyHtml from 'rehype-minify';
{
  markdown: {
    rehypePlugins: [rehypeMinifyHtml],
  }
}
```


### markdown.extendDefaultPlugins

<p>

**Tipo:** `boolean`<br>
**Por defecto:** `false`
</p>

Astro aplica los plugins [GitHub-flavored Markdown](https://github.github.com/gfm/) y [Smartypants](https://github.com/silvenon/remark-smartypants) por defecto. Si deseas añadir tus propios plugins de remark o rehype, puedes mantener los incluidos por defecto, puedes hacerlo estableciendo la flag `extendDefaultPlugins` con el valor `true`:

```js
{
  markdown: {
    extendDefaultPlugins: true,
    remarkPlugins: [exampleRemarkPlugin],
    rehypePlugins: [exampleRehypePlugin],
  }
}
```


### markdown.remarkRehype

<p>

**Tipo:** `RemarkRehype`
</p>

Puedes pasar opciones a [remark-rehype](https://github.com/remarkjs/remark-rehype#api).

```js
{
  markdown: {
    // Ejemplo: Traduce el texto de las notas de pie a otro idioma, por ejemplo, aquí están en español
    remarkRehype: { footnoteLabel: "Notas de pie", footnoteBackLabel: "Volver al contenido"},
  }
}
```


## Integraciones

Extiende Astro con integraciones personalizadas. Las integraciones sirven para agregar soporte a frameworks (como Solid.js), nuevas funcionalidades (sitemaps) y nuevas bibliotecas (como Partytown y Turbolinks).

Consulta nuestra [guía de integraciones](/es/guides/integrations-guide/) para obtener ayuda para comenzar con integraciones de Astro.

```js
import react from '@astrojs/react';
import tailwind from '@astrojs/tailwind';
{
  // Ejemplo: Agrega compatibilidad con React + Tailwind a Astro
  integrations: [react(), tailwind()],
}
```

## Vite

Pasa opciones de configuración adicionales a Vite. Útil cuando Astro no admite alguna configuración avanzada que pueda necesitar.

Consulta la documentación completa del objeto de configuración `vite` en [vitejs.dev](https://vitejs.dev/config/).

#### Ejemplos

```js
{
  vite: {
    ssr: {
      // Ejemplo: Obliga a un paquete roto a omitir el procesamiento de SSR, si es necesario
      external: ['broken-npm-package'],
    }
  }
}
```

```js
{
  vite: {
    // Ejemplo: Agrega plugins de vite personalizados directamente a tu proyecto de Astro
    plugins: [myPlugin()],
  }
}
```

## Flags legacy

Para ayudar a algunos usuarios a migrar entre versiones de Astro, ocasionalmente introducimos flags `legacy`.
Estas flags te permiten optar por algunos comportamientos desactualizados u obsoletos de Astro
en la última versión, para que pueda continuar actualizándose y aprovechar los nuevos lanzamientos de Astro.

### legacy.astroFlavoredMarkdown

<p>

**Tipo:** `boolean`<br>
**Por defecto:** `false`<br>
<Since v="1.0.0-rc.1" />
</p>

Habilita el soporte anterior a v1.0 de Astro para componentes y expresiones JSX en archivos Markdown `.md` (y extensiones alternativas para archivos markdown como ".markdown").
En Astro `1.0.0-rc`, este comportamiento original se eliminó como predeterminado, a favor de nuestra nueva [integración MDX](/es/guides/integrations-guide/mdx/).

Para habilitar este comportamiento, establece `legacy.astroFlavoredMarkdown` en `true` en el [archivo de configuración `astro.config.mjs`](/es/guides/configuring-astro/#archivo-de-configuración-de-astro).

```js
{
  legacy: {
    // Ejemplo: Agrega soporte para funcionalidades de Markdown obsoletas
    astroFlavoredMarkdown: true,
  }
}
```
