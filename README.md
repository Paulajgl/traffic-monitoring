# Sistema de Monitoreo de Tráfico en Tiempo Real y Análisis de Riesgo Vial

Proyecto Grupo 2 -  2026

## Descripción
Sistema de monitoreo de accidentes de tráfico mediante Apache Kafka,
simulando eventos en tiempo real a partir del dataset US Accidents.

El sistema busca detectar aumentos repentinos de accidentes de alto
impacto vial mediante ventanas temporales y generar alertas.

## Dataset
US Accidents (2016-2023)
https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents

## Estructura del Proyecto

* `src/traffic/`: Código fuente en Java (`AccidentProducer`, `SpatialBurstDetector`, `TrafficRiskBatch`).
* `lib/`: Dependencias `.jar` necesarias para compilación y ejecución de Kafka/Spark.
* `scripts/`: Scripts `.sh` de apoyo para levantar tópicos, consumidores y jobs de Spark.
* `data/`: Muestras locales para pruebas (ignorado en Git).
* `results/`: Salidas y registros de alertas obtenidos en las ejecuciones.
* `build.xml`: Archivo de configuración para compilar el proyecto con Apache Ant (`ant`).

## Compilación y Ejecución

1. **Compilar:** `ant` (genera `dist/mdp-traffic.jar`).
2. **Consumidor Kafka:** `java -cp "dist/mdp-traffic.jar:lib/*" traffic.SpatialBurstDetector grupo2-traffic 3600 3`
3. **Productor Kafka:** `java -cp "dist/mdp-traffic.jar:lib/*" traffic.AccidentProducer data/sample_accidents.csv grupo2-traffic 10000`
4. **Spark Batch:** `spark-submit --master spark://cluster-01:7077 --class traffic.TrafficRiskBatch dist/mdp-traffic.jar <hdfs_input> <hdfs_output>`
