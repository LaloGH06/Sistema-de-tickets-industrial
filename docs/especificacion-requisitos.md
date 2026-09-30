# Especificación de requisitos

**Sistema:** Sistema de tickets industriales
**Autor:** Jorge Eduardo García Hernández
**Versión:** 1.3
**Fecha de la última actualización:** 29 de septiembre de 2026

---

## 1. Propósito y alcance

**Propósito del documento:**
El Sistema de tickets industriales es una plataforma digital diseñada para centralizar, organizar y auditar las órdenes de trabajo y comunicados internos de una planta de producción industrial. Funciona como un centro de control donde cualquier empleado puede reportar un problema, asignarlo y supervisar los tiempos de resolución mediante métricas de SLA.

**Alcance del sistema:**
Registro de folios con descripción y prioridad, reasignación de tickets conservando el historial, carga obligatoria de evidencia (fotográfica y documental), validación de "doble visto bueno", y visualización en tiempo real de métricas en un tablero general.

**Fuera del alcance:**
El sistema no procesa pagos de nómina, no gestiona compras o cotizaciones de refacciones, no envía notificaciones por SMS/WhatsApp, y no funge como checador biométrico de asistencia.

---

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
| :--- | :--- | :--- |
| **Usuario Operativo (Emisor/Receptor)** | Reporta fallas por grupos de WhatsApp, llamadas o verbalmente. Las tareas se quedan varadas si el encargado no está en planta. | Poder levantar reportes rápidos desde su celular, adjuntar evidencia fotográfica de forma sencilla y reasignar tareas a otras áreas sin perder el folio. |
| **Administrador / Dirección** | Revisa chats buscando "palomitas azules" para saber quién leyó instrucciones. Recibe trabajos verbalmente como "terminados" pero incompletos. | Ver un tablero con estadísticas de SLA, auditar los tiempos reales de respuesta, y asegurar que los trabajos tengan evidencia comprobable antes de cerrarse. |

**Conflictos identificados entre usuarios:**
El Administrador quiere un control estricto para sus métricas. El Operativo quiere rapidez y suele equivocarse al asignar el área. Se resolvió implementando la función de "Reasignar Folio" (conservando historial) en lugar de obligar al operativo a borrar y empezar de cero.

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre del Requisito (Infinitivo) | Prioridad | Origen |
| :--- | :--- | :--- | :--- |
| **RF-001** | Iniciar sesión en la plataforma | Imprescindible | Derivado de la necesidad de identificar al responsable |
| **RF-002** | Cerrar sesión en la plataforma | Imprescindible | Derivado del control de acceso por turnos compartidos |
| **RF-003** | Generar ticket con folio único | Imprescindible | Entrevista (Problema de rastreo en WhatsApp) |
| **RF-004** | Consultar tablero general (Dashboard) | Imprescindible | Visión del producto (Métricas en tiempo real) |
| **RF-005** | Cambiar estado de ticket a en progreso | Importante | Regla de negocio operativa |
| **RF-006** | Entregar evidencia obligatoria | Imprescindible | Entrevista (Trabajos reportados verbalmente a medias) |
| **RF-007** | Rechazar solución de ticket | Imprescindible | Entrevista (Fallas reincidentes a los pocos días) |
| **RF-008** | Reabrir ticket rechazado | Imprescindible | Entrevista (Fallas reincidentes a los pocos días) |
| **RF-009** | Cerrar ticket validado | Imprescindible | Regla de negocio de "Doble visto bueno" |
| **RF-010** | Reasignar folio entre áreas | Importante | Entrevista (Descubrimiento: Ausencias y dependencias) |

### 3.2 Fichas

**RF-001 - Iniciar sesión en la plataforma**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema autentica las credenciales del usuario para permitir el acceso a la plataforma según su rol asignado. |
| **Origen** | Derivado de la necesidad de identificar al responsable. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | - Al ingresar un usuario y contraseña válidos, el sistema inicia la sesión en la interfaz correspondiente a su rol.<br>- Si las credenciales son incorrectas, el sistema bloquea el acceso.<br>- Al bloquear el acceso por credenciales incorrectas, el sistema despliega el mensaje "Credenciales inválidas". |
| **Relacionado con** | RF-002 |

