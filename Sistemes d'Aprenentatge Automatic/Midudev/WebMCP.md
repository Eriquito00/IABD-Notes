# WebMCP

Es un MCP directamente conectado en el frontend normalmente conectado en formularios para facilitar el uso de la web, por ejemplo en la reserva de entradas.

El MCP está conectado por nombre y descripción al formulario y va pidiéndote los datos necesarios para hacer la reserva de una entrada a un partido de futbol.

Para el WebMCP tiene 3 annotations para que el modelo actue de una forma u otra:
- consequentialHint: hasta que el usuario no haga la confirmacion el modelo no ejecuta.
- readOnlyHint: solo se le permite leer al modelo.
- untrustedContentHint: aisla los datos para no obedecer ordenes ocultas.

