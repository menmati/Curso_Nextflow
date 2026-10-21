[⬅️ Test Autoevaluación M2](Test_Autoevaluacion_M2.md) | [🏠 Temario Principal](../README.md)

---

# 🎯 Práctica Evaluativa - Hito 1: Mini-Pipeline de Control de Calidad FASTQ

¡Enhorabuena por llegar al primer gran hito! Es momento de poner en práctica todo lo aprendido en los Módulos 1 y 2 construyendo tu primer pipeline completo e interconectado.

---

## 🎯 Objetivo de la Práctica
Desarrollar un pipeline en Nextflow DSL2 que:
1. Cree un conjunto de datos simulados de secuenciación FASTQ (3 muestras en *Paired-End*).
2. Procese cada muestra en paralelo mediante un proceso de control de calidad individual.
3. Recopile todas las métricas individuales con `.collect()` y genere un informe global consolidado.
4. Publique los resultados limpios y organizados en una carpeta final de salida.

---

## 📋 Especificaciones Técnicas Requeridas

Debes crear un script llamado `main_hito1.nf` con los siguientes requisitos:

### 1. Parámetros (`params`)
- `params.outdir = "resultados_hito1"`
- `params.reads  = "datos_simulados/*_{1,2}.fastq"`

### 2. Generación o Entrada de Datos
- Las 3 muestras simuladas deben llamarse:
  - `Control_Rep1` (lecturas `Control_Rep1_1.fastq` y `Control_Rep1_2.fastq`)
  - `Tratado_Rep1` (lecturas `Tratado_Rep1_1.fastq` y `Tratado_Rep1_2.fastq`)
  - `Tratado_Rep2` (lecturas `Tratado_Rep2_1.fastq` y `Tratado_Rep2_2.fastq`)
- *(Tip: Puedes crear un proceso inicial `GENERAR_DATOS_PRUEBA` que cree estos archivos con unas pocas líneas FASTQ simuladas, o crearlos con un script previo).*

### 3. Proceso 1: `ANALISIS_CALIDAD` (Simulando FastQC)
- **Directivas:**
  - `tag "$sample_id"`
  - `publishDir "${params.outdir}/calidad_individual", mode: 'copy'`
- **Entrada:**
  - `tuple val(sample_id), path(reads)`
- **Salida:**
  - `path "${sample_id}_reporte.txt", emit: reporte`
- **Script:**
  - Un script en Bash o Python que cuente las líneas de los archivos R1 y R2 y calcule el número estimado de lecturas (líneas / 4).
  - Debe guardar un fichero con el formato:
    ```
    Muestra: <sample_id>
    Lecturas R1: X
    Lecturas R2: Y
    Estado QC: PASSED
    ```

### 4. Proceso 2: `CONSOLIDADOR_GLOBAL` (Simulando MultiQC)
- **Directivas:**
  - `publishDir "${params.outdir}/reporte_global", mode: 'copy'`
- **Entrada:**
  - `path reportes_individuales` (debe recibir la lista completa gracias a `.collect()`)
- **Salida:**
  - `path "resumen_global.html"`
- **Script:**
  - Un script (Bash o Python) que lea todos los reportes recibidos y genere una tabla HTML sencilla pero atractiva (`resumen_global.html`) mostrando las métricas de todas las muestras juntas.

### 5. Definición del `workflow`
- Carga las lecturas con `Channel.fromFilePairs`.
- Pasa el canal a `ANALISIS_CALIDAD`.
- Aplica `.collect()` a la salida de reportes y conéctala con `CONSOLIDADOR_GLOBAL`.

---

## 📦 Validación
Comprueba que tu pipeline `main_hito1.nf` genera la estructura de carpetas esperada con los reportes individuales y el `resumen_global.html` abriéndolo en tu navegador para revisar la tabla consolidadora.

---

| [⬅️ Test Autoevaluación M2](Test_Autoevaluacion_M2.md) | 🏠 [Temario Principal](../README.md) |
| :--- | :---: |