**RF-002 - Cerrar sesión en la plataforma**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema finaliza la sesión activa del usuario actual para liberar el dispositivo para el siguiente turno. |
| **Origen** | Derivado del control de acceso por turnos compartidos. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | - Al presionar el botón "Cerrar sesión", el sistema finaliza la sesión activa de la cuenta.<br>- Al finalizar la sesión, el sistema redirige al usuario a la pantalla principal de acceso. |
| **Relacionado con** | RF-001 |

**RF-003 - Generar ticket con folio único**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema registra una nueva incidencia asignando un folio identificador único e inalterable. |
| **Origen** | Entrevista (Problema de rastreo en WhatsApp). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | - Al presionar "Generar Folio" con los campos obligatorios llenos, el sistema guarda el registro en la base de datos.<br>- Al guardar el registro exitosamente, el sistema despliega en pantalla el código de folio generado.<br>- Si el usuario intenta guardar sin llenar un campo obligatorio, el sistema bloquea la generación del folio.<br>- Al bloquear la generación por campos vacíos, el sistema marca los recuadros faltantes en color rojo. |
| **Relacionado con** | RF-006, RF-010 |

**RF-004 - Consultar tablero general (Dashboard)**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema muestra las métricas de cumplimiento SLA y tickets activos desglosados por área. |
| **Origen** | Visión del producto (Métricas en tiempo real). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | - Al ingresar a la vista del Dashboard, el sistema carga en pantalla los datos actualizados de tickets activos.<br>- Al cargar el Dashboard, el sistema muestra el cálculo numérico del porcentaje de SLA de la semana en curso. |
| **Relacionado con** | RF-003, RF-009 |

**RF-005 - Cambiar estado de ticket a en progreso**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema actualiza el estado de un ticket a 'En proceso' una vez que un operativo comienza a atenderlo. |
| **Origen** | Regla de negocio operativa. |
| **Prioridad** | Importante |
| **Criterio de aceptación** | - Al presionar el botón "Atender", el sistema cambia la etiqueta de estado a 'En proceso'.<br>- Al registrar el cambio de estado, el sistema anota la hora exacta de inicio de atención en el historial del ticket. |
| **Relacionado con** | RF-003, RF-006 |

**RF-006 - Entregar evidencia obligatoria**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema exige la carga de un archivo fotográfico o documental para permitir el cambio de estado a 'Resuelto'. |
| **Origen** | Entrevista (Trabajos reportados verbalmente a medias). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | - Si se presiona "Confirmar Resolución" sin un archivo adjunto, el sistema bloquea el cambio de estado.<br>- Al bloquear la acción por falta de archivo, el sistema despliega el mensaje de alerta "Obligatorio adjuntar evidencia".<br>- Al detectar un archivo adjunto válido, el sistema permite cambiar el estado del ticket a 'Resuelto'. |
| **Relacionado con** | RF-005, RF-007, RF-009 |

**RF-007 - Rechazar solución de ticket**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema despliega un campo de texto obligatorio para que el emisor original justifique la inconformidad con el trabajo reportado. |
| **Origen** | Entrevista (Fallas reincidentes a los pocos días). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | - Al presionar el botón "No Aceptado", el sistema despliega una ventana modal con un campo de notas.<br>- Si el campo de notas se encuentra vacío, el sistema bloquea el envío del formulario de rechazo. |
| **Relacionado con** | RF-006, RF-008 |

**RF-008 - Reabrir ticket rechazado**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema actualiza el estado del ticket a 'Reabierto' tras confirmarse el ingreso de una nota de rechazo. |
| **Origen** | Entrevista (Fallas reincidentes a los pocos días). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | - Al guardar exitosamente las notas de corrección, el sistema cambia el estado del ticket a 'Reabierto'.<br>- Al cambiar el estado a 'Reabierto', el sistema reinicia a cero el cronómetro de cálculo de SLA. |
| **Relacionado con** | RF-007 |

