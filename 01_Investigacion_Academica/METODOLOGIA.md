# Metodología de Desarrollo para Sistemas de Chatbots con Orquestación NLU + RAG

Marco técnico basado en el sistema Transibot, generalizable a cualquier
dominio.

------------------------------------------------------------------------

# 1. Propósito de la Metodología

Proporcionar un marco técnico y metodológico para el diseño,
construcción y despliegue de chatbots inteligentes compuestos por:\
- **NLU tradicional (RASA u otro motor NLU)** - **Sistema de
Recuperación y Generación (RAG)** - **Orquestador inteligente** -
**Frontend conversacional**

Este marco se inspira en la arquitectura del sistema Transibot, pero
está diseñado para adaptarse a sectores como salud, banca, legal,
retail, educación, PropTech, gobierno, entre otros.

------------------------------------------------------------------------

# 2. Fase de Análisis y Captura de Requisitos

## 2.1 Objetivo

Identificar el dominio del negocio, los usuarios, las necesidades
conversacionales y los requerimientos técnicos del chatbot.

## 2.2 Análisis del Dominio

Comprender el contexto, procesos y regulaciones del sector.\
Sugerencias para completar:\
- ¿Qué documentos forman el conocimiento base?\
- ¿Qué normas o reglas rigen el sector?\
- ¿Qué consultas son frecuentes?\
- ¿Qué procesos se pueden automatizar vía chatbot?

## 2.3 Requisitos Funcionales

Ejemplos:\
- Responder preguntas frecuentes.\
- Explicar normativas o procedimientos.\
- Realizar acciones (reservas, trámites, consultas internas).\
- Proveer asistencia contextualizada según el perfil del usuario.

Sugerencias para completar:\
- Añadir 20--50 casos de uso reales.\
- Clasificar casos en informativos, transaccionales y mixtos.

## 2.4 Requisitos No Funcionales

Incluyen:\
- Latencia máxima aceptable.\
- Disponibilidad del sistema.\
- Niveles de precisión mínimos del NLU.\
- Confianza mínima del RAG.\
- Políticas de seguridad y privacidad.

Sugerencias para completar:\
- Definir SLAs por componente.\
- Establecer métricas de resiliencia.

## 2.5 Identificación de Fuentes para el RAG

Determinar el conjunto de documentos o bases que alimentarán el sistema
RAG.

Sugerencias para completar:\
- Catálogos de productos\
- Manuales operativos\
- Contratos\
- Normativas\
- Artículos legales\
- PDFs corporativos

------------------------------------------------------------------------

# 3. Fase de Diseño Arquitectónico y Definición de Componentes

## 3.1 Arquitectura General Propuesta

Capa Frontend\
↓\
Capa de Orquestación\
↓\
┌ RASA (Motor NLU)\
└ RAG (Recuperación y Generación)

Esta organización en capas permite desacoplar la interacción del
usuario, la interpretación del lenguaje y la generación avanzada de
respuestas.

------------------------------------------------------------------------

# 4. DESCRIPCIÓN TÉCNICA DE CADA CAPA (MARCO GENERAL)

## 4.1 Capa RAG -- Recuperación y Generación (BackRag)

### Rol de la capa

Funciona como el motor cognitivo profundo. Responde preguntas complejas
combinando búsqueda de información y generación con IA.

### Responsabilidades principales

-   Mantener una base de conocimiento vectorial (ChromaDB o similar).\
-   Realizar búsqueda híbrida (vectorial + palabras clave).\
-   Seleccionar los documentos más relevantes para cada consulta.\
-   Utilizar un modelo generativo (Claude, GPT, Llama) para construir
    respuestas naturales y contextualizadas.\
-   Devolver evidencias y fuentes.\
-   Permitir ejecución de funciones internas (function calling) si
    aplica.

### Entradas

-   Consulta en lenguaje natural.\
-   Contexto opcional (perfil del usuario, sesión, canal de
    comunicación).

### Salidas

-   Respuesta generada.\
-   Lista de fuentes utilizadas.\
-   Nivel de confianza de la respuesta.

### Cómo generalizar esta capa

Aplicable a cualquier industria que posea documentación estructurada o
no estructurada: manuales, políticas, reglamentos, contratos, fichas
técnicas, etc.

### Sugerencias para completar

-   Definir políticas de chunking.\
-   Especificar periodicidad de actualización de la base RAG.\
-   Determinar límites de seguridad para evitar alucinación del modelo.

------------------------------------------------------------------------

## 4.2 Capa NLU -- Interpretación del Lenguaje (RASA u otro motor NLU)

### Rol de la capa

Realizar la interpretación inicial del mensaje del usuario. Detectar
intención, entidades y contexto.

### Responsabilidades

-   Clasificar la intención del usuario.\
-   Extraer entidades importantes para el negocio.\
-   Activar reglas conversacionales según el dominio.\
-   Gestionar diálogos cortos y directos.\
-   Proveer puntajes de confianza.\
-   Activar mecanismos internos de fallback cuando el NLU no puede
    resolver la pregunta.

### Entradas

-   Texto del usuario.\
-   Historial conversacional.

### Salidas

