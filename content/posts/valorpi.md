---
title: "Estimando el valor de Pi con puntitos"
date: 2026-01-20
description: "Descubre cómo obtener el valor de π utilizando simulaciones de azar y Python."
categories: ["Geometría", "Python", "Simulación"]
tags: ["Montecarlo", "Matemáticas", "Data Science"]
math: true 
draft: false
image: "../../images/ValorPi.jpg"
---

## El valor de PI

Todos sabemos que el valor de $\pi$ es aproximadamente $3.1415...$ y tal vez la "demostración" nos la enseñaron en la secundaria o preparatoria. Pero ¿y si lo vemos de otra manera? A continuación veremos otra forma de obtener el valor de $\pi$ pero con una herramienta muy poderosa llamada **Simulación de Montecarlo**.

### ¿Qué es la Simulación de Montecarlo?

La simulación de Montecarlo es un tipo de algoritmo computacional que utiliza un muestreo aleatorio repetido para obtener la probabilidad de que ocurra una serie de resultados. 

Fue inventado por **John von Neumann** y **Stanislaw Ulam** durante la Segunda Guerra Mundial. Debe su nombre al famoso casino de Mónaco, ya que el elemento de azar es fundamental en el planteamiento del modelo. A diferencia de un modelo de pronóstico normal, esta simulación construye un modelo de posibles resultados aprovechando una distribución de probabilidad para cualquier variable que tenga incertidumbre inherente.

### Aplicación para hallar el valor de PI

Sabemos por las fórmulas geométricas básicas que las áreas del cuadrado y del círculo se calculan como sigue:

$$\text{Área del círculo} = \pi \cdot r^2$$
$$\text{Área del cuadrado} = l^2$$

Si inscribimos un círculo de radio $r=1$ dentro de un cuadrado de lado $l=2$, la relación de sus áreas es:

$$\frac{\text{Área del Círculo}}{\text{Área del Cuadrado}} = \frac{\pi \cdot 1^2}{2^2} = \frac{\pi}{4}$$

Por lo tanto, si lanzamos miles de puntos aleatorios al cuadrado, la proporción que caiga dentro del círculo multiplicada por 4 nos dará el valor de $\pi$.

### Implementación en Python

Aunque Hugo no ejecuta el código, aquí te comparto la lógica utilizando `numpy` y `matplotlib` para que puedas replicarlo:

```python
import numpy as np
import matplotlib.pyplot as plt

def ejecutar_simulacion(n):
    # Generar n puntos en un plano 2D entre -1 y 1
    puntos = np.random.uniform(-1, 1, (n, 2))
    
    # Calcular distancias al origen (Teorema de Pitágoras)
    distancias = np.sum(puntos**2, axis=1)
    
    # Identificar puntos dentro del círculo unitario
    dentro_circulo = distancias <= 1
    puntos_dentro = np.sum(dentro_circulo)
    
    pi_estimado = 4 * puntos_dentro / n
    return puntos, dentro_circulo, pi_estimado

# Ejecución con 10,000 puntos
n_puntos = 10000
puntos, mascara, resultado = ejecutar_simulacion(n_puntos)
```
### Explicación de los resultados
El algoritmo funciona porque estamos simulando un experimento físico de manera digital. Al calcular `np.sum(puntos**2, axis=1)`, estamos aplicando el teorema de Pitágoras para saber qué tan lejos está cada punto del centro $(0,0)$. Si la distancia es menor o igual a 1, el punto "golpeó" el círculo. A medida que aumentas el valor de $n$, notarás que el valor estimado se acerca cada vez más al valor real de $3.14159...$.

### Conclusión
La simulación de Montecarlo nos enseña que el azar, cuando se repite masivamente, revela patrones matemáticos exactos. Como Ingeniero Matemático, valoro esta técnica no solo para calcular $\pi$, sino por su papel fundamental en finanzas, física nuclear y optimización de procesos.