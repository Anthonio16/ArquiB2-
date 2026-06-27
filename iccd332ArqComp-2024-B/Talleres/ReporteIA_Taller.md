You:
weatherquito

Copilot:


You:
Escribe un script Python que consulte la API de OpenWeatherMap 
para Quito (lat=-0.2299, lon=-78.5249), guarde los resultados 
en ~/clima-quito-hoy.csv agregando una fila por ejecución, y 
un script shell get-weather.sh que lo ejecute.

Copilot:
Aquí tienes el script Python y el script shell solicitados:

### Script Python: `get_weather.py`
Este script consulta la API de OpenWeatherMap y guarda los resultados en un archivo CSV.

```python
import requests
import csv
from datetime import datetime

# Configuración
API_KEY = "TU_API_KEY"  # Reemplaza con tu clave de API de OpenWeatherMap
LAT = -0.2299
LON = -78.5249
URL = f"https://api.openweathermap.org/data/2.5/weather?lat={LAT}&lon={LON}&appid={API_KEY}&units=metric"
CSV_FILE = "/home/tu_usuario/clima-quito-hoy.csv"  # Cambia "tu_usuario" por tu nombre de usuario

def get_weather():
    response = requests.get(URL)
    if response.status_code == 200:
        data = response.json()
        weather_data = {
            "datetime": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "temperature": data["main"]["temp"],
            "humidity": data["main"]["humidity"],
            "weather": data["weather"][0]["description"]
        }
        return weather_data
    else:
        print(f"Error al consultar la API: {response.status_code}")
        return None

def save_to_csv(data):
    file_exists = False
    try:
        file_exists = open(CSV_FILE).readline() != ""
    except FileNotFoundError:
        pass

    with open(CSV_FILE, mode="a", newline="") as file:
        writer = csv.DictWriter(file, fieldnames=["datetime", "temperature", "humidity", "weather"])
        if not file_exists:
            writer.writeheader()
        writer.writerow(data)

if __name__ == "__main__":
    weather = get_weather()
    if weather:
        save_to_csv(weather)
        print("Datos del clima guardados correctamente.")
    else:
        print("No se pudo obtener el clima.")
```

### Script Shell: `get-weather.sh`
Este script ejecuta el script Python.

```bash
#!/bin/bash

# Ejecuta el script Python
python3 /ruta/a/get_weather.py
```

### Instrucciones:
1. Reemplaza `TU_API_KEY` en el script Python con tu clave de API de OpenWeatherMap.
2. Cambia `/home/tu_usuario/` en el script Python por tu directorio de usuario.
3. Guarda el script Python como `get_weather.py` en una ubicación adecuada.
4. Guarda el script shell como `get-weather.sh` y dale permisos de ejecución:
   ```bash
   chmod +x get-weather.sh
   ```
5. Ejecuta el script shell para obtener y guardar los datos del clima:
   ```bash
   ./get-weather.sh
   ```

