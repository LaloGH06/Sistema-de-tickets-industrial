# Análisis de sustantivos: Sistema de tickets industriales

## Paso 1. Sustantivos subrayados (sin filtrar)

acceso, administrador, alerta, área, archivo adjunto, auditoría, bandeja, base de datos, botón, celular, chat, código de folio, comunicados, contador, contraseña, correo electrónico, credenciales, cuenta, departamento, dependencias, detalle, dispositivo, "doble visto bueno", empleado, emisor original, encargado, estadística, estado, etiqueta, evidencia, evidencia documental, evidencia fotográfica, falla, fecha, folio único, formulario de rechazo, grupos de WhatsApp, historial, hoja de papel, hora exacta, ID del creador, identificador, identificador del usuario, incidencia, interfaz, llamadas, mensaje de alerta, métricas, nivel de prioridad, nómina, notas de corrección, operador, órdenes de trabajo, pantalla, pantalla principal, porcentaje de SLA, problema, proceso, prototipo, refacciones, registro, receptor, reportes, rol, sesión, sistema, SLA, SMS, tablero general (Dashboard), tareas, técnico, tiempo de resolución, tiempo límite, tiempos reales de respuesta, ticket, transacciones, turnos compartidos, usuario, usuario operativo, ventana modal, WhatsApp.

## Paso 2. Clases candidatas (4)

Se aplicó el filtro de las cuatro razones: es atributo, es sinónimo de otra, está fuera del sistema o es demasiado vago.

| Clase | Sustantivos de los que sale | Por qué pasó el filtro |
| :--- | :--- | :--- |
| Ticket | ticket, folio único, órdenes de trabajo, falla, problema, incidencia, reporte, tareas | Tiene identidad (folio único inalterable), atributos propios y operaciones fundamentales (cambiar de estado, validar evidencia) que dictan la lógica central del sistema. |
| Usuario | usuario, usuario operativo, administrador, empleado, emisor original, técnico, receptor, encargado, operador | Tiene identidad (ID, credenciales), un rol que define sus permisos (RNF-SEG-001) y ejecuta las acciones principales del sistema. |
| Área | área, departamento | Agrupa a los usuarios y es el destino hacia donde se reasignan los tickets (RF-010). |
| SLA | SLA | Alberga las reglas de negocio, los límites de tiempo según prioridad y la lógica para iniciar, detener, reiniciar y calcular porcentajes de cumplimiento (RF-004, RF-008, RF-009). |

## Paso 3. Qué sabe y qué hace cada clase

| Clase | Sabe | Hace |
| :--- | :--- | :--- |
| Ticket | Folio, estado (abierto, en proceso, resuelto, cerrado, reabierto), nivel de prioridad, notas de corrección, archivo de evidencia. | Cambia de estado, registra la marca de tiempo exacta de cada movimiento, impide la eliminación de sus propios registros. |
| Usuario | Credenciales (usuario, contraseña), rol (Administrador, Operativo), ID, área a la que pertenece. | Inicia y cierra sesión, genera tickets, atiende tickets, aprueba o rechaza soluciones (según su rol). |
| Área | Nombre del departamento. | Recibe tickets reasignados. |
| SLA | Tiempo límite permitido (por prioridad), reglas métricas. | Inicia, detiene y reinicia el cálculo del cronómetro de tiempo activo; calcula el porcentaje semanal de cumplimiento. |

## Paso 4. Sustantivos descartados y por qué

| Razón | Descartados | Qué son realmente |
| :--- | :--- | :--- |
| Es un atributo | Código de folio, estado, nivel de prioridad, notas de corrección, archivo adjunto, evidencia, evidencia fotográfica, evidencia documental. | Datos y componentes del Ticket. |
| | Credenciales, contraseña, identificador, ID del creador, rol, sesión. | Datos del Usuario. |
| | Tiempo límite, tiempo de resolución, tiempos reales de respuesta, porcentaje de SLA, contador. | Datos o cálculos derivados del SLA. |
| | Fecha, hora exacta, mensaje de alerta, etiqueta. | Atributos generales de estado o del registro de auditoría. |
| Es sinónimo de otra | Usuario operativo, administrador, empleado, emisor original, técnico, receptor, encargado, operador. | Todos refieren a la clase Usuario (diferenciados solo por el atributo de su rol o contexto). |
| | Folio único, órdenes de trabajo, falla, problema, incidencia, reporte, tareas. | Todos refieren o son parte de la clase Ticket. |
| | Área, departamento. | Área. |
| Está fuera del sistema | Grupos de WhatsApp, WhatsApp, SMS, celular, dispositivo, llamadas, hoja de papel. | Herramientas y sistemas de comunicación externos que el sistema reemplaza o en los que opera, pero que no gestiona internamente. |
| | Correo electrónico, nómina, refacciones. | Sistemas, procesos y entidades externas fuera del alcance del sistema. |
| Es demasiado vago | Sistema, prototipo, interfaz, pantalla, pantalla principal, tablero general (Dashboard), ventana modal, botón, cuenta, base de datos. | Elementos de la interfaz gráfica, infraestructura o términos genéricos del software. |
| | "Doble visto bueno", auditoría, métricas, estadística, detalle, chat, historial, registro, transacciones, proceso, comunicados, dependencias, turnos compartidos. | Conceptos abstractos, vistas de la interfaz o conjuntos de información, sin datos ni identidad propios. |
| | Acceso, alerta. | Acciones o eventos sin entidad propia como clase. |
