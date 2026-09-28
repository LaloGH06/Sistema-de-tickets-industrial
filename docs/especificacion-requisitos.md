# 2. Especificación de Requisitos

**Autor:** Jorge Eduardo García Hernández
**Repositorio:** https://github.com/LaloGH06/Sistema-de-tickets-industrial

---

## 1. Propósito y Alcance
*Propósito y alcance, retomado de la Visión del producto*[cite: 19].

**Propósito:**
El Sistema de Gestión de Tickets es una plataforma digital diseñada para centralizar, organizar y auditar las órdenes de trabajo y comunicados internos de una planta de producción industrial. Funciona como un centro de control donde cualquier empleado puede reportar un problema, asignarlo y supervisar los tiempos de resolución mediante métricas de SLA (Service Level Agreement).

**Alcance del Sistema:**
*   **Dentro del alcance:** Registro de folios con descripción y prioridad, reasignación de tickets conservando el historial, carga obligatoria de evidencia (fotográfica y documental), validación de "doble visto bueno", y visualización en tiempo real de métricas en un tablero general.
*   **Fuera del alcance (Exclusiones explícitas):** El sistema **no** procesa pagos de nómina, no gestiona compras o cotizaciones de refacciones con bancos, no envía notificaciones por SMS/WhatsApp, y no funge como checador biométrico de asistencia. 

---

## 2. Usuarios y su Contexto
*Usuarios y su contexto, enriquecido con lo que salió de la entrevista*[cite: 19].

*   **Usuario Operativo (Emisor / Receptor):** 
    *   *Contexto:* Personal en piso de producción (Mantenimiento, Almacén, Calidad). Interactúan con el sistema 100% desde sus dispositivos móviles personales, muchas veces usando equipo de seguridad. 
    *   *Necesidad:* Rapidez para levantar reportes, capacidad de reasignar tareas a otros departamentos (ej. pedir material a almacén) sin perder el folio original, y una interfaz simplificada para adjuntar fotos como evidencia de su trabajo.
*   **Administrador / Dirección:** 
    *   *Contexto:* Gerencia y directivos operando principalmente desde equipos de escritorio en oficinas.
    *   *Necesidad:* Supervisar el cumplimiento de los SLA, auditar los tiempos de respuesta por departamento, y asegurar que ningún ticket se cierre en falso o quede abandonado por personal ausente o de vacaciones.

---

## 3. Requisitos Funcionales
*Requisitos funcionales con ficha completa: descripción, origen, prioridad, criterio de aceptación y relaciones*[cite: 19].

| ID | Nombre del Requisito | Descripción | Origen | Prioridad | Criterio de Aceptación | Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **RF-01** | Generación de Ticket con Folio | El sistema permitirá registrar órdenes con título, descripción, prioridad, área y archivos adjuntos. Al guardar, se genera un identificador único y marca de tiempo. | Entrevista (Problema de rastreo en WhatsApp) | Alta | Al completar los campos obligatorios y presionar "Generar Folio", el sistema crea el registro, estampa la fecha/hora inalterable y regresa al usuario al Dashboard. | Base para RF-02 y RF-03 |
| **RF-02** | Entrega Obligatoria de Evidencia | El sistema bloqueará el cambio de estado a 'Resuelto' si el técnico no adjunta un informe escrito y al menos un archivo (fotografía o documento PDF). | Entrevista (Trabajos reportados verbalmente a medias) | Alta | Si se presiona "Entregar Solución" sin un archivo adjunto, el sistema despliega un error visual en rojo: "Obligatorio adjuntar evidencia" y bloquea el avance. | Depende de RF-01 |
| **RF-03** | Doble Visto Bueno y Reapertura | Solo el emisor original puede pasar un ticket de 'Resuelto' a 'Cerrado'. Si lo rechaza, se exige nota de corrección y el ticket pasa a 'Reabierto'. | Entrevista (Fallas reincidentes a los pocos días) | Alta | Al seleccionar "No Aceptado", el sistema despliega un campo de texto obligatorio. Tras guardarlo, el estado cambia a 'Reabierto' y el SLA se reinicia. | Depende de RF-02 |
| **RF-04** | Reasignación de Folio | Permite trasladar la responsabilidad de un ticket a otra área o encargado conservando el número de folio, historial de chat y SLA original. | Entrevista (Descubrimiento: Ausencias y dependencias) | Media | Al cambiar el área asignada en un ticket activo, se registra en el historial interno "Transferido de Área X a Área Y" sin borrar el ticket inicial. | Depende de RF-01 |