-   Intención detectada.\
-   Entidades extraídas.\
-   Texto de respuesta preliminar, cuando aplique.\
-   Nivel de confianza.

### Cómo generalizar esta capa

Los intents, entidades y reglas dependerán del sector:\
- Salud → síntomas, citas, medicamentos\
- Banca → cuentas, préstamos, tarjetas\
- Legal → artículos, cláusulas, trámites\
- Retail → productos, disponibilidad

### Sugerencias para completar

-   Definir una matriz de intents (mínimo 30).\
-   Establecer coverage objetivo (\>80% de consultas comunes).\
-   Documentar reglas, stories y flujos.

------------------------------------------------------------------------

## 4.3 Capa de Orquestación -- RouterBack (API Gateway Inteligente)

### Rol de la capa

Decidir dinámicamente si la respuesta debe provenir del NLU o del RAG.\
Es el "cerebro operacional" que une todas las capas.

### Responsabilidades

-   Centralizar la comunicación entre frontend, NLU y RAG.\
-   Decidir si la respuesta debe generarse con lógica conversacional
    (RASA) o con búsqueda avanzada (RAG).\
-   Ejecutar criterios de fallback como:
    -   texto vacío\
    -   baja confianza\
    -   intención no reconocida\
    -   errores internos\
    -   metadata definida por RASA\
-   Unificar el formato de respuesta que recibe el frontend.\
-   Manejar trazabilidad y logs.\
-   Implementar monitoreo de salud y telemetría.

### Entradas

-   Mensaje del usuario con metadata.\
-   Respuesta del NLU.\
-   Respuesta del RAG.

### Salidas

-   Respuesta final al usuario.\
-   Indicadores de origen (NLU o RAG).\
-   Eventuales advertencias o sugerencias internas para mejorar el
    modelo.

### Cómo generalizar esta capa

Puede orquestar cualquier combinación de motores:\
- NLU + RAG\
- RAG + Llama3\
- Sistemas externos (APIs del negocio)\
- Orquestación multi-IA

### Sugerencias para completar

-   Definir una política clara de fallback.\
-   Especificar telemetría (tiempos, tasas de fallback, tasas de
    error).\
-   Registrar decisiones del orquestador para auditoría.

------------------------------------------------------------------------

## 4.4 Capa Frontend Conversacional

### Rol de la capa

Servir como interfaz entre los usuarios y el ecosistema conversacional.

### Responsabilidades

-   Mostrar mensajes del usuario y del bot.\
-   Gestionar la sesión y su identificador único.\
-   Renderizar botones, documentos, fuentes y explicaciones.\
-   Ofrecer una experiencia fluida en móviles y desktop.\
-   Mostrar indicadores de "bot escribiendo".\
-   Integrarse con RouterBack.

### Entradas

-   Respuesta del orquestador.\
-   Acciones del usuario (mensajes, clic en botones).

### Salidas

-   Interfaz conversacional amigable.\
-   Representación de fuentes del RAG.\
-   Historial de mensajes.

### Cómo generalizar esta capa

Puede adaptarse a múltiples productos:\
- Gestión de clientes\
- Portal de trámites\
- Asistente para empleados\
- Chat de ventas

### Sugerencias para completar

-   Especificar requerimientos de accesibilidad.\
-   Definir si se almacenará historial en el navegador o en backend.\
-   Integrar autentificación si el dominio lo requiere.

------------------------------------------------------------------------

# 5. Fase de Construcción, Desarrollo e Implementación

## 5.1 Construcción de la Capa NLU

-   Definición de intents y entidades.\
-   Creación del dataset de entrenamiento.\
-   Validación y pruebas.\
-   Entrenamiento iterativo.

Sugerencias para completar:\
- Incluir criterios para aceptar nuevos intents.\
- Establecer umbrales de confianza.

## 5.2 Construcción de la Capa RAG

-   Recolección de documentos.\
-   Limpieza y normalización de datos.\
-   División en fragmentos.\
-   Generación de embeddings.\
-   Indexación en base vectorial.\
-   Creación de mecanismos de evaluación.

Sugerencias para completar:\
- Establecer estrategia de actualización periódica.\
- Documentar modelos de embeddings utilizados.

## 5.3 Construcción del Orquestador

-   Implementación de reglas de decisión.\
-   Integración de clientes NLU y RAG.\
-   Gestión de errores controlados.\
-   Trazabilidad detallada del proceso.

Sugerencias para completar:\
- Crear una tabla con políticas de decisión.\
- Documentar los criterios de fallback adoptados por el negocio.

## 5.4 Construcción del Frontend

-   Diseño de la interfaz.\
-   Implementación de la lógica de conversación.\
-   Integración con backend.

Sugerencias para completar:\
- Test de usabilidad.\
- Prototipos A/B.

------------------------------------------------------------------------

# 6. Fase de Pruebas e Iteración

-   Pruebas de intención.\
-   Pruebas de respuesta del RAG.\
-   Pruebas de orquestación.\
-   Pruebas de carga.\
-   Pruebas de recuperación ante caídas.\
-   Evaluación continua mediante logs.

------------------------------------------------------------------------
