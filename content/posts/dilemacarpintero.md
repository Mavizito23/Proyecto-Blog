---
title: "Programación Lineal para optimizar la toma de decisiones"
date: "2026-02-28"
description: "Las matematicas como herramienta para maximizar (o minimizar) cosas de la vida cotidiana."
categories: ["Optimización", "Toma de Decisiones", "Gestión de Operaciones"]
tags: ["Programación Lineal", "Matemáticas Aplicadas", "Eficiencia", "Modelado de Negocios"]
math: true 
draft: false
image: "../../images/ValorPi.jpg"
---
{{< reproductor src="/audio/pista.mp3" >}}
## El Dilema del Carpintero

En el mundo de los negocios y la tecnología, a menudo nos enfrentamos a una pregunta engañosamente simple: **¿Cómo podemos hacer más con menos?** Ya sea que estés gestionando el presupuesto de una campaña de marketing, asignando horas de desarrollo en un sprint de software o, en este caso, dirigiendo un taller de mobiliario, los recursos siempre son finitos. El verdadero reto del liderazgo no es solo producir, sino **optimizar**.

Imagina que eres el dueño de un taller de carpintería de diseño. Tienes clientes esperando, una cantidad limitada de madera premium en el almacén y un número fijo de horas hombre antes de que termine la semana. Cada decisión que tomas tiene un **costo de oportunidad**: si decides fabricar un escritorio de lujo, pierdes la oportunidad de usar ese mismo tiempo y material en varias estanterías minimalistas.

¿Cómo saber, con precisión matemática, cuál es la combinación exacta de productos que pondrá la mayor cantidad de dinero en tu bolsillo sin agotar tus recursos antes de tiempo?

Aquí es donde entra la **Programación Lineal**, una herramienta analítica que transforma dilemas logísticos en soluciones estratégicas. A continuación, desglosaremos "El Problema del Carpintero" para demostrar cómo un modelo matemático simple puede ser la diferencia entre un negocio que sobrevive y uno que maximiza su éxito.

## Ejemplo práctico

Imagina que tienes un taller donde fabricas dos productos estrella: **Escritorios Ejecutivos** y **Estanterías Minimalistas**.

Para fabricar estos muebles, dependes de dos cosas: Madera de Roble y Horas de Carpintería; pero solo tienes 120 metros cuadrados de madera disponibles esta semana y tu carpintero solo puede trabajar 40 horas por semana. Para fabricar cada mueble necesitas:

1. Escritorio: $3m^2$ de madera y 2 horas de trabajo.
2. Estanteria: $4m^2$ de madera y 1 hora de trabajo.

Ademas, cada escritorio te deja una ganancia neta de $900 mxn$, mientras que la estanteria te deja $300 mxn$.

Tu contrato con una tienda local dice que debes entregar al menos $5$ estanterías por semana y obviamente quieres maximizar tu ganancia durante la semana.
Entonces, como le hacemos?

### Planteamiento del problema

Para solucionar este problema, primero necesitamos definir nuestro problema de una manera matematica, por lo que vamos a bautizar a nuestros muebles como:
$$x_1:= Escritorio,\hspace{1cm}x_2:= Estanteria$$

Ahora, dado que solo tenemos $120m^2$ de madera y para hacer cada tipo de mueble nos gastamos $3m^2$ y $4m^2$ respectivamente, entonces podemos escribirlo como:

$$\begin{equation}
3x_1+4x_2\leq 120
\end{equation}
$$

Esto es porque para un escritorio ($x_1$) necesitamos $3m^2$ y en conjunto de los dos tipos de muebles, podemos gastar a lo más, los $120m^2$ de madera que tenemos disponibles. Es muy importante esto de "a lo mas", pues no necesariamente tenemos que gastarnos toda la madera, pero nunca nos podemos pasar.

Para las horas, tenemos que

$$\begin{equation}
2x_1+1x_2\leq 40
\end{equation}
$$

Tenemos tambien que, el contrato dictamina que minimo tenemos que entregar $5$ estanterias, por lo que podemos escribirlo como:

