[⬅️ Lección 7 - Modularización DSL2](Leccion_07_Modularizacion_DSL2.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 9 - Contenedores Docker y Singularity ➡️](Leccion_09_Contenedores_Docker.md)

---

# 📘 Módulo 3 - Lección 8: Configuración (`nextflow.config`), Perfiles y Parámetros

Una de las mayores virtudes de Nextflow es la separación absoluta entre **la lógica científica** (el código `.nf`) y **la infraestructura de ejecución** (dónde y con cuántos recursos se ejecuta). Todo esto se controla a través del archivo central: **`nextflow.config`**.

---

## 1. Jerarquía de Configuración en Nextflow

Nextflow busca y combina configuraciones en este orden de prioridad (de menor a mayor):
1. Configuración por defecto del sistema.
2. `nextflow.config` en el directorio de instalación de Nextflow.
3. `nextflow.config` en la carpeta raíz de tu pipeline.
4. Archivo pasado explícitamente con `-c mi_config.config`.
5. Parámetros pasados en la línea de comandos (`--param valor`), que tienen la máxima prioridad y sobreescriben todo lo anterior.

---

## 2. Los Scopes (Ámbitos) de Configuración

Dentro de `nextflow.config`, la configuración se organiza en bloques llamados **scopes**:

### A) El Scope `params` (Variables globales)
Define valores por defecto para tus parámetros:
```groovy
params {
    input_reads = "data/*_{1,2}.fastq.gz"
    outdir      = "resultados"
    multiqc     = true
    max_cpus    = 4
    max_memory  = '8.GB'
}
```

### B) El Scope `process` (Recursos y Directivas)
Controla cómo se ejecutan los procesos (CPUs, memoria, tiempo máximo, estrategia de errores):
```groovy
process {
    executor = 'local'
    cpus     = 2
    memory   = '4.GB'
    time     = '2h'
}
```

---

## 3. Asignación Dinámica de Recursos mediante Etiquetas (`label`)

En un pipeline real no todos los procesos consumen lo mismo:
- Un script de control de calidad o filtrado necesita pocos recursos (`process_low`: 1 CPU, 2 GB).
- Un alineador de genomas como HISAT2 o STAR necesita mucha memoria y procesadores (`process_high`: 8 CPUs, 32 GB).

En lugar de poner los números a mano dentro de cada proceso, se usan **etiquetas (`label`)**:

### En el proceso (`modules/hisat2.nf`):
```groovy
process HISAT2_ALIGN {
    label 'process_high' // Asignamos la etiqueta
    
    // ... input, output, script ...
}
```

### En el `nextflow.config`:
```groovy
process {
    // Configuración por defecto:
    cpus   = 1
    memory = '2.GB'

    // Configuración específica según la etiqueta:
    withLabel: 'process_low' {
        cpus   = 2
        memory = '4.GB'
    }

    withLabel: 'process_medium' {
        cpus   = 4
        memory = '8.GB'
    }

    withLabel: 'process_high' {
        cpus   = 8
        memory = '16.GB'
    }
}
```

---

## 4. Perfiles de Ejecución (`profiles`)

Los perfiles permiten agrupar configuraciones para diferentes entornos (tu portátil de desarrollo, un clúster de computación, o la nube). Se activan con la opción `-profile <nombre>`.

Ejemplo de `nextflow.config` profesional con perfiles:

```groovy
profiles {
    // Perfil por defecto (para probar rápido en tu portátil)
    standard {
        process.executor = 'local'
        docker.enabled   = true
    }

    // Perfil para pruebas rápidas con datos diminutos
    test {
        params.input_reads = "https://raw.githubusercontent.com/menmati/datos_test/reads.fastq"
        params.outdir      = "test_results"
        process.cpus       = 1
        process.memory     = '2.GB'
        docker.enabled     = true
    }

    // Perfil para un clúster HPC con SLURM
    hpc_slurm {
        process.executor = 'slurm'
        process.queue    = 'bioinfo_nodes'
        singularity.enabled = true
        docker.enabled   = false
    }

    // Perfil para Amazon Web Services (AWS Batch)
    awsbatch {
        process.executor = 'awsbatch'
        process.queue    = 'arn:aws:batch:eu-west-1:queue/nextflow-queue'
        aws.region       = 'eu-west-1'
        docker.enabled   = true
    }
}
```

### Cómo ejecutarlos en tu terminal:
```bash
# Ejecutar localmente con Docker:
nextflow run main.nf -profile standard

# Ejecutar el perfil de test preconfigurado:
nextflow run main.nf -profile test

# Ejecutar en el clúster hospitalario con SLURM:
nextflow run main.nf -profile hpc_slurm
```

¡El código bioinformático es exactamente el mismo! Solo cambia el perfil.

---

| [⬅️ Lección 7 - Modularización DSL2](Leccion_07_Modularizacion_DSL2.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Lección 9 - Contenedores Docker y Singularity ➡️](Leccion_09_Contenedores_Docker.md) |
| :--- | :---: | :---: |
