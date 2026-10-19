[⬅️ Lección 1 - Introducción](Leccion_01_Introduccion_y_Filosofia.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 3 - Canales ➡️](Leccion_03_Canales_Fundamentales.md)

---

# 📘 Módulo 1 - Lección 2: Anatomía de un Proceso, Entradas, Salidas y Directivas

En Nextflow, un `process` es la unidad fundamental de cómputo. Todo el trabajo real (analizar un FASTQ, recortar lecturas, mapear contra el genoma, calcular estadísticas) ocurre dentro de un proceso.

---

## 1. Estructura General de un Proceso

Un proceso consta de 4 bloques principales:

1. **Directivas:** Configuración de metadatos y recursos del proceso (`tag`, `cpus`, `memory`, `publishDir`, `container`).
2. **Input:** Declara qué datos espera recibir y desde qué canales.
3. **Output:** Declara qué ficheros o valores producirá para enviarlos a otros canales.
4. **Script:** El comando o script (en Bash, Python o R) que se ejecutará en el sistema.

```groovy
process MI_PROCESO {
    // 1. DIRECTIVAS
    tag "$muestra"
    publishDir "resultados/analisis", mode: 'copy'

    // 2. ENTRADAS
    input:
    val muestra
    path fichero_fastq

    // 3. SALIDAS
    output:
    path "*_conteo.txt", emit: conteos

    // 4. SCRIPT A EJECUTAR
    script:
    """
    echo "Analizando la muestra: ${muestra}"
    wc -l ${fichero_fastq} > ${muestra}_conteo.txt
    """
}
```

---



## 2. Tipos de Entradas (`input:`)

Nextflow necesita saber de qué tipo es cada dato que llega por un canal:


| Calificador de Input | Significado                                                                                                            | Ejemplo de uso                 |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| `val`                | Un valor simple en memoria (número, string, booleano).                                                                 | `val id_muestra`               |
| `path`               | Un archivo o directorio del sistema. Nextflow lo enlaza simbólicamente (**symlink**) en la carpeta de trabajo aislada. | `path reads`                   |
| `tuple`              | Un grupo ordenado de valores y/o archivos que viajan juntos. Es la estructura reina de la bioinformática.              | `tuple val(meta), path(reads)` |
| `each`               | Ejecuta el proceso para cada elemento de una lista fija cruzándola con el resto de entradas.                           | `each val(metodo)`             |




### ⚠️ El concepto crucial de `path`:

Cuando declaras `path fichero_fastq`, Nextflow **no copia** el archivo gigante de 50 GB. Crea un enlace simbólico (`symlink`) dentro de una carpeta temporal (`work/xx/yyyy/`) para que tu comando lo lea a máxima velocidad sin duplicar espacio en disco.

---



## 3. Tipos de Salidas (`output:`)

El bloque `output:` define qué archivos o valores creados durante la ejecución del script deben guardarse y enviarse a los canales de salida:


| Calificador de Output | Significado                                                       | Ejemplo                          |
| --------------------- | ----------------------------------------------------------------- | -------------------------------- |
| `path "patron"`       | Captura los archivos que coincidan con el patrón glob (*, ?.txt). | `path "*.html"`                  |
| `val valor`           | Emite un valor evaluado.                                          | `val "${muestra}_completada"`    |
| `stdout`              | Captura el texto impreso por consola.                             | `stdout`                         |
| `tuple`               | Emite una tupla agrupada (ej. metadatos + fichero resultante).    | `tuple val(meta), path("*.bam")` |




### La directiva `emit:`

En DSL2, puedes ponerle nombre a tus salidas usando `emit:`. Por ejemplo:

```groovy
output:
path "*.html", emit: reporte_html
path "*.zip" , emit: datos_zip
```

Así, en tu workflow puedes referenciar directamente `FASTQC.out.reporte_html` o `FASTQC.out.datos_zip`.

---



## 4. La directiva `publishDir` (Guardar resultados finales)

Por defecto, Nextflow ejecuta todo dentro de la carpeta temporal `work/`. Si el pipeline termina, tus resultados finales se quedan perdidos en subcarpetas con nombres raros como `work/a4/bc789/...`.

Para rescatar los ficheros finales y guardarlos en una carpeta bonita y ordenada para el usuario, usamos `publishDir`:

```groovy
process GENERAR_REPORTE {
    publishDir "resultados/reportes", mode: 'copy'

    input:
    path datos

    output:
    path "reporte_final.pdf"

    script:
    """
    generar_informe.py ${datos} -o reporte_final.pdf
    """
}
```

Modos habituales de `publishDir`:

- `mode: 'copy'` (**Recomendado:** Copia el archivo final a la carpeta destino).
- `mode: 'symlink'` (Crea un acceso directo/symlink para ahorrar espacio en disco).
- `mode: 'move'` (Mueve el archivo y lo borra de work/; no recomendado si usas `-resume`).

---



## 5. Escribir scripts en Python o R dentro de un proceso

No estás limitado a comandos de Bash. Puedes ejecutar código nativo de Python o R indicándolo con un *shebang* (`#!`) en la primera línea del script:

### Ejemplo con Python:

```groovy
process FILTRAR_LONGITUD {
    input:
    path fasta_input

    output:
    path "filtrado.fasta"

    script:
    """
    #!/usr/bin/env python3
    with open("${fasta_input}") as f_in, open("filtrado.fasta", "w") as f_out:
        for line in f_in:
            if not line.startswith(">") and len(line.strip()) > 50:
                f_out.write(line)
    """
}
```



### Ejemplo con R:

```groovy
process CALCULAR_DIFERENCIAL {
    input:
    path tabla_conteos

    output:
    path "volcano.png"

    script:
    """
    #!/usr/bin/env Rscript
    library(ggplot2)
    datos <- read.csv("${tabla_conteos}")
    # Tu lógica estadística en R
    png("volcano.png")
    plot(datos\$logFC, -log10(datos\$pvalue))
    dev.off()
    """
}
```

---



## 6. Variables en Nextflow: `$variable` vs `\$variable`

Este es el error número 1 de todo principiante:

- Cuando escribes `${muestra}` dentro de `""" ... """`, Nextflow lo interpreta como **una variable de Nextflow**.
- Si quieres usar una variable del propio Bash (como `$HOSTNAME` o una variable de un bucle for de bash `$i`), debes **escaparla con barra invertida**: `\$HOSTNAME` o `\$i`.

```groovy
script:
"""
# Variable de Nextflow:
echo "Procesando muestra: ${id_muestra}"

# Variable de Bash (escapada con \):
for i in \$(ls *.txt); do
    echo "Fichero encontrado: \$i"
done
"""
```

---

| [⬅️ Lección 1 - Introducción](Leccion_01_Introduccion_y_Filosofia.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Lección 3 - Canales ➡️](Leccion_03_Canales_Fundamentales.md) |
| :--- | :---: | :---: |