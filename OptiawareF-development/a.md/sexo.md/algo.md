// Función para generar las preguntas en el formulario
function generarPreguntas(data, responses) {
  let contador = 1
    const cuestionarioForm = document.getElementById("cuestionarioForm");
    const arrayRespuestas = Object.values(responses);
    data.forEach(pregunta => {
      
        const divPregunta = document.createElement("div");
        divPregunta.classList.add("pregunta");

        const preguntaTexto = document.createElement("span");
        preguntaTexto.innerHTML = `<strong>${contador}.- ${pregunta.Value}</strong><br>`;
        divPregunta.appendChild(preguntaTexto);
      
        arrayRespuestas.forEach((respuesta, index) => {
          const label = document.createElement("label");
          label.innerHTML = `<input type="radio" name="pregunta${contador}" value="${index}" /> ${respuesta}<br>`;
          divPregunta.appendChild(label);
        });

        cuestionarioForm.appendChild(divPregunta);
        contador++;
    });
    generarBoton();
}





function evaluarRespuestas(event) {
  event.preventDefault(); // Prevenir el envío del formulario (no recargará la página)

  const cuestionarioForm = document.getElementById("cuestionarioForm");
  const preguntas = cuestionarioForm.getElementsByClassName("pregunta");

  // Objeto para almacenar las respuestas seleccionadas con el ID de la pregunta como clave y el índice de la respuesta seleccionada como valor
  const respuestasSeleccionadas = {};

  // Recorremos cada pregunta para obtener la respuesta seleccionada
  for (let i = 0; i < preguntas.length; i++) {
    const pregunta = preguntas[i];
    const preguntaId = pregunta.dataset.id; // Aquí asumimos que cada pregunta tiene un atributo "data-id" que contiene el id de la pregunta

    // Obtenemos el valor de la respuesta seleccionada dentro de esta pregunta
    const respuestaSeleccionada = pregunta.querySelector('input[name="pregunta' + preguntaId + '"]:checked');

    // Si se encontró una respuesta seleccionada, la agregamos al objeto de respuestas seleccionadas
    if (respuestaSeleccionada) {
      const respuestaSeleccionadaNumero = respuestaSeleccionada.value;
      respuestasSeleccionadas[preguntaId] = Number(respuestaSeleccionadaNumero); // Convertimos el valor a número y lo guardamos
    }
  }
console.log(respuestasSeleccionadas);

  // Aquí puedes utilizar el arreglo preguntasRespondidas para obtener los IDs de las preguntas respondidas
  console.log("IDs de preguntas respondidas:", preguntasRespondidas);



  // Aquí tendrías el objeto "respuestasSeleccionadas" con las preguntas (id) y sus respuestas seleccionadas (índice) para enviarlo en la solicitud POST

  // Realizar aquí la solicitud POST a la API para guardar las respuestas
  // Puedes utilizar fetch u otra librería para hacer la solicitud POST
  // Aquí un ejemplo de cómo sería usando fetch:
  const apiUrl = 'https://dev-api.optiaware.com/api/UserResponses';
  const jwtToken = 'eyJhbGciOiJIUzM4NCIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIwOTk2YTIwMS02NjljLTQzNTktOTcwMy1kMzkwNGY4YTFiNzYiLCJzdWIiOiJTYWlkIiwiZW1haWwiOiJzYWlkZGlhekBnbWFpbC5jb20iLCJleHAiOjE2OTEzODgyMDUsImlzcyI6Im9wdGlhd2FyZS5jb20iLCJhdWQiOiJhcGkub3B0aWF3YXJlLmNvbSJ9.rAxWAyEAMLpN-5SmMEBg5gBClqRr2xIdPh8HTd6mBwELL2j_UZedfrCnjZIiwnfl'; // Reemplaza con tu token JWT válido

  const headers = {
    Authorization: `Bearer ${jwtToken}`,
    'Content-Type': 'application/json'
  };

  fetch(apiUrl, {
    method: 'POST',
    headers: headers,
    body: JSON.stringify(respuestasSeleccionadas)
  })
    .then(response => {
      if (!response.ok) {
        throw new Error('Error en la solicitud');
      }
      return response.json(); // Si la API devuelve una respuesta JSON, puedes manejarla aquí
    })
    .then(data => {
      // Aquí puedes hacer algo con la respuesta de la API si es necesario
      console.log('Respuestas enviadas y guardadas con éxito:', data);
    })
    .catch(error => {
      console.error('Error:', error);
    });

  // Mostrar el mensaje en el elemento con id "resultadoEvaluacion"
  const resultadoElement = document.getElementById("resultadoEvaluacion");
  resultadoElement.innerHTML = "Completaste el cuestionario y este es tu resultado.";

  resultadoElement.style.visibility = "visible"; // Mostrar el elemento
  return false; // Evita el envío real del formulario
}





















