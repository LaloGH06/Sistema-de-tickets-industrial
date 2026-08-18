# Visión del Producto

**Autor:** Jorge Eduardo García Hernández
**Fecha de la última versión:** 18 de agosto de 2026
**Repositorio:** https://github.com/LaloGH06/Sistema-de-tickets-industrial](https://github.com/LaloGH06/Sistema-de-tickets-industrial)

## 1. Descripción del sistema

**Nombre del sistema:** Plataforma de Gestión de Tickets
**Descripción:** Es una plataforma que ayuda a la empresa a registrar, organizar y dar seguimiento a los reportes de fallas o necesidades de mantenimiento (folios). Funciona como un centro de control donde cualquier empleado puede reportar un problema, asignarlo al área correspondiente y, lo más importante, supervisar cuánto tiempo tardan en solucionarlo mediante gráficas y medidores de tiempo.

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

## 3. Alcance

**Dentro del alcance:**
* Creación de folios con niveles de prioridad, tiempos límite y carga de evidencia fotográfica.
* Flujo de validación donde el emisor puede aceptar (botón verde) o rechazar (botón amarillo) la solución entregada.
* Tablero con gráficas estadísticas por gerencias y coordinaciones.
* Directorio de personal con asignación de roles (Administrador o Usuario Operativo).
* Sistema de notificaciones (campana) para alertar sobre cambios en los folios.

**Explícitamente fuera del alcance:**
* Módulo de inventario y compra de refacciones para los mantenimientos.
* Chat en vivo o mensajería en tiempo real entre los usuarios.

**Por qué queda fuera:**
El sistema busca organizar los tiempos de respuesta y auditar que los trabajos se realicen. Si agregamos un sistema de inventario de piezas, el proyecto se volvería demasiado grande y complejo para el tiempo del semestre, desviándonos del problema central que es la gestión de tiempos y reportes.

## 4. Tipo de sistema y restricciones

**Tipo de sistema:**
De información

**Por qué es de ese tipo:**
Su función principal es recopilar, almacenar, procesar y mostrar datos operativos (folios, usuarios, tiempos y gráficas) para que la dirección pueda tomar decisiones informadas sobre el desempeño de la empresa.

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
| :--- | :--- | :--- |
| **Usabilidad (Facilidad de uso)** | El sistema será usado por personal de diferentes áreas, algunos sin conocimientos técnicos avanzados. Debe ser fácil navegar e incluir botones como "Volver". | Si el personal no le entiende, simplemente dejarán de usarlo y volverán a reportar por WhatsApp. |
| **Confiabilidad (Integridad de datos)** | Las gráficas y reportes dependen de que los tiempos y estados de los folios sean exactos, por eso se requiere respaldo en la nube y local. | Si se pierden folios o fallan las gráficas, la Dirección tomará decisiones equivocadas y el sistema perderá credibilidad. |

**Reglas de negocio que ya identifiqué:**
1. El ticket solo puede darse por terminado (cerrado) si la persona que lo creó da su aprobación mediante el botón verde de "Liberado". El área técnica no puede cerrarlo por sí misma.
2. Si un ticket es rechazado (botón amarillo), debe ser obligatorio ingresar instrucciones de corrección antes de regresarlo al área responsable.
3. La eliminación permanente de folios (por duplicidad o pruebas) es un permiso exclusivo del rol de Administrador, los usuarios operativos no pueden borrar tickets.

## 5. Ciclo de vida elegido

**Modelo elegido:**
Desarrollo Evolutivo 

**Por qué le conviene a este proyecto:**
En un entorno industrial, los usuarios (operadores y técnicos) a menudo no saben exactamente qué datos necesitan en los reportes o cómo se sienten más cómodos reportando fallas hasta que interactúan con el sistema. El modelo evolutivo nos permite construir un núcleo básico funcional (la creación y lectura de folios), ponerlo a disposición de los usuarios para recibir retroalimentación real, y a partir de ahí "evolucionar" y refinar la interfaz y la lógica. Esto es ideal porque los requisitos van a cambiar y madurar conforme la planta empiece a usar la herramienta.

**Alternativas descartadas**

**Alternativa 1:** Modelo Incremental
**Por qué la descarté:** Aunque suena parecido al evolutivo, el modelo incremental asume que la arquitectura y todos los requisitos ya se conocen y están claros desde el principio, limitándose a entregar el sistema por "bloques" o módulos terminados. En nuestro caso, si entregamos un bloque 100% terminado y los operadores piden cambios al probarlo, la estructura incremental dificulta esas modificaciones profundas.

**Alternativa 2:** Modelo en Cascada 
**Por qué la descarté:** Requiere que todos los procesos de la planta y los roles de usuario estén congelados en la fase de diseño. Si al ver el sistema funcionando los gerentes nos piden cambiar el flujo del botón "Rechazar ticket" o modificar la forma de calcular el SLA, en el modelo de Cascada sería muy costoso y rígido regresar a las fases iniciales para arreglarlo.
