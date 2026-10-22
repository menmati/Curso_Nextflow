# 🧬 Curso Completo de Nextflow DSL2 & Pipelines Bioinformáticos
### *De Cero a Nivel Profesional para la Industria Biotecnológica*

Bienvenido/a a este curso abierto, intensivo y estructurado sobre **Nextflow DSL2** y diseño de pipelines bioinformáticos reproducibles. 

Este repositorio está organizado como un curso interactivo de **múltiples páginas**, guiándote paso a paso desde los fundamentos de la programación reactiva basada en datos (*Dataflow*) hasta la orquestación a gran escala en clústeres HPC y la nube.

---

## 🗺️ Mapa de Ruta del Curso

```mermaid
flowchart LR
    M1["Módulo 1: Fundamentos"] --> M2["Módulo 2: Operadores Avanzados"]
    M2 --> M3["Módulo 3: Modularidad y Docker"]
    M3 --> M4["Módulo 4: HPC y Cloud"]
    M4 --> M5["Módulo 5: Proyecto Final"]
```

---

## 📚 Índice de Contenidos

### 🔹 Módulo 1: Fundamentos de Nextflow DSL2
Domina las bases de la ejecución reactiva, la estructura de un pipeline, la anatomía de los procesos y la creación de canales.

| # | Documento / Lección | Temas Principales |
| :-: | :--- | :--- |
| **01** | [📖 Lección 1: Introducción, Filosofía y Estructura](Modulo_1_Fundamentos/Leccion_01_Introduccion_y_Filosofia.md) | Paradigma Dataflow, reactividad, los 3 pilares, primer pipeline `main.nf`. |
| **02** | [📖 Lección 2: Procesos, Entradas, Salidas y Directivas](Modulo_1_Fundamentos/Leccion_02_Procesos_y_Directivas.md) | Bloques `input:`, `output:`, `script:`, calificadores (`val`, `path`, `tuple`), directivas (`publishDir`, `cpus`, `memory`). |
| **03** | [📖 Lección 3: Canales (Channels) Fundamentales](Modulo_1_Fundamentos/Leccion_03_Canales_Fundamentales.md) | Queue channels vs Value channels, factorías (`Channel.of`, `fromPath`, `fromFilePairs`), operador `.view()`. |
| **🏋️** | [🏋️ Ejercicios Prácticos del Módulo 1](Modulo_1_Fundamentos/Ejercicios_M1.md) | Conteo de líneas por muestra, scripts integrados en Python para %GC, lecturas paired-end. |
| **📝** | [📝 Test de Autoevaluación del Módulo 1](Modulo_1_Fundamentos/Test_Autoevaluacion_M1.md) | 10 preguntas de opción múltiple para validar los conceptos clave. |

---

### 🔹 Módulo 2: Operadores, Metadatos y Canales Avanzados
Manipula flujos de datos complejos, domina el patrón universal `[meta, reads]` de nf-core y construye tu primer pipeline integrado.

| # | Documento / Lección | Temas Principales |
| :-: | :--- | :--- |
| **04** | [📖 Lección 4: Operadores de Transformación](Modulo_2_Operadores_y_Canales_Avanzados/Leccion_04_Operadores_Transformacion.md) | Transformación 1 a 1 (`.map`), filtrado condicional (`.filter`), aplanado (`.flatten`) y agregación global (`.collect`). |
| **05** | [📖 Lección 5: Combinación y Tuplas Meta de nf-core](Modulo_2_Operadores_y_Canales_Avanzados/Leccion_05_Combinacion_y_Tuplas_Meta.md) | El patrón estándar `tuple val(meta), path(reads)`, cruce por clave con `.join`, producto cartesiano con `.combine` y lectura de `samplesheet.csv` con `.splitCsv`. |
| **06** | [📖 Lección 6: Agrupación y Ramificación](Modulo_2_Operadores_y_Canales_Avanzados/Leccion_06_Agrupacion_y_Ramificacion.md) | Agrupación de réplicas técnicas con `.groupTuple`, bifurcación condicional de canales con `.branch` y partición múltiple con `.multiMap`. |
| **🏋️** | [🏋️ Ejercicios Prácticos del Módulo 2](Modulo_2_Operadores_y_Canales_Avanzados/Ejercicios_M2.md) | Limpieza de datos clínicos, reasociación de réplicas en secuenciación y bifurcación Single-End vs Paired-End. |
| **📝** | [📝 Test de Autoevaluación del Módulo 2](Modulo_2_Operadores_y_Canales_Avanzados/Test_Autoevaluacion_M2.md) | 10 preguntas conceptuales y de arquitectura sobre operadores avanzados. |
| **🎯** | [🎯 HITO 1: Mini-Pipeline QC FASTQ](Modulo_2_Operadores_y_Canales_Avanzados/Practica_Hito_1_MiniPipeline_QC.md) | Práctica integradora: pipeline completo de análisis individual en paralelo y consolidación global en reporte HTML interactivo. |

