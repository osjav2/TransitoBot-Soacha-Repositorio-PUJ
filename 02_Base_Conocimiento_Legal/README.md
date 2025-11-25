# Base de Conocimiento Legal
# ⚖️ Base de Conocimiento Legal (Corpus)

Este directorio almacena la **fuente de verdad** del sistema. Los documentos aquí alojados constituyen el insumo principal para el proceso de *Retrieval Augmented Generation* (RAG).

## 📂 Inventario Normativo

| Documento | Referencia Legal | Función en el Sistema |
| :--- | :--- | :--- |
| **[Código Nacional de Tránsito](./CodigoNacionaldeTransitoTerrestre.pdf)** | **Ley 769 de 2002** | **Núcleo.** Documento maestro que regula la movilidad en todo el territorio nacional. |

---

## 🧠 Integración con el Sistema RAG

Estos documentos no son estáticos. El microservicio **`backRag`** (ubicado en la carpeta `03`) realiza el siguiente proceso automatizado con los archivos de esta carpeta:

```mermaid
graph LR;
    PDF[📄 Código de Tránsito] -->|1. Ingesta & Limpieza| Text[📝 Texto Plano];
    Text -->|2. Chunking| Chunks[🧩 Fragmentos];
    Chunks -->|3. Embedding (OpenAI/Cohere)| Vectors[🔢 Vectores];
    Vectors -->|4. Almacenamiento| Chroma[(🗄️ ChromaDB)];
