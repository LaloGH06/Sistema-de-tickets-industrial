# Análisis de impacto de SC-001

---

## Solicitud que se analiza
Referencia a la solicitud: `sc-001-gestion-de-refacciones.md`, pedida por Jonathan Michel García Pacheco el 12 de noviembre de 2026.

## Traducción a términos del sistema
La solicitud pide: "solicitar refacciones al almacén con código y cantidad, y bloquear el paso a resuelto hasta confirmar la entrega con vale de salida".

Traducido a mi sistema:
1. El módulo de **Operación** debe incorporar una nueva clase de dominio `Refaccion` asociada por composición a `Ticket`.
2. La entidad `Ticket` necesita métodos para registrar refacciones (`+solicitarRefacciones()`) y consultar si todas fueron entregadas (`+todasRefaccionesDespachadas(): bool`).
3. La operación `resolver(evidencias: IServicioDeEvidencias)` de `Ticket` debe evaluar una precondición adicional: no solo exigir archivo de evidencia, sino validar que no haya refacciones pendientes.
4. El módulo de **Archivos y Evidencias** (`IServicioDeEvidencias`) debe admitir una nueva categoría de archivo adjunto: `ValeDeAlmacen`.
5. Se debe crear una vista modal en la UI para la captura de partes y un estado intermedio en el ciclo de vida del ticket (`En Espera de Refacción`) que congele el SLA.

## Impacto directo
Lo que la solicitud toca **por definición**: lo que hay que modificar para que el cambio exista.

| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| Requisito RF-005 | 2 | Permitir capturar opcionalmente una lista de refacciones al pasar a 'En proceso'. |
| Requisito RF-006 | 2 | Condicionar el paso a 'Resuelto' a la entrega confirmada de refacciones además de la evidencia técnica. |
| Requisito RF-011 (Nuevo) | 2 | Crear ficha formal para "Confirmar entrega y carga de vale de refacciones por Almacén". |
| Caso de uso CU-02 y CU-03 | 2 | Agregar flujo alterno en CU-02 para requisición de piezas y precondición de entrega en CU-03. |
| Caso de uso nuevo (CU-06) | 2 | "Despachar refacción de orden de trabajo" ejecutado por el actor Almacén. |
| Diagrama de clases | 4 | Agregar clase `Refaccion` (`-codigo: string`, `-cantidad: int`, `-despachada: bool`), componerla en `Ticket "1" *-- "*" Refaccion` y añadir métodos de gestión. |
| Diagrama de secuencia | 4 | Crear un nuevo diagrama de secuencia para modelar la llamada síncrona entre Técnico, Ticket y Almacén. |

## Impacto indirecto
Lo que se rompe o deja de ser cierto **por culpa del cambio directo**, aunque la solicitud no lo mencione.

| Artefacto | Unidad | Por qué se ve afectado |
|---|---|---|
| Visión del producto (Alcance) | 1 | Rompe la cláusula explícita de exclusión que dictaba: *"El sistema no gestiona compras, cotizaciones ni inventario de refacciones"*. |
| Registro de decisión ADR-001 | 3 | Sin impacto en la arquitectura de Monolito Modular; la nueva clase se mantiene dentro del módulo de Operación sin requerir microservicios. |
| Diagrama de contenedores C4 | 3 | Sin impacto. Las piezas de despliegue siguen siendo la App Web, el Servidor Monolítico y la Base de Datos PostgreSQL. |
| Diagrama de componentes C4 | 3 | El módulo de Operación aumenta su acoplamiento con Almacén y requiere nuevas llamadas hacia `IServicioDeEvidencias`. |
| Máquina de estados del Ticket | 4 | Requiere crear el estado intermedio `EnEsperaDeRefaccion`. Dejar el ticket corriendo en 'En proceso' violaría las métricas de SLA penalizando al técnico por demoras de compras. |
| Prototipo interactivo en Figma | 2 | Se deben diseñar 2 pantallas nuevas: la modal de solicitud de partes para el técnico y la bandeja de despacho para el encargado de almacén. |
| RNF-USA-001 (Pasos máximos) | 2 | Riesgo de violar la métrica de un máximo de 4 clics al agregar la captura manual de códigos de parte en dispositivos móviles de planta. |

