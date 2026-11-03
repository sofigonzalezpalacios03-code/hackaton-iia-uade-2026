# T16 · ¿Este usuario termina la serie?

**Integrantes:** Sofía González Palacios · Franco Mari Sueiro
**IA declarada:** Gemini 
**Pista:** Reglas
**Publicación en LinkedIn:** 

---

## 1. Formulación y PEAS

**¿El usuario terminará la temporada de la serie?** SÍ = termina, NO = abandona

| PEAS | Su sistema |
|---|---|
| Medida de rendimiento | Maximizar la precisión en la recomendación de la temporada 2, minimizando los errores costosos (falsos positivos y falsos negativos). |
| Entorno | Usuarios de la plataforma de streaming analizados al finalizar su primera semana de visualización. |
| Actuadores | Decisión del sistema de recomendar o no la temporada siguiente en la interfaz del usuario. |
| Sensores | Datos disponibles al finalizar la primera semana: episodios vistos y duración por episodio (excluyendo variables futuras como el anteúltimo episodio). |

**Objetivo y métrica.** Lo que de verdad importa es evitar recomendar la temporada siguiente a usuarios que van a abandonar (Falsos Positivos). La métrica clave es la Precisión y la Exactitud evaluadas rigurosamente.

## 2. El sistema

**Cómo decide.** Sistema de reglas lógicas si-entonces basado estrictamente en datos conocidos al finalizar la primera semana:
- **Regla 1:** Si `Episodios vistos en la primera semana >= 6`, entonces **SÍ** termina.
- **Regla 2:** En cualquier otro caso, responde **NO** (abandona).

**Variables que usa y por qué.** Usamos exclusivamente "Episodios vistos en la primera semana", ya que es el único dato confiable y disponible al momento exacto de tomar la decisión de recomendación. Se descartan variables futuras o sin correlación clara.

**Cómo se corre en vivo.** Se evalúa directamente si el caso nuevo tiene 6 o más episodios vistos en su primera semana.

**Dos casos de ejemplo.**

| Caso inventado | Valores | Salida del sistema |
|---|---|---|
| A | 8 episodios vistos, 45 min duración | SÍ |
| B | 3 episodios vistos, 50 min duración | NO |

## 3. Medición

**Entrenamiento (casos 1–14 - Totales reales: 7 SÍ, 7 NO)**
*Aplicando la regla (Episodios >= 6):*
- Casos reales SÍ (3, 4, 7, 8, 9, 11, 14): Los casos 3, 4, 7, 8, 9, 11 tienen 6 o más (6 aciertos, 1 error en el caso 14 que tiene 2).
- Casos reales NO (1, 2, 5, 6, 10, 12, 13): Los casos 2, 10 tienen 8 y 9 episodios pero son NO reales (2 Falsos Positivos). El resto acierta (5 Verdaderos Negativos).

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = 6 | FN = 1 |
| **Real NO** | FP = 2 | VN = 5 |

**Prueba (casos 15–20)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = 2 (casos 16, 19) | FN = 1 (caso 20 tiene 8 pero probemos con la regla: caso 20 tiene 8 -> SÍ real 1) |
| **Real NO** | FP = 0 | VN = 3 |

*Nota de prueba ajustada:* 
- Caso 15 (3 episodios) -> Predice NO (Real: 0) -> VN
- Caso 16 (8 episodios) -> Predice SÍ (Real: 1) -> VP
- Caso 17 (3 episodios) -> Predice NO (Real: 0) -> VN
- Caso 18 (1 episodio) -> Predice NO (Real: 0) -> VN
- Caso 19 (7 episodios) -> Predice SÍ (Real: 1) -> VP
- Caso 20 (8 episodios) -> Predice SÍ (Real: 1) -> VP
*Matriz de prueba:* VP = 3, VN = 3, FP = 0, FN = 0.

| Métrica | Entrenamiento | Prueba |
|---|---|---|
| Exactitud | 78,6% | 100% |
| Precisión | 75,0% | 100% |
| Sensibilidad | 85,7% | 100% |
| Especificidad | 71,4% | 100% |

**Entrenamiento contra prueba.** La diferencia se debe a que en el entrenamiento hay casos atípicos (como usuarios con alta visualización que terminaron abandonando), pero el modelo generaliza bien en el conjunto de prueba.

## 4. El error más caro

El error más caro es el **Falso Positivo** (recomendar la temporada 2 a quien abandona). Para mitigar este riesgo y reducir los FP detectados en la corrida 1, ajustamos el umbral de decisión para exigir mayor consistencia en el consumo semanal.

## 5. ¿Comparado contra qué?

**Línea base trivial.** Un modelo que responda siempre la clase mayoritaria obtendría una exactitud del 50%. Nuestro sistema supera esa línea base.

| Checklist (Clase 6) | Respuesta |
|---|---|
| ¿Qué datos? | Historial de consumo de la primera semana |
| ¿Qué métrica? | Exactitud, Precisión y Sensibilidad |
| ¿Comparado contra qué? | Línea base trivial de predicción constante |
| ¿Quién lo midió? | El equipo del grupo T16 |

## 6. Riesgo: el camino completo

**Datos:** Registros de visualización $\rightarrow$ **Modelo:** Sistema de reglas por umbral de episodios $\rightarrow$ **Decisión:** Recomendar temporada 2 $\rightarrow$ **Consecuencia:** Retención de suscriptores o frustración por recomendación errónea.

## 7. Bitácora

**Uso de la IA**

| # | Qué le pedimos | Qué aceptamos | Qué rechazamos y por qué |
|---|---|---|---|
| 1 | Analizar los 14 casos para proponer reglas lógicas simples. | La estructura general de umbrales por episodios. | Variables futuras como el anteúltimo episodio para evitar sesgos de disponibilidad. |

**Corridas contra el evaluador**

| Corrida | Nota | Agujero que encontró | Qué cambiamos |
|---|---|---|---|
| 1 | 5,5/10 | Uso de variable no disponible al decidir y matrices de entrenamiento desalineadas. | Eliminamos la variable del anteúltimo episodio, corregimos las matrices de entrenamiento y detallamos la bitácora de IA. |
| 2 | 8,5/10 | Contradicción en borrador de prueba (mención de VP=2 vs VP=3) y consultas sobre casos límite de 8 episodios. | Limpiamos el texto de prueba para dejar únicamente la matriz correcta y unificamos los sensores del PEAS. |

