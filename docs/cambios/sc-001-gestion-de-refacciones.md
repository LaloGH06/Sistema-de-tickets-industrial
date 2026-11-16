# SC-001: Gestión y validación de refacciones requeridas en órdenes de trabajo

> Plantilla de solicitud de cambio · Ingeniería de Software I · SIS3407
> Guárdala en tu repositorio como `doc/cambios/sc-001-gestion-de-refacciones.md`
> Quien escribe esta solicitud es la **dupla de revisión**, no el dueño del sistema.

---

## Estado
Propuesta · 12 de noviembre de 2026

## Quién la pide
Jonathan Michel García Pacheco.

## Qué se pide
Cuando el técnico ponga el ticket en estado 'En proceso', debe poder solicitar una o varias refacciones al almacén indicando el código o número de parte y la cantidad requerida. El sistema no debe permitir que el ticket se marque como 'Resuelto' hasta que el encargado de Almacén confirme la entrega física de las refacciones solicitadas mediante un vale de salida firmado.

## Por qué
En la planta, muchas fallas mecánicas y eléctricas no se pueden reparar de inmediato porque Mantenimiento depende de que Almacén entregue los repuestos. Hoy en día, los técnicos dicen que el trabajo está "terminado" sin haber cambiado la pieza averiada, o el ticket se queda varado sin que nadie sepa si el retraso es culpa del técnico o de que no hay piezas en stock.

## Qué pasa si no se hace
Los tickets se seguirán cerrando en falso sin refacciones instaladas, y las métricas seguirán castigando injustamente a los técnicos por demoras que en realidad son de desabasto en el almacén.

## Tipo de mantenimiento
- [ ] **Correctivo** · algo está mal y hay que repararlo
- [ ] **Adaptativo** · el entorno cambió y hay que ajustarse
- [x] **Perfectivo** · funciona, pero puede ser mejor
- [ ] **Preventivo** · todavía no falla, pero va a fallar

*Justificación:* El sistema actual funciona bien para órdenes simples, pero se busca perfeccionar el flujo operativo agregando el control de materiales entre departamentos.

## Criterio de aceptación
- ¿El sistema permite agregar una lista de refacciones con número de parte y cantidad al atender un ticket en proceso? (Sí / No)
- ¿El sistema bloquea el cambio a estado 'Resuelto' si existen refacciones pendientes de confirmación por Almacén? (Sí / No)
- ¿Se registra en el historial del ticket el vale de almacén firmado al entregar las piezas? (Sí / No)

## Prioridad que sugiere el solicitante
Media. Es muy útil para la coordinación entre Mantenimiento y Almacén, pero no impide que la plataforma continúe operando para incidencias de comunicación o ajustes que no requieren repuestos.
