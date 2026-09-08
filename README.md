# Machine Learning / Data Science — Mateo Vega

Repositorio con los trabajos prácticos de la materia *Introducción a la Inteligencia Artificial* (IES 21), organizados por tipo de problema. Cada dataset usado en un modelo real está acompañado de su informe técnico correspondiente (mismo nombre base, sufijo `_set.csv` / `_informe.docx`).

## Estructura

```
clasificacion/
  credit_set.csv        + credit_informe.docx      → aprobación de crédito (binaria)
  obesidad_set.csv       + obesidad_informe.docx    → nivel de obesidad (7 clases)
  bank_set.csv            + bank_informe.docx        → suscripción a depósito bancario (binaria, desbalanceada)
  SP2_Ejercicio_por_Resolver.xlsx                    → ejercicio manual de práctica (no es dataset de entrenamiento)

regresion/
  maquina_set.csv        + maquina_informe.docx     → predicción de respuesta de una máquina industrial
  Consigna_TP_Regresion.docx

proyecto-integrador/
  abalone_set.csv        + abalone_informe.docx     → estimación de edad de abalones (CRISP-DM completo)
  Consigna_TP_Integrador.docx

exploracion-datos/
  datos_dispersion.xlsx                             → ejercicios de estadística descriptiva

_otros-no-ml/
  (contenido que no pertenece a este repo, ver nota abajo)
```

Los archivos `Orange_*.ows` son los flujos de trabajo de Orange Data Mining usados para la parte de la materia resuelta con esa herramienta; `Orange_final_*` es la versión entregada, `Orange_borrador_*` son versiones previas del proceso.

## Resumen de resultados

Todos los datasets fueron auditados antes de modelar (nulos, duplicados, fugas de datos) — el detalle de qué se corrigió está en cada informe. Resultados finales sobre conjunto de test, nunca visto durante el entrenamiento:

| Dataset | Problema | Mejor modelo | Métrica principal (test) |
|---|---|---|---|
| credit_set | Clasificación binaria | Random Forest (150 árb.) | Accuracy 85.5% / AUC 0.934 |
| obesidad_set | Clasificación multiclase (7) | Random Forest (200 árb.) | Accuracy 95.2% / F1 macro 0.951 |
| bank_set | Clasificación binaria (desbalanceada) | Regresión Logística balanceada | AUC 0.757 / Recall 0.611 |
| maquina_set | Regresión | Regresión Polinómica grado 2 | R² 98.6% / RMSE 1.15 |
| abalone_set | Regresión | Regresión Polinómica grado 2 | MAE 1.51 anillos / R² 0.564 |

## Nota sobre calidad de datos

Varios datasets originales tenían problemas que se corrigieron antes de modelar (documentados en detalle en `informe_datasets.md` y en cada informe individual):
- **abalone_set**: el archivo original tenía el 50% de las filas duplicadas exactas.
- **bank_set**: el archivo original traía filas de metadata pegadas que rompían la lectura, y la columna `duration` es una fuga de datos (se descartó del modelado).
- **credit_set**: tenía 37 valores faltantes reales sin marcar, imputados antes de entrenar.
- **obesidad_set**: tenía 24 filas duplicadas exactas.

Un archivo (`jjj.csv`) resultó ser un duplicado corrupto de `obesidad_set` y se descartó directamente.

## Reproducibilidad

Los informes con modelos entrenados (`credit_informe`, `obesidad_informe`, `bank_informe`) se generaron con Python + pandas + scikit-learn, usando partición estratificada 60/20/20 (train/validación/test) y semilla fija (`random_state=42`) para que los resultados sean reproducibles.
