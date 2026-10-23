# Informe: elección de familia de desarrollo para Dogglers

## 1. Aplicación elegida y qué hace (5 líneas)

1. **Dogglers** es un catálogo de perros de un servicio de paseo o guardería.
2. Cada perro se muestra con foto, nombre, edad y hobbies, a partir de una lista fija incluida en el código (sin red ni cuentas).
3. Desde la pantalla principal se elige el formato: lista vertical, lista horizontal o cuadrícula de 2 columnas.
4. La usaría un dueño o paseador que quiere elegir o conocer a los perros del servicio, desde el móvil.
5. Situaciones: elegir perro antes de un paseo, mirar el catálogo en casa o comparar perros de un vistazo. Se ha usado en el emulador (Android 7.0) antes de escribir esto.

## 2. Censo del parque (plantilla de cinco ejes)

No hay datos reales de usuarios. Para obtenerlos: Google Play Console (dispositivos y versiones de quienes la instalan) o una encuesta corta a los clientes del servicio. **Supuestos de trabajo:** usuarios en España, móviles personales de gama media y baja, uso ocasional.

| Eje | Qué se mide | Datos supuestos | Grupos |
|---|---|---|---|
| Versión de sistema | API mínimo y objetivo | De Android 5 (API 21) a Android 15; la mayoría en Android 10–14 | A: Android 12–15 · B: Android 10–11 · C: Android 5–9 |
| Tamaño y densidad | Pulgadas y dpi | De 4,7" hdpi a 6,7" xxhdpi; pocas tablets de 8–10" | A: 6,4–6,7" xxhdpi · B: 6" xhdpi · C: 4,7–5,5" hdpi · D: tablet |
| Memoria | RAM total | De 2 GB a 12 GB | A: 6–12 GB · B: 3–4 GB · C: 2 GB |
| Conectividad | Redes y estabilidad | 4G/5G y wifi; irregular en parques y calle | Da igual: la app es offline |
| Sensores | GPS, cámara, acelerómetro | La app no usa ninguno | Sin requisitos |

**Grupos resultantes (4):**
- **A. Gama alta reciente:** Android 12–15, pantalla grande, 6 GB o más.
- **B. Gama media:** Android 10–11, 6", 3–4 GB.
- **C. Gama baja o antigua:** Android 5–9, 5", 2 GB, hdpi.
- **D. Tablets:** pocas, 8–10".

## 3. Perfil de la aplicación y contraste

| Fila | Perfil deducido del censo | Lo que declara la app (build.gradle / manifest) | ¿Coincide? |
|---|---|---|---|
| Versión mínima | Android 5.0 (API 21): cubre al grupo C | `minSdkVersion 21` | Sí |
| Versión objetivo | La última estable razonable | `targetSdkVersion 32` (`compileSdkVersion 32`) | **No**: está desfasado |
| Pantalla mínima | ~4,7" y densidad hdpi (grupo C) | Sin `supports-screens` ni restricciones | Sí, no excluye a nadie |
| Obligatorio | Nada: ni GPS, ni cámara, ni almacenamiento | Sin `uses-permission` ni `uses-feature` | Sí |
| Opcional | Internet y notificaciones, si algún día hay datos en vivo | Nada declarado | Sí (hoy no hace falta) |

**Conclusión:** el perfil del censo coincide con el declarado salvo en la versión objetivo. `targetSdk 32` habría que subirlo (Google Play exige un target reciente para publicar y actualizar; hay que comprobar el valor vigente).

## 4. Evaluación de las tres familias contra los cinco criterios

| Criterio | Nativo (Kotlin/Android) | Compilado a nativo (Flutter, KMP, React Native) | Basado en HTML5 (PWA, Ionic/Cordova) |
|---|---|---|---|
| Plataformas objetivo | Solo Android; iOS exigiría otra app | Android e iOS con una base | Android, iOS y web |
| Acceso al hardware | Total (aquí no se necesita) | Casi total, vía plugins | Limitado (aquí no se necesita) |
| Equipo disponible | Kotlin y Android (lo que ya hay) | Hay que aprender otro framework | Basta con web básica |
| Vida útil prevista | Larga: respaldado por Google | Larga, ligada al framework | Media: depende del WebView y del navegador |
| Coste de mantenimiento | Una base por plataforma | Una base, con fricción en actualizaciones | Bajo, pero con riesgo de rendimiento en gama baja |

## 5. Decisión y qué se pierde

**Decisión:** nativo (Kotlin), que es en lo que ya está hecha.

**Motivos:** el público objetivo es Android; el equipo ya domina Kotlin; la app es simple y no necesita nada de iOS hoy; y el grupo C (2 GB, hdpi) es el que mejor soporta una app nativa ligera.

**Qué se pierde:**
- No llega a iOS sin construir una segunda app.
- Si se quisiera una versión web, habría que rehacerla.
- Duplicar equipo o esfuerzo si el negocio crece a las dos plataformas.

## 6. Qué haría cambiar de opinión

- Que el servicio tenga una proporción relevante de clientes con iPhone (p. ej. más de ~30 %): pasaría a **multiplataforma compilado** (Flutter o KMP).
- Que haga falta publicar también como web sin instalación: **HTML5/PWA**.
- Que el equipo cambie y no haya nadie con Kotlin: elegiría lo que domine el equipo disponible.
- Que la app necesite hardware muy específico (cámara avanzada, NFC): reforzaría **nativo**.

## 7. Qué se ha aceptado y rechazado de la revisión recibida

> **Pendiente de completar por el equipo:** aún no hay revisión de otra persona o grupo en esta conversación. Cuando la tengáis, rellenad la tabla.

| Comentario recibido | Aceptado / Rechazado | Motivo |
|---|---|---|
| (por rellenar) | | |

### Uso de IA: prompt y registro de correcciones

**Prompt (resumen):** se pidió a la IA (Claude) completar la app Dogglers y, después, resolver las seis tareas de la actividad (descripción, censo, grupos, perfil, familias y elección), pegando el enunciado y la plantilla del profesorado.

**Registro de correcciones:**

| # | Qué hizo la IA | Corrección | Resultado |
|---|---|---|---|
| 1 | Primer borrador del censo con ejes inventados (versión, pantalla, densidad, hardware, conectividad) | Se rehízo con los cinco ejes de la plantilla: versión de sistema, tamaño y densidad, memoria, conectividad, sensores | Corregido en este informe |
| 2 | Cifras del censo | Son supuestos, no datos medidos; se declara y se indica cómo conseguir los reales | Marcado como supuesto |
| 3 | Perfil de la app | Se contrastó con los valores reales de `app/build.gradle` (minSdk 21, targetSdk 32) y el `AndroidManifest.xml` (sin permisos) | Verificado |
