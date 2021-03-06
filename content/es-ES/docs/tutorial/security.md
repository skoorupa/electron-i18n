# Seguridad, capacidades nativas y su responsabilidad

Como desarrolladores web, disfrutamos la fuerte seguridad del buscador generalmente - El riesgo asociado con el código que escribimos es relativamente pequeño. Nuestros sitios web se les concede poderes limitados en una caja de arena, y confiamos que nuestros usuarios disfrutan un buscador construido por un gran equipo de ingenieros que es capaz de responder rápidamente a amenazas de seguridad recién descubiertas.

Cuando se trabaje con Electron, es importante entender que Electron no es un navegador web. Te permite construir aplicaciones de escritorio llenas de utilidades con tecnologías web familiares, pero tu código tiene mucho más poder. JavaScript puede acceder a los archivos del sistema, actividades del usuario y más. Esto le permite construir aplicaciones nativas de gran calidad, pero los riesgos de seguridad suben con el poder adicional concedido a tu código.

Con eso en mente, ten en cuenta que mostrar contenido arbitrario proveniente de fuentes poco confiables viene con un riesgo severo que Electron no está diseñado para manejar. De hecho, las aplicaciones de electron más populares (Atom, Slack, Visual Studio Code, etc) muestran contenido local primero (o confiable, asegurado contenido remoto sin integración nodal) - si tu aplicación ejecuta código de una fuente online es tu responsabilidad asegurar que el código no es malicioso.

## Reportando problemas de seguridad

Para información sobre cómo revelar las vulnerabilidad de Electrón dirigirse a [SECURITY.md](https://github.com/electron/electron/tree/master/SECURITY.md)

## Actualizaciones y problemas de seguridad Chromium

Mientras Electron se esfuerza para apoyar nuevas versiones de Chromium lo más pronto posible, los desarrolladores deben estar consientes que actualizar es una tarea muy serio - involucrando ediciones a mano a docenas o hasta cientos de archivos. Dados los recurso y contribuciones disponibles hoy, Electron no va a estar usando la última versión de Chomium, rezagandose por días o semanas.

Creemos que nuestro sistema de actualizaciones de Chromium alcanza un balance apropiado entre los recursos que tenemos disponibles y las necesidad de la mayoría de las aplicaciones en el marco de referencia. Estamos definitivamente interesados en escuchar más sobre casos específicos de gente que construye cosas encima de Electron. Solicitudes de extracción y contribuciones apoyando este esfuerzo son siempre bienvenidas.

## Ignorando Consejos

Un problema de seguridad existe sin importar si recibes un código de un lugar remoto y lo ejecutas localmente. Como ejemplo, considere una página web remota siendo mostrada dentro de una ventana del navegador. Si el atacante sabe como arreglarselas para cambiar dicho contenido (bien sea atacando la fuente directamente, o interviniendo entre su aplicación y el destino real), Será capaz de ejecutar códigos nativos en la máquina del usuario,.

> :warning: Bajo ninguna circunstancia usted debería cargar y ejecutar un código remoto con la integración de nodos activada.:warning. En cambio, use solo archivos locales (guardados juntos con su aplicación) para ejecutar el código nodal. Para mostrar contenido remoto use la etiqueta `webview` y asegúrese de desactivar el `nodeIntegration`.

#### Lista de verificación

Esto no es aprueba de balas, pero debería intentar lo siguiente al menos:

* Solo mostrar contenido seguro (https)
* Deshabilitar los nodos de integración en todos los renders que muestran contenido remoto (setting `nodeIntegration` to `false` in `webPreferences`)
* Habilitar aislamiento del contexto en todos los renders que muestren contenido remoto (ajustando `contextIsolation` a `true` en `webPreferences`)
* Usar `ses.setPermissionRequestHandler()` en todas las sesiones que cargan contenido remoto
* No desactive `webSecurity`. desactivarlo deshabilitará la política de mismo origen.
* Defina un [`Content-Security-Policy`](http://www.html5rocks.com/en/tutorials/security/content-security-policy/) y use reglas extrictas (i.e. `script-src 'self'`)
* [Override and disable `eval`](https://github.com/nylas/N1/blob/0abc5d5defcdb057120d726b271933425b75b415/static/index.js#L6-L8) que permite a las cadenas ser ejecutadas como código.
* No ajuste `allowRunningInsecureContent` a verdad.
* No habilite `experimentalFeatures` o `experimentalCanvasFeatures` a menos que sepa que está haciendo.
* No use `blinkFeatures` a menos que sepa qué está haciendo.
* Visor web: no agregue el atributo `nodeintegration`.
* Visor web: no use `disablewebsecurity`
* Visor web: no use `allowpopups`
* Visor web: no use `insertCSS` o `executeJavaScript` con CSS/JS remotos.
* Visor web: verifique las opciones y parámetros de todos `<webview>` los tags antes de que sean atacados usando el evento `will-attach-webview`:

```js
app.on('web-contents-created', (event, contents) => {
  contents.on('will-attach-webview', (event, webPreferences, params) => {
    // desactive los scrips predeterminados si están en desuso o verifique que su locación sea legítima
    delete webPreferences.preload
    delete webPreferences.preloadURL

    // Inhabilite la integración de los nodos
    webPreferences.nodeIntegration = false

    // Verifica que la URL estén cargadas
    if (!params.src.startsWith('https://yourapp.com/')) {
      event.preventDefault()
    }
  })
})
```

De nuevo, esta lista solo minimiza los riesgos, no los remueve. Si su meta es mostrar una página web, un navegador sería una opción más segura.