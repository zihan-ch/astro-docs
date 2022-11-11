---
layout: ~/layouts/MainLayout.astro
title: Imágenes
description: Aprende cómo usar imágenes en Astro.
i18nReady: true
setup: |
  import Since from '../../../components/Since.astro';
---

¡Astro proporciona varias formas de usar imágenes en tu proyecto, tanto si están almacenadas localmente, enlazadas remotamente o almacenadas en un CMS o una CDN!

### En archivos `.astro`

Astro utiliza elementos HTML `<img>` para mostrar imágenes, y todos los atributos de imagen HTML son compatibles. 

Se requiere el atributo `src` y su formato dependerá de dónde se almacenen las imágenes:

```astro title="src/pages/index.astro"
---
import rocket from '../images/rocket.svg';
---
<!-- Imagen remota en otro servidor -->
<img src="https://astro.build/assets/logo.png" width="25" alt="Astro">

<!-- Imagen local almacenada en public/assets/stars.png -->
<img src="/assets/stars.png" alt="Un cielo nocturno estrellado.">

<!-- Imagen local almacenada en src/images/rocket.svg -->
<img src={rocket} alt="Un cohete en el espacio." />
```

### En archivos Markdown

Puedes usar la sintaxis estándar de Markdown `![]()` o las etiquetas estándar de HTML `<img>` en tus archivos `.md` para mostrar imágenes locales en tu carpeta `public/` o imágenes remotas en otro servidor.

Si no puedes almacenar tus imágenes en `public/`, te recomendamos usar el formato de archivo `.mdx` , que te permite combinar componentes importados con sintaxis de Markdown. Usa la [integración de MDX](/es/guides/integrations-guide/mdx/) para agregar soporte para MDX en Astro.

```md
// src/pages/post-1.md

# Mi página Markdown

<!-- Imagen local almacenada en public/assets/stars.png -->
![Un cielo nocturno estrellado.](/assets/stars.png)
<img src="/assets/stars.png" alt="Un cielo nocturno estrellado.">

<!-- Imagen remota en otro servidor -->
![Astro](https://astro.build/assets/logo.png)
<img src="https://astro.build/assets/logo.png" width="25" alt="Astro">
```

### En archivos MDX

Puedes usar la sintaxis estándar de Markdown `![]()` o las etiquetas JSX `<img />` en tus archivos `.mdx`. Al igual que los archivos Markdown, los archivos MDX pueden mostrar imágenes desde tu carpeta `public/` o un servidor remoto. 

Además, puedes importar y usar imágenes ubicadas en el directorio `src/` de tu proyecto, como lo harías en los componentes de Astro.

```mdx
// src/pages/post-1.mdx

import rocket from '../images/rocket.svg';

# Mi página MDX

// Imagen local almacenada en src/images/rocket.svg
<img src={rocket} alt="Un cohete en el espacio." />

// Imagen local almacenada en public/assets/stars.png
![Un cielo nocturno estrellado.](/assets/stars.png)
<img src="/assets/stars.png" alt="Un cielo nocturno estrellado." />

// Imagen remota en otro servidor
![Astro](https://astro.build/assets/logo.png)
<img src="https://astro.build/assets/logo.png" width="25" alt="Astro" />
```

### En componentes de un framework de UI

Cuando agregues imágenes en un [componente de un framework de UI](/es/core-concepts/framework-components/) (ej. React, Svelte), usa la sintaxis de imágenes apropiada para ese framework en particular.

## Dónde almacenar imágenes

### `src/`

Tus imágenes almacenadas en `src/` pueden ser usadas por otros componentes (`.astro`, `.mdx` y otros frameworks), pero no en archivos de Markdown.

