# Changelog (Historial de Actualizaciones)
Proyecto: Bio-Guitar Geometric Synth (Planta Musical)

Todas las actualizaciones notables de este proyecto se documentarán en este archivo. El formato está basado en los estándares de control de versiones de la industria (Keep a Changelog).

## [v11] - Versión E0.3 - MIGRACIÓN A WEBSOCKETS
### Añadido
- **WebSocketsServer:** Se implementó comunicación persistente bidireccional en el puerto 81 (`<WebSocketsServer.h>`), reemplazando el antiguo método de HTTP Polling (`fetch()`).
- **Telemetría de Alta Velocidad:** La gráfica web ahora se actualiza cada 150 milisegundos de forma ultra fluida.
- **Indicador de Red en Vivo:** Se añadió un indicador visual (punto verde/rojo) en el frontend web para monitorear el estado y salud de la conexión WebSocket.

### Optimizado
- **Código minificado:** Se redujo el peso del código fuente en casi 450 líneas al compactar el bloque HTML/CSS/JS, ahorrando memoria Flash y RAM al despachar la página web.

## [v10] - Versión E0.2 - ESTABILIDAD DE RED (ANTI-DDOS)
### Añadido
- **Candado Frontend:** Se añadió la variable booleana `isFetching` en JavaScript para evitar el solapamiento de peticiones asíncronas que causaba caídas del servidor tras 45 minutos de uso.
- **Desactivación de Suspensión WiFi:** Se añadió el comando `WiFi.setSleep(false)` en el backend para evitar que el radio entre en modo de ahorro de energía, reduciendo el lag de la red local significativamente.

## [v9] - Versión E0.1 - INYECCIÓN DE FIRMAS Y SOCKET STARVATION
### Añadido
- **Telemetría Inyectada:** El ESP32 ahora envía su ID dinámico y número de versión (`versionFIRMWARE`) en el payload JSON. Se muestran como una firma en el footer de la interfaz web.
- **Fallback Seguro:** Si la interfaz web pierde conexión o el JSON falla, la página rellena los campos críticos con el valor "--" en lugar de colapsar la vista.

### Corregido
- **Bug del "Grillo 0000":** Se agregó un `delay(150)` crítico antes de leer la MAC Address para dar tiempo de arranque físico al radio WiFi asíncrono en Core v3.0.0+.
- **Fuga de Sockets HTTP:** Se añadió el header `Connection: close` en las respuestas de `handleData()` para forzar el cierre inmediato de puertos, mitigando el *Socket Starvation*.

## [v8] - Versión D0.3 - M5UNIFIED Y DEBUG OTA
### Añadido
- **Redirecciones Estrictas OTA:** Se implementó `HTTPC_STRICT_FOLLOW_REDIRECTS` para asegurar descargas OTA exitosas desde URLs dinámicas directas de GitHub.
- **Debug Visual OTA:** El sistema ahora imprime los errores de la librería `httpUpdate` directamente en la pantalla física del dispositivo.

### Cambiado
- **Core de Hardware:** Migración definitiva de la librería `<M5Stack.h>` a `<M5Unified.h>` para garantizar soporte a largo plazo con versiones de ESP32 Core v3.0.0+ y placas modernas.

## [v6 a v7] - Versiones D0.1 y D0.2 - FACTORY RESET Y LIMPIEZA DE RAM
### Añadido
- **Identificador Dinámico (Base):** Se automatizó la creación de la red de Setup (`BioSynth_Setup_XXXX`) y de la URL local mDNS (`biosynth-XXXX.local`) usando los últimos 4 caracteres de la MAC Address.
- **Modo Factory Reset:** Mantener presionado el Botón A durante el arranque (logo M5) borra la partición de la librería `Preferences`, eliminando configuraciones corruptas.
- **Recolector de Basura (Garbage Collector):** Se incluyó un temporizador (`timerLimpiezaRAM`) que libera procesos huérfanos del WiFi si la memoria RAM desciende por debajo de los 20KB críticos.

## [v4] - Versión D (PRO) - MEMORIA NO VOLÁTIL Y FILTRO EMA
### Añadido
- **Memoria Persistente:** Integración de `<Preferences.h>` para guardar automáticamente el `enfoqueActual` y `modoFiltro` en la memoria flash, restaurando el estado tras apagarse.
- **Filtro Matemático EMA:** Se implementó *Exponential Moving Average* en las señales del ADC, mitigando drásticamente el ruido electromagnético y picos de estática.
- **Controles Avanzados Web:** Se añadieron `<input type="range">` en HTML para manipular en tiempo real el Volumen Maestro y el Feedback (Eco/Reverb) de los osciladores web.

## [v3] - Versión C (OFFLINE) - MONITOR NATIVO Y UI FÍSICA
### Añadido
- **Osciloscopio Web Nativo:** Se eliminó la dependencia externa de `Chart.js`. Se programó un lienzo `<canvas>` en HTML5 para renderizar ondas biométricas de forma 100% offline.
- **Motor Gráfico en Dispositivo:** El M5Stack ahora dibuja animaciones geométricas (gafas estudiando, casco trabajando, Zzz descansando) en respuesta al modo seleccionado por el usuario.

### Corregido
- **Fuga de Memoria JSON:** Se arregló un bug de fragmentación de RAM al refactorizar `handleData()`. La concatenación insegura de Strings fue reemplazada por las rutinas de serialización seguras de `ArduinoJson`.

## [v2] - Versión 2 - ESTABILIDAD VISUAL Y RED LOCAL
### Añadido
- **mDNS Integrado:** Permite acceder a la interfaz mediante la dirección amigable `http://biosynth.local` sin depender de direcciones IP numéricas.

### Corregido
- **Parpadeo de Pantalla (Flicker):** Se eliminó la limpieza de pantalla completa (`M5.Lcd.fillRect()`) en cada ciclo y se reemplazó por espaciado formateado (`%-20s`), logrando un refresco visual fluido.

## [v1] - Versión 1 - ARQUITECTURA ORIGINAL
### Añadido
- Inicialización base del proyecto.
- Configuración del puente de Wheatstone para lectura cruda en pines 35 y 36.
- Portal cautivo inicial usando `WiFiManager`.
- Creación de las matrices estáticas bidimensionales de frecuencias musicales.
