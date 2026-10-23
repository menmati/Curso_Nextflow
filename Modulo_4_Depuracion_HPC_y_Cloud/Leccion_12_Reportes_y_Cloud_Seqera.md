[⬅️ Lección 11 - Manejo de Errores](Leccion_11_Manejo_de_Errores.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Ejercicios M4 ➡️](Ejercicios_M4.md)

---

# 📘 Módulo 4 - Lección 12: Reportes Automáticos, Computación Cloud y Seqera Platform

En el sector biofarmacéutico y hospitalario no solo importa que el pipeline termine: importa **demostrar cuánto cómputo se usó, cuánto costó y que los resultados son 100% trazables y auditables**.

Nextflow incorpora un motor de telemetría y generación de informes de nivel industrial.

---

## 1. Los 4 Reportes Nativos de Nextflow

Al lanzar cualquier pipeline, puedes añadir 4 banderas (*flags*) que generarán informes interactivos en HTML:

```bash
nextflow run main.nf \
    -profile docker \
    -with-report reporte_ejecucion.html \
    -with-timeline cronograma_tiempos.html \
    -with-trace matriz_metricas.txt \
    -with-dag grafo_pipeline.html
```

### ¿Qué contiene cada reporte?

1. `-with-report reporte.html`**:**
   - Un dashboard interactivo espectacular en HTML.
   - Gráficos de distribución de memoria real vs reservada, uso de CPU por proceso, tiempos de lectura/escritura en disco (I/O) y porcentaje de tareas exitosas.
   - Ideal para adjuntar en informes de auditoría o entregas a clientes.
2. `-with-timeline timeline.html`**:**
   - Un diagrama de Gantt que muestra exactamente cuándo empezó y terminó cada proceso y tarea en paralelo a lo largo del tiempo.
   - Te permite identificar cuellos de botella (ej. qué proceso está retrasando a los demás).
3. `-with-trace trace.txt`**:**
   - Una tabla con métricas crudas de cada tarea (PID, nombre del proceso, código de salida, memoria real consumida en bytes, tiempo en cola, duración exacta).
4. `-with-dag grafo.html`**:**
   - Genera el **DAG (Directed Acyclic Graph)** del pipeline: un diagrama visual interactivo que muestra cómo se conectan todos los canales y procesos de principio a fin.

---

## 2. Configuración Permanente de Reportes en `nextflow.config`

En lugar de escribir las banderas a mano en la terminal cada vez, puedes dejarlas configuradas por defecto:

```groovy
// nextflow.config
report {
    enabled   = true
    file      = "${params.outdir}/pipeline_info/execution_report.html"
    overwrite = true
}

timeline {
    enabled   = true
    file      = "${params.outdir}/pipeline_info/execution_timeline.html"
    overwrite = true
}

trace {
    enabled   = true
    file      = "${params.outdir}/pipeline_info/execution_trace.txt"
    overwrite = true
}

dag {
    enabled   = true
    file      = "${params.outdir}/pipeline_info/pipeline_dag.html"
    overwrite = true
}
```

---

## 3. ¿Qué es Seqera Platform (anteriormente Nextflow Tower)?

**Seqera Platform** es la plataforma SaaS/On-Premise que monitoriza, gestiona y ejecuta pipelines de Nextflow en la nube:

- **Monitorización en tiempo real:** Ver qué tareas están corriendo en AWS/GCP desde una interfaz web con barras de progreso en vivo.
- **Launchpad:** Permite que médicos o investigadores sin conocimientos de terminal lancen pipelines seleccionando parámetros en un formulario web amigable.
- **Gestión de credenciales seguras:** Conecta clústeres de AWS Batch, Azure o Google Cloud sin exponer contraseñas en los servidores.

Para conectar cualquier ejecución local con Seqera Platform basta con añadir:

```bash
nextflow run main.nf -with-tower
```

---

## 4. Conceptos Clave de AWS Cloud con Nextflow

Para ejecutar en Amazon Web Services (AWS), Nextflow se integra con:

- **Amazon S3 (**`s3://mi-bucket/`**):** Nextflow puede leer y escribir archivos FASTQ directamente desde buckets de S3 sin descargarlos a tu disco local.
- **AWS Batch:** Servicio de Amazon que levanta máquinas virtuales (instancias EC2) bajo demanda, ejecuta los contenedores Docker de cada proceso y apaga las máquinas al terminar para ahorrar costes.
- **Fusion File System:** Tecnología puntera desarrollada por Seqera que acelera hasta 3 veces la velocidad de lectura/escritura de datos NGS entre S3 y las máquinas de cálculo.

---

| [⬅️ Lección 11 - Manejo de Errores](Leccion_11_Manejo_de_Errores.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Ejercicios M4 ➡️](Ejercicios_M4.md) |
| :--- | :---: | :---: |
