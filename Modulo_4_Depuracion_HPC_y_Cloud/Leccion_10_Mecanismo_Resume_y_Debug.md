[⬅️ Módulo 3 - Test Autoevaluación](../Modulo_3_Modularidad_Config_y_Contenedores/Test_Autoevaluacion_M3.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 11 - Manejo de Errores ➡️](Leccion_11_Manejo_de_Errores.md)

---

# 📘 Módulo 4 - Lección 10: El Mecanismo `-resume`, Caché y Depuración Profesional

Uno de los mayores atractivos de Nextflow frente a cualquier otro orquestador es su capacidad para **reanudar ejecuciones fallidas al instante** sin recalcular nada que ya estuviera completado con éxito.

En esta lección aprenderás cómo funciona el sistema de caché y cómo depurar fallos en segundos.

---

## 1. Cómo funciona el directorio `work/` y el Hashing

Cada vez que Nextflow lanza una tarea, crea una carpeta aislada dentro de `work/`.
El nombre de esa carpeta viene determinado por un código hash único de 128 bits (ej. `work/f3/a1b2c3d4...`):

Nextflow calcula ese hash combinando:
1. El código exacto del script en el proceso.
2. Los valores y metadatos de las entradas.
3. El contenido (checksum) de los archivos de entrada.
4. Las variables de entorno y los parámetros utilizados.

**Regla de oro:** Si ninguno de esos 4 elementos cambia, Nextflow sabe matemáticamente que el resultado sería idéntico, por lo que **no vuelve a ejecutar la tarea: recupera el resultado previo de la caché**.

---

## 2. La magia de `-resume`

Imagina que ejecutas un pipeline de 100 muestras con 5 procesos (500 tareas en total).
En la tarea 490, un proceso falla porque se fue la luz o hubo una errata en un script:

1. Corriges la errata en tu archivo `.nf`.
2. Vuelves a lanzar el pipeline añadiendo el flag **`-resume`**:
   ```bash
   nextflow run main.nf -resume
   ```
3. **¿Qué ocurre?** Nextflow revisa las 489 tareas anteriores, comprueba que sus hashes son idénticos y muestra:
   ```
   [f3/a1b2c3] Cached process > FASTQC (Muestra_01)
   [8d/4e5f6a] Cached process > FASTQC (Muestra_02)
   ...
   [c9/112233] Submitted process > ALINEAR (Muestra_90)
   ```
   ¡Las 489 tareas tardan 0 segundos! El pipeline continúa exactamente desde la tarea 490 que falló.

---

## 3. Anatomía de una carpeta de tarea: Los archivos `.command.*`

Cuando una tarea falla, Nextflow imprime un mensaje de error indicando la ruta de la carpeta donde ocurrió el fallo. Por ejemplo:
```
Error executing process > 'HISAT2_ALIGN (Muestra_01)'

Caused by:
  Process `HISAT2_ALIGN (Muestra_01)` terminated with an error exit status (1)

Command executed:
  hisat2 -x genoma -U muestra1.fq -S salida.sam

Work dir:
  /home/user/nextflow_project/work/5b/e7819fa2b3...
```

Si entras en esa carpeta (`cd work/5b/e7819fa2b3...`) y haces `ls -la`, encontrarás los archivos ocultos más importantes de Nextflow:

| Archivo Oculto | ¿Qué contiene? | ¿Para qué sirve? |
| :--- | :--- | :--- |
| **`.command.sh`** | El script exacto de Bash/Python que Nextflow ejecutó. | Puedes abrirlo para ver qué variables se expandieron o ejecutarlo a mano: `bash .command.sh`. |
| **`.command.run`** | El script envoltorio que configura el entorno, Docker y los límites de memoria. | Ver cómo Nextflow monta los volúmenes de Docker. |
| **`.command.log`** | Toda la salida combinada (stdout y stderr) producida por la tarea. | El registro completo de lo que pasó. |
| **`.command.err`** | Los mensajes de error específicos producidos por la herramienta (stderr). | **Aquí está la causa exacta del fallo.** |
| **`.command.out`** | La salida estándar de la consola (stdout). | Comprobar textos o números generados. |
| **`.exitcode`** | El código de salida del comando (0 si triunfó, 1+ si falló). | Diagnosticar por qué se detuvo. |

### 🛠️ Técnica de depuración rápida:
Cuando un proceso falle:
1. Abre `.command.err` para ver qué dijo la herramienta bioinformática:
   ```bash
   cat work/5b/e7819fa2b3.../.command.err
   ```
2. Si tienes dudas de cómo quedó el script tras evaluar las variables, abre `.command.sh`:
   ```bash
   cat work/5b/e7819fa2b3.../.command.sh
   ```

---

## 4. ¿Por qué a veces `-resume` no funciona cuando debería?

Si lanzas `-resume` y Nextflow vuelve a ejecutar pasos que ya estaban hechos, suele deberse a:
1. **Modificar un archivo de entrada:** Cambió la fecha de modificación o el checksum del fichero.
2. **Uso de rutas relativas flotantes:** Usar rutas que cambian según el directorio donde estés. Usa siempre rutas absolutas o canales `Channel.fromPath`.
3. **Scripts no deterministas:** Usar comandos que generan nombres con la fecha/hora actual (ej. `date > archivo.txt`).

---

| [⬅️ Módulo 3 - Test Autoevaluación](../Modulo_3_Modularidad_Config_y_Contenedores/Test_Autoevaluacion_M3.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Lección 11 - Manejo de Errores ➡️](Leccion_11_Manejo_de_Errores.md) |
| :--- | :---: | :---: |
