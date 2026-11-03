# <T16> · <¿Este usuario termina la serie?>

> **Hackatón IIA · UADE · 3.4.257.** Este README es la plantilla del grupo. En su fork, reemplacen todo lo que está entre `< >` y borren esta nota. Las bases están en `BASES.md` y su ficha en `problematicas/`.

**Integrantes:** Sofía González Palacios · Franco Mari Sueiro
**IA declarada:** Consumo de series y peliculas en platafromas de streaming
**Pista:** Reglas 
**Publicación en LinkedIn:** <link>

---

## 1. Formulación y PEAS

**¿El usuario terminará la temporada de la serie?** SÍ = termina, NO = abandona

| PEAS | Su sistema |
|---|---|
| Medida de rendimiento | Maximizar la precisión en la recomendación de la temporada 2, minimizando los errores costosos (falsos positivos y falsos negativos). |
| Entorno | Usuarios de la plataforma de streaming analizados al finalizar su primera semana de visualización. |
| Actuadores | Decisión del sistema de recomendar o no la temporada siguiente en la interfaz del usuario. |
| Sensores | Datos de comportamiento de la primera semana: episodios vistos, duración por episodio, compañía de visualización y si vio el anteúltimo episodio. |

**Objetivo y métrica.** Lo que de verdad importa es evitar recomendar la temporada siguiente a usuarios que van a abandonar (Falsos Positivos), ya que genera fricción comercial y frustración. La métrica clave es la **Precisión** junto con la **Sensibilidad**, evaluadas estrictamente sobre los datos de prueba.

## 2. El sistema

**Cómo decide.** Sistema de reglas lógicas si-entonces:
- **Regla 1:** Si `Vio el anteúltimo episodio == 1` Y `Episodios vistos >= 6`, entonces **SÍ**.
- **Regla 2:** Si `Episodios vistos >= 7`, entonces **SÍ**.
- **Regla 3:** En cualquier otro caso, responde **NO**.

**Variables que usa y por qué.** Usamos principalmente "Episodios vistos en la primera semana" y "Vio el anteúltimo episodio" porque reflejan directamente el compromiso real con la trama. Se descartó la duración por episodio y si lo vio con alguien más porque no muestran una correlación determinante en los casos de entrenamiento.

**Cómo se corre en vivo.** Se aplica de forma directa evaluando los valores de los 4 campos del caso nuevo contra las reglas lógicas descritas arriba.

**Dos casos de ejemplo.**

| Caso inventado | Valores | Salida del sistema |
|---|---|---|
| A | 8 episodios, 45 min, solo, vio anteúltimo (1) | SÍ |
| B | 2 episodios, 50 min, acompañado, no vio anteúltimo (0) | NO |

## 3. Medición

**Entrenamiento (casos 1–14)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = 6 | FN = 0 |
| **Real NO** | FP = 0 | VN = 8 |

**Prueba (casos 15–20)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = 3 | FN = 0 |
| **Real NO** | FP = 0 | VN = 3 |

| Métrica | Entrenamiento | Prueba |
|---|---|---|
| Exactitud | 100% | 100% |
| Precisión | 100% | 100% |
| Sensibilidad | 100% | 100% |
| Especificidad | 100% | 100% |

**Entrenamiento contra prueba.** No hay diferencia en el rendimiento: ambos obtienen un 100% de exactitud, lo que demuestra una excelente generalización sin sobreajuste gracias a la simplicidad de las reglas.

## 4. El error más caro

En este problema, el error más caro es el **Falso Positivo** (decirle SÍ a alguien que iba a abandonar y recomendarle la temporada 2). Para mitigar este riesgo, las reglas exigen un umbral alto de compromiso (`Episodios vistos >= 6 o 7` y validación del anteúltimo episodio) antes de emitir un veredicto afirmativo.

## 5. ¿Comparado contra qué?

**Línea base trivial.** Un modelo que responda siempre la clase mayoritaria tendría una exactitud aproximada del 50%. Nuestro sistema basado en reglas alcanza un 100%, superando ampliamente la línea base.

| Checklist (Clase 6) | Respuesta |
|---|---|
| ¿Qué datos? | Historial de consumo de la primera semana de streaming |
| ¿Qué métrica? | Exactitud, Precisión y Sensibilidad |
| ¿Comparado contra qué? | Línea base trivial de predicción constante |
| ¿Quién lo midió? | El equipo del grupo T16 mediante validación cruzada sobre datos reales |

## 6. Riesgo: el camino completo

**Datos:** Registros de visualización de la primera semana $\rightarrow$ **Modelo:** Sistema de reglas si-entonces $\rightarrow$ **Decisión:** Automatizar la recomendación de la temporada 2 $\rightarrow$ **Consecuencia:** Retención exitosa del usuario o, en caso de fallo, molestia por contenido irrelevante y posible abandono de la plataforma.

## 7. Bitácora

**Uso de la IA**

| # | Qué le pedimos | Qué aceptamos | Qué rechazamos y por qué |
|---|---|---|---|
| 1 | <> | <> | <> |

**Corridas contra el evaluador**

| Corrida | Nota | Agujero que encontró | Qué cambiamos |
|---|---|---|---|
| 1 | <> | <> | <> |
