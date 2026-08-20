# Changelog (Historial de Actualizaciones)
Proyecto: Bio-Guitar Geometric Synth (Planta Musical)

Todas las actualizaciones notables de este proyecto se documentarán en este archivo.

## [v11] - Versión E0.3 - MIGRACIÓN A WEBSOCKETS
### Añadido
- **WebSocketsServer:** Se implementó una comunicación persistente (Socket) en el puerto 81, reemplazando el antiguo método de HTTP Polling (`fetch`).
- **Telemetría de Alta Velocidad:** La gráfica web ahora se actualiza cada 150ms de forma ultra fluida sin colapsar el procesador.
- **Indicador de Red en Vivo:** Se añadió un indicador visual (punto verde/rojo) en el frontend web para monitorear el estado del WebSocket.

### Optimizado
- Código minificado: Se redujo el peso del código fuente en más de 400 líneas al compactar el HTML/CSS/JS, ahorrando memoria Flash y RAM al enviar la página web.

## [v10] - Versión E0.2 - ESTABILIDAD DE RED (ANTI-DDOS)
### Corregido
- **Fuga de Sockets y Reinicios:** Se arregló un bug crítico donde el M5Stack se reiniciaba a los 45 minutos. 
- **Candado Frontend:** Se añadió la variable `isFetching` en JavaScript para evitar que el celular abrume al ESP32 con múltiples peticiones superpuestas.
- **WiFi Asíncrono:** Se añadió `WiFi.setSleep(false)` para evitar que el radio WiFi entre en suspensión, reduciendo el lag de la red interna de 600ms a 10ms.

## [v9] - Versión D0.4 / E0.1 - PARCHE DE IDENTIDAD
### Añadido
- **Telemetría Inyectada:** El M5Stack ahora envía su ID dinámico y su versión de Firmware directamente al frontend, mostrándose como una firma en la parte inferior de la interfaz web.
- **Fallback Seguro:** Si el grillo pierde conexión, la interfaz web rellena la pantalla con "v--" en lugar de colapsar.

### Corregido
- **Bug del "Grillo 0000":** En ESP32 Core v3.x, el radio WiFi tarda en encender. Se agregó un `delay(150)` antes de leer la MAC Address para asegurar que los grillos obtengan su ID único real y no generen la red de error "0000".

## [v8] - Versión D0.3 - MIGRACIÓN M5UNIFIED Y PARTICIONES
### Añadido
- **Soporte ESP32 Core 3.0.0+:** Migración completa de la vieja librería `<M5Stack.h>` a la moderna `<M5Unified.h>`.
- **Modo de Recuperación (Factory Reset):** Ahora, si el usuario mantiene presionado el Botón A al encender, el grillo borra su partición de preferencias (`Preferences.h`) para eliminar memoria corrupta.
- **OTA Redirecciones:** Se implementó `HTTPC_STRICT_FOLLOW_REDIRECTS` para que el grillo pueda seguir los enlaces crudos de GitHub sin fallar la descarga.

### Optimizado
- Código preparado para compilarse bajo el esquema de partición **"Minimal SPIFFS (1.9MB APP con OTA)"**.

## [v4 a v7] - Versiones D0.1 y D0.2 - INFRAESTRUCTURA OTA
### Añadido
- **ID Dinámico:** Generación automática de nombre de red (`BioSynth_Setup_XXXX`) y mDNS (`biosynth-XXXX.local`) basados en los últimos 4 caracteres de la dirección MAC del dispositivo, evitando colisiones en el aula.
- **Portal Cautivo:** Integración de `WiFiManager` para configurar redes sin codificar credenciales en el hardware.
- **Actualizaciones Inalámbricas (OTA):** Sistema nativo que lee un `version.json` alojado en GitHub y descarga silenciosamente archivos `.bin`.

## [v3] - Versión C - MONITOR BIOMÉTRICO NATIVO
### Añadido
- **Osciloscopio Web Offline:** Se eliminó la dependencia de la librería web *Chart.js*. Se programó un canvas de HTML5 puro que dibuja la onda biométrica en tiempo real sin requerir acceso a internet.
- **Animaciones en M5Stack:** El grillo ahora dibuja caras geométricas (estudiando, trabajando, durmiendo) y parpadea usando la pantalla física basándose en el modo seleccionado en el teléfono.

### Corregido
- Bug de desbordamiento de memoria (Memory Leak) causado por la serialización ineficiente de `ArduinoJson`.