**RF-009 - Cerrar ticket validado**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema finaliza el ciclo de vida del ticket cambiando su estado a 'Cerrado' tras la validación del emisor original. |
| **Origen** | Regla de negocio de "Doble visto bueno". |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | - Al presionar el botón "Liberado", el sistema cambia el estado del ticket permanentemente a 'Cerrado'.<br>- Al actualizar el estado a 'Cerrado', el sistema detiene el contador de tiempo del SLA de forma definitiva. |
| **Relacionado con** | RF-006 |

**RF-010 - Reasignar folio entre áreas**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema transfiere la visibilidad de un ticket a otra área manteniendo su número de folio intacto. |
| **Origen** | Entrevista (Descubrimiento: Ausencias y dependencias). |
| **Prioridad** | Importante |
| **Criterio de aceptación** | - Al seleccionar una nueva área y confirmar, el sistema mueve el ticket hacia la bandeja del departamento destino.<br>- Al concretar el traslado, el sistema inserta un registro automático sobre el cambio de área en el historial del chat. |
| **Relacionado con** | RF-003 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
| :--- | :--- | :--- | :--- | :--- |
| **RNF-USA-001** | Usabilidad | Pasos máximos para resolución de ticket | Imprescindible | Visión del Producto y confirmación en entrevista |
| **RNF-SEG-001** | Seguridad | Restricción de liberación por roles | Imprescindible | Regla de negocio operativa (Doble visto bueno) |
| **RNF-CON-001** | Confiabilidad / Trazabilidad | Registro inmutable de auditoría por ticket | Imprescindible | Derivado del tipo de sistema (Sistemas de Información) |
| **RNF-REN-001** | Rendimiento | Tiempo límite de actualización de estado | Imprescindible | Derivado del uso operativo en dispositivos móviles |

### 4.2 Fichas

**RNF-USA-001 - Pasos máximos para resolución de ticket**
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Usabilidad |
| **Descripción** | El proceso completo para reportar una solución y subir la evidencia desde la vista de detalle se realiza en menos de cuatro pasos de navegación en la pantalla táctil. |
| **Métrica** | Un máximo de 4 clics o toques de pantalla desde que se presiona "Entregar Solución" hasta la visualización del estado 'Resuelto'. |
| **Origen** | Confirmado en entrevista (Descubrimiento de uso en móviles en piso de planta). |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Los operativos utilizan equipo de seguridad y operan en zonas con ruido. Si el software requiere demasiados pasos, abandonarán el sistema para reportar verbalmente. |
| **Afecta a** | RF-005, RF-006 |

**RNF-SEG-001 - Restricción de liberación por roles**
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Seguridad (Control de Acceso) |
| **Descripción** | El sistema restringe la acción de aprobar o rechazar un ticket (botones 'Liberado' y 'No Aceptado') únicamente al usuario emisor original del reporte o a los usuarios con rol de Administrador. |
| **Métrica** | 0% de accesos permitidos a las acciones de cierre definitivo desde sesiones con rol operativo que no coincidan con el ID del creador del folio. |
| **Origen** | Regla de negocio (Doble visto bueno). |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Evita que el técnico ejecutor apruebe su propio trabajo sin la validación del solicitante, garantizando una auditoría cruzada sin conflictos de interés. |
| **Afecta a** | RF-007, RF-008, RF-009 |

**RNF-CON-001 - Registro inmutable de auditoría por ticket**
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Confiabilidad (Trazabilidad e Integridad de datos) |
| **Descripción** | Todo cambio de estado, generación de ticket, entrega de evidencia o reasignación almacena automáticamente fecha, hora exacta y el identificador del usuario, impidiendo la eliminación posterior del registro. |
| **Métrica** | 100% de las transacciones guardan marca de tiempo (1 segundo de precisión) e ID de usuario, bloqueando comandos de eliminación (DELETE) en la tabla principal de folios. |
| **Origen** | Derivado del tipo de sistema (Sistemas de Información). |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Elimina la incertidumbre sobre quién atendió, retrasó o reasignó un folio, permitiendo calcular las métricas del SLA con total precisión y sin alteración manual. |
| **Afecta a** | RF-003, RF-005, RF-006, RF-008, RF-009, RF-010 |

