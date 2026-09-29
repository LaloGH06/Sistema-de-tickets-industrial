# Especificación de requisitos

**Sistema:** Sistema de Gestión de Tickets Industrial
**Autor:** Jorge Eduardo García Hernández
**Versión:** 2.1
**Fecha de la última actualización:** 28 de septiembre de 2026

---

## 1. Propósito y alcance

**Propósito del documento:**
Este documento define de forma precisa, comprobable y detallada los requisitos funcionales y no funcionales de la plataforma de tickets industrial. Está dirigido al equipo de desarrollo, a la dirección de la planta y a la dupla evaluadora.

**Alcance del sistema:**
El sistema abarca la gestión operativa, comunicación formal y auditorías mediante:
*   Autenticación y cierre de sesión de personal operativo y administrativo.
*   Registro de órdenes de trabajo (tickets) con asignación de folio único, área, prioridad y descripción.
*   Reasignación de tickets entre departamentos conservando el historial de transferencia.
*   Carga y validación obligatoria de evidencia (fotográfica o documento) para permitir el cambio de estado a "Resuelto".
*   Mecanismo de "doble visto bueno" que permite al emisor original cerrar el ticket o rechazarlo, reabriendo el caso y reiniciando las métricas de SLA.
*   Visualización en tiempo real de estados y métricas de resolución en un tablero general (Dashboard).

**Fuera del alcance:**
*   Procesamiento de pagos de nómina o destajo.
*   Gestión de compras, cotizaciones de refacciones o conexión con portales bancarios.
*   Envío de notificaciones mediante plataformas externas como SMS o WhatsApp.
*   Fungir como checador biométrico o sistema de control de asistencia del personal.

---

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
| :--- | :--- | :--- |
| **Administrador / Dirección** | Recibe reportes verbales o notas físicas incompletas. No tiene forma de medir cuánto tiempo real toma resolver una falla. | Supervisar el cumplimiento de los SLA en un tablero en vivo. Auditar los tiempos de respuesta por departamento y asegurar que los técnicos no cierren reportes sin haber hecho el trabajo real. |
| **Usuario Operativo (Emisor / Receptor)** | Usa WhatsApp personal para reportar fallas, los mensajes se pierden y los problemas reinciden. Si le falta material, tiene que buscar físicamente al de almacén. | Rapidez para levantar reportes desde su dispositivo móvil sin quitarse el equipo de seguridad. Capacidad de reasignar tareas a otras áreas con un par de toques y adjuntar fotos directamente para amparar su trabajo. |

**Conflictos identificados entre usuarios:**
*   **Auditoría estricta vs. Agilidad en piso:** La dirección requiere documentación detallada de cada trabajo realizado, pero el personal operativo rechaza llenar formularios largos porque interrumpe su labor física. **Solución:** Se implementó la carga de evidencia fotográfica como requerimiento central, sustituyendo la captura exhaustiva de texto por pruebas visuales de un solo toque.

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

---

### 3.2 Fichas

#### RF-001 · Iniciar sesión en la plataforma
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema autentica al usuario mediante sus credenciales (correo y contraseña) para otorgarle acceso a la plataforma según su rol (Administrador u Operativo). |
| **Origen** | Derivado de la necesidad de identificar al autor y responsable de cada ticket. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al ingresar credenciales válidas, el sistema inicia la sesión y redirige al Dashboard. Si son incorrectas, despliega el mensaje "Credenciales inválidas" y bloquea el acceso. |
| **Relacionado con** | RF-002, RF-004 |

#### RF-002 · Cerrar sesión en la plataforma
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema finaliza la sesión activa del usuario actual, revoca el token de acceso y retorna a la pantalla de autenticación. |
| **Origen** | Derivado del control de acceso por turnos operativos. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al presionar el botón "Cerrar sesión", el sistema destruye la sesión activa, bloquea el acceso a las funciones de DIPROPOL y muestra la pantalla de inicio de sesión. |
| **Relacionado con** | RF-001, RNF-CON-001 |

