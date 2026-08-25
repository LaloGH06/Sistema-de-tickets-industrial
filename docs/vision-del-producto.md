# Visión del Producto

**Autor:** Jorge Eduardo García Hernández
**Fecha de la última versión:** 18 de agosto de 2026
**Repositorio:** https://github.com/LaloGH06/Sistema-de-tickets-industrial

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

### Dentro del alcance

- Registra de folios con descripción técnica, nivel de prioridad, área asignada, límite de tiempo  y archivos adjuntos.
- Reasigna el ticket a otra coordinación o área operativa cuando el emisor se equivoca al clasificarlo en el levantamiento.
- Almacena informes de solución técnica junto con fotografías de evidencia cargadas por el personal ejecutor.
- Reabre folios resueltos marcándolos como No Aceptado, guardando notas de corrección y archivos de guía para el personal.
- Grafica en tiempo real la cantidad de folios activos y resueltos desglosados por gerencia y coordinación en el tablero general.

### Explícitamente fuera del alcance

- No procesa pagos, compras de refacciones ni cotizaciones de materiales requeridos para atender el folio.
- No envía notificaciones por SMS ni mensajes de WhatsApp al personal en campo las alertas ocurren únicamente dentro de la plataforma web.
- No registra asistencia laboral, checador biométrico ni cálculo de nómina de los colaboradores registrados en el módulo de personal.

**Por qué queda fuera:**

Sobre la exclusión de compras y pagos:
Queda fuera porque el propósito del sistema es la comunicación técnica y el cumplimiento de tiempos de respuesta en planta. Integrar pagos o compras involucraría facturación fiscal y conexión con sistemas bancarios, lo cual duplicaría la complejidad del proyecto y desviaría el objetivo central de resolución de incidencias.

### Funcionalidad futura (fuera de alcance actual)

- **Diagnóstico predictivo de fallas con IA:** Analizar mediante visión por computadora la fotografía de la falla adjunta en el ticket para sugerir automáticamente la pieza a reparar y el técnico más capacitado para atenderla.

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
