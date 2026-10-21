[⬅️ Lección 4 - Operadores de Transformación](Leccion_04_Operadores_Transformacion.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 6 - Agrupación y Ramificación ➡️](Leccion_06_Agrupacion_y_Ramificacion.md)

---

# 📘 Módulo 2 - Lección 5: Combinación de Canales y el Patrón `[meta, reads]` de nf-core

En proyectos bioinformáticos reales no trabajas con cadenas de texto sueltas. Trabajas con muestras que tienen metadatos asociados: si son *Single-End* o *Paired-End*, el paciente al que pertenecen, la réplica biológica, el tejido, la condición clínica o el lote de secuenciación.

En esta lección aprenderás el patrón de arquitectura más importante de la comunidad internacional de Nextflow: **el mapa de metadatos `meta`**.

---

## 1. El Patrón Universal `[meta, reads]` (El estándar nf-core)

Si abres cualquier pipeline oficial de **nf-core** (como `nf-core/rnaseq` o `nf-core/sarek`), verás que prácticamente todos los procesos reciben y devuelven una tupla con esta estructura:

```groovy
tuple val(meta), path(reads)
```

Donde `meta` es un **Map (diccionario)** de Groovy que viaja pegado a los archivos de secuenciación:

```groovy
def meta = [
    id: 'Paciente_01',
    single_end: false,
    strandedness: 'reverse',
    condicion: 'Tratado'
]
```

### ¿Por qué se hace esto?
1. **Trazabilidad absoluta:** A lo largo de 20 pasos de análisis (filtrado, alineamiento, ordenación, marcado de duplicados, recuento), el ID y los metadatos nunca se pierden.
2. **Lógica condicional dentro del proceso:**
   ```groovy
   process ALINEAR {
       input:
       tuple val(meta), path(reads)

       script:
       def comando_reads = meta.single_end ? "-U ${reads}" : "-1 ${reads[0]} -2 ${reads[1]}"
       """
       hisat2 -x genoma ${comando_reads} -S ${meta.id}.sam
       """
   }
   ```
   ¡El mismo proceso sirve tanto para *Single-End* como para *Paired-End* leyendo `meta.single_end`!

---

## 2. El operador `.join` (Unir canales por una clave común)

Imagina que tienes:
- Un canal con archivos FASTQ: `[ 'M1', '/data/M1.fastq' ]`
- Un canal con datos clínicos de pacientes: `[ 'M1', [edad: 54, grupo: 'UCI'] ]`

El operador `.join` busca la **clave coincidente** (por defecto, el primer elemento de la tupla) y fusiona ambos canales:

```groovy
ch_fastqs = Channel.of(
    ['M1', 'm1.fastq'],
    ['M2', 'm2.fastq']
)

ch_clinica = Channel.of(
    ['M1', 'Paciente_Grave'],
    ['M2', 'Paciente_Leve']
)

ch_unido = ch_fastqs.join(ch_clinica)
ch_unido.view()
// Salida:
// [M1, m1.fastq, Paciente_Grave]
// [M2, m2.fastq, Paciente_Leve]
```

---

## 3. El operador `.combine` (Producto Cartesiano)

`.combine` cruza cada elemento del primer canal con **todos los elementos** del segundo canal:

```groovy
ch_muestras = Channel.of('MuestraA', 'MuestraB')
ch_metodos  = Channel.of('Metodo_Bowtie2', 'Metodo_BWA')

ch_muestras.combine(ch_metodos).view()
// Salida:
// [MuestraA, Metodo_Bowtie2]
// [MuestraA, Metodo_BWA]
// [MuestraB, Metodo_Bowtie2]
// [MuestraB, Metodo_BWA]
```

### Uso en bioinformática:
Evaluar múltiples algoritmos de llamada de variantes o diferentes parámetros de filtrado sobre el mismo conjunto de muestras de forma masiva.

---

## 4. Ejemplo Práctico: Construcción de un Samplesheet (Hoja de Muestras)

En pipelines profesionales, las muestras no se escriben a mano en el código. Se leen desde un archivo CSV (`samplesheet.csv`):

```csv
sample,fastq_1,fastq_2,strandedness
M1,datos/m1_1.fastq.gz,datos/m1_2.fastq.gz,reverse
M2,datos/m2_1.fastq.gz,datos/m2_2.fastq.gz,forward
```

Podemos transformar este CSV en canales estructurados `[meta, [reads]]` con solo 5 líneas:

```groovy
Channel
    .fromPath("samplesheet.csv")
    .splitCsv(header: true)
    .map { row ->
        def meta = [
            id: row.sample,
            single_end: row.fastq_2 ? false : true,
            strandedness: row.strandedness
        ]
        def fastq_1 = file(row.fastq_1)
        def fastq_2 = row.fastq_2 ? file(row.fastq_2) : null
        def reads = row.fastq_2 ? [fastq_1, fastq_2] : [fastq_1]

        return tuple(meta, reads)
    }
    .view { meta, reads -> "Muestra cargada: ${meta.id} (SingleEnd: ${meta.single_end})" }
```

Este bloque de código es **exactamente el que utiliza Seqera y nf-core** en todos sus pipelines de producción.

---

| [⬅️ Lección 4 - Operadores de Transformación](Leccion_04_Operadores_Transformacion.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Lección 6 - Agrupación y Ramificación ➡️](Leccion_06_Agrupacion_y_Ramificacion.md) |
| :--- | :---: | :---: |
