# Proyecto Final: Predicción de Fuga de Clientes
El operador de telecomunicaciones **Interconnect** busca reducir la cancelación de clientes (`Churn`) mediante un sistema que identifique con anticipación a quienes podrían darse de baja, para ofrecerles promociones y planes especiales.

**Objetivo:** Desarrollar y comparar modelos capaces de predecir si un cliente se dará de baja próximamente (Sí/No).
- El modelo se evaluará principalmente con AUC-ROC y con Recall como métrica adicional.

## Etapa 1: Plan de Trabajo
*Escribe aquí tu plan de trabajo inicial. Aborda brevemente:*
1. *¿Cómo planeas unir los datos y qué harás con los valores nulos generados?*
2. *¿Cuál será tu variable objetivo y qué tipo de problema de Machine Learning resolverás?*
3. *¿Qué pasos de preprocesamiento (codificación categórica, fechas, etc.) consideras necesarios y sobre que variables?*
4. *¿Qué modelos planeas entrenar?*

## Etapa 2: Código de Solución

### 2.1. Exploración de Datos (EDA)
*Carga de datos, análisis de distribuciones, identificación de anomalías.*

### 2.2. Preprocesamiento e Ingeniería de Características
*Procesar valores nulos, creación de la variable objetivo, codificación de variables categóricas (justifica tu elección de método).*
*Pista: Los modelos predictivos no entienden de fechas en formato texto. ¿Cómo puedes transformar las fechas de inicio y fin en una variable numérica útil para el modelo?*

### 2.3. Selección de Variables y Entrenamiento de Modelos (Baseline)
*Entrena al menos dos modelos distintos sin aplicar técnicas de balanceo de clases. Evalúa su AUC-ROC y Recall.*

### 2.4. Optimización y Manejo de Desbalance
*Aplica al menos una técnica para manejar el desbalance de clases (upsampling, downsampling, o ajuste de pesos) y busca los mejores hiperparámetros. Evalúa nuevamente.*

## Etapa 3: Informe de Solución
*Escribe aquí tu informe final para el equipo de negocio. Asegúrate de responder:*
1. *¿Qué modelo elegiste finalmente y por qué?*
2. *¿Cuáles fueron las métricas finales (AUC-ROC y Recall) en el conjunto de prueba?*
3. *En términos de negocio: ¿Qué significa tu valor de Recall? ¿Cómo impactaría tu modelo en la retención de clientes si el equipo de marketing lo utiliza hoy?*


## Criterios de éxito del modelo

Adicional a lo mencionado en la sección introductoria del proyecto, en cuanto al orden y justificación de las decisiones tomadas, a la empresa también le interesa evaluar tu modelo. 

Para ello el equipo de marketing lo evaluará basándose en dos métricas:

AUC-ROC (Métrica Principal): 
 Para que tu proyecto sea aprobado, tu modelo principal debe alcanzar un AUC-ROC de al menos 0.75 en el conjunto de prueba. Un AUC-ROC mayor a 0.88 se considerará sobresaliente.
AUC-ROC ≥ 0.88 — ⭐ Sobresaliente
0.87 ≤ AUC-ROC < 0.88 — Excelente
0.85 ≤ AUC-ROC < 0.87 — Muy bueno
0.81 ≤ AUC-ROC < 0.85 — Bueno
0.75 ≤ AUC-ROC < 0.81 — Aprobado
AUC-ROC < 0.75 — No aprobado
Recall (Métrica Secundaria): 
 Observa de cerca esta métrica. En nuestro contexto, un falso negativo (un cliente que se va, pero tu modelo dijo que se quedaba) significa una pérdida de ingresos para la empresa porque no le ofrecimos la promoción.
¡Mucho éxito!



## Contexto
El operador de telecomunicaciones Interconnect se enfrenta a un desafío crítico: una tasa de cancelación de clientes (Churn) cada vez mayor. El equipo de marketing sabe que retener a un cliente actual es mucho más barato que adquirir uno nuevo. Por ello, quieren implementar un sistema proactivo: si descubrimos a tiempo que un usuario planea irse, se le ofrecerán códigos promocionales y opciones de planes especiales para retenerlo.

El problema es que actualmente no saben a quién ofrecerle estas promociones. Aquí es donde entras tú como Científico de Datos.

Tu objetivo es desarrollar un modelo predictivo que responda a la siguiente pregunta: ¿Este cliente se dará de baja pronto (Sí o No)?

Para desarrollar tu modelo, el equipo de ingeniería de datos ha recopilado el siguiente historial sobre los clientes de la compañía. 

## Descripción de los Datos
Los datos están divididos en cuatro archivos:

contract.csv: Información del contrato (tipo de facturación, método de pago, fechas de inicio y fin).
personal.csv: Datos demográficos del cliente.
internet.csv: Información sobre los servicios de Internet contratados (fibra óptica, DSL, antivirus, etc.).
phone.csv: Información sobre los servicios telefónicos (líneas múltiples).
Par acceder a los datos dentro de la plataforma, usa la ruta  /datasets/final_provider/.

También puedes descargarlos: final_provider

## Consideraciones
Interconnect ofrece servicios separados. Algunos clientes solo tienen teléfono, otros solo internet, y otros ambos. Cuando unas las tablas, es normal y esperado que se generen valores nulos (NaN) en los servicios que un cliente no contrató. Piensa cómo debes rellenar esos valores nulos para que el modelo entienda que significan la "ausencia de un servicio" y no un error en los datos.

Por otro lado, no encontrarás una columna explícita que se llame "Fuga" o "Churn". Deberás inferir el estado actual del cliente analizando la fecha en la que finalizó su contrato (EndDate).

Si un contrato tiene una fecha de finalización concreta, sabemos qué ocurrió con ese cliente.
Si en lugar de una fecha dice "No", significa que la historia del cliente con nosotros aún continúa.
Deberás usar esta lógica para crear tu variable objetivo.