# Agregar Instrucciones al Agente  

En el capítulo anterior, creaste tu primer agente básico e iniciaste una conversación con él.  
Ahora, daremos un paso más al aprender sobre **prompts del sistema** y por qué son esenciales para dar forma al comportamiento de tu agente.  


## ¿Qué es un Prompt del Sistema?  

Un prompt del sistema es un conjunto de **instrucciones** que proporcionas al modelo al crear un agente.  
Piensa en él como la **personalidad y el libro de reglas** para tu agente: define cómo debe responder el agente, qué tono debe usar y qué limitaciones debe seguir.  

Sin un prompt del sistema, tu agente puede responder de manera genérica. Al agregar instrucciones claras, puedes adaptarlo a tus necesidades.  

### Los prompts del sistema:  

- Aseguran que el agente se mantenga **consistente** a través de las conversaciones  
- Ayudan a guiar el **tono y rol** del agente (por ejemplo, maestro amigable, revisor de código estricto, bot de soporte técnico)  
- Reducen el riesgo de que el agente dé **respuestas irrelevantes o fuera de tema**  
- Te permiten **codificar reglas** que el agente debe seguir (por ejemplo, "siempre responde en JSON")  


## Agregar Instrucciones a Tu Agente  

Al crear un agente, puedes pasar el parámetro `instructions`.  
Aquí hay un ejemplo:  

```python
agent = project_client.agents.create_agent(
    model="gpt-4o",
    name="mi-agente",
    instructions="Eres un asistente de soporte útil para Microsoft Foundry. Siempre proporciona respuestas concisas, paso a paso."
)
print(f"Agente creado con prompt del sistema, ID: {agent.id}")
```

Ahora, cada vez que el agente procese una conversación, intentará seguir tus **instrucciones del sistema**.  


## Usar un Archivo de Instrucciones Externo  

En lugar de codificar las instrucciones en tu script de Python, a menudo es mejor almacenarlas en un **archivo de texto separado**.  
Esto hace que sean más fáciles de editar y mantener.  

Primero, crea un archivo llamado **`instrucciones.txt`** en la carpeta workshop con el siguiente contenido:  

```txt
Eres Contoso PizzaBot, un asistente de IA que ayuda a los usuarios a ordenar pizza.

Tu función principal es asistir a los usuarios en ordenar pizza, consultar menús y rastrear el estado de los pedidos.

## directrices
Al interactuar con los usuarios, sigue estas directrices:
1. Sé amigable, útil y conciso en tus respuestas.
1. Cuando los usuarios quieran ordenar pizza, asegúrate de recopilar toda la información necesaria (tipo de pizza, opciones).
1. Contoso Pizza tiene tiendas en múltiples ubicaciones. Antes de realizar un pedido, verifica si el usuario ha especificado la tienda desde la cual ordenar.
   Si no lo ha hecho, asume que está ordenando desde la tienda de San Francisco, USA.
1. Tus herramientas proporcionarán precios en USD.
   Al proporcionar precios al usuario, convierte a la moneda apropiada para la tienda desde la cual el usuario está ordenando.
1. Tus herramientas proporcionarán horarios de recogida en UTC.
   Al proporcionar horarios de recogida al usuario, convierte a la zona horaria apropiada para la tienda desde la cual el usuario está ordenando.
1. Cuando los usuarios pregunten sobre el menú, proporciona las opciones disponibles claramente. Lista como máximo 5 entradas del menú a la vez, y pregunta al usuario si desea escuchar más.
1. Si los usuarios preguntan sobre el estado del pedido, ayúdalos a verificarlo usando su ID de pedido.
1. Si no estás seguro acerca de alguna información, haz preguntas aclaratorias.
1. Siempre confirma los pedidos antes de realizarlos para asegurar la precisión.
1. No hables de nada más que no sea Pizza
1. Si no tienes un UserId y Nombre, siempre comienza solicitando eso.

## Herramientas y Acceso a Datos
- Usa el **Contoso Pizza Store Information Vector Store** para buscar información sobre tiendas, como dirección y horarios de apertura.
    - **Herramienta:** `file_search`
    - Solo devuelve información encontrada en el vector store o archivos cargados.
    - Si la información es ambigua o no se encuentra, solicita aclaración al usuario.

## Respuesta
Interactuarás con los usuarios principalmente a través de voz, por lo que tus respuestas deben ser naturales, cortas y conversacionales.
1. **Solo usa texto plano**
2. Sin emoticones, Sin marcado, Sin markdown, Sin html, solo texto plano.
3. Usa lenguaje corto y conversacional.

Cuando los clientes pregunten cuánta pizza necesitan para un grupo, usa la función calculadora de pizza para proporcionar recomendaciones útiles basadas en el número de personas y su nivel de apetito.
```


## Modificar el Código del Agente  

Ahora, actualiza tu `agent.py` para cargar estas instrucciones y establecer parámetros de generación (`top_p` y `temperature`):  

Encuentra el código 

```python
agent = project_client.agents.create_agent(
    model="gpt-4o",
    name="mi-agente"
)
print(f"Agente creado, ID: {agent.id}")
```

Reemplaza este código con 

```python
agent = project_client.agents.create_agent(
    model="gpt-4o",
    name="pizza-bot",
    instructions=open("instrucciones.txt").read(),
    top_p=0.7,
    temperature=0.7,
)
print(f"Agente creado con prompt del sistema, ID: {agent.id}")
```

Al hacer esto:  
- El agente **seguirá las instrucciones de PizzaBot** de tu `instrucciones.txt`.  
- Los parámetros `top_p` y `temperature` te dan control sobre la **creatividad y aleatoriedad** en las respuestas.  


## Ejecutar el Agente  

Prueba el Agente:  

```shell
python agent.py
```

Intenta modificar tu `instrucciones.txt` y vuelve a ejecutar el agente. Verás cómo las instrucciones del sistema influyen directamente en la personalidad y el comportamiento del agente.  

Ahora puedes chatear con tu agente directamente en el terminal. Escribe `salir` o `terminar` para detener la conversación.  


## Resumen  

En este capítulo, has:  

- Aprendido qué es un **prompt del sistema**  
- Comprendido por qué agregar **instrucciones** es importante  
- Creado un agente con un **prompt del sistema personalizado**  
- Usado un **archivo de instrucciones externo (`instructions.txt`)**  
- Experimentado con **configuraciones de generación** (`top_p` y `temperature`)  




## Muestra de código final

```python 
<!--@include: ../codesamples/es/agent_3_instructions.py-->
```

*Traducido usando GitHub Copilot.*
