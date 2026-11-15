# Sistema de Monitoreo de Tráfico en Tiempo Real y Análisis de Riesgo Vial

Proyecto Grupo 2 -  2026

## Descripción

Sistema híbrido de monitoreo y análisis masivo de accidentes de tráfico desarrollado sobre el dataset **US Accidents (2016–2023)** (~7,7 millones de registros), combinando procesamiento de flujos en tiempo real y análisis histórico distribuido:

* **Detección de alertas en tiempo real (Apache Kafka):** Un productor (`AccidentProducer`) simula la ingesta continua de reportes viales acelerando el tiempo y particionando los eventos por estado. En paralelo, un consumidor (`SpatialBurstDetector`) filtra incidentes de alta severidad ($\ge 3$), agrupa las coordenadas en celdas geográficas y utiliza colas con ventanas de tiempo deslizantes para detectar aumentos repentinos (*bursts*) y emitir alertas de emergencia.

* **Análisis histórico agregado (Apache Spark Core):** Sobre el conjunto de datos almacenado en HDFS, un trabajo distribuido basado en RDDs (`TrafficRiskBatch`) emplea transformaciones sobre `PairRDD` (`aggregateByKey` y `reduceByKey`) para calcular la severidad promedio según las condiciones climáticas e identificar puntos negros (*blackspots*) con mayor concentración histórica de accidentes.

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
