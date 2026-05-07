# telecom-analysis
# Análisis de Clientes y Uso – ConnectaTel

## Objetivo del proyecto

Este proyecto analiza el comportamiento de los clientes de ConnectaTel a partir de registros de usuarios (`users`) y de uso (`usage`), con el objetivo de:
- detectar problemas de calidad de datos (nulos, sentinels, fechas imposibles),
- explorar patrones de uso de llamadas y mensajes,
- segmentar clientes por nivel de uso y por grupo de edad,
- generar insights accionables para el negocio sobre planes y oportunidades comerciales.

## Datasets utilizados

- `users`: información de clientes (id de usuario, nombre, edad, ciudad, plan, fecha de registro, churn, etc.).
- `usage`: registros de uso histórico (id, usuario, tipo de uso `text` o `call`, fecha, duración, longitud, etc.).

Ambos datasets se combinan para construir un perfil agregado por usuario (`user_profile`) con métricas de uso (cantidad de mensajes, llamadas y minutos de llamada).

## Principales análisis realizados

1. **Calidad de datos**
   - Identificación y tratamiento de valores sentinels: `age = -999` y `city = "?"`, convertidos a nulos y corregidos con reglas de imputación o recodificación.
   - Revisión de fechas fuera de rango en `reg_date` y marcado como nulas de las fechas posteriores a 2024.
   - Análisis de valores nulos en `duration` y `length`, concluyendo que muchos corresponden a casos “no aplica” según el tipo de evento (`text` vs `call`).

2. **Agregación de uso por usuario**
   - Construcción de la tabla `user_profile` con:
     - `cant_mensajes`: total de mensajes de texto.
     - `cant_llamadas`: total de llamadas.
     - `cant_minutos_llamada`: minutos totales en llamadas.

3. **Exploración estadística**
   - Resúmenes estadísticos de variables numéricas (edad, mensajes, llamadas, minutos).
   - Distribuciones por tipo de plan (Básico vs Premium) y revisión de valores atípicos mediante histogramas y boxplots.

4. **Detección de outliers**
   - Uso del método IQR para identificar outliers en `cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada`.
   - Decisión de mantener los outliers como “heavy users” reales, aplicando si es necesario transformaciones o métodos robustos en etapas posteriores de modelado.

5. **Segmentación de clientes**
   - Segmentación por **nivel de uso**:
     - `Bajo uso`: llamadas < 5 y mensajes < 5.
     - `Uso medio`: llamadas < 10 y mensajes < 10 (sin cumplir la condición de bajo uso).
     - `Alto uso`: resto de los casos.
   - Segmentación por **edad**:
     - `Joven`: edad < 30.
     - `Adulto`: 30 <= edad < 60.
     - `Adulto Mayor`: edad >= 60.
   - Visualización de la distribución de clientes por grupo de uso y grupo de edad.

6. **Insight ejecutivo**
   - Identificación de problemas de datos y su impacto.
   - Descripción de los segmentos de clientes más relevantes por edad y nivel de uso.
   - Discusión de patrones de uso extremo (outliers) y qué implican para el negocio.
   - Recomendaciones para ajustar planes de bajo uso, reforzar ofertas para altos usuarios y adaptar comunicaciones según grupos de edad.

## Cómo ejecutar el notebook

1. Abre el enlace al notebook en GitHub y haz clic en el botón **Open in Colab** (si está configurado), o bien:
2. Descarga el archivo `.ipynb` y súbelo manualmente a Google Colab.
3. Asegúrate de que los archivos de datos (`users`, `usage`) estén accesibles (por ejemplo, en la misma ruta que el notebook o en Google Drive).
4. Ejecuta las celdas en orden desde el inicio; el notebook está organizado en pasos:
   - limpieza y tratamiento de nulos/sentinels,
   - agregación de uso,
   - análisis exploratorio y visualización,
   - segmentación de clientes,
   - generación del insight ejecutivo.

## Reproducibilidad

- El análisis es determinista dado el mismo conjunto de datos.
- Todas las transformaciones (limpieza, agregación, segmentación) se realizan dentro del notebook, sin pasos manuales externos.
- Cualquier persona con acceso a los datos puede ejecutar el notebook en Colab y reproducir los resultados y gráficos descritos en este repositorio.
