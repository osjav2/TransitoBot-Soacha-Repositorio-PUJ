# Base de Conocimiento Legal
# ⚖️ Base de Conocimiento Legal (Corpus)

Almacena la **fuente de verdad** del sistema. Los documentos aquí alojados constituyen el insumo principal para el proceso de *Retrieval Augmented Generation* (RAG).

## 📂 Inventario Normativo

| Documento | Referencia Legal | Función en el Sistema |
| :--- | :--- | :--- |
| **[Código Nacional de Tránsito](./CodigoNacionaldeTransitoTerrestre.pdf)** | **Ley 769 de 2002** | **Núcleo.** Documento maestro que regula la movilidad en todo el territorio nacional. |

---

## 🧠 Integración con el Sistema RAG

El microservicio **`backRag`** (ubicado en la carpeta `03`) realiza el siguiente proceso automatizado con los archivos de esta carpeta:

```mermaid
graph LR;
    PDF[📄 Código de Tránsito] -->|1. Ingesta| Text[📝 Texto Plano];
    Text -->|2. Fragmentos| Chunks[🧩 Chunks];
    Chunks -->|3. Embedding| Vectors[🔢 Vectores];
    Vectors -->|4. Almacenamiento| Chroma[(🗄️ ChromaDB)];
