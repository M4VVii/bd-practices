# Práctica 2: Modelo Entidad-Relación Extendido (EER) e Ingeniería Inversa

**Instituto Politécnico Nacional | Escuela Superior de Cómputo (ESCOM)**  
**Unidad de Aprendizaje:** Bases de Datos  
**Autor:** Valencia Villanueva Adrian

## 🎯 Objetivo de la Práctica
Evolucionar el diseño conceptual de la Práctica 1 aplicando los conceptos del **Modelo Entidad-Relación Extendido (EER)** bajo la notación de Peter Chen. Esta práctica demuestra el dominio sobre reglas de negocio complejas mediante la implementación de jerarquías de herencia (superclases/subclases), entidades débiles con dependencias de existencia, participación total y restricciones estructurales explícitas (min, max). Adicionalmente, aplicar técnicas de ingeniería inversa sobre un sistema en producción.

## 📁 Estructura del Repositorio

El directorio `P2/` está organizado conforme a la rúbrica institucional:

```text
📦 P2
 ┣ 📂 articulos/
 ┃ ┗ 📜 resumenes.pdf                      # Resúmenes analíticos de 3 artículos sobre BD
 ┣ 📂 exposicion/
 ┃ ┗ 📜 presentacion.pdf                   # Presentación para la defensa técnica de la práctica
 ┣ 📂 propuestas/
 ┃ ┗ 📜 propuestas-de-mejora.pdf           # Propuestas con integración de IA (Req. ESCOM)
 ┣ 📂 proyecto-asignado/
 ┃ ┣ 🖼️ evidencias                         # Capturas del arranque, de la aplicación y de la consulta.
 ┃ ┣ 📜 correspondencia-con-el-esquema.pdf # Auditoría, ingeniería inversa y hallazgos
 ┃ ┣ 🖼️ eer.webp                           # Diagrama conceptual inferido del sistema de sismos
 ┃ ┗ 📜 levantamiento.md                   # Bitácora de clonación, ejecución y consultas SQL
 ┣ 📂 proyecto-propio/
 ┃ ┣ 🖼️ eer-chen.webp                      # Diagrama EER final (Notación Chen)
 ┃ ┣ 🖼️ eer-crows-feet.webp                # Esquema relacional lógico (Crow's Feet)
 ┃ ┗ 📜 requisitos-ampliados.pdf           # Justificación de las nuevas reglas de negocio
 ┗ 📜 README.md
```

## Desglose de Entregables

### 1. Proyecto Propio (Sistema de Trazabilidad Machine Learning)
Se amplió el caso de estudio original para gestionar el ciclo de vida de los modelos de IA, integrando las siguientes características del Modelo EER:
*   **Jerarquía de Especialización (Disjunta):** La superclase `USUARIO` se dividió en los subtipos `ESTUDIANTE` y `DOCENTE`, heredando identificadores base pero manteniendo atributos exclusivos.
*   **Entidades Débiles:** Se modelaron `VERSION_MODELO` (dependiente de `MODELO_ML`) y `DESPLIEGUE` (dependiente de `VERSION_MODELO`) utilizando rectángulos dobles y claves parciales punteadas.
*   **Notación de Cardinalidad Exacta:** Implementación de métricas `(min, max)` en todas las conexiones conceptuales y líneas dobles para denotar la participación total obligatoria de las entidades débiles.

### 2. Proyecto Asignado (Caso de Estudio: Sismos)
Se aplicó ingeniería inversa al esquema relacional proporcionado, clonando el entorno y realizando pruebas directas sobre la base de datos PostgreSQL. Se generó una **tabla de correspondencia** detallada y se identificaron fallas sistémicas críticas en la extracción de datos (ETL) que provocan el descarte masivo de registros sísmicos al omitir los estados de la República Mexicana.

### 3. Investigación y Análisis
Lectura, análisis y síntesis de tres artículos académicos relacionados con el diseño e implementación de Bases de Datos.

### 4. Propuestas de Mejora
Elaboración de tres propuestas de mejora registradas como issues en este repositorio, incluyendo una orientada a la integración de modelos de Machine Learning (detección de anomalías) basándose en los datos sísmicos actuales.

## Datos Administrativos

*   **Proyecto Asignado:** Sistema de visualización de datos sísmicos de México.
*   **Dirección del Fork (Clonación):** [PEGA_AQUÍ_LA_URL_DE_TU_FORK_EN_GITHUB]
*   **Confirmación (Commit) puesta en funcionamiento:** `[PEGA_EL_HASH_DEL_COMMIT]`
*   **Issues de Propuestas de Mejora:**
    *   Propuesta 1: [ENLACE_AL_ISSUE_1]
    *   Propuesta 2: [ENLACE_AL_ISSUE_2]
    *   Propuesta 3 (Machine Learning): [ENLACE_AL_ISSUE_3]
*   **Integridad Académica (Uso de IA):** Se utilizó un asistente de inteligencia artificial (Gemini) estrictamente como apoyo para la revisión ortotipográfica, validación del formato Markdown y estructuración de los documentos de texto, manteniendo la autoría original de todo el análisis técnico, modelado de datos e ingeniería inversa.

**Repositorio Analizado:**
> [Seismic Data Visualization System for Mexico](https://github.com/gabrielhuav/DWSismos) (gabrielhuav, 2026).