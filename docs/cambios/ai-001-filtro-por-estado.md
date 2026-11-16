# Análisis de impacto de SC-001
---

## Solicitud que se analiza
Referencia a la solicitud: `sc-001-filtro-por-estado.md`, pedida por Jonathan Michel García Pacheco el 12 de noviembre de 2026.

## Traducción a términos del sistema
La solicitud pide filtrar visualmente la lista de tickets por estado. En mi sistema:
1. En el frontend (UI), se agregan botones de filtro rápido en la vista de lista.
2. La entidad `Ticket` ya cuenta con el atributo `-estado: string` y el método `+estadoActual()`, por lo que **no se modifica ninguna clase**.
3. El filtrado se realiza en memoria sobre la lista de tickets ya cargada en la pantalla.

## Impacto directo
| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| Requisito RF-004 (Visualizar tickets) | 2 | Agregar la capacidad de aplicar filtro por estado en la vista de lista. |
| Caso de uso CU-01 / CU-02 | 2 | Mencionar el filtro como opción de visualización en la interfaz. |
| Prototipo en Figma | 2 | Agregar los botones de filtro (*Todos*, *Abiertos*, *En proceso*, *Resueltos*) en la parte superior de la tabla. |

## Impacto indirecto
| Artefacto | Unidad | Por qué se ve afectado |
|---|---|---|
| Visión del producto | 1 | Ninguno. No cambia el alcance ni las reglas del negocio. |
| Arquitectura C4 y ADR-001 | 3 | Ninguno. No afecta componentes, servidores ni base de datos. |
| Diagrama de clases | 4 | Ninguno. La clase `Ticket` ya tiene el atributo `estado`. |
| Diagrama de secuencia (CU-04) | 4 | Ninguno. La secuencia modela la validación de cierre, no la consulta de lista. |
| Máquina de estados y SLA | 4 | Ninguno. No se alteran tiempos ni transiciones de ciclo de vida. |

## Lo que queda sin tocar
- Todas las clases e interfaces del backend (`Ticket`, `Usuario`, `SLA`, `Evidencia`, etc.) quedan 100% intactas.
- El módulo de **Archivos y Evidencias** y el de **Identidad** no se tocan.
- La base de datos y sus consultas permanecen exactamente iguales.

## Riesgos
- **Riesgo técnico:** Ninguno. Es un simple filtro de interfaz de usuario.

## Alternativas consideradas
1. **Opción A (Filtrado en frontend - Seleccionada):** Filtrar en memoria en el cliente con JavaScript/UI los tickets que ya están cargados en pantalla.
- *Qué implica:* Cero cambios en el servidor o la base de datos.
2. **Opción B (Filtrado por consulta en base de datos):** Enviar el parámetro de estado en la consulta SQL.
- *Qué implica:* Modificar el endpoint del servidor, innecesario para el volumen actual de tickets.

## Estimación de esfuerzo
| Concepto | Esfuerzo |
|---|---|
| Actualizar redacción de RF-004 | 15 minutos |
| Diseñar barra de filtros en Figma | 30 minutos |
| Implementación en interfaz | 45 minutos |
| Pruebas visuales de filtrado | 30 minutos |
| **Total** | **2 horas** |

**De dónde sale este número:** El cambio es 100% de interfaz de usuario: no crea tablas, no altera clases del dominio y aprovecha los datos que la pantalla ya recibe.

## Recomendación
**Aceptar e implementar en la versión actual.**
*Por qué:* Es una mejora inmediata para la usabilidad en planta, no conlleva ningún riesgo de arquitectura y su costo de desarrollo es mínimo (2 horas).
