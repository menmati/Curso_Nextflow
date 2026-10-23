[⬅️ Ejercicios M4](Ejercicios_M4.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Práctica Hito 2 ➡️](Practica_Hito_2_Pipeline_Robusto.md)

---

# 📝 Módulo 4 - Test de Autoevaluación

Responde a estas 10 preguntas para verificar tu comprensión de los conceptos clave de este módulo:

---

### Pregunta 1
**¿Cómo determina Nextflow si una tarea previa puede recuperarse de la caché al usar `-resume`?**
- A) Compara si el nombre del archivo de salida existe en la carpeta actual.
- B) Calcula un código hash único de 128 bits basado en el script, las entradas, el contenido de los archivos y los parámetros.
- C) Comprueba si el ordenador no se ha reiniciado desde la última ejecución.
- D) Pregunta al usuario por consola si desea reanudarla.

### Pregunta 2
**Si un proceso falla, ¿qué archivo oculto dentro de la subcarpeta de `work/` contiene el mensaje de error exacto generado por la herramienta (stderr)?**
- A) `.command.run`
- B) `.command.sh`
- C) `.command.err`
- D) `.exitcode`

### Pregunta 3
**¿Qué archivo oculto en la carpeta de trabajo contiene el script exacto de Bash o Python tal y como fue evaluado por Nextflow para poder probarlo a mano?**
- A) `.command.sh`
- B) `.command.out`
- C) `.command.log`
- D) `nextflow.log`

### Pregunta 4
**¿Qué ocurre si un proceso tiene la directiva `errorStrategy = 'ignore'` y falla para una muestra?**
- A) El proceso se reintenta 10 veces seguidas.
- B) Nextflow ignora el error de esa muestra concreta, la descarta del flujo y continúa ejecutando el resto de muestras normalmente.
- C) Todo el ordenador se reinicia para liberar memoria RAM.
- D) Se borran todos los resultados generados hasta ese momento.

### Pregunta 5
**¿Cómo se implementa el escalado dinámico de memoria para que Nextflow doble la memoria RAM cada vez que un proceso se reintenta tras un fallo?**
- A) `memory = 'auto'`
- B) `memory = { 2.GB * task.attempt }`
- C) `scale_memory(2)`
- D) `nextflow.double_ram()`

### Pregunta 6
**¿Qué código de salida numérico en Linux/Docker suele indicar que el proceso fue terminado por el sistema operativo por exceder el límite de memoria RAM (Out Of Memory / OOM)?**
- A) 0
- B) 127
- C) 137 (o 143)
- D) 404

### Pregunta 7
**¿Qué información muestra el reporte generado con la opción `-with-timeline timeline.html`?**
- A) Un mapa de la ubicación de los servidores de Amazon.
- B) Un diagrama de Gantt interactivo que muestra los tiempos de inicio, duración y finalización de cada tarea paralela.
- C) El historial de commits de Git del repositorio.
- D) La lista de errores de sintaxis del código.

### Pregunta 8
**¿Qué bandera permite generar un informe HTML interactivo con gráficos de consumo de memoria, uso de CPU y tiempos de disco de todos los procesos?**
- A) `-with-report reporte.html`
- B) `-with-stats stats.html`
- C) `--generate-pdf`
- D) `-debug-full`

### Pregunta 9
**¿Qué es Seqera Platform (anteriormente llamada Nextflow Tower)?**
- A) Un lenguaje de programación nuevo que sustituye a Python.
- B) Una plataforma empresarial para monitorizar, gestionar y ejecutar pipelines de Nextflow en entornos cloud (AWS, Azure, GCP) y clústeres HPC.
- C) Un editor de texto para programar scripts de Bash.
- D) Un antivirus para servidores de bioinformática.

### Pregunta 10
**¿Cómo puede Nextflow leer y escribir archivos directamente en el almacenamiento en la nube de Amazon Web Services sin necesidad de descargarlos a tu disco duro local?**
- A) Utilizando URLs nativas con el protocolo de buckets S3: `s3://mi-bucket/datos/`.
- B) No es posible; siempre hay que descargar los archivos primero con FileZilla.
- C) Enviando los archivos por correo electrónico a Amazon.
- D) Mediante un cable de red virtual USB.

---

| [⬅️ Ejercicios M4](Ejercicios_M4.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Práctica Hito 2 ➡️](Practica_Hito_2_Pipeline_Robusto.md) |
| :--- | :---: | :---: |
