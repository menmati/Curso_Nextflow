[⬅️ Lección 10 - Mecanismo Resume y Debug](Leccion_10_Mecanismo_Resume_y_Debug.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 12 - Reportes y Cloud ➡️](Leccion_12_Reportes_y_Cloud_Seqera.md)

---

# 📘 Módulo 4 - Lección 11: Manejo de Errores y Tolerancia Dinámica a Fallos

En entornos de producción (hospitales, farmacéuticas o secuenciación masiva en clústeres), que un proceso falle por falta de memoria o por un corte puntual de red **no puede detener un análisis que lleva 2 días corriendo**.

Nextflow ofrece un sistema de tolerancia a fallos extremadamente sofisticado.

---

## 1. La Directiva `errorStrategy`

Por defecto, si cualquier tarea falla (código de salida != 0), Nextflow detiene inmediatamente todo el pipeline (`errorStrategy = 'terminate'`).

Puedes modificar este comportamiento con las siguientes estrategias:

| Estrategia | Comportamiento ante un error | Caso de uso bioinformático |
| :--- | :--- | :--- |
| `'terminate'` | Detiene todo el pipeline al instante (por defecto). | Errores de sintaxis o fallos de referencias críticas. |
| `'finish'` | Espera a que las tareas que ya están corriendo terminen, pero no inicia nuevas. | Salida controlada para salvar datos en curso. |
| `'ignore'` | Ignora el error de esa muestra, la descarta y continúa con el resto. | Una muestra dañada de 500 no debe parar el estudio completo. |
| `'retry'` | Vuelve a intentar ejecutar la tarea automáticamente. | Fallos intermitentes de red, saturación o falta de memoria. |

---

## 2. Reintentos con `maxRetries`

Si utilizas `errorStrategy = 'retry'`, debes especificar cuántas veces como máximo puede reintentarse antes de darse por vencido:

```groovy
process ALINEAR {
    errorStrategy 'retry'
    maxRetries 3

    // ...
}
```

---

## 3. Escalado Dinámico de Recursos (`Dynamic Resource Scaling`)

Este es uno de los trucos más impresionantes de Nextflow:

¿Qué pasa si asignas 4 GB de RAM a un proceso, pero una muestra especialmente densa necesita 6 GB?
Normalmente, el kernel de Linux o Docker matará el proceso con el código de error `137` (Out Of Memory / OOM Killer).

En Nextflow puedes definir los recursos como una **función dinámica de** `task.attempt` (el número de intento actual):

```groovy
process ENSAMBLAR_TRANSCRIPTO {
    errorStrategy 'retry'
    maxRetries 3

    // Si task.attempt es 1 -> 4 GB
    // Si falla y task.attempt es 2 -> 8 GB
    // Si falla y task.attempt es 3 -> 12 GB
    memory = { 4.GB * task.attempt }
    cpus   = { 2 * task.attempt }
    time   = { 1.hour * task.attempt }

    // ...
}
```

¡El pipeline se auto-repara solo! Empieza gastando pocos recursos (ahorrando costes en la nube de AWS) y solo aumenta la memoria para las muestras rebeldes que realmente la necesitan.

---

## 4. Reintentar solo en errores específicos (Código 137 / 143)

No tiene sentido reintentar un error de comando no encontrado (`exit code 127`) o archivo inexistente (`exit code 1`). Solo quieres reintentar fallos de memoria (`137` o `143`):

```groovy
// nextflow.config
process {
    errorStrategy = { task.exitStatus in [143, 137, 104, 134, 139] ? 'retry' : 'finish' }
    maxRetries    = 3
    memory        = { 4.GB * task.attempt }
}
```

Explicación:

- Si el código de salida es `137` o `143` (típicos de exceso de memoria), Nextflow aplica `'retry'` y dobla la memoria.
- Si el código de salida es cualquier otro (error de sintaxis en el script), aplica `'finish'` y avisa al usuario sin gastar cómputo inútilmente.

---

| [⬅️ Lección 10 - Mecanismo Resume y Debug](Leccion_10_Mecanismo_Resume_y_Debug.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Lección 12 - Reportes y Cloud ➡️](Leccion_12_Reportes_y_Cloud_Seqera.md) |
| :--- | :---: | :---: |
