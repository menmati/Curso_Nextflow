[⬅️ Lección 12 - Reportes y Cloud](Leccion_12_Reportes_y_Cloud_Seqera.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test Autoevaluación M4 ➡️](Test_Autoevaluacion_M4.md)

---

# 🏋️ Módulo 4 - Ejercicios Prácticos de Depuración y Resiliencia

---

### Ejercicio 1: Experimentando con `-resume` y la Caché
**Objetivo:** Comprobar el funcionamiento del mecanismo de reanudación y entender qué invalida la caché.

**Instrucciones:**
1. Crea un pipeline con dos procesos consecutivos:
   - `PASO_LENTO`: Simula un proceso pesado ejecutando `sleep 5` en Bash y generando un archivo `intermedio.txt`.
   - `PASO_RAPIDO`: Lee `intermedio.txt` y le añade una línea final en `resultado_final.txt`.
2. Ejecuta el pipeline por primera vez:
   ```bash
   nextflow run ejercicio1_m4.nf
   ```
   (Tardará unos 5 segundos en completarse).
3. Vuelve a ejecutarlo de inmediato añadiendo `-resume`:
   ```bash
   nextflow run ejercicio1_m4.nf -resume
   ```
   Comprueba que tarda 0 segundos y que ambas tareas aparecen marcadas con `[Cached]`.
4. Modifica únicamente el texto del segundo proceso (`PASO_RAPIDO`) y vuelve a lanzar con `-resume`. Observa cómo `PASO_LENTO` se recupera de la caché sin esperar los 5 segundos y solo se ejecuta `PASO_RAPIDO`.

---

### Ejercicio 2: Auto-recuperación dinámica con `errorStrategy = 'retry'`
**Objetivo:** Diseñar un proceso que falle a propósito en el primer intento pero triunfe en el segundo gracias a un reintento dinámico.

**Instrucciones:**
1. Crea un proceso llamado `PROCESO_RESILIENTE`.
2. Configura las directivas:
   - `errorStrategy 'retry'`
   - `maxRetries 2`
3. En el bloque `script:`, utiliza la variable intrínseca `task.attempt`:
   ```groovy
   script:
   """
   echo "Ejecutando intento número: ${task.attempt}"
   if [ "${task.attempt}" -eq 1 ]; then
       echo "Simulando fallo por falta de memoria en el intento 1..." >&2
       exit 137
   fi
   echo "¡Éxito en el intento 2!" > exito.txt
   """
   ```
4. Ejecuta el script y comprueba en la terminal cómo Nextflow detecta el fallo del intento 1, reintenta automáticamente la tarea y finaliza con éxito.

---

### Ejercicio 3: Generación e Inspección de Reportes de Telemetría
**Objetivo:** Obtener las métricas visuales completas de una ejecución.

**Instrucciones:**
1. Toma el pipeline del Ejercicio 1 y lánzalo solicitando la generación de los reportes HTML:
   ```bash
   nextflow run ejercicio1_m4.nf -with-report reporte.html -with-timeline timeline.html -with-trace trace.txt
   ```
2. Abre `reporte.html` en tu navegador web.
3. Identifica en el informe:
   - ¿Qué porcentaje de CPU y qué memoria RAM pico consumió cada proceso?
   - ¿En qué carpeta exacta de `work/` se ejecutó cada tarea?
   - Analiza los 3 primeros registros del archivo de texto `trace.txt`.

---

| [⬅️ Lección 12 - Reportes y Cloud](Leccion_12_Reportes_y_Cloud_Seqera.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Test Autoevaluación M4 ➡️](Test_Autoevaluacion_M4.md) |
| :--- | :---: | :---: |
