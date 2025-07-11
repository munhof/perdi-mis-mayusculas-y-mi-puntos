# Perdí mis puntos y mis mayusculas: Restauración de capitalizacion y puntuación

En mi camino para dominar el Procesamiento del Lenguaje Natural (PLN), me he encontrado con el fascinante desafío de restaurar la puntuación y capitalización en textos. Este documento detalla mi enfoque para construir un modelo robusto que aborde este problema. Abordaremos desde la creación de una base de datos equilibrada hasta el desarrollo de una pipeline completa y la eventual presentación de un reporte de producción.

## 1. Base de Datos

Para entrenar un modelo efectivo, es crucial contar con una base de datos diversa y equilibrada. Mi estrategia se centra en la integración de múltiples fuentes de datos en español, provenientes principalmente de Hugging Face. Las bases de datos seleccionadas son:

- Wikimedia Foundation: Wikimedia Downloads

  Disponibilidad: https://dumps.wikimedia.org

   Descripción: Proporciona una vasta colección de textos que pueden ser una fuente rica para aprender patrones de lenguaje y estructuras gramaticales.

- Hythyt: Preguntas y Respuestas RAG

    Disponibilidad: https://huggingface.co/datasets/hythyt/preguntas-respuestas-RAG

    Descripción: Ideal para capturar el lenguaje coloquial y estructurado de preguntas y respuestas, lo que es vital para la restauración de la puntuación en diálogos.

- Fernandoefg: Cuentos en Español

    Disponibilidad: https://huggingface.co/datasets/Fernandoefg/cuentos_es

    Descripción: Ofrece narrativas extensas que pueden ayudar al modelo a entender el flujo de las oraciones y la colocación de puntos en un contexto más literario.

- JAlexanderSanchez: Preguntas

    Disponibilidad: https://huggingface.co/datasets/JAlexanderSanchez/Preguntas

    Descripción: Similar a la anterior, pero con un enfoque más directo en la estructura de las preguntas, lo que es fundamental para identificar los signos de interrogación.

- Google Research: PAWS-X (es)

    Disponibilidad: https://huggingface.co/datasets/google-research-datasets/paws-x/tree/main/es

    Descripción: Contiene pares de oraciones con paráfrasis, lo que es útil para el modelo al aprender a diferenciar entre oraciones con significado similar pero estructuras distintas.

- Facebook AI: MLQA (mlqa-translate-train.es)

    Disponibilidad: https://huggingface.co/datasets/facebook/mlqa

    Descripción: Un conjunto de datos multilingüe de preguntas y respuestas, la parte en español será crucial para la comprensión de contextos diversos.

- PlanTL-GOB-ES: SQuAD en Español (squad_es v2.0.0)

    Disponibilidad: https://huggingface.co/datasets/PlanTL-GOB-ES/squad_es

    Referencia: Carrino, C. P., Costa-jussà, M. R., & Fonollosa, J. A. R. Automatic Spanish Translation of the SQuAD Dataset for Multilingual Question Answering. arXiv:1912.05200v2, 2019.

    Descripción: Otro dataset de preguntas y respuestas que complementará los anteriores, ofreciendo una amplia gama de estructuras oracionales.

La combinación de estas fuentes busca proporcionar un corpus diverso que cubra distintos estilos, temáticas y complejidades lingüísticas, esencial para un modelo robusto.

## 2. Pipeline de Desarrollo

El proceso de construcción y evaluación del modelo se estructurará en una pipeline clara y reproducible.

1. Preparación de Datos:

   a. Extracción y normalización de texto de todas las fuentes.

   b. Limpieza: eliminación de caracteres especiales no deseados, URLs, etc.

   c. Generación de pares (texto sin formato, texto original con formato) para el entrenamiento.

   d. Consideración de estrategias para el manejo de casos especiales (e.g., números, abreviaturas).

2. Separación de Datos:

   División de los datos preparados en conjuntos de entrenamiento, validación y prueba para asegurar una evaluación imparcial del modelo. Se buscará una distribución equilibrada para evitar sesgos.

3. Desarrollo del Modelo:

   a. Selección de Modelos: Investigación y evaluación de arquitecturas de modelos de PLN adecuadas para la tarea de secuenciación y clasificación (e.g., Transformers, RNNs con atención). Se considerarán modelos pre-entrenados y se evaluará la necesidad de fine-tuning.

   b. Entrenamiento del Mejor Modelo: Entrenamiento iterativo del modelo seleccionado utilizando el conjunto de entrenamiento, con monitoreo del rendimiento en el conjunto de validación.

   c. Evaluación del Mejor Modelo: Métricas específicas para la restauración de puntuación (e.g., F1-score para cada tipo de puntuación, accuracy en capitalización de inicio de oración) serán utilizadas.

4. Fase de Prueba (Test):

   Evaluación final del modelo en el conjunto de prueba, un conjunto de datos completamente nuevo para el modelo, para obtener una estimación realista de su rendimiento en producción.

5. Reporte de Producción y Exportación del Modelo:

  a. Documentación detallada de la arquitectura, hiperparámetros, resultados de evaluación y cualquier limitación identificada.

  b. Exportación del modelo entrenado en un formato que facilite su despliegue y uso en entornos de producción.

## 3. Ecosistema Tecnológico

Para la implementación de esta pipeline, se emplearán las siguientes herramientas y librerías:

- Pasos 1, 2, 3.a (Preparación, Separación, Selección de Modelos):
  Python: Como lenguaje principal de programación.

  Pandas/Numpy: Para la manipulación y análisis eficiente de datos.

  Hugging Face datasets library: Para cargar y gestionar los datasets de manera sencilla.

  Scikit-learn: Para utilidades de preprocesamiento y división de datos.

  transformers library (Hugging Face): Para explorar y cargar modelos pre-entrenados, así como para tokenización.

- Pasos 3.b, 3.c, 4, 5 (Entrenamiento, Evaluación, Test, Reporte y Exportación):

  PyTorch / TensorFlow: Frameworks de deep learning para construir y entrenar modelos. Se elegirá uno basado en la flexibilidad y los recursos disponibles.

  transformers library (Hugging Face): Para fine-tuning de modelos y acceso a utilidades de entrenamiento.

  TensorBoard / Weights & Biases: Para el seguimiento y visualización de métricas de entrenamiento.

  MLflow / DVC: Para la gestión de experimentos y versionado de modelos.

  Herramientas de reporte: Posiblemente Jupyter Notebooks para la presentación interactiva de resultados o librerías como ReportLab para la generación de PDFs.

4. Reporte

El reporte final servirá como una documentación exhaustiva del proyecto. Incluirá:

- Introducción: Contexto del problema y objetivos del proyecto.

- Revisión de la Literatura: Breve descripción de enfoques existentes para la restauración de puntuación y capitalización.

- Recolección y Preparación de Datos: Detalles sobre las fuentes de datos, los pasos de preprocesamiento y las estadísticas del dataset final.

- Arquitectura del Modelo: Descripción detallada del modelo seleccionado, su justificación y cualquier modificación realizada.

- Estrategia de Entrenamiento: Hiperparámetros, función de pérdida, optimizador y criterios de parada.

- Resultados y Análisis: Presentación de métricas de evaluación clave (precisión, recall, F1-score, accuracy) en los conjuntos de validación y prueba. Análisis de errores comunes.

- Limitaciones y Trabajos Futuros: Discusión de las limitaciones del modelo actual y sugerencias para mejoras futuras.

- Conclusiones: Resumen de los hallazgos y el impacto del modelo.

Anexos: Código fuente, configuraciones y cualquier material adicional relevante.
