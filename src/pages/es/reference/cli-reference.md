---
layout: ~/layouts/MainLayout.astro
title: Referencia del CLI
i18nReady: true
setup: |
    import Since from '~/components/Since.astro';
    import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro'
---

Puedes usar la Interfaz de Línea de Comandos (CLI) provista por Astro para desarrollar, construir y previsualizar tu proyecto desde una ventana de la terminal.

Usa la CLI corriendo algún **comando** documentado en esta página, seguido opcionalmente por una o más **flags**. Las flags indican el comportamiento de un comando. Por ejemplo, para iniciar el servidor de desarrollo en el puerto `8080`, deberías combinar el comando `astro dev` con la flag `--port`: `astro dev --port 8080`.

En la mayoría de los casos utilizarás la CLI por medio de tu gestor de paquetes:

<PackageManagerTabs>
  <Fragment slot="npm">
  ```shell
  npx astro dev --port 8080
  ```
  </Fragment>
  <Fragment slot="pnpm">
  ```shell
  pnpm astro dev --port 8080
  ```
  </Fragment>
  <Fragment slot="yarn">
  ```shell
  yarn astro dev --port 8080
  ```
  </Fragment>
</PackageManagerTabs>

Si has inicializado tu proyecto usando [el asistente `create astro`](/es/install/auto/#1-ejecuta-el-asistente-de-configuración), también puedes usar los scripts en `package.json` para usar una versión más corta de esos comandos. Puedes ver el `README.md` de tu proyecto para ver detalles de qué comandos están disponibles.

<PackageManagerTabs>
  <Fragment slot="npm">
  ```shell
  # corre el servidor de desarrollo en el puerto 8080 usando el script `start` en `package.json`
  npm run start -- --port 8080
  ```
  (Los guiones `--` extra antes del flag `--port` son necesarios para que `npm` pase tus flags al comando `astro`.)
  </Fragment>
  <Fragment slot="pnpm">
  ```shell
  # corre el servidor de desarrollo en el puerto 8080 usando el script `start` en `package.json`
  pnpm start --port 8080
  ```
  </Fragment>
  <Fragment slot="yarn">
  ```shell
  # corre el servidor de desarrollo en el puerto 8080 usando el script `start` en `package.json`
  yarn start --port 8080
  ```
  </Fragment>
</PackageManagerTabs>

## `astro dev`

Corre el servidor de desarrollo de Astro. Es un servidor HTTP local que no empaqueta recursos. Usa Hot Module Replacement (HMR) para actualizar tu navegador a medida que guardas los cambios en tu editor.

<h3>Flags</h3>

Usa estas flags para personalizar el comportamiento del servidor de desarrollo de Astro. Para flags compartidas con otros comandos de Astro, puedes ver [flags comunes](#flags-comunes) más abajo.

#### `--port <number>`

Especifica en qué puerto se ejecuta el servidor. El valor predeterminado es `3000`.

#### `--host [dirección de host opcional]`

Establece qué direcciones IP de red debe escuchar el servidor de desarrollo (es decir, direcciones IP que no son de localhost). Esto puede ser útil para probar tu proyecto en dispositivos locales tales como un teléfono durante el desarrollo.

- `--host` - escucha todas las direcciones, incluidas LAN y direcciones públicas
- `--host <dirección-personalizada>` - expone la dirección IP especificada en `<dirección-personalizada>`

:::caution
No uses la flag `--host` para exponer el servidor de desarrollo en un entorno de producción. El servidor de desarrollo está diseñado únicamente para uso local mientras desarrollas tu proyecto.
:::

## `astro build`

Construye tu proyecto para producción. Por defecto, Astro generará archivos estáticos y los colocará en el directorio `dist/`. Si [SSR está habilitado](/es/guides/server-side-rendering/), Astro generará los archivos necesarios para que el servidor renderice tu proyecto.

<h3>Flags</h3>

Usa estas flags para personalizar tu compilación. Para flags compartidas con otros comandos de Astro, puedes ver [flags comunes](#flags-comunes) más abajo.

#### `--drafts`

Incluye las [páginas de Markdown en borradores](/es/guides/markdown-content/#borradores-en-markdown) en la compilación.

## `astro preview`

Inicia un servidor local para servir tus archivos estáticos compilados en `dist/`.

Este comando es útil para obtener una vista previa de tu proyecto usando los archivos generados en la compilación final, antes de ser desplegado. Este comando no está diseñado para ejecutarse en producción. Para obtener ayuda con el despliegue a producción, consulte nuestra guía de [despliegue de un sitio web de Astro](/es/guides/deploy/).

Desde Astro 1.5.0, `astro preview` también funciona para compilaciones con SSR si usas un adaptador que lo soporte. Actualmente, solo el [adaptador de Node](/es/guides/integrations-guide/node/) soporta `astro preview`.

Puede combinarse con las [flags comunes](#flags-comunes) documentadas más abajo.

## `astro check`

Ejecuta diagnósticos (como verificación de tipos dentro de archivos `.astro`) en tu proyecto y reporta errores en la consola. Si se encuentran errores, el proceso finalizará con el código **1**.

Este comando está diseñado para usarse en workflows de CI.

:::note
Este comando solo verifica los tipos dentro de los archivos `.astro`.
:::

📚 Lee más sobre la [compatibilidad con TypeScript en Astro](/es/guides/typescript/).

## `astro add`

Agrega una integración a tu configuración. Lee más en la [guía de integraciones](/es/guides/integrations-guide/#configuración-de-integración-automática).

## `astro docs`

Inicia el sitio web de la documentación de Astro directamente desde la terminal.

## `astro telemetry`

Establece la configuración de telemetría para el usuario de la CLI actual. La telemetría son datos anónimos que proporcionan información al equipo de Astro sobre qué características de Astro se utilizan con más frecuencia.

La telemetría se puede desactivar con este comando:

```shell
astro telemetry disable
```

La telemetría se puede volver a habilitar con:

```shell
astro telemetry enable
```

El comando `clear` restablece los datos de telemetría:

```shell
astro telemetry clear
```

:::tip[¿Quieres deshabilitar la telemetría en entornos CI?]
Asegúrate de agregar el comando `astro telemetry disabled` a tus scripts de CI.
:::

## Flags Comunes

### `--root path`

Especifica la ruta a la raíz del proyecto. Si no se especifica, se asume que la carpeta de trabajo actual es la raíz.

La raíz se utiliza para encontrar el archivo de configuración de Astro.

```shell
astro --root miCarpetaRaiz/miCarpetaDeProyecto dev
```

### `--config <path>`

Especifica la ruta relativa al archivo de configuración desde la raíz del directorio. El valor predeterminado es `astro.config.mjs`. Usa esta opción si usas un nombre diferente en el archivo de configuración o si tienes tu archivo de configuración en otra carpeta.

```shell
astro --config config/astro.config.mjs dev
```

### `--site`

Configura el valor de [`site`](/es/reference/configuration-reference/#site) para tu proyecto. Usando este flag sobreescribirá el valor actual de `site` en tu archivo `astro.config.mjs`, si es que existe.

### `--base`

<Since v="1.4.1" />

Configura el valor de [`base`](/es/reference/configuration-reference/#base) para tu proyecto. Usando este flag sobreescribirá el valor actual de `base` en tu archivo `astro.config.mjs`, si es que existe.

### `--verbose`

Habilita el registro detallado, que es útil al debuggear un problema.

### `--silent`

Habilita el registro silencioso, que correrá el servidor sin ninguna salida en la consola.

## Flags Globales

Usa estas flags para obtener información sobre la CLI de `astro`.

### `--version`

Imprime el número de versión de Astro y finaliza el proceso.

### `--help`

Imprime un mensaje de ayuda.