---

### 🔹 Módulo 3: Modularidad, Configuración y Contenedores
Diseña pipelines mantenibles y reproducibles: arquitectura de módulos DSL2, importaciones con alias, desacoplamiento de recursos en `nextflow.config` y ejecución aislada con Docker y BioContainers.

| # | Documento / Lección | Temas Principales |
| :-: | :--- | :--- |
| **07** | [📖 Lección 7: Modularización en DSL2 y Subworkflows](Modulo_3_Modularidad_Config_y_Contenedores/Leccion_07_Modularizacion_DSL2.md) | Módulos atómicos, directiva `include`, alias con `as` para múltiples instancias y arquitectura de subworkflows (`take`, `main`, `emit`). |
| **08** | [📖 Lección 8: Configuración, Perfiles y Parámetros](Modulo_3_Modularidad_Config_y_Contenedores/Leccion_08_Nextflow_Config_y_Params.md) | Jerarquía de configuración, scopes (`params`, `process`), directivas dinámicas por etiquetas (`withLabel`) y perfiles de ejecución (`-profile`). |
| **09** | [📖 Lección 9: Contenedores con Docker y Singularity](Modulo_3_Modularidad_Config_y_Contenedores/Leccion_09_Contenedores_Docker.md) | Reproducibilidad nativa, directiva `container`, ecosistema BioContainers (Quay.io), mapeo UID:GID y Docker vs Singularity en entornos HPC. |
| **🏋️** | [🏋️ Ejercicios Prácticos del Módulo 3](Modulo_3_Modularidad_Config_y_Contenedores/Ejercicios_M3.md) | Creación de módulos reutilizables, configuración elástica con perfiles y ejecución bioinformática real con Samtools en Docker. |
| **📝** | [📝 Test de Autoevaluación del Módulo 3](Modulo_3_Modularidad_Config_y_Contenedores/Test_Autoevaluacion_M3.md) | 10 preguntas sobre modularidad DSL2, jerarquía de configuración y contenedores. |

---

### 🔹 Próximos Módulos *(En desarrollo progresivo)*
- **Módulo 4: Depuración, HPC y Cloud:** Mecanismo `-resume`, inspección del árbol `work/`, estrategias ante errores (`retry`, `ignore`), monitorización y Seqera Platform / AWS Batch.
- **Módulo 5: Proyecto Final de Graduación:** Pipeline NGS modular de control de calidad y alineamiento de extremo a extremo, listo para producción y cartera técnica.

---

## 🚀 Requisitos Previos e Instalación

Para poner en práctica los ejemplos y ejercicios de este curso, se recomienda disponer de:

1. **Linux, macOS o Windows con WSL2 (Ubuntu 22.04 LTS o superior).**
2. **Java 17 o superior:**
   ```bash
   java -version
   ```
3. **Nextflow (versión 23.04 o superior):**
   ```bash
   curl -s https://get.nextflow.io | bash
   sudo mv nextflow /usr/local/bin/
   nextflow info
   ```
4. **Motor de contenedores (opcional para Módulo 1, recomendado para módulos avanzados):** Docker o Singularity/Apptainer.

---

## 🎯 Metodología de Aprendizaje Recomendada

1. **Lectura activa e interactiva:** Sigue el orden secuencial de las lecciones utilizando la barra de navegación situada al principio y al final de cada página.
2. **Experimentación práctica:** Copia y ejecuta los fragmentos de código en tu propia terminal para observar cómo Nextflow aísla las tareas en directorios hash dentro de `work/`.
3. **Resolución de problemas:** Realiza los desafíos propuestos de cada módulo antes de consultar soluciones.
4. **Autoevaluación:** Responde a los tests conceptuales para afianzar la arquitectura interna de Nextflow.

---

## 📖 Recursos y Documentación Oficial

- [Documentación Oficial de Nextflow](https://www.nextflow.io/docs/latest/)
- [Seqera Training Platform](https://training.seqera.io/)
- [Guías de Buenas Prácticas de nf-core](https://nf-co.re/docs/guidelines/components/modules)
- [Nextflow Slack Community](https://www.nextflow.io/slack-invite.html)

---

## 👤 Autora / Mantenimiento

Repositorio creado y mantenido por [@menmati](https://github.com/menmati).
Contribuciones, mejoras y sugerencias son bienvenidas a través de Pull Requests e Issues.
