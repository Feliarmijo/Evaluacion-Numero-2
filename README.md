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

Codigo de Python


import requests

def geocodificar_direccion(direccion):
    api_key = 'abaa733d-0f84-41c8-ac3b-62019ac28cd6'
    url = f"https://graphhopper.com/api/1/geocode?q={direccion}&key={api_key}&locale=es"
    try:
        respuesta = requests.get(url)
        respuesta.raise_for_status()
        datos = respuesta.json()
        if datos['hits'] and len(datos['hits']) > 0:
            primer_resultado = datos['hits'][0]
            lat = primer_resultado['point']['lat']
            lng = primer_resultado['point']['lng']
            return f"{lat},{lng}"
        else:
            return None
    except requests.exceptions.RequestException as e:
        print(f"Error al geocodificar la dirección: {e}")
        return None

def obtener_direcciones(punto_inicio, punto_destino):
    api_key = 'abaa733d-0f84-41c8-ac3b-62019ac28cd6'
    url = f"https://graphhopper.com/api/1/route?point={punto_inicio}&point={punto_destino}&key={api_key}&locale=es"
    try:
        respuesta = requests.get(url)
        respuesta.raise_for_status()
        datos = respuesta.json()
        return datos
    except requests.exceptions.RequestException as e:
        print(f"Error al obtener la ruta: {e}")
        return None

while True:
    entrada_usuario = input("Ingrese su punto de partida como dirección (ej: Calle X 123, Comuna Y, Ciudad Z, Chile) (o 's' para salir): ").strip().lower()
    
    if entrada_usuario in ['s', 'salir']:
        print("Saliendo del programa. ¡Hasta luego!")
        break
    
    direccion_inicio = entrada_usuario
    coordenadas_inicio = geocodificar_direccion(direccion_inicio)
    
    if coordenadas_inicio is None:
        print("No se pudo geocodificar la dirección de partida. Intente con una dirección válida.")
        continue 
    
    direccion_destino = input("Ingrese su punto de destino como dirección (ej: Calle A 456, Comuna B, Ciudad C, Chile): ").strip()
    coordenadas_destino = geocodificar_direccion(direccion_destino)
    
    if coordenadas_destino is None:
        print("No se pudo geocodificar la dirección de destino. Intente con una dirección válida.")
        continue 
    
    datos_ruta = obtener_direcciones(coordenadas_inicio, coordenadas_destino)
    
    if datos_ruta and 'paths' in datos_ruta and len(datos_ruta['paths']) > 0:
        ruta = datos_ruta['paths'][0]
        
        
        distancia_metros = ruta['distance'] 
        distancia_km = distancia_metros / 1000 
        distancia_formateada = f"{distancia_km:.2f}"  s
        
        
        tiempo_milisegundos = ruta['time']  
        tiempo_segundos = tiempo_milisegundos / 1000  
        tiempo_minutos = tiempo_segundos / 60  
        tiempo_formateado = f"{tiempo_minutos:.2f}"  
        
        print(f"Distancia: {distancia_formateada} km")
        print(f"Tiempo estimado: {tiempo_formateado} minutos")
        
        instrucciones = ruta['instructions']
        print("Instrucciones del viaje:")
        for instruccion in instrucciones:
            print(f"- {instruccion['text']}")  
    else:
        print("No se pudo obtener la ruta. Verifique las direcciones e intente de nuevo.")
