# Especificación de requisitos

**Sistema:** Sistema de Gestión de Tickets (Dipropol)
**Autor:** Jorge Eduardo García Hernández
**Versión:** 1.2
**Fecha de la última actualización:** 28 de septiembre de 2026

---

## 1. Propósito y alcance

**Propósito del documento:**
El Sistema de Gestión de Tickets es una plataforma digital diseñada para centralizar, organizar y auditar las órdenes de trabajo y comunicados internos de una planta de producción industrial. Funciona como un centro de control donde cualquier empleado puede reportar un problema, asignarlo y supervisar los tiempos de resolución mediante métricas de SLA.

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
El Administrador quiere un control estricto (que nada se borre y se llene toda la información) para sus métricas. El Operativo quiere rapidez y suele equivocarse al asignar el área en el levantamiento. Se resolvió implementando la función de "Reasignar Folio" (conservando historial) en lugar de obligar al operativo a borrar y empezar de cero.

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre | Prioridad | Origen |
| :--- | :--- | :--- | :--- |
| RF-001 | Generación de ticket con folio único | Imprescindible | Entrevista (Problema de WhatsApp) |
| RF-002 | Entrega obligatoria de evidencia | Imprescindible | Entrevista (Trabajos incompletos) |
| RF-003 | Doble visto bueno y reapertura | Imprescindible | Entrevista (Fallas reincidentes) |
| RF-004 | Reasignación de folio | Importante | Entrevista (Dependencias y ausencias) |

### 3.2 Fichas

**RF-001 - Generación de ticket con folio único**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema genera un registro con título, descripción, prioridad, área y archivos adjuntos asignando un folio único y marca de tiempo inalterable. |
| **Origen** | Entrevista, 22 de septiembre de 2026. Necesidad de formalizar reportes fuera de WhatsApp. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al llenar los campos obligatorios y presionar "Generar Folio", el sistema crea el registro en la base de datos, muestra el folio generado en pantalla y regresa al usuario al Dashboard. |
| **Relacionado con** | RF-002, RF-004, RNF-TRA-001 |

**RF-002 - Entrega obligatoria de evidencia**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema bloquea el cambio de estado a 'Resuelto' si no se adjunta un informe técnico escrito y al menos un archivo de evidencia (fotografía o documento). |
| **Origen** | Entrevista, 22 de septiembre de 2026. Validación de que el trabajo físico se realizó. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Si el usuario presiona "Confirmar Resolución" en el modal sin haber subido un archivo, el sistema despliega el error "Obligatorio adjuntar evidencia" y no permite avanzar. |
| **Relacionado con** | RF-001, RF-003 |

**RF-003 - Doble visto bueno y reapertura**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema impide cambiar el estado de 'Resuelto' a 'Cerrado' si el emisor original no valida la evidencia; si la rechaza, exige una nota de corrección y cambia el estado a 'Reabierto'. |
| **Origen** | Supuesto confirmado en entrevista, 22 de septiembre de 2026. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al presionar "No Aceptado" en la vista de revisión, se despliega un campo de texto obligatorio. Tras guardar la nota, el ticket cambia de estado y el SLA se reinicia. |
| **Relacionado con** | RF-002 |

**RF-004 - Reasignación de folio**
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema transfiere la responsabilidad de un ticket a otra área conservando el número de folio, el historial de chat y el SLA original. |
| **Origen** | Descubrimiento inesperado en entrevista, 22 de septiembre de 2026 (ausencias y dependencias operativas). |
| **Prioridad** | Importante |
| **Criterio de aceptación** | Al editar el área asignada en un ticket activo, este aparece inmediatamente en la bandeja del nuevo departamento y se registra un mensaje en el historial indicando el traslado. |
| **Relacionado con** | RF-001 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
| :--- | :--- | :--- | :--- | :--- |
| RNF-REN-001 | Rendimiento | Tiempo de respuesta de interfaz | Imprescindible | Derivado del uso operativo |
| RNF-USA-001 | Usabilidad | Flujo táctil de cierre | Importante | Descubrimiento de uso en móviles |
| RNF-DIS-001 | Disponibilidad | Tiempo en línea operativo | Imprescindible | Derivado del tipo de sistema |
| RNF-TRA-001 | Trazabilidad | Inmutabilidad de registros | Imprescindible | Supuesto propio / Regla de negocio |

### 4.2 Fichas

**RNF-REN-001 - Tiempo de respuesta de interfaz**
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Rendimiento |
| **Descripción** | Los cambios de estado de un ticket y el envío de mensajes en el chat se reflejan en pantalla en un tiempo máximo de 2 segundos sin recargar la página web. |
| **Métrica** | Menos de 2 segundos de tiempo de respuesta desde la acción del usuario hasta la confirmación visual. |
| **Origen** | Derivado del uso operativo para evitar retrasos en el piso de producción. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Si el sistema tarda o se congela, los operativos en piso abandonarán la plataforma y regresarán a reportar por canales informales. |
| **Afecta a** | RF-001, RF-002 |

