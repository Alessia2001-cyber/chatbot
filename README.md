# Instrucciones de Ejecución y Entrega - Sistema SegurPlus v5.0

## 1. Orden Recomendado de Ejecución
Para probar el sistema completo, se debe seguir el orden cronológico de los notebooks:
1. `Chatbot5.2 (1).ipynb`: Versión inicial con el flujo básico.
2. `Chatbot5.2.ipynb`: Versión final con arquitectura híbrida (Knowledge Base/RAG) y LLM (Groq).

## 2. Cómo Lanzar la Demo Final
Sigue estos pasos concretos para ejecutar el chatbot en tu entorno local:
1. Asegúrate de tener instalado Python 3.10+ y Jupyter Notebook.
2. Instala las dependencias necesarias ejecutando en tu terminal:
   ```bash
   pip install langchain-groq langchain-core



# Sistema de Asistencia Inteligente - SegurPlus v5.0

**Desarrollado por:** @Alessia2001-cyber  
**Asignatura:** Proyecto de Sistemas Inteligentes / IA  

Este repositorio contiene la entrega correspondiente al **Checkpoint 2** para el sistema de atención al cliente de **SegurPlus**. La solución implementa un flujo híbrido que combina una máquina de estados para la gestión de datos sensibles y validación de pólizas, junto con un modelo de lenguaje (LLM) enriquecido mediante **RAG (Retrieval-Augmented Generation)** para la consulta de directivas contractuales de la empresa.

---

## 📂 Estructura del Repositorio

*   **`Chatbot5.2 (1).ipynb`**: Primera iteración del sistema centrado en la lógica de estados, anonimización de PII (DNI, teléfonos) y estructura conversacional inicial.
*   **`Chatbot5.2.ipynb`**: Versión final y avanzada del chatbot. Integra la conexión con la API de Groq (`llama-3.3-70b-versatile`) y el motor RAG que recupera información específica del contrato según el tipo de seguro.
*   **`RUN.md`**: Archivo con las instrucciones detalladas de instalación de dependencias, configuración de la API Key y orden de ejecución de los cuadernos.

---

## 🎯 Control de Calidad: "Quality Gate" (10 Casos de Prueba)

Para asegurar la robustez del sistema frente a fallos y el cumplimiento estricto del contrato del cliente, se han diseñado y ejecutado los siguientes casos de prueba:

| ID | Escenario de Prueba | Entrada de Usuario | Comportamiento Esperado | Estado (Pass/Fail) |
|---|---|---|---|---|
| **TC01** | Selección de Contexto de Coche | "1" o "coche" | El sistema activa el flujo de asistencia de vehículos. | **PASS** |
| **TC02** | Entrada de Contexto Erróneo | "Quiero una pizza" | El bot gestiona el fallo y vuelve a ofrecer las opciones 1, 2 o 3. | **PASS** |
| **TC03** | Validación de Póliza Correcta | "98765432" | Valida el formato de 8 dígitos e inicia el chat del LLM. | **PASS** |
| **TC04** | Póliza con Formato Erróneo | "123" o "abc" | Detecta que no cumple el patrón y vuelve a solicitar los 8 dígitos. | **PASS** |
| **TC05** | Escudo de Privacidad: DNI | "Mi DNI es 12345678Z" | El motor regex anonimiza el DNI antes de enviar el prompt al LLM. | **PASS** |
| **TC06** | Escudo de Privacidad: Teléfono | "Llama al 654321098" | Detecta el teléfono y aplica la máscara protectora. | **PASS** |
| **TC07** | Ventana de Memoria Activa | Conversación de 3 turnos | El modelo retiene el contexto previo respetando el límite de 5 turnos. | **PASS** |
| **TC08** | Comando de Cierre de Sesión | "exit" o "salir" | El chatbot detiene la ejecución del bucle de forma limpia. | **PASS** |
| **TC09** | Recuperación RAG (Coche) | "Tengo un pinchazo" | El LLM responde aplicando la regla RAG (no cubre pinchazos simples). | **PASS** |
| **TC10** | Bloqueo de Datos de Pago | "Mi IBAN es ES12..." | El filtro de seguridad oculta el número de cuenta bancaria. | **PASS** |

### Gestión de Fallos Críticos y Mitigaciones
1. **Fuga de Información Sensible (PII):** Cualquier dato como DNI o IBAN introducido por el usuario es procesado localmente por la función `analizar_seguridad()` antes de realizar la llamada a la API externa de Groq, eliminando el riesgo de filtraciones.
2. **Alucinaciones en Coberturas:** Para evitar que el LLM prometa servicios no cubiertos, se inyecta dinámicamente la información contractual recuperada (RAG) en el *System Prompt* y se configura la temperatura a `0.3` para una respuesta determinista.