$$x_2\geq 5$$

Por ultimo, dado que queremos ganar el mayor monto posible de dinero, tenemos que:

$$\text{Max }900x_1+300x_2$$

Al juntar todo esto, tenemos lo siguiente, y se le conoce como forma estandar de un problema de Optimizacion Lineal:

$$\text{Max }900x_1+300x_2$$
$$s.a
\begin{cases} 
3x_1+4x_2\leq 120 \\\\
2x_1+1x_2\leq 40 \\\\
x_2\geq 5 \\\\
x_1, x_2 \geq 0 
\end{cases}
$$

Lo ultimo, dado que no podemos hacer $-8$ escritorios o cosas por el estilo.

### Resolucion del problema

Ahora que tenemos nuestro problema planteado, podemos resolverlo con el método más famoso, el **Método Simplex** en cualquiera de sus variantes. Pero dado que en este blog trataremos en medida de lo posible utilizar Python para demostrar que todo tipo de problema puede ser solucionado con algoritmia, haremos uso de la biblioteca llamada scipy con una funcion que tiene por nombre linprog, la cual nos sirve para resolver problemas de Programacion Lineal. El codigo de python para resolver este problema es el siguiente:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import linprog

# 1. Definir los coeficientes de la Función Objetivo
# Nota: SciPy solo minimiza, así que usamos valores negativos para maximizar.
c = [-900, -300]

# 2. Definir las restricciones de desigualdad
A = [[3, 4], [2, 1]]
b = [120, 40]

# 3. Definir límites para x (Escritorios) e y (Estanterías)
x_bounds = (0, None)
y_bounds = (5, None)

# 4. Resolver el problema
res = linprog(c, A_ub=A, b_ub=b, bounds=[x_bounds, y_bounds], method='highs')

# --- Resultados ---
print(f"Estado: {res.message}")
print(f"Cantidad de Escritorios (x): {round(res.x[0], 2)}")
print(f"Cantidad de Estanterías (y): {round(res.x[1], 2)}")
print(f"Utilidad Máxima: ${round(-res.fun, 2)} MXN")

# --- Visualización ---
x = np.linspace(0, 25, 400)
y1 = (120 - 3*x) / 4  
y2 = 40 - 2*x        

plt.figure(figsize=(10, 6))
plt.plot(x, y1, label=r'$3x + 4y \leq 120$ (Madera)', color='blue')
plt.plot(x, y2, label=r'$2x + y \leq 40$ (Tiempo)', color='red')
plt.axhline(5, color='green', linestyle='--', label=r'$y \geq 5$ (Contrato)')

plt.fill_between(x, 5, np.minimum(y1, y2), where=(np.minimum(y1, y2) > 5), color='gray', alpha=0.3)

plt.xlim(0, 25); plt.ylim(0, 35)
plt.xlabel('Escritorios (x)'); plt.ylabel('Estanterías (y)')
plt.title('Optimización de la Producción - Región Factible')
plt.legend(); plt.grid(True)
plt.show()
```

### Resuktados y conclusiones del problema

Al resolver el problema, obtenemos lo siguiente:

Se deben de fabricar solo $5$ Estanterias, el resto, se deben hacer escritorios, lo cual corresponde a $17.5$ piezas.

Y si, sabemos que no se puede fabricar $0.5$ escritorios, por lo que podriamos decir que se estarian haciendo $35$ escritorios de manera quincenal si no se modifican entre semanas los recursos que tenemos actualmente. Tambien se puede modificar el algoritmo para obtener soluciones meramente enteras, esto agregando y modificando la linea siguiente

```python
integrality = [1, 1] # 1:= Variables enteras, 0:= Variables continuas 
res = linprog(c, A_ub=A, b_ub=b, bounds=[x_bounds, y_bounds], integrality=integrality,
method='highs')
```

Con esto, hemos podido resolver este problema que al tener solo dos variables, resulta un poco sencillo de resolver, pero que con el mismo codigo podemos resolver problemas incluso más complejos sin importar el número de variables.