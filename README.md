# Skill de Alexa - Clima 210458

Este proyecto implementa un **skill de Alexa** que proporciona información sobre el clima basado en las coordenadas específicas **20.239603 (latitud)** y **-97.954323 (longitud)**. Utiliza la API de OpenWeather para obtener los datos meteorológicos.

## 🚀 Características
- Proporciona el clima actual en una ubicación específica.
- Utiliza la API de OpenWeather para información precisa.
- Responde en español con descripciones claras.

---

## 📋 Requisitos
- Una cuenta de [Amazon Developer](https://developer.amazon.com/).
- Una cuenta en [OpenWeather](https://openweathermap.org/) con una API Key activa.
- Node.js instalado localmente para el desarrollo.

---

## 🛠️ Configuración

### **1. Configurar la API de OpenWeather**
1. Regístrate o inicia sesión en [OpenWeather](https://openweathermap.org/).
2. Obtén tu **API Key** desde el panel de usuario.
3. Asegúrate de que funcione accediendo a la siguiente URL en tu navegador o herramienta API:
   ```bash
   https://api.openweathermap.org/data/2.5/weather?lat=20.239603&lon=-97.954323&appid={TU_API_KEY}&units=metric&lang=es
   ```

---

### **2. Crear el Skill en Alexa Developer Console**
1. Accede a la [consola de Amazon Developer](https://developer.amazon.com/).
2. Crea un nuevo Skill:
   - **Skill Name**: Clima jorge.
   - **Default Language**: Español (MX).
   - **Skill Type**: Custom Skill.

3. Define el modelo de interacción:
   - **Invocation Name**: clima jorge.
   - **Intents**: Agrega un Intent llamado `clima jorge` con ejemplos como:
     ```
     ¿Cuál es el clima?
     Dime el clima.
     Me puedo bañar hoy?
     es buen dia para lavar la ropa?
     ```

4. Guarda y construye el modelo.

---

### **3. Configurar el backend con AWS Lambda**
1. Accede a [AWS Lambda](https://aws.amazon.com/lambda/).(nos saltamos el punto 1,2,3 ya que ocupamos el codigo que te da por default amazon)
2. Crea una nueva función Lambda:
   - **Runtime**: Node.js 18.x.
3. Copia y pega el siguiente código en tu función Lambda:(nosotros solo ingresamos nuestro apikey , latitud, longitud y editamos el mensaje que lanza alexa al inicio)

   ```javascript
   const axios = require('axios');
   const Alexa = require('ask-sdk-core');

   const skillBuilder = Alexa.SkillBuilders.custom();

   exports.handler = skillBuilder
     .addRequestHandlers({
       LaunchRequest() {
         return handlerInput.responseBuilder
           .speak("Bienvenido al skill del clima. Pregunta por el clima actual.")
           .getResponse();
       },
       GetWeatherIntent(handlerInput) {
         const lat = 20.239603;
         const lon = -97.954323;
         const API_KEY = 'TU_API_KEY_DE_OPENWEATHER';
         const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric&lang=es`;

         return axios.get(url)
           .then(response => {
             const weather = response.data.weather[0].description;
             const temp = response.data.main.temp;
             const speechText = `El clima actual es ${weather} con una temperatura de ${temp} grados Celsius.`;

             return handlerInput.responseBuilder
               .speak(speechText)
               .getResponse();
           })
           .catch(error => {
             console.error(error);
             return handlerInput.responseBuilder
               .speak("Lo siento, no pude obtener la información del clima.")
               .getResponse();
           });
       },
       HelpIntent() {
         return handlerInput.responseBuilder
           .speak("Pregunta por el clima actual.")
           .getResponse();
       },
       CancelAndStopIntent() {
         return handlerInput.responseBuilder
           .speak("Adiós")
           .getResponse();
       }
     })
     .lambda();
   ```

4. **Instala dependencias**:
   Ejecuta `npm install axios` y sube tu proyecto como un archivo `.zip`.(otorgado por el profesor)

5. Configura el **ARN** de Lambda en la consola de Alexa en la pestaña **Endpoint**.(se implemento un icono )

---

### **4. Probar el Skill**
1. Activa el modo de prueba en la consola de Alexa.
2. Usa frases como:
 ```
     ¿Cuál es el clima?
     Dime el clima.
     Me puedo bañar hoy?
     es buen dia para lavar la ropa?
     ```
---
### **5. Publicar el Skill**
1. Completa los detalles de distribución, incluyendo nombre, íconos e información adicional.
2. Envía para revisión.

---

## 📂 Estructura del Proyecto
```
├── index.js          # Código principal del skill
├── package.json      # Dependencias
└── README.md         # Documentación del proyecto
```

---

## 📜 Licencia
Este proyecto solo se desarrollo como una practica, por eso al momento de lanzarla a revision pusimos que no en todas las opciones .

---

Elaborado por jorge cazarez cruz  🚀
``` 

