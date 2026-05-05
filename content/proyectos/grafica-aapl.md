---
title: "Monitoreo de AAPL en Tiempo Real"
date: 2026-05-04
description: "Visualización de datos financieros usando APIs y Lightweight Charts."
tags: ["Finanzas", "Python", "Trading"]
#image: "images/portada-aapl.png"
---

## Visualización de Activos Financieros

Este proyecto surge de la necesidad de monitorear la volatilidad de **Apple Inc. (AAPL)** mediante el uso de datos en tiempo real. Como estudiante de **Ingeniería Matemática**, me interesa observar cómo las fluctuaciones de corto plazo se alinean con los modelos estocásticos aprendidos en la ESFM.

### Gráfica de Velas (Live)
A continuación, se presenta la caja dinámica que integra la API de Alpha Vantage con la librería *Lightweight Charts*. 

{{< stock-chart >}}

### Detalles Técnicos
* **Lenguaje:** JavaScript (Client-side rendering).
* **Frecuencia:** Actualización cada 60 segundos.
* **Datos:** Velas de 1 minuto que cubren las últimas 24 horas de operación.