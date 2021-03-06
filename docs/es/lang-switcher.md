# Selector de  idiomas

Cuando **nuxt-i18n** se carga en su aplicación, agrega su configuración `locales` a `this.$i18n` (o `app.i18n`), lo que hace que sea muy fácil mostrar un selector de  idiomas en cualquier lugar de su aplicación.

Aquí hay un ejemplo de selector de idiomas donde se ha agregado una tecla `name` a cada objeto de configuración local para mostrar títulos más amigables para cada enlace:
```vue
<nuxt-link
  v-for="locale in availableLocales"
  :key="locale.code"
  :to="switchLocalePath(locale.code)">{{ locale.name }}</nuxt-link>
```

```js
computed: {
  availableLocales () {
    return this.$i18n.locales.filter(i => i.code !== this.$i18n.locale)
  }
}
```

```js
// nuxt.config.js

['nuxt-i18n', {
  locales: [
    {
      code: 'en',
      name: 'English'
    },
    {
      code: 'es',
      name: 'Español'
    },
    {
      code: 'fr',
      name: 'Français'
    }
  ]
}]
```

Si las opciones  `detectBrowserLanguage.useCookie` y `detectBrowserLanguage.alwaysRedirect` están habilitadas, es posible que desee conservar el cambio a la configuración local llamando a `this.$i18n.setLocaleCookie(locale)` o (`app.i18n.setLocaleCookie(locale)`) método. De lo contrario, la configuración regional volverá a la guardada durante la navegación.

## Parámetros de ruta dinámica

Tratar con parámetros de ruta dinámicos requiere un poco más de trabajo porque necesita proporcionar traducciones de parámetros a **nuxt-i18n**.  Para este propósito, el módulo de tienda de **nuxt-i18n** expone una propiedad de estado `routeParams` que se fusionará con los parámetros de ruta al generar rutas del selector de idiomas con `switchLocalePath()`.

> NOTA: Asegúrese de que Vuex [esté habilitado](https://nuxtjs.org/guide/vuex-store) en su aplicación y que no haya configurado la opción  `vuex` en `false` en opciones de **nuxt-i18n**.

Para proporcionar traducciones de parámetros dinámicos, envíe el `i18n/setRouteParams`  lo antes posible al cargar una página, por ejemplo:

```vue
<template>
  <!-- pages/_slug.vue -->
</template>

<script>
export default {
  async asyncData ({ store }) {
    await store.dispatch('i18n/setRouteParams', {
      en: { slug: 'my-post' },
      fr: { slug: 'mon-article' }
    })
    return {
      // your data
    }
  }
}
</script>
```

> NOTA: **nuxt-i18n** no restablecerá las traducciones de parámetros por usted, esto significa que si utiliza parámetros idénticos para diferentes rutas, navegar entre esas rutas podría generar parámetros conflictivos. Asegúrese de establecer siempre traducciones de parámetros en tales casos.
