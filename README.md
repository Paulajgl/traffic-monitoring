# Sistema de Monitoreo de Tráfico en Tiempo Real y Análisis de Riesgo Vial

Proyecto Grupo 2 -  2026

## Descripción
Sistema de monitoreo de accidentes de tráfico mediante Apache Kafka,
simulando eventos en tiempo real a partir del dataset US Accidents.

El sistema busca detectar aumentos repentinos de accidentes de alto
impacto vial mediante ventanas temporales y generar alertas.

Adicionalmente, se considera utilizar HDFS y Apache Spark para
realizar análisis históricos de accidentes y condiciones climáticas.

## Dataset
US Accidents (2016-2023)
https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents

## Tecnologías
- Python
- Apache Kafka
- Apache Hadoop HDFS (propuesto)
- Apache Spark Core (propuesto)

## Estructura
- data/: muestras de datos
- producer/: productor de eventos Kafka
- consumer/: procesamiento de eventos y alertas
- spark/: análisis batch
- scripts/: preprocesamiento
- experiments/: experimentos de rendimiento
- tests/: pruebas del sistema
- results/: resultados obtenidos
- config/: configuraciones de ejemplo
