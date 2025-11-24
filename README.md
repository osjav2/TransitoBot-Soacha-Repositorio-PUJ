# TransitoBot-Soacha-Repositorio-PUJ
<div align="center">

<img src="./01_Investigacion_Academica/EC381959-8F90-405C-A843-FA705D85F3DF.jpeg" alt="Logo oficial TransitoBot" width="19%">

<br/>
<br/>

<h1>🤖 TuGuía_Vial / TransitoBot</h1>
<h3>Asistente Inteligente de Normativa Vial para Soacha, Cundinamarca</h3>

<div align="center">

[![Estado](https://img.shields.io/badge/Estado-Tesis_Finalizada-2ea44f?style=for-the-badge&logo=github)](./01_Investigacion_Academica)

[![Stack](https://img.shields.io/badge/Stack-Rasa_|_FastAPI_|_React-blueviolet?style=for-the-badge&logo=python)](./03_Sistema_TransitoBot)

[![AI](https://img.shields.io/badge/AI-RAG_+_Claude_Haiku-FF9900?style=for-the-badge&logo=anthropic)](./03_Sistema_TransitoBot/backRag)

[![Deploy](https://img.shields.io/badge/Despliegue-Docker_Compose-0db7ed?style=for-the-badge&logo=docker)](./04_Despliegue_Arquitectura)

</div>

<br/>

> **Resumen Ejecutivo:** Solución de Inteligencia Artificial Híbrida (NLU + Generativa) diseñada para democratizar el acceso a la normativa de tránsito en Soacha, abordando el incremento del **75% en infracciones** durante 2023.

</div>

---

## 🧭 Arquitectura del trabajo de grado

Acceso rápido a los componentes del producto:

<div align="center">
<table>
  <tr>
    <td align="center" width="25%">
      <a href="./01_Investigacion_Academica">
        <img src="https://img.icons8.com/fluency/96/learning.png" width="50px"><br>
        <br>
        <b>01. Investigación</b>
      </a><br>
      <sub>Fundamentación & Tesis</sub>
    </td>

    <td align="center" width="25%">
      <a href="./02_Base_Conocimiento_Legal">
        <img src="https://img.icons8.com/fluency/96/law.png" width="50px"><br>
        <br>
        <b>02. Base Legal</b>
      </a><br>
      <sub>Corpus Normativo (RAG)</sub>
    </td>

    <td align="center" width="25%">
      <a href="./03_Sistema_TransitoBot">
        <img src="https://img.icons8.com/fluency/96/source-code.png" width="50px"><br>
        <br>
        <b>03. Código Fuente</b>
      </a><br>
      <sub>React • FastAPI • Rasa</sub>
    </td>

    <td align="center" width="25%">
      <a href="./04_Despliegue_Arquitectura">
        <img src="https://img.icons8.com/fluency/96/server.png" width="50px"><br>
        <br>
        <b>04. Infraestructura</b>
      </a><br>
      <sub>Docker • Orquestación</sub>
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
