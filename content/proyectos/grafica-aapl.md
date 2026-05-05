---
title: "Terminal de Monitoreo y Simulación de Activos: BTC/USDT"
date: 2026-05-04
description: "Interfaz técnica para el seguimiento de mercado en tiempo real y ejecución de operaciones simuladas con gestión de fricción financiera."
tags: ["Finanzas", "APIs", "JavaScript", "Trading"]
#image: "images/PortadaLineal.png"
---

## Arquitectura de Monitoreo y Ejecución

Esta terminal ha sido diseñada como un entorno integrado para el análisis de microestructura de mercado y la validación de estrategias de inversión. El sistema opera mediante la sincronización de flujos de datos en tiempo real y un motor de cálculo que procesa la gestión de capital bajo condiciones operativas reales.

### 1. Visualización de Datos de Mercado
El módulo de visualización emplea la librería **Lightweight Charts** para el renderizado de velas japonesas de alta resolución. La integración se realiza mediante el consumo asíncrono de *endpoints* de mercado, permitiendo una observación precisa de la acción del precio y la volatilidad intradía.

{{< stock-chart >}}

*   **Frecuencia:** Actualización de datos cada 20 segundos.
*   **Fuente:** Conexión directa con API de mercado para precios *spot*.
*   **Interactividad:** Capacidad de ajuste de escala temporal y seguimiento de precio actual.

---

### 2. Motor de Simulación de Cartera
El simulador de gestión de capital permite ejecutar operaciones de compra y venta sobre un balance inicial de **$25,000 MXN**. Su propósito fundamental es medir el impacto del rendimiento neto frente a los costos operativos.

{{< trading-sim >}}

#### Funcionamiento del Motor:
*   **Gestión de Fricción:** El sistema aplica automáticamente una tasa de comisión del **0.1%** por cada transacción. Esto permite observar cómo los costos de operación afectan el punto de equilibrio (*breakeven*) y el retorno final.
*   **Cálculo de P&L Realizado:** El indicador de Ganancias y Pérdidas (P&L) se actualiza de forma dinámica comparando el valor total de la cuenta (Efectivo + Valor de Mercado de los Activos) contra el capital inicial.
*   **Control de Liquidez:** El algoritmo de ejecución valida la disponibilidad de fondos y activos antes de procesar cada orden, emulando las restricciones de un entorno de corretaje profesional.

---

### Especificaciones Técnicas
*   **Data Pipeline:** Implementación de peticiones `fetch` asíncronas para garantizar que la interfaz no sufra bloqueos durante la actualización de precios.
*   **Cómputo en Cliente:** Toda la lógica de cálculo de portafolio y renderizado se realiza en el navegador, asegurando una respuesta inmediata a las acciones del usuario.
*   **Estructura de Datos:** Los registros de transacciones se almacenan en un historial dinámico que permite auditar el precio de ejecución, el monto operado y la comisión pagada en cada evento.

La intención principal de este desarrollo es proporcionar una herramienta educativa que permita comprender la relación entre la volatilidad del mercado, el tamaño de posición y el impacto de los costos transaccionales en la rentabilidad a largo plazo.