#### RF-003 · Generar ticket con folio único
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema registra órdenes de trabajo capturando título, descripción, prioridad y área, asignando automáticamente un folio inmutable y marca de tiempo. |
| **Origen** | Entrevista (Problema de rastreo en WhatsApp). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al completar los datos y presionar "Generar Folio", el sistema crea el registro en la base de datos, estampa la fecha/hora y muestra el nuevo folio en el Dashboard. |
| **Relacionado con** | RF-004, RF-010, RNF-SEG-001 |

#### RF-004 · Consultar tablero general (Dashboard)
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema muestra un panel consolidado con los tickets activos filtrados por área, estado y prioridad, junto con indicadores de tiempo transcurrido. |
| **Origen** | Visión del producto (Métricas en tiempo real). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al ingresar al sistema, el usuario visualiza los tickets que corresponden a su área (si es operativo) o el total de la planta (si es administrador), actualizados sin necesidad de recargar la página. |
| **Relacionado con** | RF-001, RNF-REN-001 |

#### RF-005 · Cambiar estado de ticket a en progreso
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema permite al responsable asignar el ticket a sí mismo y marcarlo como "En progreso", iniciando formalmente el tiempo de atención. |
| **Origen** | Regla de negocio operativa. |
| **Prioridad** | Importante |
| **Criterio de aceptación** | Al presionar "Atender solicitud", el estado del ticket cambia visualmente a "En progreso" y el ID del usuario actual queda registrado como el técnico asignado. |
| **Relacionado con** | RF-003, RF-006 |

#### RF-006 · Entregar evidencia obligatoria
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema bloquea el cambio de estado de un ticket a 'Resuelto' si el responsable no adjunta un reporte escrito y un archivo multimedia (foto/documento). |
| **Origen** | Entrevista (Trabajos reportados verbalmente a medias). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Si el usuario presiona "Entregar Solución" sin haber cargado un archivo adjunto, el sistema despliega el error "Obligatorio adjuntar evidencia" y detiene el proceso. |
| **Relacionado con** | RF-005, RF-007, RF-009, RNF-USA-001 |

#### RF-007 · Rechazar solución de ticket
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema exige una nota de corrección obligatoria cuando el usuario emisor original selecciona la opción de no aceptar el trabajo reportado como 'Resuelto'. |
| **Origen** | Entrevista (Fallas reincidentes a los pocos días). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al seleccionar el botón "No Aceptado", el sistema despliega un campo de texto que impide guardar la acción si se encuentra vacío. |
| **Relacionado con** | RF-006, RF-008 |

#### RF-008 · Reabrir ticket rechazado
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema actualiza el estado del ticket a 'Reabierto' y reinicia los contadores de la métrica SLA al registrarse un rechazo de solución. |
| **Origen** | Entrevista (Fallas reincidentes a los pocos días). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Tras guardar la nota de corrección (RF-007), el estado cambia a 'Reabierto' y el tiempo SLA de resolución vuelve a 00:00. |
| **Relacionado con** | RF-007, RF-009 |

#### RF-009 · Cerrar ticket validado
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema cambia el estado del ticket a "Cerrado" y detiene permanentemente el contador SLA cuando el emisor original aprueba la evidencia enviada. |
| **Origen** | Regla de negocio de "Doble visto bueno". |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al presionar "Aceptar Solución", el sistema marca el folio como Cerrado, registra la fecha final de validación y lo archiva en el historial. |
| **Relacionado con** | RF-006, RF-008 |

#### RF-010 · Reasignar folio entre áreas
| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema transfiere la responsabilidad de un ticket abierto a otra área operativa, registrando el movimiento en el historial sin alterar el folio original. |
| **Origen** | Entrevista (Descubrimiento: Ausencias y dependencias operativas). |
| **Prioridad** | Importante |
| **Criterio de aceptación** | Al seleccionar un área distinta en un ticket activo, se añade el evento "Transferido de Área X a Área Y" en la bitácora del ticket y aparece en la bandeja del nuevo responsable. |
| **Relacionado con** | RF-003, RNF-SEG-001 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
| :--- | :--- | :--- | :--- | :--- |
| **RNF-REN-001** | Rendimiento | Actualización sin recarga de página | Imprescindible | Supuesto propio (Naturaleza de SPA con React/Supabase) |
| **RNF-USA-001** | Usabilidad | Flujo táctil minimizado para técnicos | Importante | Observación del contexto de uso industrial |
| **RNF-CON-001** | Confiabilidad | Disponibilidad operativa en turno | Imprescindible | Supuesto propio (Horario de planta 06:00 a 22:00) |
| **RNF-SEG-001** | Seguridad | Inmutabilidad de folios de auditoría | Imprescindible | Entrevista (Necesidad de trazabilidad estricta) |

