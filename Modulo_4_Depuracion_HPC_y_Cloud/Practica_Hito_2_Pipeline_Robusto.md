[⬅️ Test Autoevaluación M4](Test_Autoevaluacion_M4.md) | [🏠 Temario Principal](../README.md) | [🎓 Proyecto Final ➡️](../Modulo_5_Proyecto_Final_de_Graduacion/Proyecto_Final_Guia.md)

---

# 🎯 Práctica Evaluativa - Hito 2: Pipeline Modular Robusto y Contenerizado con Auto-Recuperación

¡Enhorabuena por alcanzar el segundo gran hito! Aquí demostrarás tu solvencia técnica desarrollando un pipeline modularizado según los estándares de **nf-core y Seqera**, preparado para resistir fallos y contenerizado con Docker.

---

## 🎯 Objetivo de la Práctica
Construir un pipeline modular en Nextflow DSL2 con estructura de carpetas profesional que:
1. Esté separado en **módulos independientes** dentro de una carpeta `modules/`.
2. Utilice un archivo **`nextflow.config`** con perfiles de ejecución (`standard` y `docker`) y etiquetas de recursos (`label`).
3. Implemente una **estrategia de reintentos dinámicos** (`errorStrategy = 'retry'` y escalado de memoria con `task.attempt`).
4. Ejecute al menos un proceso bioinformático real dentro de un contenedor **Docker** de BioContainers (o imagen oficial).
5. Genere automáticamente el informe de ejecución (`report.html`) y el cronograma de tareas (`timeline.html`).

---

## 📁 Estructura del Proyecto Requerida

Crea una carpeta para esta práctica con la siguiente arquitectura:

```
practica_hito2/
├── nextflow.config
├── main.nf
└── modules/
    ├── qc_lecturas.nf
    └── agregador_metricas.nf
```

---

## 📋 Especificaciones Paso a Paso

### 1. `nextflow.config`
- Define un bloque `params` con:
  - `params.muestras = "data/*.txt"`
  - `params.outdir   = "resultados_hito2"`
- Define un bloque `process`:
  - `errorStrategy = { task.exitStatus in [137, 143, 1] ? 'retry' : 'finish' }`
  - `maxRetries = 2`
  - Con etiqueta `label 'ligero'` (cpus: 1, memory: `{ 1.GB * task.attempt }`)
- Define los perfiles:
  - `standard` (ejecución local sin Docker).
  - `docker` (`docker.enabled = true`, `docker.runOptions = '-u $(id -u):$(id -g)'`).
- Configura la generación automática de `report` y `timeline` dentro de `${params.outdir}/pipeline_info/`.

### 2. Módulo `modules/qc_lecturas.nf`
- Proceso `QC_LECTURAS`:
  - Recibe: `tuple val(meta), path(reads)`
  - Utiliza contenedor Docker oficial (por ejemplo, `quay.io/biocontainers/fastqc:0.12.1--hdfd78af_0` o `python:3.11-slim`).
  - Simula un proceso que verifica que el archivo no esté vacío y calcula estadísticas básicas.
  - Publica los resultados en `${params.outdir}/qc_individual`.
  - Emite: `tuple val(meta), path("*.json"), emit: metricas`.

### 3. Módulo `modules/agregador_metricas.nf`
- Proceso `AGREGADOR_METRICAS`:
  - Recibe todos los archivos `.json` juntos mediante `.collect()`.
  - Genera un archivo final `resumen_ejecucion.html` combinando los resultados.
  - Publica en `${params.outdir}/resumen_final`.

### 4. `main.nf`
- Importa ambos módulos usando `include`.
- Define un canal de entrada con 3 muestras simuladas con metadatos:
  `[ [id: 'Control_1'], 'c1.txt' ]`, `[ [id: 'Tratado_1'], 't1.txt' ]`, `[ [id: 'Tratado_2'], 't2.txt' ]`.
- Orquesta la ejecución del pipeline y muestra la ruta final de los reportes.

---

## 🚀 Comando de Ejecución de Prueba
Lanza tu práctica con el perfil Docker:
```bash
nextflow run main.nf -profile docker
```

---

## 📦 Validación
Comprueba que tu pipeline `main.nf` genera la estructura de carpetas esperada, ejecuta los procesos dentro del contenedor Docker y produce los reportes HTML (`report.html` y `timeline.html`) en la carpeta `resultados_hito2/pipeline_info/`.

---

| [⬅️ Test Autoevaluación M4](Test_Autoevaluacion_M4.md) | 🏠 [Temario Principal](../README.md) | [🎓 Proyecto Final ➡️](../Modulo_5_Proyecto_Final_de_Graduacion/Proyecto_Final_Guia.md) |
| :--- | :---: | :---: |