---

## 4. Requisitos No Funcionales
*Requisitos no funcionales agrupados por atributo de calidad, cada uno con su métrica*[cite: 19].

| ID | Atributo de Calidad | Descripción y Justificación | Métrica de Evaluación |
| :--- | :--- | :--- | :--- |
| **RNF-01** | **Rendimiento** | Para evitar retrasos operativos en piso, las interacciones no deben interrumpir el flujo de trabajo físico del usuario. | Los cambios de estado y envío de mensajes en el chat deben reflejarse en un tiempo máximo de **2 segundos** sin recargar la página. |
| **RNF-02** | **Usabilidad** | El entorno industrial y el uso en pantallas móviles exigen flujos cortos y botones accesibles. | El flujo de resolución (escribir informe, subir archivo y confirmar) debe completarse en un máximo de **4 interacciones táctiles**. |
| **RNF-03** | **Disponibilidad** | El sistema centraliza la operación de planta; una caída prolongada paraliza la comunicación formal. | Se debe garantizar un uptime (tiempo en línea) del **99.5%** durante los turnos operativos (Lunes a Sábado, 06:00 a 22:00 hrs). |
| **RNF-04** | **Trazabilidad** | Necesario para deslindar responsabilidades y auditar tiempos de respuesta. | El **100%** de los folios generados deben ser inmutables (no se pueden eliminar de la base de datos, solo cambiar de estado a 'Cancelado' o 'Cerrado'). |

---

## 5. Tabla de Trazabilidad y Registro de Cambios
*Tabla de trazabilidad y registro de cambios*[cite: 19]. *Completa la columna de pantallas en la tabla de trazabilidad*[cite: 20].

### Tabla de Trazabilidad
| Requisito | Necesidad del Usuario / Regla de Negocio | Pantallas (Prototipo)[cite: 20] |
| :--- | :--- | :--- |
| **RF-01** | Formalizar órdenes (eliminar WhatsApp) | Dashboard General y Pantalla "Crear Nuevo Ticket" |
| **RF-02** | Garantizar que el trabajo físico se realizó | Vista de Detalle y Modal "Entregar Evidencia" |
| **RF-03** | Evitar cierres falsos por los técnicos | Pantalla "Revisión del emisor original" y Modal "No Aceptado" |
| **RF-04** | Manejar ausencias o dependencias operativas | Botón "Reasignar" en Vista de Detalle |
| **RNF-02** | Navegación rápida en piso de producción | Aplicado a todas las pantallas (Flujo máximo de 4 toques) |

### Registro de Cambios (Control de Versiones)
*Corrige los requisitos según los hallazgos y registra cada cambio en la tabla de control de cambios*[cite: 20].

| Versión | Fecha | Autor | Descripción del Cambio |
| :--- | :--- | :--- | :--- |
| 1.0 | 18/08/2026 | Jorge Eduardo García Hernández | Creación inicial del documento con requisitos base. |
| 1.1 | 22/09/2026 | Jorge Eduardo García Hernández | Modificación de usuarios a roles binarios (Administrador / Operativo) según Ficha de Dominio. |
| 1.2 | 28/09/2026 | Jorge Eduardo García Hernández | Corrección de requisitos según hallazgos: Inclusión de RF-04 (Reasignación) y ajustes en RF-03 tras la entrevista de elicitación con Jonathan. Se completó la columna de pantallas[cite: 20]. |
