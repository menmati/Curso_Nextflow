[⬅️ Lección 9 - Contenedores](Leccion_09_Contenedores_Docker.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test Autoevaluación M3 ➡️](Test_Autoevaluacion_M3.md)

---

# 🏋️ Módulo 3 - Ejercicios Prácticos de Modularidad y Docker

---

### Ejercicio 1: Creación y reutilización de un Módulo DSL2

**Objetivo:** Separar un proceso en un archivo independiente y reutilizarlo dos veces mediante importación con alias (`as`).

**Instrucciones:**

1. Crea una carpeta llamada `modulos/` y dentro un archivo `convertir.nf`.
2. Define un proceso llamado `CONVERTIR_CASE`:
   - Input: `val texto`
   - Output: `stdout`
   - Directiva `tag "$texto"`
   - Script: Un comando Bash `echo "${texto}" | tr '[:lower:]' '[:upper:]'`
3. En tu script principal `ejercicio1_m3.nf`:
   - Importa el proceso dos veces usando alias:
    `include { CONVERTIR_CASE as MAYUSCULAS } from './modulos/convertir'`
    `include { CONVERTIR_CASE as SEGUNDA_PASADA } from './modulos/convertir'`
   - Pásale al primer proceso un canal con las palabras `'dna'`, `'rna'`, `'protein'`.
   - Conecta la salida de `MAYUSCULAS` al segundo proceso `SEGUNDA_PASADA`.
   - Muestra el resultado final con `.view()`.

---

### Ejercicio 2: Construcción de un archivo `nextflow.config` con Labels y Perfiles

**Objetivo:** Configurar recursos dinámicos y perfiles para ejecución local y de test.

**Instrucciones:**
Crea un archivo `nextflow.config` que contenga:

1. Un bloque `params` con:
   - `input = "datos/*.txt"`
   - `outdir = "salida_config"`
   - `modo_estricto = true`
2. Un bloque `process` con directivas por etiqueta:
   - Los procesos con `label 'ligero'` deben recibir `cpus = 1` y `memory = '1.GB'`.
   - Los procesos con `label 'pesado'` deben recibir `cpus = 4` y `memory = '8.GB'`.
3. Un bloque `profiles` con:
   - Perfil `standard`: ejecución local con `docker.enabled = false`.
   - Perfil `docker`: `docker.enabled = true` y `docker.runOptions = '-u $(id -u):$(id -g)'`.
   - Perfil `test`: sobreescribe `params.input` con una ruta a un archivo de test ficticio `test_data/dummy.txt`.

---

### Ejercicio 3: Proceso contenerizado con imagen de BioContainers

**Objetivo:** Ejecutar una herramienta bioinformática real (FastQC o Samtools) dentro de un contenedor Docker mediante Nextflow.

**Instrucciones:**

1. Crea un script `ejercicio3_m3.nf` con un proceso llamado `EJECUTAR_SAMTOOLS_DOCKER`.
2. Asigna la imagen oficial de BioContainers:

   `container 'quay.io/biocontainers/samtools:1.19.2--h50ea8bc_1'`
3. En el bloque `script:`, ejecuta `samtools --help > ayuda_samtools.txt`.
4. Define en el output la captura de `ayuda_samtools.txt` y guárdalo en `resultados_samtools/` con `publishDir`.
5. Ejecuta el pipeline asegurando que Docker está activo:
   ```bash
    nextflow run ejercicio3_m3.nf -with-docker
   ```
6. Comprueba que se ha generado el archivo `ayuda_samtools.txt`.

---

| [⬅️ Lección 9 - Contenedores](Leccion_09_Contenedores_Docker.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Test Autoevaluación M3 ➡️](Test_Autoevaluacion_M3.md) |
| :--- | :---: | :---: |