**RNF-USA-001 - Flujo táctil de cierre**
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Usabilidad |
| **Descripción** | El flujo para reportar una solución (abrir modal, escribir informe, subir archivo y confirmar) requiere un máximo de 4 interacciones táctiles en dispositivos móviles. |
| **Métrica** | Máximo de 4 clics o toques para completar el flujo de entrega de evidencia. |
| **Origen** | Descubrimiento en entrevista: los operativos usarán sus celulares personales en el piso de planta. |
| **Prioridad** | Importante |
| **Por qué importa** | Los operativos a menudo usan equipo de seguridad y tienen poco tiempo. Si es complejo, no subirán la evidencia. |
| **Afecta a** | RF-002 |

**RNF-DIS-001 - Tiempo en línea operativo**
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Disponibilidad |
| **Descripción** | El sistema mantiene un uptime garantizado del 99.5% durante los turnos operativos de la planta. |
| **Métrica** | Uptime del 99.5% medido en el horario de Lunes a Sábado, de 06:00 a 22:00 hrs. |
| **Origen** | Derivado del tipo de sistema (gestión centralizada). |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Centraliza toda la operación; una caída prolongada paraliza la comunicación formal y detiene mantenimientos críticos. |
| **Afecta a** | Todos |

**RNF-TRA-001 - Inmutabilidad de registros**
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Trazabilidad |
| **Descripción** | El sistema impide la eliminación permanente de folios creados; los registros erróneos solo pueden cambiar a estado 'Cancelado' conservando su registro original. |
| **Métrica** | 100% de los folios creados permanecen en la base de datos sin opción de "delete" físico. |
| **Origen** | Supuesto propio / Regla de negocio administrativa. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Necesario para deslindar responsabilidades, auditar tiempos de respuesta y evitar manipulación de métricas de desempeño. |
| **Afecta a** | RF-001, RF-004 |

---

## 5. Casos de uso

*(Desarrollados a partir de los requisitos funcionales. El diagrama de casos de uso general se adjunta en el repositorio. Detalle escrito enfocado en el flujo de validación).*

**Caso de Uso Principal: Doble Visto Bueno y Reapertura**
*   **Requisito relacionado:** RF-003.
*   **Actores:** Administrador/Dirección o Usuario Operativo Emisor (Principal), Usuario Operativo Receptor (Secundario).
*   **Precondición:** Ticket en estado 'Resuelto' con evidencia cargada.
*   **Flujo principal:**
    1. El Emisor recibe notificación de resolución.
    2. El Emisor abre el detalle del ticket y revisa la evidencia.
    3. El Emisor aprueba la solución y selecciona 'Liberado'.
    4. El sistema actualiza el estado a 'Cerrado' y detiene el SLA.
*   **Flujos alternos:**
    *   *A. Trabajo Incompleto:* En el paso 3, el Emisor selecciona 'No Aceptado'. El sistema exige una nota de corrección, cambia el estado a 'Reabierto' y reinicia el SLA.

---

## 6. Trazabilidad

| Requisito | Origen | Caso de Uso | Elemento del prototipo (Pantalla) |
| :--- | :--- | :--- | :--- |
| **RF-001** | Entrevista 22 sep. | Levantar orden de trabajo | Dashboard y Pantalla "Crear Ticket" |
| **RF-002** | Entrevista 22 sep. | Entregar solución con evidencia | Vista de Detalle y Modal "Entregar Evidencia" |
| **RF-003** | Supuesto confirmado 22 sep. | Validar y liberar ticket | Pantalla "Revisión de emisor" y Modal "No Aceptado" |
| **RF-004** | Descubrimiento 22 sep. | Reasignar folio | Botón "Reasignar" en Vista de Detalle |
| **RNF-USA-001** | Descubrimiento 22 sep. | N/A | Flujo Mobile First en el Prototipo |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
| :--- | :--- | :--- | :--- |
| 18/08/2026 | Todos | Creación inicial del documento | Primera versión basada en la Visión del Producto. |
| 22/09/2026 | General | Modificación de roles a esquema binario (Administrador/Operativo) | Simplificación de arquitectura derivada de la Ficha de Dominio. |
| 28/09/2026 | RF-004 | Se añadió el requisito funcional de reasignación | Descubrimiento durante la entrevista con el cliente (ausencias operativas). |
| 28/09/2026 | RF-003 | Se eliminó el límite estricto de 48 horas | Supuesto refutado en entrevista; generaría falsos cierres de tickets. |
| 28/09/2026 | Trazabilidad | Se completó la columna de pantallas del prototipo | Alineación con la entrega de diseño en Figma. |

---
**Lista de verificación aplicada antes de entregar:**
- [x] Todos los requisitos tienen identificador único (RF-### / RNF-ATR-###).
- [x] Cada requisito expresa una sola idea y usa verbos/condiciones firmes.
- [x] Cada requisito funcional tiene criterio de aceptación comprobable.
- [x] Cada requisito no funcional tiene una métrica numérica o comprobable.
- [x] El campo Origen distingue lo confirmado de lo supuesto.
- [x] La tabla de trazabilidad está completa conectando requisitos con el prototipo.
- [x] Mi dupla revisó el documento y su revisión está registrada (Ver sección de Entrevista).
