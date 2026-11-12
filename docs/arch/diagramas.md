# Diagramas de Arquitectura y Diseño · Sistema de Tickets Industriales

Este documento reúne la arquitectura de software (Modelo C4) y los diagramas estructurales y de comportamiento en lenguaje UML, renderizados en bloques de texto Mermaid nativos.

---

## 1. Arquitectura C4 · Nivel 1: Diagrama de Contexto
Delimita la frontera del sistema frente a los actores operativos y directivos de la planta de producción.

```mermaid
flowchart TD
    op(["Usuario Operativo\nEmisor / Receptor"])
    adm(["Administrador / Dirección"])
    sis["Sistema de tickets industriales"]
    
    op -->|levanta tickets, atiende solicitudes y entrega evidencia| sis
    op -->|valida o rechaza los tickets que levantó| sis
    adm -->|consulta métricas SLA y audita tiempos de respuesta| sis
    adm -->|valida y libera tickets| sis
    
    style sis fill:#ff5d00,color:#fff
```

---

## 2. Arquitectura C4 · Nivel 2: Diagrama de Contenedores
Muestra la estructura interna a alto nivel, separando la interfaz de usuario, el servidor y la base de datos.

```mermaid
flowchart TD
    op(["Usuario Operativo\nEmisor / Receptor"])
    adm(["Administrador / Dirección"])
    
    subgraph sistema["Sistema de tickets industriales"]
        web["App web\nmobile first"]
        srv["Servidor de la plataforma\nmonolito modular"]
        bd[("Base de datos\nfolios, historial y evidencias")]
    end
    
    op -->|usa desde su celular| web
    adm -->|usa desde el navegador| web
    web -->|peticiones por internet| srv
    srv -->|lee y escribe| bd
    
    style srv fill:#ff5d00,color:#fff
```

---

## 3. Arquitectura C4 · Nivel 3: Diagrama de Componentes
Desglosa el monolito modular del servidor en sus componentes principales.

```mermaid
flowchart TD
    web["App web"]
    bd[("Base de datos")]
    
    subgraph srv["Servidor de la plataforma · monolito modular"]
        ident["Identidad y estructura\npersonas, roles y departamentos"]
        oper["Operación\nciclo de vida del ticket y cronómetro"]
        arch["Archivos\nvalida, sube y guarda evidencia"]
    end
    
    web -->|inicia y cierra sesión| ident
    web -->|levanta, atiende, valida y adjunta evidencia| oper
    oper -->|pregunta a qué área asignar y quién puede liberar| ident
    oper -->|pregunta si hay evidencia y le envía la adjunta| arch
    ident -->|guarda personas, roles y áreas| bd
    oper -->|guarda tickets y tiempos de cada fase| bd
    arch -->|guarda los documentos| bd
    
    style oper fill:#ff5d00,color:#fff
```

---

## 4. Diseño Estructural · Diagrama de Clases UML
Representa las entidades principales del dominio y sus métodos de validación y operación.

```mermaid
classDiagram
    direction LR
    
    class Usuario {
        -id: string
        -nombre: string
        -nombreUsuario: string
        -contrasenaHash: string
        -rol: string
        -permisoLevantar: bool
        +iniciarSesion(contrasena: string) bool
        +cerrarSesion()
        +puedeLevantarTickets() bool
        +puedeValidarCierre(idEmisor: string) bool
    }
    
    class Area {
        -id: string
        -nombre: string
        -tipo: string
        +esGerencia() bool
    }
    
    class IValidadorDeCierre {
        <<interface>>
    }
```

---

## 5. Diseño de Comportamiento · Diagrama de Secuencia UML
Modela el flujo de interacción paso a paso durante la validación y cierre (o rechazo) de un ticket.

```mermaid
sequenceDiagram
    actor Emisor as Usuario (Emisor / Admin)
    participant UI as VistaDetalleTicket
    participant T as ticket : Ticket
    participant V as validador : IValidadorDeCierre
    participant Obs as sla : IObservadorDeEstado
    participant Notif as notificador : Notificador

    Emisor->>+UI: presiona botón de validación
    
    alt Emisor aprueba la solución (presiona "Liberado")
        UI->>+T: liberar(quien: Usuario)
        T->>+V: validarCierre(idUsuario, idEmisor)
        V-->>-T: true
        T->>T: cambiarEstado("Cerrado")
        T->>T: registrarMovimiento(idUsuario, "Ticket Liberado y Cerrado")
        T->>+Obs: actualizar(ticket)
        Note over Obs: SLA ejecuta detener() y fija tiempo final
        Obs-->>-T: ok
        T->>+Notif: actualizar(ticket)
        Notif-->>-T: ok
        T-->>-UI: true
        UI-->>Emisor: Notifica "Ticket cerrado definitivamente" y redirige al Dashboard
        
    else Emisor rechaza el trabajo (presiona "No Aceptado" con notas)
        UI->>+T: rechazar(quien: Usuario, notas: string)
        T->>+V: validarCierre(idUsuario, idEmisor)
        V-->>-T: true
        T->>T: cambiarEstado("Reabierto")
        T->>T: registrarMovimiento(idUsuario, "Solución Rechazada: " + notas)
        T->>+Obs: actualizar(ticket)
        Note over Obs: SLA ejecuta reiniciar() restableciendo contador
        Obs-->>-T: ok
        T->>+Notif: actualizar(ticket)
        Notif-->>-T: ok
        T-->>-UI: true
        UI-->>Emisor: Notifica "Ticket reabierto enviado a corrección"
    end
    
    deactivate UI
```

---

## 6. Diseño de Comportamiento · Diagrama de Estados UML
Define el ciclo de vida completo de un ticket de trabajo y los eventos que provocan sus transiciones.

```mermaid
stateDiagram-v2
    [*] --> Abierto : generar() [RF-003]
    Abierto --> EnProceso : atender(receptor) [RF-005]
    Abierto --> Abierto : reasignar(nuevaArea) [RF-010]
    
    EnProceso --> Resuelto : resolver(evidencias) [RF-006 / tieneEvidencia == true]
    EnProceso --> EnProceso : reasignar(nuevaArea) [RF-010]
    
    Resuelto --> Cerrado : liberar(quien) [RF-009 / RNF-SEG-001 validado]
    Resuelto --> Reabierto : rechazar(quien, notas) [RF-007, RF-008]
    
    Reabierto --> EnProceso : atender(receptor) [RF-005]
    Reabierto --> Reabierto : reasignar(nuevaArea) [RF-010]
    
    Cerrado --> [*]
```
