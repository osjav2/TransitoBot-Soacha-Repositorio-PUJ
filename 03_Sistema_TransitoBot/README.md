# 💻 Sistema TransitoBot (Código Fuente)

Esta carpeta contiene el núcleo técnico de la solución, dividido en microservicios independientes.

## 📂 Arquitectura de Microservicios

| Carpeta | Tecnología | Puerto | Función Principal |
| :--- | :--- | :--- | :--- |
| **`/frontend`** | React + Vite | `5173` | **Interfaz de Usuario.** Chat moderno y responsivo para el ciudadano. |
| **`/routerback`** | Python FastAPI | `8080` | **Orquestador.** Cerebro central que recibe el mensaje y decide quién responde. |
| **`/rasa`** | RASA Open Source | `5005` | **Agente NLU.** Maneja saludos, despedidas e intenciones simples (no legales). |
| **`/backRag`** | Python + LangChain | `8000` | **Experto Legal.** Motor RAG que busca en la base vectorial (ChromaDB) y genera respuesta con IA. |

---

## 🛠️ Flujo de Comunicación (Para Desarrolladores)

Si estás estudiando este código, el flujo de un mensaje es el siguiente:

1.  **Usuario** escribe en el `frontend`.
2.  **Frontend** envía petición POST al `routerback`.
3.  **Routerback** consulta primero a `rasa`:
    * *¿Tienes confianza alta (>0.8)?* → RASA responde.
    * *¿Confianza baja?* → Se activa el `backRag`.
4.  **BackRag** busca en el Código Nacional de Tránsito y responde con Claude/OpenAI.
5.  **Routerback** devuelve la respuesta final al `frontend`.

> **Nota:** Para ejecutar todo el sistema junto, regresa a la carpeta `04_Despliegue_Arquitectura` y usa Docker Compose.