---

### 4.2 Fichas

#### RNF-REN-001 · Actualización sin recarga de página
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Rendimiento |
| **Descripción** | El sistema refleja los cambios de estado y la llegada de nuevos tickets en el Dashboard sin necesidad de recargar el navegador. |
| **Métrica** | Tiempo máximo de respuesta visual de **2 segundos** para cambios de estado. |
| **Origen** | Supuesto propio (Naturaleza web de la plataforma). |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Para evitar retrasos operativos en piso y mantener la fluidez del usuario. |
| **Afecta a** | RF-004, RF-005, RF-010 |

#### RNF-USA-001 · Flujo táctil minimizado para técnicos
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Usabilidad |
| **Descripción** | La interfaz móvil agrupa las opciones de evidencia y resolución para requerir una manipulación táctil mínima en piso de producción. |
| **Métrica** | El flujo de resolución (escribir nota, tomar foto y confirmar) se completa en un máximo de **4 interacciones táctiles**. |
| **Origen** | Observación del contexto de uso industrial. |
| **Prioridad** | Importante |
| **Por qué importa** | Los técnicos usan equipo de seguridad (guantes, lentes) y tienen tiempo limitado. Si el sistema es complejo, evadirán el proceso. |
| **Afecta a** | RF-006, RF-010 |

#### RNF-CON-001 · Disponibilidad operativa en turno
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Confiabilidad |
| **Descripción** | El sistema mantiene disponibilidad operativa continua durante los horarios de producción. |
| **Métrica** | Uptime del **99.5%** de Lunes a Sábado, entre las 06:00 y 22:00 hrs. |
| **Origen** | Supuesto propio (Horario de planta). |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Una caída del servidor paraliza la comunicación formal de la planta y retrasa el mantenimiento. |
| **Afecta a** | Todos los RF |

#### RNF-SEG-001 · Inmutabilidad de folios de auditoría
| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Seguridad (Trazabilidad) |
| **Descripción** | La base de datos impide la eliminación permanente (Hard Delete) de cualquier folio generado, permitiendo únicamente cambios a estados de cierre o cancelación (Soft Delete). |
| **Métrica** | **100%** de los folios creados son inmutables a eliminación mediante la interfaz y conservan su registro histórico. |
| **Origen** | Entrevista (Necesidad de trazabilidad estricta). |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Previene que empleados eliminen tickets comprometedores para manipular los tiempos de respuesta o evadir responsabilidades. |
| **Afecta a** | RF-003, RF-010 |

---

## 5. Casos de uso

### 5.1 Relación de Casos de Uso del Sistema

1.  **CU-01:** Levantar orden de trabajo
2.  **CU-02:** Atender solicitud asignada y entregar evidencia (Caso de uso principal detallado)
3.  **CU-03:** Validar y liberar ticket (Doble visto bueno)
4.  **CU-04:** Reasignar folio a otro departamento

---

### 5.2 Detalle del Caso de Uso Principal: CU-02 Atender solicitud y entregar evidencia

*   **Identificador:** CU-02
*   **Título:** Atender solicitud asignada y entregar evidencia
*   **Actor principal:** Usuario Operativo (Receptor)
*   **Objetivo:** Registrar el trabajo físico realizado sobre una avería adjuntando pruebas para enviar el ticket a revisión del emisor.
*   **Precondición:** El usuario ha iniciado sesión (RF-001), tiene un ticket en estado "En progreso" asignado a su área.

