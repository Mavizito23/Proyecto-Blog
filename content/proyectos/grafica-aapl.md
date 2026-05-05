---
title: "Terminal Quant: Visualización y Simulación de Activos"
date: 2026-05-04
description: "Desarrollo de una interfaz dinámica para el monitoreo de mercados y ejecución de estrategias con gestión de capital."
tags: ["Finanzas", "Python", "JavaScript", "Trading Quant"]
#image: "images/PortadaLineal.png"
---

## Análisis de Microestructura y Volatilidad

Este proyecto integra herramientas de ingeniería de software con modelado financiero para analizar la microestructura del mercado en tiempo real. Como **Ingeniero Matemático**, mi objetivo es observar cómo las fluctuaciones de corto plazo se alinean con los modelos estocásticos y procesos de difusión aprendidos en la **ESFM**.

La terminal se divide en dos módulos críticos: monitoreo visual y gestión de capital.

### 1. Monitoreo de Mercado en Tiempo Real
A continuación, se presenta un panel dinámico que consume datos directamente de la API de Binance. Utilizo la librería **Lightweight Charts** para renderizar velas japonesas con alta eficiencia, permitiendo identificar patrones de volatilidad sin latencia significativa.

{{< stock-chart >}}

> **Nota técnica:** La gráfica está configurada con una paleta de colores personalizada (basada en mis visualizaciones de *Manim*) para mantener una identidad visual técnica y profesional.

---

### 2. Simulador de Gestión de Portafolio
Más allá de la visualización, la toma de decisiones financieras requiere un control estricto del capital. He implementado un motor de simulación con un balance inicial de **$25,000 MXN**. 

Este módulo permite ejecutar operaciones de compra/venta sobre un universo de 5 activos distintos, considerando:
* **Costos de Transacción:** Aplicación de comisiones reales (0.1%) para medir el impacto del *fee* en el rendimiento total.
* **Balance Dinámico:** Seguimiento de capital invertido vs. efectivo disponible.
* **Control de P&L:** Registro histórico de operaciones para análisis post-trade.

{{< trading-sim >}}

---

### Arquitectura del Sistema
*   **Data Pipeline:** Consumo asíncrono de APIs REST para precios *spot*.
*   **Front-end:** Renderizado del lado del cliente para asegurar una respuesta inmediata de la interfaz.
*   **Lógica de Inversión:** Algoritmo de gestión que valida la liquidez del portafolio antes de cada ejecución, emulando un sistema de gestión de riesgos profesional.

Este proyecto demuestra mi capacidad para transformar datos abstractos en herramientas de decisión estratégica, combinando la precisión matemática con el desarrollo de aplicativos de alto impacto.