function evaluarRespuestas(event) {
  event.preventDefault(); // Prevenir el envío del formulario (no recargará la página)

  const respuestasSeleccionadas = [];

  const cuestionarioForm = document.getElementById("cuestionarioForm");
  const preguntas = cuestionarioForm.getElementsByClassName("pregunta");

  // Recorremos cada pregunta para obtener la respuesta seleccionada
  for (let i = 0; i < preguntas.length; i++) {
    const pregunta = preguntas[i];
  console.log(preguntas[i]);
    for (let j = 0; j < 4; j++) {
      const respuestaSeleccionada = pregunta.querySelector('input[name="pregunta' + i + '"][value="' + j + '"]:checked');
         console.log(respuestaSeleccionada);

      if (respuestaSeleccionada) {
        respuestasSeleccionadas.push(respuestaSeleccionada.value);
      }
    }
  }
  // Puedes utilizar fetch u otra librería para hacer la solicitud POST
  // Aquí un ejemplo de cómo sería usando fetch:
  console.log(respuestasSeleccionadas);
console.log(preguntasRespondidas);


  const apiUrl = 'https://dev-api.optiaware.com/api/UserResponses';
  const jwtToken = 'eyJhbGciOiJIUzM4NCIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIwOTk2YTIwMS02NjljLTQzNTktOTcwMy1kMzkwNGY4YTFiNzYiLCJzdWIiOiJTYWlkIiwiZW1haWwiOiJzYWlkZGlhekBnbWFpbC5jb20iLCJleHAiOjE2OTEzOTIwODMsImlzcyI6Im9wdGlhd2FyZS5jb20iLCJhdWQiOiJhcGkub3B0aWF3YXJlLmNvbSJ9.xUUsD6QeaiXloVTXT6dHLn1iGVddrRh5ds30p6wZFeRXx0Vcr3SA3M9cCBH1jLxO'; // Tu token JWT válido
  const headers = {
    Authorization: `Bearer ${jwtToken}`,
    'Content-Type': 'application/json'
  };

  fetch(apiUrl, {
    method: 'POST',
    headers: headers,
    body: JSON.stringify()
  })
  .then(response => {
    if (!response.ok) {
      throw new Error('Error en la solicitud');
    }
    return response.json(); // Si la API devuelve una respuesta JSON, puedes manejarla aquí
  })
  .then(data => {
    // Aquí puedes hacer algo con la respuesta de la API si es necesario
    console.log('Respuestas enviadas y guardadas con éxito:', data);
  })
  .catch(error => {
    console.error('Error:', error);
  });

  // Mostrar el mensaje en el elemento con id "resultadoEvaluacion"
  const resultadoElement = document.getElementById("resultadoEvaluacion");
  resultadoElement.innerHTML = "Completaste el cuestionario y este es tu resultado.";

  resultadoElement.style.visibility = "visible"; // Mostrar el elemento
  return false; // Evita el envío real del formulario
}




  // Recorremos cada pregunta para obtener las respuestas seleccionadas
  for (let i = 0; i < preguntas.length; i++) {
    const pregunta = preguntas[i];
    console.log(pregunta); // Muestra la pregunta en la consola

    // Realizamos una iteración para obtener las respuestas (índices) seleccionadas en esta pregunta
    for (let j = 0; j < 4; j++) {
      const respuestaSeleccionada = pregunta.querySelector('input[name="pregunta' + i + '"][value="' + j + '"]:checked');
      console.log(respuestaSeleccionada); // Muestra la respuesta seleccionada en la consola

      // Si se encontró una respuesta seleccionada, la agregamos al arreglo de respuestas seleccionadas
      if (respuestaSeleccionada) {
        respuestasSeleccionadas.push(respuestaSeleccionada.value);
      }
    }
  }