#### Escenario Principal (Flujo Feliz):
1.  El usuario operativo selecciona un ticket de su bandeja en el Dashboard.
2.  El sistema despliega los detalles del folio, descripción y prioridad.
3.  El usuario presiona el botón "Entregar Solución".
4.  El sistema despliega el formulario de cierre solicitando nota de trabajo y archivo adjunto.
5.  El usuario escribe un breve reporte de las acciones realizadas.
6.  El usuario toma una fotografía de la máquina reparada desde su dispositivo móvil y la adjunta al formulario.
7.  El usuario presiona "Confirmar Envío".
8.  El sistema valida la presencia del texto y la imagen (RF-006), cambia el estado del ticket a "Resuelto", pausa el cronómetro del SLA y notifica en el sistema al emisor original para su validación (CU-03).

#### Flujos Alternos:
*   **Flujo Alterno 7a (Falta de evidencia - RF-006):**
    1.  En el paso 7, el usuario presiona "Confirmar Envío" sin haber adjuntado una fotografía o documento.
    2.  El sistema detiene la petición, resalta el campo de adjuntos en rojo y despliega el mensaje de error: *"Obligatorio adjuntar evidencia"*.
    3.  El flujo regresa al paso 6 para que el usuario capture la imagen requerida.

*   **Flujo Alterno 3a (El problema corresponde a otra área - RF-010):**
    1.  En el paso 3, el usuario revisa el detalle y determina que la falla reportada no es mecánica, sino eléctrica.
    2.  El usuario presiona el botón "Reasignar folio".
    3.  El sistema despliega un menú desplegable con los departamentos disponibles.
    4.  El usuario selecciona "Mantenimiento Eléctrico" y confirma.
    5.  El sistema actualiza el responsable del ticket (RF-010), registra el cambio en el historial y lo elimina de la bandeja del usuario actual.

*   **Postcondición:** El ticket queda en estado "Resuelto" a la espera de la liberación del creador, y queda un respaldo inmutable de la fecha y hora de entrega.
*   **Requisitos que realiza:** RF-005, RF-006, RF-010, RNF-USA-001.

---

## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo | Estado |
| :--- | :--- | :--- | :--- | :--- |
| **RF-001** | Derivado de seguridad | Todos | Pantalla Login | Vigente |
| **RF-002** | Derivado de seguridad | Todos | Menú lateral / Botón Salir | Vigente |
| **RF-003** | Eliminar WhatsApp | CU-01 Levantar orden | Formulario "Crear Nuevo Ticket" | Vigente |
| **RF-004** | Visión del producto | Todos | Tablero General (Dashboard) | Vigente |
| **RF-005** | Regla operativa | CU-02 Atender solicitud | Botón "Atender" en Detalle | Vigente |
| **RF-006** | Garantizar trabajo real | CU-02 Atender solicitud | Modal "Entregar Evidencia" | Vigente |
| **RF-007** | Evitar cierres falsos | CU-03 Validar ticket | Botón "No Aceptado" en Detalle | Vigente |
| **RF-008** | Evitar cierres falsos | CU-03 Validar ticket | Estado "Reabierto" en Dashboard | Vigente |
| **RF-009** | Regla operativa | CU-03 Validar ticket | Botón "Aceptar Solución" | Vigente |
| **RF-010** | Manejar ausencias | CU-04 Reasignar folio | Select "Reasignar Área" | Vigente |
| **RNF-USA-001** | Trabajo en piso | CU-02 Atender solicitud | Interfaz de 4 toques en móvil | Vigente |
| **RNF-SEG-001** | Auditoría estricta | CU-01, CU-04 | Base de datos (Sin botón Delete) | Vigente |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
| :--- | :--- | :--- | :--- |
| 18/08/2026 | Todos | Creación inicial de especificación (v1.0). | Requerimientos base. |
| 22/09/2026 | Usuarios | Modificación de usuarios. | Alineación con la Ficha de Dominio. |
| 28/09/2026 | Todos | Reestructuración total a formato desglosado con tablas por ficha y verbos en infinitivo. | Mejora en la trazabilidad, atomicidad e incorporación de Casos de Uso detallados y flujos alternos. |
| 28/09/2026 | RF-002 | Adición del requisito funcional "Cerrar sesión en la plataforma". | Corrección de omisión técnica. Garantizar la seguridad de sesiones en dispositivos compartidos. |
