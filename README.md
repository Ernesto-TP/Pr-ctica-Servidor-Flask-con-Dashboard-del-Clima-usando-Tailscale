# Practica-Servidor-Flask-con-Dashboard-del-Clima-usando-Tailscale
## Practica para Sistemas Programables 2PM

Ernesto Torres Pineda 
22211665

```
# ╔═══════════════════════════════════════════════════════════╗
# ║   Mini Dashboard del Clima con Flask                      ║
# ║   Lenguajes de Interfaz - TECNM / ITT - 2025              ║
# ║   Autor: [Ernesto Torres Pineda]                          ║
# ║	  No. Control: 22211665						   	          ║
# ║	  Fecha: 18/09/2025							              ║
# ║   Descripción: Servidor Flask consultando OpenWeatherMap  ║
# ╚═══════════════════════════════════════════════════════════╝

import os
import requests
from flask import Flask, render_template_string
from dotenv import load_dotenv

# Load variables from .env if present
load_dotenv()

app = Flask(_name_)
# CONFIGURACIÓN
API_KEY = os.getenv("API_KEY", "c445f1a141b0f55a9c4ae17aa1ec221e")
CITY = os.getenv("CITY", "Tijuana")

HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Clima en {{ city }}</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background: #f0f0f0;
        }
        .card {
            margin: 10vh auto;
            padding: 20px;
            max-width: 400px;
            border-radius: 12px;
            color: white;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
        }
        .cold { background: linear-gradient(135deg, #1e3c72, #2a5298); }
        .mild { background: linear-gradient(135deg, #56ab2f, #a8e063); }
        .hot { background: linear-gradient(135deg, #ff512f, #dd2476); }
        .error { background: #444; }
        .temp { font-size: 2em; margin: 10px 0; }
    </style>
</head>
<body>
    <div class="card {{ theme }}">
        <h1>Clima en {{ city }}</h1>
        <p class="temp">🌡️ {{ temp }}</p>
        <p>Condiciones: {{ weather }}</p>
    </div>
</body>
</html>
"""


# Esta ruta consulta el clima actual desde la API de OpenWeatherMap
# y devuelve una página HTML simple con la información del clima.
@app.route("/")
def clima():
    try:
        url = f"https://api.openweathermap.org/data/2.5/weather?q={CITY}&appid={API_KEY}&units=metric&lang=es"
        resp = requests.get(url, timeout=5)
        resp.raise_for_status()
        data = resp.json()

        temp = f"{data['main']['temp']} °C"
        weather = data["weather"][0]["description"]

        # Define theme based on temperature
        t = data["main"]["temp"]
        if t < 10:
            theme = "cold"
        elif t < 25:
            theme = "mild"
        else:
            theme = "hot"

    except Exception as e:
        temp = "N/D"
        weather = "Error al obtener datos"
        theme = "error"

    return render_template_string(HTML_TEMPLATE, city=CITY, temp=temp, weather=weather, theme=theme)


# host="0.0.0.0" permite que se pueda acceder desde otras máquinas
# port=5000 es el puerto por defecto, se puede cambiar con variable de entorno
# debug=False en producción (True solo para desarrollo)
if _name_ == "_main_":
    # Use 0.0.0.0 so it's reachable remotely
    app.run(host="0.0.0.0", port=5000, debug=False)

```

## Loom 
https://www.loom.com/share/32a656f2f9f949e086dd4f9c4a4881a1?sid=b2779d0b-0e19-4e91-bc90-6e12d9828a4c

## Imagenes de app en PC
### Ciudad: Tijuana
<img width="1919" height="1029" alt="Screenshot 2025-09-19 154358" src="https://github.com/user-attachments/assets/6092c5da-85d4-491c-bb42-4331ac99f63f" />

### Ciudad: Liverpool
<img width="1919" height="1030" alt="Screenshot 2025-09-19 154647" src="https://github.com/user-attachments/assets/b787397f-53f0-4122-8d6b-85c6007186b1" />

