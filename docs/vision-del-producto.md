# Visión del Producto

**Autor:** Jorge Eduardo García Hernández
**Fecha de la última versión:** 18 de agosto de 2026
**Repositorio:** https://github.com/LaloGH06/Sistema-de-tickets-industrial](https://github.com/LaloGH06/Sistema-de-tickets-industrial)

---

## 1. Descripción del sistema

**Nombre del sistema:** Plataforma de Gestión de Tickets
**Descripción:** Es una plataforma que ayuda a la empresa a registrar, organizar y dar seguimiento a los reportes de fallas o necesidades de mantenimiento (folios). Funciona como un centro de control donde cualquier empleado puede reportar un problema, asignarlo al área correspondiente y, lo más importante, supervisar cuánto tiempo tardan en solucionarlo mediante gráficas y medidores de tiempo.

---

## 2. Problema y usuarios

**El problema:** Las solicitudes de mantenimiento o reportes de fallas se pierden, no se sabe quién las está atendiendo ni cuánto tiempo toman en resolverse, lo que genera retrasos y falta de información clara para evaluar el desempeño del personal.
**Cómo se resuelve hoy sin el sistema:** Los empleados reportan los problemas de manera informal (por mensajes, llamadas o libretas), sin una forma estandarizada de exigir evidencia de que el trabajo realmente se hizo o de medir los tiempos de respuesta.

**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
| :--- | :--- | :--- |
| **Usuario Operativo (Emisor / Receptor)** | Poder crear un reporte fácilmente, subir fotos de la falla y tener un botón para aceptar o rechazar el trabajo final. | Que el sistema sea enredado de navegar o que cierren su reporte sin haber arreglado el problema. |
| **Administrador / Dirección** | Ver el tablero general con gráficas y estadísticas de los tiempos límite de cada departamento. | Que los usuarios dupliquen folios, llenen el sistema de "basura" o que la información se pierda. |

**Un conflicto entre usuarios:**
El Administrador quiere tener un control estricto y total sobre la información, requiriendo que los reportes tengan descripciones detalladas, áreas correctas y que nada se borre. Por otro lado, el Usuario Operativo quiere rapidez; a veces asigna los reportes al área equivocada por las prisas. Si bloqueamos la edición para complacer al Administrador, el Operativo se frustrará cuando se equivoque; pero si le damos mucha libertad al Operativo, el Administrador perderá el control de las métricas. (Esto se resolvió con la función de "Reasignar Ticket" que guarda el historial).

---

## 3. Alcance

*Instrucción: lo que escribes en "fuera del alcance" es lo que después evita que el proyecto crezca sin control. Sé específico: "reportes" no dice nada, "reportes de ventas mensuales exportables a PDF" sí.*

### Dentro del alcance

-
-
-
-

### Explícitamente fuera del alcance

-
-
-

**Por qué queda fuera:**

*Instrucción: para al menos una de las exclusiones, explica la razón. Puede ser tiempo, complejidad, o que no aporta al problema central.*

---

## 4. Tipo de sistema y restricciones

*Instrucción: identifica de qué tipo es tu sistema y qué te obliga a garantizar ese tipo. Un sistema de información y un sistema crítico no se diseñan igual.*

**Tipo de sistema:**

*(De información · Embebido · Crítico · Web y SaaS · De datos y análisis)*

**Por qué es de ese tipo:**

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| | | |
| | | |
| | | |

**Reglas de negocio que ya identifiqué:**

*Instrucción: reglas que no son obvias desde fuera y que alguien que conoce el dominio tendría que explicarte. Si no encuentras ninguna, tu caso puede ser demasiado simple.*

1.
2.
3.

---

## 5. Ciclo de vida elegido

*Instrucción: este apartado se trabaja en la semana 3, después de ver los modelos de desarrollo. La justificación pesa más que la elección: no hay un modelo correcto, hay uno defendible para tu caso.*

**Modelo elegido:**

**Por qué le conviene a este proyecto:**

*Instrucción: argumenta con las características reales de tu caso. Estabilidad de los requisitos, disponibilidad del cliente, nivel de riesgo, tamaño del equipo, frecuencia de entregas esperada.*

### Alternativas descartadas

**Alternativa 1:**

*Por qué la descarté:*

**Alternativa 2:**

*Por qué la descarté:*

---

## Antes de entregar

Reviso que el documento cumpla lo siguiente:

- [ ] La descripción del apartado 1 se entiende sin ser del área
- [ ] Hay al menos dos tipos de usuario con necesidades distintas
- [ ] Identifiqué un conflicto real entre usuarios
- [ ] El alcance dice qué queda fuera, no solo qué queda dentro
- [ ] Las exclusiones son específicas, no genéricas
- [ ] Identifiqué el tipo de sistema y al menos dos atributos de calidad
- [ ] Anoté al menos tres reglas de negocio no obvias
- [ ] Justifiqué el ciclo de vida contra dos alternativas descartadas
- [ ] El documento está en mi repositorio y se puede leer desde el navegador
- [ ] Borré todas las instrucciones en cursiva de la plantilla