**RNF-REN-001 - Tiempo límite de actualización de estado**
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Rendimiento |
| **Descripción** | Las transacciones operativas en la plataforma (cambios de estado o reasignaciones) se reflejan en pantalla de forma fluida sin paralizar el dispositivo del usuario. |
| **Métrica** | Un máximo de 2 segundos de tiempo de respuesta del servidor desde el clic de confirmación hasta el despliegue visual del nuevo estado bajo una conexión estándar 4G. |
| **Origen** | Derivado del contexto de uso (Sistemas utilizados en movimiento). |
| **Prioridad** | Imprescindible |
| **Por qué importa** | En el piso de producción la fluidez es crítica; tiempos de espera prolongados generan duplicidad de acciones (el usuario toca el botón varias veces pensando que no funcionó) y corrompen los datos. |
| **Afecta a** | RF-003, RF-005, RF-009, RF-010 |

---

## 5. Casos de uso

### 5.1 Relación de Casos de Uso del Sistema

1. **CU-01:** Levantar orden de trabajo.
2. **CU-02:** Atender solicitud asignada.
3. **CU-03:** Entregar solución con evidencia.
4. **CU-04:** Validar y liberar ticket (Caso de uso principal detallado).
5. **CU-05:** Reasignar folio entre áreas.

### 5.2 Detalle de los Casos de Uso del Sistema

**CU-04 Validar y liberar ticket**

*   **Identificador:** CU-04
*   **Título:** Validar y liberar ticket (Doble Visto Bueno)
*   **Actor principal:** Administrador / Dirección o Usuario Operativo (Emisor Original)
*   **Actor secundario:** Usuario Operativo (Receptor/Técnico)
*   **Objetivo:** Permitir que el emisor original valide la evidencia de un trabajo y apruebe su cierre definitivo o lo rechace para exigir correcciones.
*   **Precondición:** El usuario ha iniciado sesión en el sistema (RF-001) y existe al menos un ticket en estado 'Resuelto' (RF-006) en su bandeja de revisión.

**Escenario Principal (Happy path):**

1. El Emisor selecciona el ticket en estado 'Resuelto' desde su bandeja de revisión.
2. El sistema despliega la Vista de Detalle, cargando el historial y el archivo de evidencia adjunto.
3. El Emisor revisa el informe técnico y visualiza la evidencia fotográfica o documental.
4. El Emisor presiona el botón "Liberado" para aprobar el trabajo (RF-009).
5. El sistema actualiza el estado del ticket a 'Cerrado' de forma permanente.
6. El sistema detiene definitivamente el contador de tiempo del SLA.
7. El sistema registra la transacción en el sistema con fecha, hora e ID del Emisor (RNF-CON-001).
8. El sistema redirige al Emisor de vuelta al Dashboard (RF-004).

**Flujos Alternos:**

*   **Flujo Alterno 4a (Trabajo Incompleto / Rechazo Exitoso - RF-007, RF-008):**
    1. En el paso 4, si el Emisor considera que el trabajo está incompleto o mal realizado, presiona el botón "No Aceptado".
    2. El sistema despliega una ventana con un campo de notas de corrección (RF-007).
    3. El Emisor ingresa el texto detallando las correcciones necesarias y presiona "Guardar".
    4. El sistema guarda la nota de rechazo en el sistema.
    5. El sistema actualiza el estado del ticket a 'Reabierto' (RF-008).
    6. El sistema reinicia a cero el contador de cálculo de SLA de atención (RF-008).
    7. El flujo termina redirigiendo al Emisor al Dashboard, donde el ticket vuelve a estar activo.

