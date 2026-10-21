[⬅️ Lección 6 - Agrupación y Ramificación](Leccion_06_Agrupacion_y_Ramificacion.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test Autoevaluación M2 ➡️](Test_Autoevaluacion_M2.md)

---

# 🏋️ Módulo 2 - Ejercicios Prácticos de Operadores y Canales

Pon a prueba los conceptos avanzados de transformación, agrupación y bifurcación implementando los siguientes tres ejercicios en archivos `.nf` independientes.

---

### Ejercicio 1: Limpieza y estructuración con `.map` y `.filter`
**Objetivo:** Procesar una lista de metadatos hospitalarios simulados y preparar el canal para bioestadística.

**Instrucciones:**
Dado el siguiente canal de entrada:
```groovy
ch_pacientes = Channel.of(
    [id: 'PAC_001', edad: 65, estado: 'UCI',     apache_score: 18, archivo: 'p1_datos.csv'],
    [id: 'PAC_002', edad: 42, estado: 'PLANTA',  apache_score: 8,  archivo: 'p2_datos.csv'],
    [id: 'PAC_003', edad: 71, estado: 'UCI',     apache_score: 24, archivo: 'p3_datos.csv'],
    [id: 'PAC_004', edad: 19, estado: 'PLANTA',  apache_score: 4,  archivo: 'p4_datos.csv'],
    [id: 'PAC_005', edad: 58, estado: 'UCI',     apache_score: 12, archivo: 'p5_datos.csv']
)
```
1. Utiliza `.filter` para seleccionar **únicamente a los pacientes que están en `'UCI'` y tienen más de 60 años**.
2. Encadena un operador `.map` para transformar cada elemento restante en una tupla simplificada:
   `[ id, [edad: <edad>, severidad: 'GRAVE'], archivo ]`
3. Muestra la salida con `.view()`.

---

### Ejercicio 2: Reasociación de réplicas técnicas con `.groupTuple`
**Objetivo:** Simular un escenario habitual en secuenciación NGS donde un paciente fue secuenciado en dos carriles distintos y deben fusionarse.

**Instrucciones:**
Dado el canal de carriles:
```groovy
ch_lanes = Channel.of(
    ['Paciente_A', 'carril_L001_R1.fq.gz'],
    ['Paciente_B', 'carril_L001_R1.fq.gz'],
    ['Paciente_A', 'carril_L002_R1.fq.gz'],
    ['Paciente_C', 'carril_L001_R1.fq.gz'],
    ['Paciente_B', 'carril_L002_R1.fq.gz']
)
```
1. Aplica `.groupTuple()` para que cada paciente tenga una lista con todos sus archivos asociados.
2. Añade un operador `.map` para contar cuántos carriles tiene cada paciente, emitiendo una tupla del estilo:
   `[ Paciente_ID, numero_de_carriles, lista_de_archivos ]`
3. Muestra el resultado por consola.

---

### Ejercicio 3: Bifurcación de lecturas con `.branch`
**Objetivo:** Separar un flujo mixto de lecturas *Single-End* y *Paired-End* para dirigirlos a diferentes procesos.

**Instrucciones:**
Dado el canal:
```groovy
ch_mixto = Channel.of(
    [ [id: 'S1', single_end: false], ['s1_1.fq.gz', 's1_2.fq.gz'] ],
    [ [id: 'S2', single_end: true],  ['s2.fq.gz'] ],
    [ [id: 'S3', single_end: false], ['s3_1.fq.gz', 's3_2.fq.gz'] ],
    [ [id: 'S4', single_end: true],  ['s4.fq.gz'] ]
)
```
1. Utiliza el operador `.branch` para dividir el canal en dos ramas: `single` y `paired`.
2. Asigna el resultado con `.set { ch_evaluado }`.
3. Muestra los elementos de `ch_evaluado.single` con un mensaje indicando *"Modo SE: <id>"* y los de `ch_evaluado.paired` con *"Modo PE: <id>"*.

---

| [⬅️ Lección 6 - Agrupación y Ramificación](Leccion_06_Agrupacion_y_Ramificacion.md) | 🏠 [Temario Principal](../README.md) | [Siguiente: Test Autoevaluación M2 ➡️](Test_Autoevaluacion_M2.md) |
| :--- | :---: | :---: |
