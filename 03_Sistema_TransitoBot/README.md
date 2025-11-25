# 💻 Sistema TransitoBot (Código Fuente)

Directorio que contiene la implementación técnica de la solución, estructurada bajo un patrón de **Microservicios** contenerizados. Cada subdirectorio representa un servicio autónomo.

---

## 🧬 Flujo de Ejecución (Sequence Diagram)

El diagrama ilustra cómo interactúan los módulos de código cuando un ciudadano realiza una consulta compleja (ej: *"¿De cuánto es la multa por pasarme un semáforo?"*).

```mermaid
sequenceDiagram
    autonumber
    actor User as 👤 Ciudadano
    participant Front as 💻 Frontend (React)
    participant Router as 🚦 RouterBack (FastAPI)
    participant Rasa as 🤖 RASA (NLU)
    participant Rag as 🧠 BackRag (RAG)
    participant Chroma as 🗄️ ChromaDB

    User->>Front: Escribe consulta
    Front->>Router: POST /api/v1/chat/message
    
    Note over Router,Rasa: Paso 1: Intento de Clasificación
    Router->>Rasa: Enviar texto a NLU
    Rasa-->>Router: Retorna Intent + Confianza
    
    alt Confianza Alta (> 0.8)
        Router-->>Front: Respuesta Predefinida (RASA)
    else Confianza Baja (Fallback)
        Note over Router,Rag: Paso 2: Activación de IA Generativa
        Router->>Rag: Solicitar contexto legal
        
        rect rgb(20, 20, 20)
            Note right of Rag: Lógica RAG
            Rag->>Chroma: Búsqueda Vectorial (Embeddings)
            Chroma-->>Rag: Retorna Artículos Ley 769
            Rag->>Rag: Generar Prompt + Contexto
            Rag->>Rag: Invocar Claude AI
        end
        
        Rag-->>Router: Respuesta Generada Natural
        Router-->>Front: Respuesta Final
    end
    
    Front-->>User: Muestra mensaje
