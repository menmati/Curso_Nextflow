[⬅️ Lección 5 - Combinación y Tuplas Meta](Leccion_05_Combinacion_y_Tuplas_Meta.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Ejercicios M2 ➡️](Ejercicios_M2.md)

---

# 📘 Módulo 2 - Lección 6: Agrupación y Ramificación (`groupTuple`, `branch`, `multiMap`)

En esta lección aprenderás a resolver dos problemas habituales en bioinformática:
1. Agrupar réplicas técnicas o biológicas de un mismo paciente para un análisis conjunto.
2. Dividir un flujo de datos en caminos diferentes según el tipo de archivo o metadatos.

---

## 1. El operador `.groupTuple` (Agrupar por Clave)

Cuando tienes varios archivos asociados a una misma muestra (por ejemplo, múltiples carriles de secuenciación *lanes* `L001`, `L002`, o réplicas técnicas), necesitas agruparlos antes de continuar.

`.groupTuple` recopila todos los elementos que compartan la misma clave (por defecto, el primer elemento de la tupla) y reúne sus valores en una lista:

```groovy
ch_lanes = Channel.of(
    ['PacienteA', 'lane1.fastq'],
    ['PacienteB', 'lane1.fastq'],
    ['PacienteA', 'lane2.fastq'],
    ['PacienteA', 'lane3.fastq'],
    ['PacienteB', 'lane2.fastq']
)

ch_lanes
    .groupTuple()
    .view()
// Salida:
// [PacienteA, [lane1.fastq, lane2.fastq, lane3.fastq]]
// [PacienteB, [lane1.fastq, lane2.fastq]]
```

### Opciones avanzadas de `groupTuple`:
- `size`: Espera exactamente N elementos antes de emitir (útil si sabes de antemano cuántos carriles hay).
  ```groovy
  ch_lanes.groupTuple(size: 3)
  ```
- `sort`: Ordena los elementos agrupados antes de emitir (por nombre o fecha).

---

## 2. El operador `.branch` (Bifurcación Condicional)

En lugar de crear múltiples filtros `.filter`, `.branch` evalúa una serie de condiciones y divide un único canal en varios canales nombrados:

```groovy
ch_reads = Channel.of(
    [id: 'M1', type: 'RNA-seq', file: 'm1.fastq'],
    [id: 'M2', type: 'DNA-seq', file: 'm2.fastq'],
    [id: 'M3', type: 'ChIP-seq', file: 'm3.fastq'],
    [id: 'M4', type: 'RNA-seq', file: 'm4.fastq']
)

ch_reads
    .branch { sample ->
        rna:  sample.type == 'RNA-seq'
        dna:  sample.type == 'DNA-seq'
        otro: true // 'true' captura cualquier otro caso restante
    }
    .set { ch_bifurcado }

// Acceso directo a cada rama como canal independiente:
ch_bifurcado.rna.view  { "Enviando a pipeline de expresión génica: ${it.id}" }
ch_bifurcado.dna.view  { "Enviando a llamada de variantes germinales: ${it.id}" }
ch_bifurcado.otro.view { "Muestra no clasificada: ${it.id}" }
```

---

## 3. El operador `.multiMap` (Descomposición en Múltiples Canales)

A veces un canal contiene tuplas complejas y necesitas separar sus componentes para alimentar diferentes procesos que esperan entradas distintas.

`.multiMap` descompone cada elemento en varios canales simultáneamente:

```groovy
ch_experimento = Channel.of(
    [ [id: 'S1', tissue: 'Liver'], 's1.fastq', 's1.bed' ],
    [ [id: 'S2', tissue: 'Brain'], 's2.fastq', 's2.bed' ]
)

ch_experimento
    .multiMap { meta, fastq, bed ->
        fastqs: tuple(meta, fastq)
        beds:   tuple(meta, bed)
    }
    .set { ch_separados }

// Ahora tienes dos canales limpios:
ch_separados.fastqs.view { "Canal FASTQ: ${it}" }
ch_separados.beds.view   { "Canal BED: ${it}" }
```

---

## 4. Resumen Visual de los Operadores del Módulo 2

| Operador | ¿Qué hace? | Caso de uso bioinformático típico |
| :--- | :--- | :--- |
| `.map` | Transforma cada elemento individualmente | Construir tuplas `[meta, reads]` a partir de rutas |
| `.filter` | Descarta elementos que no cumplen una regla | Filtrar muestras que no pasaron el control de calidad |
| `.flatten` | Descompone listas en elementos sueltos | Procesar archivos individuales generados en un tarball |
| `.collect` | Junta todos los elementos en una sola lista | Pasar todas las salidas de FastQC juntas a MultiQC |
| `.join` | Une dos canales por una clave compartida | Cruzar archivos BAM con sus respectivos archivos de índices BAI |
| `.combine`| Producto cartesiano entre dos canales | Probar múltiples referencias contra múltiples muestras |
| `.groupTuple`| Agrupa elementos que tienen la misma clave | Fusionar réplicas técnicas de un mismo paciente |
| `.branch` | Bifurca un canal en varios según condiciones | Dirigir muestras a ramas de Single-End o Paired-End |

---

| [⬅️ Lección 5 - Combinación y Tuplas Meta](Leccion_05_Combinacion_y_Tuplas_Meta.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Ejercicios M2 ➡️](Ejercicios_M2.md) |
| :--- | :---: | :---: |