Nostros recomendamos que mantengas tus imágenes en [`public/`](#public) o almacenarlas [remotamente](#usar-imágenes-de-un-cms-o-una-cdn) si tu necesitas usar archivos de Markdown.

Impórtalas desde una **ruta relativa** o un [alias de importación](/es/guides/aliases/) en cualquier archivo de componente y después usa la importación como un atributo `src` de imagen.

```astro
---
// src/pages/index.astro

// Acceso a imágenes en `src/images/`
import logo from '../images/logo.png';
---
<img src={logo} width="40" alt="Astro" />
```

### `public/`

Tus imágenes pueden ser almacenadas en `src/` y pueden ser usadas por otros componentes (`.astro`, `.mdx` y otros frameworks UI), pero no en archivos de Markdown.

Sin embargo, los archivos en el directorio `/public` son siempre servidos o copiados sin modificaciones. Si estás usando imágenes fuera de los archivos de Markdown, te recomendamos que las imágenes locales sean guardadas en `/src` cuando sea posible para que Astro pueda transformarlas, optimizarlas y empaquetarlas.

El atributo `src` es **relativo a la carpeta public**. En Markdown, también puedes usar la notación `![]()`.

```astro title="src/pages/index.astro"
---

// Acceso a imágenes en `public/images/`
---
<img src="/images/logo.png" />
```

## Integración Image de Astro

La integración oficial de Astro proporciona dos componentes de Astro diferentes para renderizar imágenes optimizadas: `<Image />` y `<Picture />`. Ambas son compatibles con sitios estáticos y [algunos hosts de server-side-rendering](/es/guides/integrations-guide/image/#installation)

Después de [instalar la integración](/es/guides/integrations-guide/image/#installation), puedes usar estos dos componentes en cualquier lugar donde puedas usar componentes de Astro: en `.astro` y archivos `.mdx`.

### `<Image />`

El [componente `<Image />`](/es/guides/integrations-guide/image/#image-) de Astro permite optimizar una imagen individual y especificar el ancho, el alto y/o la relación de aspecto. También puedes transformar tu imagen a un formato particular.

Este componente es útil para imágenes en las que quieres mantener un tamaño consistente en pantallas, o controlar la calidad de una imagen (ej. logos).

Para imágenes responsivas, o dirección de arte, mejor utiliza el componente `<Picture />`.

#### Imágenes locales en `src/`

(atributos requeridos: [`src`](/es/guides/integrations-guide/image/#src), y [`alt`](/es/guides/integrations-guide/image/#alt))

Importa tu imagen en frontmatter y pásala directamente al atributo `src` del componente `<Image />`.

`alt` es requerido, pero [todas las demás propiedades](/es/guides/integrations-guide/image/#image-) son opcionales y se ajustarán de forma predeterminada a las propiedades originales del archivo de imagen si no se proporcionan.

#### Imágenes remotas

(atributos requeridos: [`src`](/es/guides/integrations-guide/image/#src), [`alt`](/es/guides/integrations-guide/image/#alt), [`format`](/es/guides/integrations-guide/image/#format), y dimensiones)

Pasa el URL completo al atributo `src` del componente `<Image />` e incluye un valor para `alt`.

El componente `<Image />` no puede determinar el formato original del archivo de una imagen remota, por esto mismo debes proporcionar un `format` de salida (por ejemplo png, avif) para transformarla.

También debes proporcionar [`width`](/es/guides/integrations-guide/image/#width) y [`height`](/es/guides/integrations-guide/image/#height), o una de las dimensiones más un [`aspectRatio`](/es/guides/integrations-guide/image/#aspectratio) para evitar cambios de diseño de contenido porque el componente `<Image />` no conoce las dimensiones de una imagen remota.

[Todas las demás propiedades](/es/guides/integrations-guide/image/#image-) son opcionales.

#### Imágenes locales en `public/`

(atributos requeridos: [`src`](/es/guides/integrations-guide/image/#src), [`alt`](/es/guides/integrations-guide/image/#alt), [`format`](/es/guides/integrations-guide/image/#format), y dimensiones)

Pasa al atributo `src` del componente una ruta relativa a la carpeta public e incluye un valor para `alt`.

Se tratará como una imagen remota, que requiere tanto [`width`](/es/guides/integrations-guide/image/#width) como [`height`](/es/guides/integrations-guide/image/#height), o una dimensión y un atributo [`aspectRatio`](/es/guides/integrations-guide/image/#aspectratio). 

Se requiere un valor para el atributo `format` (por ejemplo png, avif) para transformar la imagen.

[Todas las demás propiedades](/es/guides/integrations-guide/image/#image-) son opcionales.

Tu imagen original será copiada sin procesar a la carpeta de compilación, al igual que todos los archivos ubicados en `public/`, y la integración de imágenes de Astro también devolverá versiones optimizadas de la imagen.

#### Ejemplos

```astro
---
// src/pages/index.astro
import { Image } from '@astrojs/image/components';
import localImage from '../assets/logo.png';
const remoteImage = 'https://picsum.photos/id/957/300/200.jpg';
const localAlt = 'El Logo de Astro';
const remoteAlt = 'Una vista de un bosque durante el día';
---

<!-- Imagen local optimizada, manteniendo el ancho, alto y formato de imagen original -->
<Image src={localImage} alt={localAlt} />

<!-- height se recalculará para que coincida con la relación de aspecto original -->
<Image src={localImage} width={300} alt={localAlt} />

<!-- Para imágenes remotas, las dimensiones deseadas y el formato son requeridos -->
<Image src={remoteImage} width={300} aspectRatio="1:1" format="png" alt={remoteAlt} />

<!-- Ajustar a un ancho y alto específicos -->
<Image src={localImage} width={300} height={600} alt={localAlt} />
<Image src={remoteImage} width={544} height={184} format="png" alt={remoteAlt} />

<!-- Ajustar a una relación de aspecto específica y convertir a formato avif -->
<Image src={localImage} aspectRatio="16:9" format="avif" alt={localAlt} />
<Image src={remoteImage} height={200} aspectRatio="16:9" format="avif" alt={remoteAlt} />

<!-- Importaciones de imágenes locales pueden hacerse en línea -->
<Image src={import('../assets/logo.png')} alt={localAlt} />
<!-- Si una imagen es alojada en el directorio `/public`, usa una ruta relativa a `/public` -->
<Image src="/penguin.jpg" width="300" aspectRatio={1} format="png" alt="A happy penguin" />
```

### `<Picture /> `

El [componente `<Picture />`](/es/guides/integrations-guide/image/#picture-) de Astro puede ser usado para proporcionar imágenes adaptables en tu proyecto, incluyendo imágenes con diferentes tamaños, formatos y disposiciones. 

Puedes permitir que el navegador elija los tamaños de imágenes apropiados, resoluciones y tipos de archivo basados en factores como el tamaño de la pantalla y la conexión. O bien puedes especificar reglas que el navegador debe seguir basadas en media queries.

Este componente es útil para optimizar lo que el usuario ve en diferentes tamaños de pantalla o para el art direction.

:::tip
Revisa la guía de MDN para más información sobre [imágenes adaptables y art direction](https://developer.mozilla.org/es/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images).
:::

#### Imágenes locales

(atributos requeridos: [`src`](/es/guides/integrations-guide/image/#src-1), [`widths`](/es/guides/integrations-guide/image/#widths), [`size`](/es/guides/integrations-guide/image/#sizes), [`alt`](/es/guides/integrations-guide/image/#alt-1)

Importa tu imagen en frontmatter y pásala directamente al atributo `<Picture />` del componente `src` .

Debes proporcionar al componente una guía para anchos de imagen y tamaños de pantalla, pero [todas las demás propiedades](/es/guides/integrations-guide/image/#picture-) son opcionales.

Por defecto, el componente `<Picture />` de [`formats`](/es/guides/integrations-guide/image/#formats) incluirá `avif` y `webp` además del formato original de la imagen si no es especificado.

#### Imágenes remotas

(atributos requeridos: [`src`](/es/guides/integrations-guide/image/#src-1), [`widths`](/es/guides/integrations-guide/image/#widths), [`sizes`](/es/guides/integrations-guide/image/#sizes), [`alt`](/es/guides/integrations-guide/image/#alt-1) y [`aspectRatio`](/es/guides/integrations-guide/image/#aspectratio-1))

Pasa una URL completa al atributo `src` del componente  `<Picture />` .

También se requiere un valor para `aspectRatio` para garantizar que se pueda calcular la altura correcta en el momento de compilación para imágenes remotas.

Debes proporcionar al componente una guía para anchos de imagen y tamaños de pantalla, pero [todas las demás propiedades](/es/guides/integrations-guide/image/#picture-) son opcionales.

Aunque [`formats`](/es/guides/integrations-guide/image/#formats) no es necesario, el formato de imagen original de las imágenes remotas es desconocido y no se incluirá por defecto. Si no se proporciona, solo se incluirán `webp` y `avif`.

#### Imágenes Locales en `public/`

(atributos requeridos: [`src`](/es/guides/integrations-guide/image/#src-1), [`widths`](/es/guides/integrations-guide/image/#widths), [`sizes`](/es/guides/integrations-guide/image/#sizes), [`alt`](/es/guides/integrations-guide/image/#alt-1) y [`aspectRatio`](/es/guides/integrations-guide/image/#aspectratio-1)

Pasa al atributo `src` del componente una ruta relativa a la carpeta public e incluye un valor para `alt`.

La imagen será tratada como una imagen remota, por lo que se requiere un valor de `aspectRatio` para garantizar que la altura correcta se puede calcular en el momento de la construcción.

Debes proporcionar al componente una guía para anchos de imagen y tamaños de pantalla, pero [todas las demás propiedades](/es/guides/integrations-guide/image/#picture-) son opcionales.

Aunque [`formats`](/es/guides/integrations-guide/image/#formats) no es necesario, el formato de imagen original de las imágenes en la carpeta `public/` es desconocido y no se incluirá por defecto. Si no se proporciona, solo se incluirán `webp` y `avif`.

Tu imagen original será copiada sin procesar a la carpeta de compilación, como todos los archivos ubicados en `public/`, y la integración de imágenes de Astro también devolverá versiones optimizadas de la imagen.

#### Ejemplos

```astro
---
import { Picture } from '@astrojs/image/components';
import localImage from '../assets/logo.png';
const remoteImage = 'https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png';
---

<!-- Imagen local con múltiples tamaños y formatos -->
<Picture src={localImage} widths={[200, 400, 800]} sizes="(max-width: 800px) 100vw, 800px" formats={['avif', 'jpeg', 'png', 'webp']} alt="El logo de Astro" />

<!-- Imagen remota (la relación de aspecto es requerida)-->
<Picture src={remoteImage} widths={[200, 400, 800]} aspectRatio="4:3" sizes="(max-width: 800px) 100vw, 800px" alt="El logo de Google" />

<!-- Imágenes en /public funcionan como imágenes remotas -->
<Picture src="/logo.png" widths={[200, 400, 800]} aspectRatio="4:3" sizes="(max-width: 800px) 100vw, 800px" alt="El logo de Google" />
<!-- Importaciones en linea están soportadas -->
<Picture src={import("../assets/logo.png")} widths={[200, 400, 800]} sizes="(max-width: 800px) 100vw, 800px" alt="El logo de Astro" />
```

### Uso en MDX

En archivos `.mdx`, `<Image />` y `<Picture />` pueden recibir el `src` de la imagen a través de importaciones y exportaciones.

```mdx
// src/pages/index.mdx

import { Image, Picture } from '@astrojs/image/components';
import rocket from '../assets/rocket.png';
export const galaxy = 'https://astro.build/assets/galaxy.jpg';

<Image src={import('../assets/logo.png')} alt="Astro" />
<Image src={rocket} width={300} alt="Cohete acercándose a la luna." />
<Picture src={rocket} widths={[200, 400, 800]} sizes="(max-width: 800px) 100vw, 800px" alt="Un cohete despegando." />
<Picture src={galaxy} widths={[200, 400, 800]} aspectRatio={16/9} sizes="(max-width: 800px) 100vw, 800px" alt="Espacio exterior." />
```

### Configuración de valores por defecto

Actualmente, no hay manera de especificar los valores por defecto para todos los componentes `<Image />` y `<Picture />`. Los atributos requeridos deben establecerse en cada componente individual.

Como alternativa, puedes envolver estos componentes en otro componente Astro para su reutilización. Por ejemplo, podrías crear un componente para las imágenes de tu blog:

```astro title="src/components/BlogPostImage.astro"
---
import { Picture } from '@astrojs/image/components';

const {src, ...attrs} = Astro.props;
---
<Picture src={src} widths={[400, 800, 1500]} sizes="(max-width: 767px) 100vw, 736px" {...attrs} />

<style>
  img, picture :global(img), svg {
    margin-block: 2.5rem;
    border-radius: 0.75rem;
  }
</style>
```

## Usar imágenes de un CMS o una CDN

Las CDNs de imágenes funcionan con Astro. Utiliza la URL completa de una imagen como atributo `src` en una etiqueta `<img>` o notación de Markdown. 

Alternativamente, si la CDN proporciona un SDK para Node.js, puedes usarlo en tu proyecto. Por ejemplo, el [SDK de Cloudinary](https://cloudinary.com/documentation/node_integration) puede generar la etiqueta `<img>` con el `src` apropiado para ti.

Para usar [imágenes externas con los componentes `<Image />`](#imágenes-remotas) y [`<Picture />`](#imágenes-remotas-1) de la integración de imágenes de Astro, debes especificar la dimensión y los valores de formato adecuados para trabajar con imágenes remotas.

## Alt Text

No todos los usuarios pueden ver imágenes de la misma forma, así que la accesibilidad es una preocupación especialmente importante cuando se utilizan imágenes. Usa el atributo `alt` para proveer [texto alt descriptivo](https://www.w3.org/WAI/tutorials/images/) para las imágenes.

Este atributo es requerido para los componentes de integración de imagen `<Image />` y `<Picture />`. Estos componentes arrojarán un error si no se provee un texto alt.

Si la imagen es meramente decorativa (p. ej. no contribuye al entendimiento de la página), establece `alt=""` para que la imagen sea entendida adecuadamente e ignorada por los lectores de pantalla.

## Integraciones de la comunidad

Además de la integración oficial [`@astrojs/image`](/es/guides/integrations-guide/image/), existen varias [integraciones de imágenes de la comunidad](https://astro.build/integrations/css+ui/?q=image) para optimizar y trabajar con imágenes en tu proyecto.
