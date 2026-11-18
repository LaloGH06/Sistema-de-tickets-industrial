# Sistema de Tickets Industriales

**Ingeniería de Software I (SIS3407)** · *Universidad Anáhuac Querétaro*

- **Alumno:** Jorge Eduardo García Hernández (00545791)
- **Dupla:** Jonathan Michel García Pacheco

---

## ¿Qué es este proyecto?
Es una plataforma para gestionar y auditar órdenes de trabajo e incidencias en una planta industrial de manufactura de plásticos. Reemplaza los reportes por WhatsApp y llamadas informales mediante:
- Asignación de **folios únicos** y seguimiento de tiempos (SLA).
- **Evidencia obligatoria:** No se puede marcar un ticket como resuelto sin adjuntar foto o documento.
- **Doble visto bueno:** Solo el usuario emisor o dirección pueden liberar y cerrar el ticket definitivamente.

---

##  Enlaces principales
-  **Video de Presentación (YouTube):** https://youtu.be/V7q0pghPHVk
-  **Prototipo Interactivo (Figma):** https://www.figma.com/design/1zI6qyqK3WlLbo0p0qJ9mE/Sin-t%C3%ADtulo?node-id=0-1&t=whWTJceFO5E2I7PL-1

---

##  Arquitectura resumida
- **Estilo:** Monolito Modular (`docs/arch/adr-001-monolito-modular.md`) dividido en tres módulos: *Identidad*, *Operación* y *Archivos*.
- **Patrones:**
  - **Strategy:** Para la validación de cierre (`IValidadorDeCierre`).
  - **Observer:** Para actualizar cronómetros de SLA al cambiar estados (`IObservadorDeEstado`).

---
## Video de presentación
[Ver el video en YouTube](https://youtu.be/MTBlmfP5iLc)
##  Contenido del repositorio
Toda la documentación técnica se encuentra en la carpeta `docs/`:

```text
Sistema-de-tickets-industrial/
├── README.md
└── docs/
    ├── vision-del-producto.md           # Alcance y límites del sistema
    ├── guion-entrevista.md              # Entrevista y levantamiento en planta
    ├── especificacion-requisitos.md     # Requisitos funcionales y no funcionales
    ├── diagramas/                       # Casos de uso (.drawio y .png)
    ├── arch/
    │   ├── adr-001-monolito-modular.md  # Registro de decisión arquitectónica
    │   └── diagramas.md                 # Diagramas C4 y UML en Mermaid
    └── cambios/
        ├── sc-001-filtro-por-estado.md  # Solicitud de cambio de la dupla
        └── ai-001-filtro-por-estado.md  # Análisis de impacto y decisión
