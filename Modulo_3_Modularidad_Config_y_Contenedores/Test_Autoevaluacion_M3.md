[⬅️ Ejercicios M3](Ejercicios_M3.md) | [🏠 Temario Principal](../README.md)

---

# 📝 Módulo 3 - Test de Autoevaluación

---

### Pregunta 1

**¿Para qué sirve la palabra clave** `include` **en Nextflow DSL2?**

- A) Para incluir librerías dinámicas de C++ en tiempo de compilación.
- B) Para importar procesos o subworkflows definidos en otros archivos `.nf`.
- C) Para adjuntar un archivo adjunto en un correo electrónico.
- D) Para incluir variables de entorno de Windows en Linux.

### Pregunta 2

**Si quieres importar el mismo proceso** `FASTQC` **dos veces en un workflow para ejecutarlo antes y después del trimming, ¿cómo resuelves la colisión de nombres?**

- A) Duplicando el archivo `fastqc.nf` con otro nombre.
- B) Usando la cláusula `as`: `include { FASTQC as FASTQC_TRIMMED } from './modules/fastqc'`.
- C) Nextflow no permite ejecutar el mismo proceso dos veces en un pipeline.
- D) Creando dos workflows independientes en dos terminales distintas.

### Pregunta 3

**¿Qué bloque de código define un Subworkflow en DSL2?**

- A) `subworkflow NOMBRE { ... }`
- B) `workflow NOMBRE { take: ... main: ... emit: ... }`
- C) `process_group NOMBRE { ... }`
- D) `def workflow NOMBRE { ... }`

### Pregunta 4

**¿Qué prioridad tiene un parámetro pasado por línea de comandos (ej.** `--outdir mi_carpeta`**) frente al valor definido en** `nextflow.config`**?**

- A) La línea de comandos tiene la máxima prioridad y sobreescribe cualquier valor del archivo de configuración.
- B) El archivo `nextflow.config` siempre gana sobre la línea de comandos.
- C) Se produce un error de conflicto y Nextflow se detiene.
- D) Se combinan en una lista de dos elementos.

### Pregunta 5

**¿Qué ventaja aporta usar** `withLabel: 'process_high'` **en** `nextflow.config` **en lugar de fijar** `cpus = 8` **dentro de cada proceso?**

- A) Desacopla la lógica del código de los recursos físicos, permitiendo ajustar los recursos para diferentes máquinas o clústeres desde un único archivo de configuración.
- B) Duplica la velocidad del procesador.
- C) Evita que Nextflow use memoria swap.
- D) Es obligatorio para poder usar Python.

### Pregunta 6

**¿Cómo se activa un perfil de configuración llamado** `slurm` **al lanzar un pipeline?**

- A) `nextflow run main.nf --slurm`
- B) `nextflow run main.nf -profile slurm`
- C) `nextflow run main.nf -config slurm.json`
- D) `nextflow config slurm`

### Pregunta 7

**¿Qué hace Nextflow cuando ejecutas un proceso que tiene la directiva** `container` **con Docker activado?**

- A) Abre una máquina virtual completa con VirtualBox.
- B) Descarga automáticamente la imagen del contenedor si no existe localmente, monta la carpeta temporal como volumen y ejecuta el script dentro del contenedor.
- C) Instala el software en tu sistema operativo anfitrión usando `apt-get`.
- D) Convierte el script de Bash a un binario ejecutable de Windows.

### Pregunta 8

**¿Qué repositorio comunitario mantiene imágenes Docker estandarizadas para miles de herramientas bioinformáticas (FastQC, Samtools, etc.)?**

- A) NPM Registry.
- B) BioContainers (Quay.io).
- C) PyPI.
- D) Biopython Cloud.

### Pregunta 9

**¿Por qué en los clústeres de supercomputación (HPC) suele utilizarse Singularity / Apptainer en lugar de Docker?**

- A) Porque Docker no soporta sistemas operativos Linux.
- B) Por motivos de seguridad, ya que Docker requiere privilegios de superusuario (`root`) para ejecutarse en el nodo, mientras que Singularity corre con los permisos del usuario normal.
- C) Porque Singularity es más lento pero consume menos electricidad.
- D) Porque Nextflow no es compatible con Docker en servidores.

### Pregunta 10

**¿Qué opción de Docker en** `nextflow.config` **(**`docker.runOptions =`**) garantiza que los archivos resultantes pertenezcan a tu usuario y no al usuario root?**

- A) La opción `-u` (User UID:GID mapping).
- B) La opción `--privileged`.
- C) La opción `-d` (daemon mode).
- D) La opción `-m` (memory limit).

---

| [⬅️ Ejercicios M3](Ejercicios_M3.md) | 🏠 [Temario Principal](../README.md) |
| :--- | :---: |
