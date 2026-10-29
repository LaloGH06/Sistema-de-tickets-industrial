# ADR-001: Monolito modular en lugar de microservicios

## Estado
Aceptada, 20 de octubre de 2026

## Contexto
El Sistema de tickets industriales sirve a una planta de manufactura, transformación y distribución de productos plásticos, con oficinas, almacén, líneas de producción y taller de mantenimiento. Hoy los reportes de fallas viajan por recados, llamadas y grupos de WhatsApp: se pierden, no traen evidencia de que el trabajo se hizo y no se pueden medir los tiempos de respuesta.

Tiene dos tipos de usuario, el Usuario Operativo y el Administrador / Dirección, y el operativo lo usa desde su celular en el piso de planta. El sistema debe responder en máximo 2 segundos con conexión 4G y guardar un registro inmutable de cada cambio de estado.

Lo construye una sola persona. Las fronteras entre módulos ya están claras: Identidad y estructura, Operación (ciclo de vida del ticket y cronómetro de cada fase) y Archivos (validar, subir y guardar evidencia). Operación le pregunta a Identidad y a Archivos; Archivos no necesita hablar con ningún otro módulo.

Se evaluó separar cada módulo en un microservicio, pero con una sola persona construyéndolo eso significaría operar varios servicios en red sin nadie más que los mantenga.

## Decisión
Construimos un monolito modular: un solo programa y un solo desplegable, con los tres módulos separados por carpetas e interfaces, y con la regla de que ningún módulo lee las tablas de otro: lo que necesita de otro módulo se lo pide.

## Consecuencias
Un solo despliegue y un solo registro de errores, que es lo que una sola persona puede operar hoy. Las consultas entre módulos, como preguntar si ya hay evidencia antes de resolver un ticket, son llamadas dentro del mismo programa y no peticiones en red, lo que ayuda a cumplir los 2 segundos en 4G.

No se puede escalar un módulo por separado. Si los operadores suben muchos archivos pesados, el servidor se satura y se afecta el rendimiento del resto de los módulos.

Si algún día hay que separarlo, el primero en salir es Archivos. La señal será la saturación de recursos del servidor por archivos pesados que degrade a los demás módulos. Archivos es el que tiene la frontera más limpia, porque solo recibe peticiones de Operación.

El riesgo real es que, al ser una sola persona, se tome un atajo y se lean las tablas de otro módulo, y la frontera se pierda sin que nadie lo note. Hay que revisarlo en cada cambio que se suba al repositorio. Si el equipo crece, habrá que revisar esta decisión.
