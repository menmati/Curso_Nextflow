[🏠 Temario Principal](../README.md) | [Siguiente: Lección 2 - Procesos y Directivas ➡️](Leccion_02_Procesos_y_Directivas.md)

---

# 📘 Módulo 1 - Lección 1: Introducción, Filosofía y Estructura de Nextflow

## 1. El gran problema de la bioinformática y por qué nació Nextflow

Tradicionalmente, los bioinformáticos unían herramientas mediante scripts de Bash tipo:

```bash
# Script clásico frágil (NO reproducible)
fastqc muestra1.fastq.gz
hisat2 -x genoma_ref -U muestra1.fastq.gz -S muestra1.sam
samtools view -bS muestra1.sam > muestra1.bam
```

Este enfoque tiene problemas gravísimos en producción:

1. **Falta de paralelismo:** Si tienes 100 muestras, procesarlas secuencialmente tarda días. Paralelizar a mano en Bash con `&` o `nohup` es caótico.
2. **Sin tolerancia a fallos:** Si la muestra 87 falla tras 14 horas de cálculo, todo el script se detiene y tienes que empezar desde cero.
3. **Dependencias del sistema:** Si ejecutas el script en tu ordenador funciona, pero si lo pasas al clúster de un hospital o a la nube de AWS, falla porque faltan librerías o las versiones no coinciden.
4. **Portabilidad nula:** Pasar de tu portátil a un clúster HPC con SLURM o a AWS Batch requiere reescribir todo el código.

**Nextflow nació (en el CRG de Barcelona, por Paolo Di Tommaso y Evan Floden) para resolver esto.**

---



## 2. Los 3 Pilares de Nextflow



### Pilar 1: Programación Reactiva basada en Flujo de Datos (Dataflow Programming)

Nextflow no ejecuta las instrucciones línea a línea como un script convencional de Python o Bash.
En Nextflow, **las tareas se ejecutan tan pronto como sus datos de entrada están disponibles**.

- Imagina una fábrica: los operarios (**procesos**) están esperando piezas.
- Las piezas viajan por cintas transportadoras (**canales**).
- Cuando una pieza llega al puesto de trabajo, el operario se activa de forma automática, realiza su trabajo y coloca el resultado en otra cinta transportadora hacia el siguiente puesto.



### Pilar 2: Separación estricta entre la lógica del pipeline y la infraestructura

Escribes el pipeline **una sola vez**. Luego, mediante configuración, decides dónde corre:

- En tu portátil (modo local).
- En un clúster universitario/hospitalario con gestores de colas (SLURM, SGE, PBS).
- En la nube (AWS Batch, Google Cloud Life Sciences, Azure Batch).
- En la plataforma de orquestación de Seqera (Seqera Platform / Tower).



### Pilar 3: Contenerización y Reproducibilidad Nativa

Nextflow se conecta de forma transparente con **Docker**, **Singularity** y **Conda**. No necesitas instalar `samtools` o `fastqc` en tu máquina: Nextflow descarga automáticamente la imagen del contenedor oficial de BioContainers y ejecuta el comando dentro de él.

---



## 3. ¿Qué es DSL2 (Domain Specific Language 2)?

En versiones antiguas de Nextflow (DSL1), los procesos estaban acoplados rígidamente a los canales. En la versión actual (**DSL2**, estándar universal desde Nextflow 20+), Nextflow introdujo **modularidad pura**:

- Los procesos se definen como funciones reutilizables.
- Los flujos de trabajo (`workflow`) conectan procesos independientes.
- Puedes importar procesos creados por la comunidad internacional (como el ecosistema **nf-core**) usando `include`.

---



## 4. Anatomía del primer script en Nextflow (`main.nf`)

Crea un archivo llamado `primer_pipeline.nf` con este contenido:

```groovy
// Indicamos los parámetros con 'params.'
params.saludo = "Hola, Mundo"

// 1. DEFINICIÓN DEL PROCESO
process SALUDAR {
    tag "Generando saludo"

    input:
    val mensaje

    output:
    stdout

    script:
    """
    echo "${mensaje}! Bienvenida al mundo de Nextflow DSL2."
    """
}

// 2. DEFINICIÓN DEL FLUJO DE TRABAJO (WORKFLOW)
workflow {
    // Creamos un canal con un dato
    ch_mensaje = Channel.of(params.saludo)

    // Conectamos el canal con el proceso
    SALUDAR(ch_mensaje)

    // Mostramos la salida del proceso por consola con el operador .view()
    SALUDAR.out.view()
}
```



### Explicación línea a línea:

- `params.saludo`: Variable configurable desde la línea de comandos (por ejemplo: `nextflow run script.nf --saludo "Buenas tardes"`).
- `process SALUDAR { ... }`: Bloque ejecutable independiente.
  - `input: val mensaje`: Espera recibir un valor a través de un canal.
  - `output: stdout`: Captura la salida estándar del comando para enviarla al siguiente paso.
  - `script: """ ... """`: El comando real que se ejecutará en la shell (Bash).
- `workflow { ... }`: El director de orquesta. Define cómo se interconectan los canales y procesos.
- `SALUDAR.out.view()`: Escucha la salida del proceso `SALUDAR` y la imprime en tu terminal.

---



## 5. Cómo ejecutarlo en tu terminal de WSL

```bash
nextflow run primer_pipeline.nf
```

Salida esperada:

```
N E X T F L O W   ~   version 24.x.x
[f3/a12b45] Submitted process > SALUDAR (Generando saludo)
Hola, Mundo! Bienvenid@ al mundo de Nextflow DSL2.
```

Nextflow crea automáticamente un directorio oculto y un hash de ejecución (`f3/a12b45`), donde se aisló la ejecución del comando. Si ejecutas:

```bash
nextflow run primer_pipeline.nf --saludo "Hola Seqera"
```

Verás cómo el parámetro sobreescribe el valor por defecto sin tocar el código.

---

## 💡 Conceptos Clave

- **¿Qué es Dataflow?** Es un modelo de computación donde la ejecución de una tarea está condicionada exclusivamente a la presencia de sus entradas, garantizando concurrencia masiva sin necesidad de sincronizaciones manuales con hilos (`threads`) o bloqueos (`locks`).
- **¿Qué diferencia a DSL2 de DSL1?** DSL2 permite que los procesos y subworkflows se definan como componentes independientes y reutilizables en múltiples proyectos mediante la directiva `include`, similar a `import` en Python.

---

| 🏠 [Temario Principal](../README.md) | [Siguiente: Lección 2 - Procesos y Directivas ➡️](Leccion_02_Procesos_y_Directivas.md) |
| :--- | :---: |