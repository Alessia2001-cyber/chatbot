# Instrucciones de Ejecución - Sistema SegurPlus v5.0

## 1. Orden Recomendado de Ejecución
Para probar el sistema completo de este repositorio, ejecute los notebooks en el siguiente orden:
1. `Chatbot5.2 (1).ipynb`: Versión inicial con el flujo conversacional básico y la estructura lógica de estados.
2. `Chatbot5.2.ipynb`: Versión final que integra la arquitectura híbrida con nuestra base de conocimiento (RAG) y el modelo de lenguaje (Groq).

## 2. Cómo Lanzar la Demo Final (Pasos concretos)
Siga estos pasos para ejecutar y probar el chatbot de manera local:
1. Clone este repositorio o descargue los archivos.
2. Asegúrese de tener instalado **Python 3.10 o superior**.
3. Instale las librerías necesarias ejecutando en su terminal:

   ```bash
   pip install langchain-groq langchain-core
