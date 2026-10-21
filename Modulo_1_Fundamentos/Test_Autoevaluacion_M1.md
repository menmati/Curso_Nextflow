[⬅️ Ejercicios M1](Ejercicios_M1.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Módulo 2 - Lección 4 ➡️](../Modulo_2_Operadores_y_Canales_Avanzados/Leccion_04_Operadores_Transformacion.md)

---

# 📝 Módulo 1 - Test de Autoevaluación

Responde a estas 10 preguntas para verificar tu comprensión de los conceptos clave de este módulo:

---

### Pregunta 1
**¿Cuál es la diferencia fundamental entre el paradigma de programación Dataflow de Nextflow y un script tradicional de Bash o Python?**
- A) Nextflow compila el código en código máquina C++ antes de ejecutarlo.
- B) En Nextflow las tareas se ejecutan automáticamente tan pronto como sus entradas están disponibles, sin orden secuencial rígido.
- C) Nextflow solo puede ejecutarse en clústeres HPC y no en ordenadores locales.
- D) Nextflow requiere que el programador gestione los hilos de ejecución mediante semáforos y mutexes.

### Pregunta 2
**¿Qué ocurre cuando declaras una entrada como `path mi_fichero` dentro de un proceso?**
- A) Nextflow copia físicamente el archivo completo a la carpeta temporal `work/`.
- B) Nextflow carga todo el contenido del fichero en la memoria RAM del sistema.
- C) Nextflow crea un enlace simbólico (`symlink`) dentro de una carpeta de ejecución aislada en `work/`.
- D) Nextflow convierte el archivo a formato JSON.

### Pregunta 3
**Si no defines la directiva `publishDir` en un proceso, ¿dónde terminan los archivos de salida generados?**
- A) Se eliminan automáticamente al finalizar el proceso.
- B) Se guardan en la carpeta donde ejecutaste el comando `nextflow run`.
- C) Se quedan en subcarpetas temporales dentro del directorio `work/`.
- D) Se suben automáticamente a un bucket de Amazon S3.

### Pregunta 4
**¿Qué diferencia a un Queue Channel de un Value Channel?**
- A) Un Queue channel solo almacena números enteros y un Value channel almacena texto.
- B) Un Queue channel se consume de forma destructiva al ser leído por un proceso, mientras que un Value channel puede reutilizarse infinitas veces.
- C) Un Value channel no permite pasar archivos, solo valores booleanos.
- D) No hay ninguna diferencia funcional, son sinónimos en DSL2.

### Pregunta 5
**En el bloque `script:`, ¿cómo debes escribir una variable propia de la shell de Bash para evitar que Nextflow la interprete como una variable de Nextflow?**
- A) `${variable}`
- B) `\$variable`
- C) `@variable`
- D) `nextflow.bash(variable)`

### Pregunta 6
**¿Qué hace el operador `.view()` cuando se añade a un canal?**
- A) Abre una ventana gráfica en el navegador web con un gráfico interactivo.
- B) Pausa la ejecución del pipeline hasta que el usuario pulsa Enter.
- C) Imprime por la salida estándar (consola) cada elemento que pasa a través del canal.
- D) Exporta el canal a un archivo PDF.

### Pregunta 7
**¿Por qué es una buena práctica utilizar `checkIfExists: true` en `Channel.fromPath("datos/*.fastq", checkIfExists: true)`?**
- A) Porque aumenta la velocidad de lectura del disco en un 50%.
- B) Porque si el archivo o patrón no existe, Nextflow lanza un error de inmediato en lugar de continuar con un canal vacío.
- C) Porque desencripta los archivos FASTQ comprimidos.
- D) Porque es obligatorio por especificación en DSL2.

### Pregunta 8
**¿Qué estructura de datos devuelve la factoría `Channel.fromFilePairs("reads/*_{1,2}.fastq.gz")`?**
- A) Una lista plana con todos los nombres de archivo.
- B) Un mapa asociativo con claves `sample` y `paths`.
- C) Una tupla compuesta por el identificador del par y una lista con las dos rutas de archivos: `[id, [R1, R2]]`.
- D) Un único archivo concatenado con ambas lecturas fusionadas.

### Pregunta 9
**¿Cómo se ejecuta un script de Python en lugar de Bash dentro de un proceso de Nextflow?**
- A) Declarando la directiva `language 'python'`.
- B) Escribiendo `#!/usr/bin/env python3` en la primera línea del bloque `script:`.
- C) Renombrando el proceso a `process_python`.
- D) No es posible; Nextflow solo soporta Bash.

### Pregunta 10
**¿Para qué sirve la cláusula `emit:` en la sección `output:` de un proceso DSL2?**
- A) Para enviar un correo electrónico al usuario cuando la tarea finaliza.
- B) Para asignar un nombre identificador a un canal de salida y acceder a él como `PROCESO.out.nombre`.
- C) Para obligar a que el archivo se comprima en formato `.tar.gz`.
- D) Para transmitir los datos a través de una conexión WebSocket.

---

| [⬅️ Ejercicios M1](Ejercicios_M1.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Módulo 2 - Lección 4 ➡️](../Modulo_2_Operadores_y_Canales_Avanzados/Leccion_04_Operadores_Transformacion.md) |
| :--- | :---: | :---: |
