[⬅️ Módulo 2 - Hito 1](../Modulo_2_Operadores_y_Canales_Avanzados/Practica_Hito_1_MiniPipeline_QC.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 8 - Configuración y Parámetros ➡️](Leccion_08_Nextflow_Config_y_Params.md)

---

# 📘 Módulo 3 - Lección 7: Modularización en DSL2, Importaciones y Subworkflows

En proyectos profesionales de bioinformática, los pipelines no se escriben en un único archivo monolítico de 2.000 líneas. Se dividen en **módulos atómicos** y **subworkflows** reutilizables.

---

## 1. ¿Qué es un Módulo en Nextflow DSL2?

Un módulo es simplemente un archivo `.nf` que contiene la definición de **un solo proceso**. Por convención de la comunidad nf-core, se guardan en una carpeta `modules/`:

```
mi_pipeline/
├── main.nf
└── modules/
    ├── fastqc.nf
    ├── trimmomatic.nf
    └── hisat2.nf
```

### Ejemplo de módulo independiente (`modules/fastqc.nf`):
```groovy
process FASTQC {
    tag "$meta.id"
    publishDir "${params.outdir}/fastqc", mode: 'copy'

    input:
    tuple val(meta), path(reads)

    output:
    tuple val(meta), path("*.html"), emit: html
    tuple val(meta), path("*.zip") , emit: zip

    script:
    """
    fastqc -q ${reads}
    """
}
```

---

## 2. La palabra clave `include` (Importar Procesos)

En tu archivo principal `main.nf`, importas el proceso usando la sintaxis `include { PROCESO } from './ruta'`:

```groovy
nextflow.enable.dsl=2

// Importamos el módulo
include { FASTQC } from './modules/fastqc'

workflow {
    ch_reads = Channel.fromFilePairs("data/*_{1,2}.fastq.gz")
        .map { id, files -> tuple([id: id], files) }

    // Ejecutamos el proceso importado
    FASTQC(ch_reads)
}
```

---

## 3. Renombrar Procesos al Importar con `as` (Aliasing)

Imagina que en tu pipeline quieres ejecutar FastQC **dos veces**:
1. Antes de limpiar las lecturas (control de calidad inicial).
2. Después del filtrado y recorte de adaptadores con Trimmomatic (control de calidad post-trimming).

Si intentas llamar a `FASTQC()` dos veces en el mismo workflow, Nextflow dará un error de colisión de nombres.
La solución es importar el mismo proceso con dos alias diferentes usando **`as`**:

```groovy
include { FASTQC as FASTQC_RAW     } from './modules/fastqc'
include { FASTQC as FASTQC_TRIMMED } from './modules/fastqc'
include { TRIMMOMATIC              } from './modules/trimmomatic'

workflow {
    // 1. QC inicial
    FASTQC_RAW(ch_reads)

    // 2. Limpieza
    TRIMMOMATIC(ch_reads)

    // 3. QC final sobre las lecturas recortadas
    FASTQC_TRIMMED(TRIMMOMATIC.out.trimmed_reads)
}
```
Esto genera reportes y directorios completamente independientes sin duplicar ni una sola línea de código.

---

## 4. ¿Qué es un Subworkflow?

Un **subworkflow** es un flujo de trabajo que agrupa varios procesos relacionados para realizar una tarea compleja, actuando como un bloque de construcción de mayor nivel.

Por ejemplo, puedes crear un subworkflow llamado `QC_AND_TRIM` que agrupe FastQC y Trimmomatic:

```groovy
// subworkflows/qc_and_trim.nf
include { FASTQC      } from '../modules/fastqc'
include { TRIMMOMATIC } from '../modules/trimmomatic'

workflow QC_AND_TRIM {
    take:
    ch_raw_reads // Entrada del subworkflow

    main:
    FASTQC(ch_raw_reads)
    TRIMMOMATIC(ch_raw_reads)

    emit:
    clean_reads = TRIMMOMATIC.out.reads // Salida exportada
    qc_reports  = FASTQC.out.zip
}
```

Y en tu `main.nf`:
```groovy
include { QC_AND_TRIM } from './subworkflows/qc_and_trim'
include { ALINEAR     } from './modules/alinear'

workflow {
    QC_AND_TRIM(ch_reads)
    ALINEAR(QC_AND_TRIM.out.clean_reads)
}
```

Esta arquitectura limpia y jerárquica es exactamente la que hace posible que los pipelines de Seqera y nf-core escalen a cientos de pasos de forma mantenible.

---

| [⬅️ Módulo 2 - Hito 1](../Modulo_2_Operadores_y_Canales_Avanzados/Practica_Hito_1_MiniPipeline_QC.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Lección 8 - Configuración y Parámetros ➡️](Leccion_08_Nextflow_Config_y_Params.md) |
| :--- | :---: | :---: |