## Lo que queda sin tocar
- El módulo de **Identidad y estructura** (`Usuario`, `Area`, `IValidadorDeCierre`, roles y permisos) permanece 100% intacto.
- El flujo de validación y liberación final (**CU-04 / RF-009 / Doble Visto Bueno**) no se modifica: una vez resuelto, el emisor valida exactamente igual.
- Las tablas y almacenamiento físico de evidencias binarias no cambian de motor.

## Riesgos
1. **Rechazo por lentitud en piso (Probabilidad Alta):** Si obligamos a los operadores a escribir códigos de catálogo en un celular con guantes de seguridad, dejarán de usar el sistema y regresarán a WhatsApp.
2. **Cuellos de botella en Almacén (Probabilidad Media):** Si el personal de almacén no entra a la plataforma a confirmar el vale, las órdenes de mantenimiento quedarán congeladas indefinidamente en la planta.
3. **Desvirtuar el alcance del sistema:** Convertir un software ágil de tickets en un mini-sistema ERP de inventarios que requerirá catálogos de proveedores y existencias más adelante.

## Alternativas consideradas
1. **Opción A (Desarrollo completo en plataforma):** Crear la clase `Refaccion`, el estado `EnEsperaDeRefaccion` y las pantallas de despacho de almacén en el sistema.
- *Qué implica:* 19 horas de trabajo técnico, modificación profunda de clases y máquinas de estado.
- *Qué cuesta:* Alto esfuerzo de diseño y riesgo de fricción operativa en planta.
2. **Opción B (Auditoría documental mediante el Servicio de Evidencias existente - Seleccionada):** No modificar el código ni el diagrama de clases. El técnico solicita sus piezas con el vale de papel actual de almacén y, al terminar, toma una foto del vale firmado y la adjunta como una segunda evidencia en `IServicioDeEvidencias`.
- *Qué implica:* Cero horas de rediseño de arquitectura y software.
- *Qué cuesta:* Depende de la inspección visual del emisor al liberar el ticket en lugar de un bloqueo automático por sistema.

## Estimación de esfuerzo

| Concepto | Esfuerzo |
|---|---|
| Cambios de diseño y documentación (Visión, RF-011, CU-06, C4 N3 y Figma) | 7 horas |
| Implementación (Clase `Refaccion`, métodos en `Ticket`, nuevo estado y controladores) | 8 horas |
| Pruebas (Pruebas unitarias de estados, validación de bloqueo a 'Resuelto' y pruebas de UI) | 4 horas |
| **Total** | **19 horas** |

**De dónde sale este número:** El cálculo se basa en modificar 4 documentos de especificación existentes, prototipar 2 pantallas nuevas en Figma (3 horas), modelar y codificar 1 clase nueva con sus pruebas unitarias (4 horas), modificar la lógica de transición de la máquina de estados en `Ticket` (4 horas) y realizar pruebas de integración de punta a punta entre los módulos de Operación y Archivos (4 horas), sumando un total estimado de 19 horas hombre (aproximadamente 3 a 4 días de trabajo de un solo desarrollador).

## Recomendación
**Rechazar para la versión actual (1.3) y aplazar para la versión 2.0.**
*Por qué:* El sistema actual está diseñado para comunicación ágil e inspección de tiempos de respuesta. Introducir el control formal de refacciones en esta etapa desvirtúa la frontera del sistema y agrega burocracia a los técnicos en planta. Para retomar este cambio en una versión 2.0, Almacén tendría que digitalizar primero su catálogo de inventario con códigos de barras o QR para que la captura no sea manual, y la empresa tendría que asignar un rol formal de Almacén con equipo de cómputo en mostrador. Mientras tanto, la **Opción B** cubre la auditoría sin arriesgar la estabilidad del software actual.
