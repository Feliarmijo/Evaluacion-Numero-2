# Evaluacion-Numero-2
Archivo .py sobre la geolocalizacion 
El código es un script de Python diseñado para:

Permitir que los usuarios ingresen direcciones en formato de texto (como "Calle X 123, Comuna Y, Chile") en lugar de coordenadas GPS.
Convertir esas direcciones en coordenadas geográficas usando la API de Graphhopper.
Calcular y mostrar la ruta entre dos puntos en Chile, incluyendo la distancia, el tiempo estimado y las instrucciones paso a paso.
Incluir mejoras como:
Todos los textos en español para una mejor interacción.
Formateo de números (como distancia y tiempo) con un máximo de dos decimales.
Una opción para salir del programa ingresando "s" o "salir".
Está optimizado para direcciones en Chile, limitando los resultados de búsqueda a ese país para mayor precisión.
Este script se ejecuta en un entorno como Visual Studio Code dentro de una VM (como DEVASC), y requiere un token de Graphhopper para acceder a su API. Es útil para aplicaciones de geolocalización, como planificar viajes o rutas dentro de Chile.
Ejemplo de ejecución:
El programa pregunta: "Ingrese su punto de partida como dirección (ej: Calle X 123, Comuna Y, Chile) (o 's' para salir): "
Tú ingresas: "Avenida Apoquindo 6415, Comuna Las Condes, Chile"
El programa geocodifica eso a coordenadas.
Luego pregunta por el destino: "Ingrese su punto de destino como dirección..."
Tú ingresas: "Plaza Ñuñoa, Comuna Ñuñoa, Chile"
El programa calcula la ruta y muestra:
Distancia: 10.45 km
Tiempo estimado: 25.67 minutos
Instrucciones del viaje: - "Salga por la avenida principal" - "Gire a la izquierda en la calle secundaria" (en español)
Si escribes "s", sale con: "Saliendo del programa. ¡Hasta luego!"
