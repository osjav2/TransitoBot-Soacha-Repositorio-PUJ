# ☁️ Infraestructura y Arquitectura de Despliegue

Este directorio contiene los planos técnicos, guías de contenedores y scripts de orquestación necesarios para poner en marcha **TransitoBot**.

## 📂 Recursos de Ingeniería

| Recurso | Tipo | Descripción |
| :--- | :---: | :--- |
| **[📘 Documentación de Arquitectura](./arquitectura.md)** | 📐 Diseño | Diagramas detallados de flujo, comunicación entre microservicios y lógica de fallback. |
| **[🐳 Guía de Despliegue Docker](./DOCKER.md)** | 📖 Manual | Instrucciones paso a paso para levantar el entorno, troubleshooting y comandos útiles. |
| **[⚙️ Script de Orquestación](./docker-compose.yml)** | 🛠️ YAML | Configuración de servicios, redes y volúmenes para el despliegue automatizado. |

---

## 🏗️ Vista Rápida del Sistema

El sistema utiliza una arquitectura de **Microservicios** contenerizada. A continuación se presenta el diagrama de alto nivel de los contenedores orquestados:

```mermaid
graph TD;
    %% Estilos Dark Mode Tech
    classDef front fill:#0c2546,stroke:#38bdf8,stroke-width:2px,color:white;
    classDef logic fill:#0f3928,stroke:#4ade80,stroke-width:2px,color:white;
    classDef ai fill:#381808,stroke:#fb923c,stroke-width:2px,color:white;
    classDef db fill:#2e1065,stroke:#a78bfa,stroke-width:2px,color:white;

    subgraph "Docker Compose Network"
        Front[💻 Frontend Container]:::front <-->|HTTP 8080| Router[🚦 RouterBack Container]:::logic;
        
        Router <-->|HTTP 5005| Rasa[🤖 RASA Container]:::logic;
        Router <-->|HTTP 8000| BackRag[🧠 BackRag Container]:::ai;
        
        BackRag <-->|Volumen Persistente| Chroma[(🗄️ ChromaDB)]:::db;
    end

    linkStyle default stroke:#9ca3af,stroke-width:1px;