function evaluarRespuestas(event) {
  event.preventDefault(); // Prevenir el envío del formulario (no recargará la página)

  const cuestionarioForm = document.getElementById("cuestionarioForm");
  const preguntas = Array.from(cuestionarioForm.getElementsByClassName("pregunta"));
  console.log(preguntas);
  // Creamos un arreglo para almacenar las respuestas seleccionadas
  const respuestasSeleccionadas = preguntas.map((pregunta, index) => {
    const respuestaSeleccionada = pregunta.querySelector(`input[name="pregunta${index + 1}"]:checked`);
    return respuestaSeleccionada ? respuestaSeleccionada.value : null;
  });

  // Validar que todas las preguntas tengan una respuesta seleccionada
  if (respuestasSeleccionadas.includes(null)) {
    alert("Por favor, responde todas las preguntas antes de enviar el cuestionario.");
    return;
  }
console.log(respuestasSeleccionadas);
 console.log(preguntasRespondidas);

 const responses = {};
  preguntasRespondidas.forEach((id, index) => {
    responses[id] = respuestasSeleccionadas[index];
  });
console.log(responses);
  // Realizar aquí la solicitud POST a la API para guardar las respuestas
 // Puedes utilizar fetch u otra librería para hacer la solicitud POST
  // Aquí un ejemplo de cómo sería usando fetch:
 

  const apiUrl = 'https://dev-api.optiaware.com/api/UserResponses';
  const jwtToken = 'eyJhbGciOiJIUzM4NCIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIwOTk2YTIwMS02NjljLTQzNTktOTcwMy1kMzkwNGY4YTFiNzYiLCJzdWIiOiJTYWlkIiwiZW1haWwiOiJzYWlkZGlhekBnbWFpbC5jb20iLCJleHAiOjE2OTEzOTYwNDgsImlzcyI6Im9wdGlhd2FyZS5jb20iLCJhdWQiOiJhcGkub3B0aWF3YXJlLmNvbSJ9.S-dV2hYBvMJfGcI8nGi-VCL_0GJ1nnI7tCihmrvju3ZXstv-xcokiQeqdQzbq2vM'; // Tu token JWT válido
  const headers = {
    Authorization: `Bearer ${jwtToken}`,
    'Content-Type': 'application/json'
  };

  fetch(apiUrl, {
    method: 'POST',
    headers: headers, 
    body: JSON.stringify({ Responses: responses }) // Enviamos el objeto "responses" en el body
  })
  .then(response => {
    if (!response.ok) {
      throw new Error('Error en la solicitud');
    }
    return response.json(); // Si la API devuelve una respuesta JSON, puedes manejarla aquí
  })
  .then(data => {
    // Aquí puedes hacer algo con la respuesta de la API si es necesario
    console.log('Respuestas enviadas y guardadas con éxito:', data);
  })
  .catch(error => {
    console.error('Error:', error);
  });

  // Mostrar el mensaje en el elemento con id "resultadoEvaluacion"
  const resultadoElement = document.getElementById("resultadoEvaluacion");
  resultadoElement.innerHTML = "Completaste el cuestionario y este es tu resultado.";

  resultadoElement.style.visibility = "visible"; // Mostrar el elemento
  return false; // Evita el envío real del formulario
}










 <header>
    <nav class="headerNav">
        <ul>
            <li class="headerNav__li"><a href="home.html">Inicio</a></li>
            <li class="headerNav__li"><a href="registro.html">Registrarse</a></li>
            <li class="headerNav__li"><a href="Ingresar.html">Ingresar</a></li>
            <li class="headerNav__li"><a href="informacion.html">Información</a></li>
            <li class="headerNav__li"><a href="preguntas.html">Cuestionario</a></li>
            <li class="headerNav__li"><a href="ejercicios.html">Ejercicios</a></li>
            <li class="headerNav__li"><a href="nosotros.html">Nosotros</a></li>
        </ul>
    </nav>
    <button class="menuButton"></button>
    <div id="resultadoEvaluacion"></div> 
    <h1 class="htitle">Test Visual</h1>
</header> 