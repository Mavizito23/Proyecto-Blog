---
title: "Dos chicas, un juego de billar y probabilidad"
author: "Mauricio Leon"
date: "2026-01-30"
categories: [probabilidad, python, simulación]
tags: [probabilidad, bayes, montecarlo]
math: true
draft: false
image: "../../images/Billar-Portada.png"
---

En muchos juegos por puntos, como el billar o el tenis de mesa, surge una pregunta natural: dado el marcador actual, ¿qué probabilidad tiene cada jugador de ganar? 

Este planteamiento da lugar al **Bayesian Billiards Problem**, un ejemplo que ilustra la diferencia entre el enfoque frecuentista y el bayesiano. En este escenario, las jugadoras A y B compiten en una mesa. Se lanza una bola al azar que define la "habilidad" (punto de corte). Gana quien llegue primero a **6 puntos**. El marcador actual es **5–3** a favor de A. 

¿Qué probabilidad tiene B de remontar?

---

### Enfoque frecuentista

Desde este punto de vista, asumimos que la probabilidad $p$ de que A gane un punto es fija y se estima con lo observado (5 de 8 jugadas):

$$p = \dfrac{5}{8} \rightarrow q = 1-p = \dfrac{3}{8}$$

Para que B gane, debe ganar 3 puntos seguidos:
$$P(\text{B gane}) = \left(\dfrac{3}{8} \right)^3 = \dfrac{27}{512} \approx 5.27\%$$

---

### Enfoque bayesiano

Aquí, $p$ no es un valor fijo, sino una **variable aleatoria**. Si asumimos una distribución previa uniforme $p \sim Beta(1,1)$, al actualizar con los datos (5 éxitos, 3 fracasos) obtenemos:

$$p | \text{datos} \sim Beta(1+5, 1+3) = Beta(6,4)$$

La probabilidad de que B gane es el promedio posterior de esa variable:
$$P(B \text{gana} | \text{datos}) = E[(1-p)^3]$$

Utilizando las propiedades de la distribución Beta:
$$E[(1-p)^3] = \dfrac{4\cdot 5 \cdot 6}{10\cdot 11 \cdot 12} = \dfrac{1}{11} \approx 9.09\%$$

**¡Es casi el doble que el enfoque frecuentista!** Pero, ¿quién tiene la razón?

---

### Simulación de Montecarlo

Para resolver la duda, simularemos miles de partidas. El proceso es:
1. Lanzar una bola "separadora" al azar ($p$ aleatorio).
2. Generar 8 jugadas. Si el resultado es 5-3, mantenemos la partida; si no, la descartamos (muestreo por rechazo).
3. Continuar las partidas válidas hasta que alguien llegue a 6.



```python
import numpy as np
import matplotlib.pyplot as plt

contador = 0
n = 10_000
i = 0

while i < n:
    separador = np.random.random()
    bolas = np.random.random(8)
    jugador_A = np.sum(bolas < separador)
    jugador_B = np.sum(bolas >= separador)

    if jugador_A == 5 and jugador_B == 3:
        i += 1
        while jugador_A < 6 and jugador_B < 6:
            bola = np.random.random()
            if bola < separador:
                jugador_A += 1
            else:
                jugador_B += 1
        if jugador_B == 6:
            contador += 1

# Visualización de una partida tipo
plt.figure(figsize=(8, 4))
ax = plt.gca()
ax.set_facecolor("#3a7352")
y = np.random.random(8)
plt.scatter(bolas[bolas < separador], y[:5], color='#2ecc71', s=100, label='A')
plt.scatter(bolas[bolas >= separador], y[5:], color='#e74c3c', s=100, label='B')
plt.axvline(x=separador, color='black', linestyle='--', linewidth=2)
plt.title("Estado inicial: 5 (verde) vs 3 (rojo)")
plt.legend()
plt.show()

print(f"Probabilidad simulada de que B gane: {contador/n}")
```

### Conclusión
La simulación arroja un valor cercano al 9.09%, confirmando que el enfoque bayesiano es más robusto para este problema. Esto sucede porque el enfoque frecuentista ignora la incertidumbre sobre qué tan "buena" es realmente la jugadora A basándose en una muestra tan pequeña.