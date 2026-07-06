# TFM: Machine Learning para MS-CFLP-CI

Trabajo de Fin de Máster que explora la hibridación de metaheurísticas clásicas
con componentes de Machine Learning para el **Multi-Site Capacitated Facility
Location Problem with Incompatibility Constraints (MS-CFLP-CI)**.

## El problema

Dado un conjunto de almacenes candidatos y tiendas, hay que decidir:

1. Qué almacenes abrir (pagando un coste fijo por cada uno).
2. A qué almacén asignar cada tienda (pagando coste de suministro × demanda).

Sujeto a:

- La suma de demandas asignadas a cada almacén no puede superar su capacidad.
- Algunos pares de tiendas son incompatibles entre sí (no pueden compartir almacén).

## Contenido

Todo el desarrollo —lectura de instancias, algoritmos y experimentos— está en
[ml_metaheuristics.ipynb](ml_metaheuristics.ipynb). El notebook implementa
cuatro familias de metaheurísticas, cada una en su versión base y con un
componente de ML que adapta uno de sus hiperparámetros en tiempo de
ejecución:

| Familia | Base | + ML |
|---|---|---|
| GRASP | alfa fijo | selector de alfa por regresión polinómica |
| GVNS (VNS general) | orden fijo de vecindarios | selección adaptativa de vecindarios con Q-learning |
| Enfriamiento simulado (SA) | esquema de movimientos fijo | control adaptativo de temperatura (UCB1) |
| ALNS | selección uniforme de operadores | selección adaptativa de operadores destrucción/reparación |

La búsqueda local comparte tres movimientos (reasignar cliente, cerrar
instalación, abrir instalación) usados por GRASP, GVNS y SA.

La sección final del notebook ejecuta las ocho variantes sobre las instancias
`wlp01`–`wlp08`, con 10 semillas cada una y presupuesto de tiempo compartido
entre familias, comparando el gap frente a los óptimos/mejores valores
conocidos (CPLEX/Gurobi).

## Instancias

Las instancias del problema están en
[Instaces_MS-CFLP-CI/Public/](Instaces_MS-CFLP-CI/Public/), en formato
MiniZinc (`.dzn`), de `wlp01` (más pequeña) a `wlp20` (más grande, en `.zip`).

## Requisitos

- Python 3.10+
- `numpy`
- `scikit-learn`
- `scipy`
- `matplotlib`

```bash
pip install numpy scikit-learn scipy matplotlib
```

## Uso

Abrir y ejecutar [ml_metaheuristics.ipynb](ml_metaheuristics.ipynb) con Jupyter
en el directorio raíz del repositorio (las rutas a las instancias son
relativas a `Instaces_MS-CFLP-CI/Public/`). Algunas celdas de comparación y el
benchmark final están controladas por flags booleanos (p. ej.
`_EJECUTAR_BENCHMARK_FINAL`) para evitar reejecutar experimentos lentos por
accidente.
