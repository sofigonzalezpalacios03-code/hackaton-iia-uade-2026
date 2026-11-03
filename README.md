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

**Cómo se corre en vivo.** <Pasos concretos para resolver un caso nuevo: link al Colab, a la planilla o la cuenta a mano.>

**Dos casos de ejemplo.**

| Caso inventado | Valores | Salida del sistema |
|---|---|---|
| A | <…> | <SÍ/NO> |
| B | <…> | <SÍ/NO> |

## 3. Medición

**Entrenamiento (casos 1–14)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = <> | FN = <> |
| **Real NO** | FP = <> | VN = <> |

**Prueba (casos 15–20)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = <> | FN = <> |
| **Real NO** | FP = <> | VN = <> |

| Métrica | Entrenamiento | Prueba |
|---|---|---|
| Exactitud | <> | <> |
| Precisión | <> | <> |
| Sensibilidad | <> | <> |
| Especificidad | <> | <> |

**Entrenamiento contra prueba.** <¿Hay diferencia? ¿Por qué? ¿Sobreajuste o generalización?>

## 4. El error más caro

<¿Es peor el falso positivo o el falso negativo en su problema? ¿Por qué, según la ficha? ¿Qué cambiaron en el sistema para cometer menos ese error?>

## 5. ¿Comparado contra qué?

**Línea base trivial.** <Qué resultado saca un sistema que responde siempre lo mismo, y cómo se compara el suyo.>

| Checklist (Clase 6) | Respuesta |
|---|---|
| ¿Qué datos? | <> |
| ¿Qué métrica? | <> |
| ¿Comparado contra qué? | <> |
| ¿Quién lo midió? | <> |

## 6. Riesgo: el camino completo

<Datos → modelo → decisión → consecuencia aplicado a su caso. Al menos un riesgo concreto.>

## 7. Bitácora

**Uso de la IA**

| # | Qué le pedimos | Qué aceptamos | Qué rechazamos y por qué |
|---|---|---|---|
| 1 | <> | <> | <> |

**Corridas contra el evaluador**

| Corrida | Nota | Agujero que encontró | Qué cambiamos |
|---|---|---|---|
| 1 | <> | <> | <> |
