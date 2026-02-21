# 📺 Sistema de Cartelería Digital Serverless - Típicos Margoth

Sistema de gestión de contenidos multimedia (Digital Signage) desarrollado a medida para el control centralizado de +70 pantallas distribuidas en 14 sucursales. Diseñado con una arquitectura *Serverless* de alta disponibilidad, bajo consumo de red y tolerancia a fallos en hardware de bajos recursos (Smart TVs).

## 🚀 Características Principales

* **100% Serverless y Centralizado:** Infraestructura alojada en DigitalOcean Spaces (CDN). Permite actualizar todas las pantallas del país simultáneamente modificando un solo archivo JSON, sin necesidad de conectarse remotamente a cada TV.
* **Gestión Automática de Memoria (RAM):** Sistema de *Auto-Refresh* implementado en los cambios de turno (ej. 10:30 a.m. y 2:00 p.m.) para prevenir *Memory Leaks* y congelamientos nativos en los navegadores de las Smart TVs.
* **Sincronización de Zona Horaria (Failsafe):** Motor UTC integrado que fuerza la hora exacta de El Salvador (UTC-6), ignorando si la TV local tiene la configuración regional o el reloj desajustado.
* **Tolerancia a Caídas de Red (Offline Mode):** Bloques `try...catch` que garantizan que, ante una caída del proveedor de internet (Tigo/Claro) en la sucursal, el reproductor no muestre errores y mantenga el video actual en bucle infinito desde la caché local.
* **Optimización de Ancho de Banda (Peticiones HEAD):** El sistema consulta el servidor cada 30 segundos usando peticiones asíncronas ultraligeras que solo leen metadatos (`Last-Modified`). Solo descarga el video de la nube si detecta que IT ha reemplazado el archivo, protegiendo el internet del sistema de facturación (POS).
* **Compresión Extrema:** Protocolo estricto de compresión de video (reducción de 3.50 GB a ~280 MB por archivo) para garantizar descargas rápidas y reproducción fluida en procesadores de TV de gama de entrada.

## 🏗️ Arquitectura y Tecnologías

* **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES6+).
* **Backend / Storage:** DigitalOcean Spaces (Object Storage con CDN global).
* **Base de Datos:** Archivos estáticos `.json` para configuración de listas de reproducción.
* **Control de Versiones:** Git / GitHub.

## 📂 Estructura del Proyecto

El sistema divide el contenido de forma modular para facilitar su escalabilidad:

```text
/
├── pantalla1/                 # Menú Principal (Rotación Quincenal S1/S2)
│   ├── player_p1.html         # Reproductor cliente
│   ├── playlist_p1.json       # Lógica de horarios y URLs
│   └── p1_manana.mp4          # Assets multimedia
├── pantalla2/                 # Acompañamientos / Sucursal (Rotación S1/S2)
│   ├── player_p2.html
│   └── playlist_p2.json
└── pantalla3/                 # Publicidad General / Promociones
    ├── player_p3.html
    └── playlist_p3.json
