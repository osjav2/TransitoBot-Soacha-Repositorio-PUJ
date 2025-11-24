# TransitoBot-Soacha-Repositorio-PUJ
<div align="center">

<img src="https://via.placeholder.com/1280x640.png?text=TuGuia+Vial+Soacha+-+Asistente+IA" alt="Banner TransitoBot" width="100%">

<br/>
<br/>

<h1>🤖 TuGuía_Vial / TransitoBot</h1>
<h3>Asistente Inteligente de Normativa Vial para Soacha, Cundinamarca</h3>

[![Estado](https://img.shields.io/badge/Estado-Tesis_Finalizada-success?style=for-the-badge&logo=github)](https://github.com/)
[![Tech](https://img.shields.io/badge/Stack-Rasa_|_FastAPI_|_React-blueviolet?style=for-the-badge&logo=python)](https://github.com/)
[![AI](https://img.shields.io/badge/AI-RAG_+_Claude_Haiku-orange?style=for-the-badge&logo=anthropic)](https://github.com/)
[![Deploy](https://img.shields.io/badge/Despliegue-Docker_Compose-blue?style=for-the-badge&logo=docker)](https://github.com/)

<br/>

> **Resumen Ejecutivo:** Solución de Inteligencia Artificial Híbrida (NLU + Generativa) diseñada para democratizar el acceso a la normativa de tránsito en Soacha, abordando el incremento del **75% en infracciones** durante 2023.

</div>

---

## 🧭 Panel de Navegación

Explora los componentes de este producto de investigación:

<div align="center">
<table>
  <tr>
    <td align="center" width="25%">
      <a href="./01_Investigacion_Academica">
        <img src="https://img.icons8.com/clouds/100/book.png" width="60px"><br>
        <b>01. Investigación</b>
      </a><br>Tesis y Metodología
    </td>
    <td align="center" width="25%">
      <a href="./02_Base_Conocimiento_Legal">
        <img src="https://img.icons8.com/clouds/100/law.png" width="60px"><br>
        <b>02. Base Legal</b>
      </a><br>Normativa (Fuente RAG)
    </td>
    <td align="center" width="25%">
      <a href="./03_Sistema_TransitoBot">
        <img src="https://img.icons8.com/clouds/100/code.png" width="60px"><br>
        <b>03. Sistema (Código)</b>
      </a><br>Frontend, Backend, AI
    </td>
    <td align="center" width="25%">
      <a href="./04_Despliegue_Arquitectura">
        <img src="https://img.icons8.com/clouds/100/server.png" width="60px"><br>
        <b>04. Despliegue</b>
      </a><br>Docker y Arquitectura
    </td>
  </tr>
</table>
</div>

---

## 🏙️ Problemática: El Caso Soacha

En 2023, Soacha impuso **9,640 órdenes de comparendo**, evidenciando una brecha crítica entre la complejidad de la ley y el conocimiento del ciudadano.

| Indicador | Dato 2023 | Interpretación |
| :--- | :--- | :--- |
| **Crecimiento Infracciones** | 📈 **+75%** | Incremento drástico respecto a 2022. |
| **Infracción C14** | 🚗 **4,901 casos** | Transitar en sitios/horas prohibidas (Falta de información). |
| **Impacto Social** | 👥 **700k Habitantes** | Afectados por congestión y falta de cultura vial. |

*Fuente: Federación Colombiana de Municipios (FCM) - SIMIT.*

---

## 🧠 Arquitectura Híbrida del Sistema

El sistema implementa una arquitectura de microservicios orquestada que decide inteligentemente entre respuestas predefinidas (Rasa) y generación basada en contexto legal (RAG con ChromaDB).

```mermaid
graph TD;
    User((👤 Ciudadano)) -->|HTTP| Front[💻 Frontend React+Vite];
    Front -->|REST| Router[🚦 RouterBack FastAPI];
    
    subgraph "Core de Decisión"
    Router -->|1. Intento?| Rasa[🤖 RASA NLU];
    Rasa -- "Si tiene confianza" --> Router;
    Rasa -- "Fallback / No sabe" --> Router;
    end
    
    Router -->|2. Consulta Compleja| BackRag[🧠 BackRag System];
    
    subgraph "Retrieval Augmented Generation"
    BackRag -->|Query| Chroma[(🗄️ ChromaDB)];
    Chroma -- "Contexto Legal (CNT)" --> BackRag;
    BackRag -->|Prompt + Contexto| Claude[☁️ Claude AI API];
    Claude -->|Respuesta Natural| BackRag;
    end
    
    BackRag --> Router;
    Router --> Front;
