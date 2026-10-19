[⬅️ Lección 3 - Canales](Leccion_03_Canales_Fundamentales.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test Autoevaluación ➡️](Test_Autoevaluacion_M1.md)

---

# 🏋️ Módulo 1 - Ejercicios Prácticos de Programación

Pon a prueba los conocimientos adquiridos en las tres primeras lecciones implementando los siguientes tres ejercicios prácticos en archivos `.nf` independientes.

---

### Ejercicio 1: Creación de un Canal de Muestras y Conteo de Palabras
**Objetivo:** Escribir un pipeline completo que procese tres archivos de texto creados dinámicamente y cuente cuántas líneas tiene cada uno.

**Instrucciones:**
1. Crea un proceso llamado `CREAR_ARCHIVO` que reciba un nombre de muestra (string), genere un archivo llamado `<muestra>.txt` con un texto de 3 líneas y lo emita en el `output`.
2. Crea un segundo proceso llamado `CONTAR_LINEAS` que reciba el archivo del paso anterior y ejecute el comando `wc -l` de Linux para guardar el resultado en `<muestra>_conteo.txt`.
3. Haz que el proceso `CONTAR_LINEAS` guarde el resultado final en una carpeta llamada `resultados_ejercicio1/` usando la directiva `publishDir`.
4. En el `workflow`, alimenta el pipeline con un canal que contenga tres muestras: `'control'`, `'tratado_dosis_baja'`, `'tratado_dosis_alta'`.

---

### Ejercicio 2: Procesamiento de Secuencias FASTA con Python dentro de Nextflow
**Objetivo:** Integrar un script en Python dentro del bloque `script:` de Nextflow para calcular el contenido GC de secuencias biológicas.

**Instrucciones:**
1. Crea un canal con 3 secuencias de ADN simples:
   - Muestra 1: `'ATGCGATCGATCGATCGATC'` (ID: `seq1`)
   - Muestra 2: `'GCGCGCGCATATAT'` (ID: `seq2`)
   - Muestra 3: `'AAAAAAAAAATTTTTTTTTT'` (ID: `seq3`)
2. Crea un proceso llamado `CALCULAR_GC` que reciba como tupla `tuple val(id), val(secuencia)`.
3. En el bloque `script:`, utiliza el shebang `#!/usr/bin/env python3` para escribir un pequeño script en Python que:
   - Calcule el porcentaje de G y C en la secuencia: `((G + C) / total) * 100`.
   - Escriba en un archivo `<id>_gc.txt` el resultado: `Muestra <id>: XX.XX% GC`.
4. Muestra el resultado de los archivos generados en la consola usando `.view()`.

---

### Ejercicio 3: Canales de Paired-End y Variables de Bash vs Nextflow
**Objetivo:** Manejar lecturas emparejadas simuladas y dominar el escape de variables de Bash.

**Instrucciones:**
1. Simula con comandos de Bash (o crea a mano) dos archivos falsos en una carpeta temporal `datos_test/`:
   - `paciente1_R1.fastq.gz`
   - `paciente1_R2.fastq.gz`
2. Carga los archivos utilizando la factoría `Channel.fromFilePairs("datos_test/*_R{1,2}.fastq.gz")`.
3. Crea un proceso `INSPECCIONAR_PARES` que reciba la tupla `tuple val(sample_id), path(reads)`.
4. En el `script:`, crea un bucle en Bash (`for r in ...`) que itere sobre los dos archivos del par y extraiga el tamaño del archivo con el comando `ls -lh`.
5. ⚠️ **Atención:** Asegúrate de escapar correctamente las variables del bucle Bash con `\$` para que Nextflow no intente evaluarlas.

---

| [⬅️ Lección 3 - Canales](Leccion_03_Canales_Fundamentales.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Test Autoevaluación ➡️](Test_Autoevaluacion_M1.md) |
| :--- | :---: | :---: |