*   **Flujo Alterno 4b (Intento de rechazo sin notas - Validación RF-007):**
    1. En el paso 3 del Flujo Alterno 4a, el Emisor intenta enviar el formulario dejando el campo de notas vacío.
    2. El sistema bloquea el envío, ya que la nota de corrección es obligatoria.
    3. El flujo regresa al paso 3 del Flujo Alterno 4a, a la espera de que el Emisor ingrese el texto.

*   **Flujo Alterno 1a (Intento de validación por rol no autorizado - RNF-SEG-001):**
    1. Previo al paso 1, un usuario con rol Operativo intenta acceder a la pantalla de revisión de un ticket del cual no es el emisor original.
    2. El sistema bloquea la visualización de los botones "Liberado" y "No Aceptado".
    3. El flujo termina, impidiendo que el usuario valide el ticket (RNF-SEG-001).

*   **Postcondición:** El ticket cambia su estado a 'Cerrado' (deteniendo el SLA permanentemente) o a 'Reabierto' (reiniciando el SLA y notificando al usuario operativo), y se genera un registro en el historial de la transacción (RNF-CON-001).

*   **Requisitos que realiza:** RF-004, RF-007, RF-008, RF-009, RNF-SEG-001, RNF-CON-001, RNF-REN-001.

## 6. Trazabilidad

| Requisito | Origen | Caso de Uso | Elemento del prototipo |
| :--- | :--- | :--- | :--- |
| **RF-001** | Derivado del sistema | Levantar orden de trabajo | Pantalla "Login" (No prototipada para MVP) |
| **RF-002** | Derivado del sistema | Validar y liberar ticket | Botón "Cerrar sesión" en Menú Lateral |
| **RF-003** | Entrevista 22 sep. | Levantar orden de trabajo | Dashboard y Pantalla "Crear Ticket" |
| **RF-004** | Visión de producto | Levantar orden de trabajo | Dashboard General (Tarjetas de métricas) |
| **RF-005** | Regla operativa | Atender solicitud asignada | Vista de Detalle (Cambio de estado) |
| **RF-006** | Entrevista 22 sep. | Entregar solución con evidencia | Vista de Detalle y Modal "Entregar Evidencia" |
| **RF-007** | Entrevista 22 sep. | Validar y liberar ticket | Modal "No Aceptado" (Entrada de texto) |
| **RF-008** | Entrevista 22 sep. | Validar y liberar ticket | Dashboard (Ticket vuelve con etiqueta roja) |
| **RF-009** | Regla de negocio | Validar y liberar ticket | Pantalla "Revisión de emisor" (Botón Liberado) |
| **RF-010** | Descubrimiento 22 sep. | Reasignar folio | Botón "Reasignar" en Vista de Detalle |
| **RNF-USA-001** | Descubrimiento 22 sep. | N/A | Flujo Mobile First en todo el Prototipo |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
| :--- | :--- | :--- | :--- |
| 18/08/2026 | Todos | Creación inicial del documento | Primera versión basada en la Visión del Producto. |
| 22/09/2026 | General | Modificación de roles a esquema binario | Simplificación de arquitectura derivada de la entrevista. |
| 28/09/2026 | RF-001 a 010 | Desglose y expansión detallada de requisitos funcionales | Corrección para cumplir la regla de "Una sola idea por requisito", eliminando acciones compuestas y detallando cada función de acuerdo con la tabla de 10 puntos. |
| 29/09/2026 | RF-001 a 010 | Refinamiento de Criterios de Aceptación | Ajuste estructural mediante listas con viñetas para garantizar que cada respuesta del sistema se evalúe como una prueba unitaria independiente, eliminando conectores lógicos compuestos. |
| 29/09/2026 | Trazabilidad | Se completó la columna de pantallas del prototipo | Alineación con la entrega de diseño en Figma. |

Figma: https://www.figma.com/design/1zI6qyqK3WlLbo0p0qJ9mE/Sin-t%C3%ADtulo?node-id=0-1&t=kOaKsDbDYWdnbeDx-1
