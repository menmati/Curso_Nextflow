[⬅️ Ejercicios M2](Ejercicios_M2.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Práctica Hito 1 ➡️](Practica_Hito_1_MiniPipeline_QC.md)

---

# 📝 Módulo 2 - Test de Autoevaluación

Responde a estas 10 preguntas para verificar tu comprensión de los conceptos clave de este módulo:

---

### Pregunta 1
**¿Qué hace el operador `.map` en Nextflow?**
- A) Filtra elementos que contienen errores.
- B) Aplica una función de transformación a cada elemento del canal y emite el resultado transformado.
- C) Dibuja un mapa geográfico de las muestras.
- D) Reordena los elementos alfabéticamente.

### Pregunta 2
**Si un canal emite la lista `[['A', 'B'], ['C', 'D']]`, ¿qué emitirá tras aplicarle `.flatten()`?**
- A) `['A', 'B', 'C', 'D']` como una única lista.
- B) Cuatro emisiones individuales sucesivas: `'A'`, luego `'B'`, luego `'C'`, luego `'D'`.
- C) `['A', 'C']` y `['B', 'D']`.
- D) Un canal vacío.

### Pregunta 3
**¿Por qué es indispensable usar `.collect()` antes de pasar los reportes de FastQC a MultiQC?**
- A) Porque FastQC genera archivos binarios ilegibles sin MultiQC.
- B) Porque si no usas `.collect()`, MultiQC se ejecutaría 100 veces por separado (una por cada muestra) en lugar de 1 sola vez recibiendo todas las muestras juntas.
- C) Porque `.collect()` comprime los reportes en un zip.
- D) Porque MultiQC solo admite canales de valor.

### Pregunta 4
**En el patrón estándar de nf-core `tuple val(meta), path(reads)`, ¿qué suele contener la variable `meta`?**
- A) Una contraseña cifrada para acceder al servidor.
- B) El código binario compilado de Nextflow.
- C) Un mapa (Map/diccionario) con identificadores y metadatos biológicos o experimentales de la muestra (`id`, `single_end`, etc.).
- D) El número de versión de Java.

### Pregunta 5
**¿Qué resultado produce el operador `.join` sobre los canales `[ ['S1', 'archivoA'] ]` y `[ ['S1', 'archivoB'] ]`?**
- A) `[ ['S1', 'archivoA'], ['S1', 'archivoB'] ]`
- B) `[ 'S1', 'archivoA', 'archivoB' ]`
- C) `[ 'archivoA_archivoB' ]`
- D) Un error de conflicto de nombres.

### Pregunta 6
**¿Qué operador utilizarías si necesitas que un proceso de alineamiento pruebe 3 genomas de referencia distintos contra cada una de tus 10 muestras (obteniendo 30 ejecuciones en total)?**
- A) `.groupTuple()`
- B) `.filter()`
- C) `.combine()`
- D) `.flatten()`

### Pregunta 7
**¿Cómo agrupa elementos el operador `.groupTuple()` por defecto?**
- A) Por el tamaño en megabytes de los archivos.
- B) Por el primer elemento de la tupla (clave o clave de muestra).
- C) Por la fecha de creación del fichero.
- D) De forma aleatoria.

### Pregunta 8
**¿Qué ventaja ofrece `.branch` frente a usar múltiples operadores `.filter`?**
- A) Permite dividir un canal en múltiples rutas condicionales de una sola pasada y de forma más legible y eficiente.
- B) Elimina la necesidad de definir procesos.
- C) Permite que los procesos se ejecuten sin archivos FASTQ.
- D) Aumenta la memoria RAM disponible en el clúster.

### Pregunta 9
**¿Qué operador permite descomponer un canal con tuplas complejas en múltiples canales nombrados simultáneos?**
- A) `.multiMap`
- B) `.splitCsv`
- C) `.collate`
- D) `.distinct`

### Pregunta 10
**Si quieres leer un archivo CSV con cabeceras llamado `samples.csv` para construir tus canales, ¿cuál es la combinación idónea de operadores?**
- A) `Channel.fromPath("samples.csv").splitCsv(header: true).map { ... }`
- B) `Channel.of("samples.csv").filter { ... }`
- C) `Channel.fromFilePairs("samples.csv").collect()`
- D) `Channel.value("samples.csv").groupTuple()`

---

| [⬅️ Ejercicios M2](Ejercicios_M2.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Práctica Hito 1 ➡️](Practica_Hito_1_MiniPipeline_QC.md) |
| :--- | :---: | :---: |
