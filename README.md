API [openweathermap](https://openweathermap.org/)

Paso 1: Crear un nuevo proyecto en Glitch
Visita [Glitch](https://glitch.com):

Ve a [Glitch](https://glitch.com).
Crear un nuevo proyecto:

Haz clic en New Project y selecciona Create a Node App.
Paso 2: Configurar el proyecto en Glitch
Configura el proyecto:

Glitch te dará un proyecto básico de Node.js con algunos archivos preconfigurados. Vamos a modificarlos.
  Editar el archivo package.json:
  
  Agrega las dependencias necesarias (asegúrate de que express y node-fetch están en la lista de dependencias). Tu package.json debería verse algo así:
  json


    {
      "name": "weather-bot",
      "version": "1.0.0",
      "description": "A simple weather bot for Twitch",
      "main": "server.js",
      "scripts": {
        "start": "node server.js"
      },
      "dependencies": {
        "express": "^4.17.1",
        "node-fetch": "^2.6.1"
      },
      "engines": {
        "node": "14.x"
      },
      "license": "MIT"
    }
  
Editar el archivo server.js:
  Reemplaza el contenido de server.js con el siguiente código:
  javascript

    const express = require('express');
    const fetch = require('node-fetch');
    const app = express();
    const API_KEY = 'YOUR_OPENWEATHERMAP_API_KEY';
    
    app.get('/weather', async (req, res) => {
        const city = req.query.city;
        if (!city) {
            return res.send('Por favor, proporciona una ciudad.');
        }

    try {
        const response = await fetch(`http://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric&lang=es`);
        const data = await response.json();
        
        if (data.cod !== 200) {
            return res.send(`Error: ${data.message}`);
        }

        const weatherDescription = data.weather[0].description;
        const temperature = data.main.temp;
        const feelsLike = data.main.feels_like;

        res.send(`El clima en ${city} es ${weatherDescription} con una temperatura de ${temperature}°C, pero se siente como ${feelsLike}°C.`);
    } catch (error) {
        res.send('Ocurrió un error al obtener el clima.');
    }
    });
    
    const listener = app.listen(process.env.PORT, () => {
        console.log('Your app is listening on port ' + listener.address().port);
    });

Importante: Reemplaza 'YOUR_OPENWEATHERMAP_API_KEY' con tu clave de API de OpenWeatherMap.
Guardar los cambios:
[Glitch](https://glitch.com) guarda los cambios automáticamente. Una vez que hayas pegado el código, tu aplicación debería estar funcionando.
Paso 3: Obtener la URL del proyecto en Glitch
Obtener la URL:
En la parte superior del editor de Glitch, encontrarás un botón Show o View App. Haz clic en él para abrir tu aplicación en una nueva pestaña.
Copia la URL de tu aplicación (será algo como https://your-project-name.glitch.me).
Paso 4: Configurar el comando en Nightbot
Ve a Nightbot:

Inicia sesión en Nightbot.
Crear el comando:

En el panel de control de Nightbot, ve a Comandos y selecciona Agregar comando.
Configura el comando de la siguiente manera:
yaml
Copiar código

     Comando: !clima
     Mensaje: $(urlfetch https://your-project-name.glitch.me/weather?city=$(querystring))
     Alias: (déjalo en blanco)
     Usuario: Todos
     Enfriamiento: 5 segundos
     
Reemplaza https://your-project-name.glitch.me con la URL de tu proyecto Glitch.
Paso 5: Probar el comando
Ve a tu canal de Twitch:

    Abre el chat de tu canal de Twitch y escribe !clima [Ciudad] (por ejemplo, !clima Madrid).

Nightbot debería responder con el clima de la ciudad especificada.
¡Y eso es todo! Ahora tienes un comando en Nightbot que responde con el clima de cualquier ciudad usando un script alojado en Glitch.





