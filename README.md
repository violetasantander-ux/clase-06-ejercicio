# clase-06-ejercicio
ejercicio en clases
<!doctype html>
<html lang="es">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <title>Un fetch</title>
        <style>
            body { font-family: Helvetica, Arial, sans-serif; }
        </style>
    </head>
    <body>
        <h1>Hola mundo</h1>

        <!-- Este <ol> (lista ordenada) empieza vacío. Lo vamos a llenar con JavaScript una vez tengamos la respueta al fetch -->
        <ol id="instituciones"></ol>
		
		<h2>Employer reputation</h2>
        <ol id="employer"></ol>
		<h3>Academic reputation</h3>
        <ol id="academic"></ol>
		

        <script>
            // 1) Guardamos una referencia al elemento <ol> del DOM.

            const uno = document.querySelector("#employer");
			
			const otro = document.querySelector("#academic");

            // 2) La URL del "endpoint": el lugar de internet donde están los datos. En este caso, es una API que devuelve JSON.
            const URL = "https://api.myjson.online/v1/records/43144fc9-94f4-4928-9509-bfc1fda8ba48";

            // 3) fetch() hace una petición HTTP a esa URL.

            fetch(URL)
                // 4) Primer .then(): se ejecuta cuando el servidor responde
                .then((respuesta) => {
                    // 5) respuesta.ok es true solo si el código de estado HTTP
                    if (!respuesta.ok) {
                        throw new Error("Error HTTP: " + respuesta.status);
                    }

                    // 6) respuesta.json() lee el cuerpo de la respuesta y lo convierte
                    return respuesta.json();
                })

                // 7) Segundo .then(): se ejecuta cuando ya tenemos los datos
                .then((datos) => {
                    // 8) Según cómo esté armada esta API, los registros vienen
                    var qs = datos.data;

                    // 9) console.log() no muestra nada en la página: aparece en la
                    console.log("Datos recibidos:", qs);

                    // 10) qs es un array. forEach() recorre cada elemento uno por uno.

                    qs.forEach((x) => {
                        // Nota: usar += con innerHTML dentro de un forEach funciona por ahora, pero en listas grandes es poco eficiente y seguro. 
						if(x.employer_reputation > 75.5){
                             uno.innerHTML += `<li>󠁧󠁢󠁥󠁮󠁧${x.name} - ${x.location}</li>`;			
							 }
					    if(x.academic_reputation > 72){
                             otro.innerHTML += `<li>󠁧󠁢󠁥󠁮󠁧${x.name} - ${x.location}</li>`;
							 }
                    });
                })

                // 11) .catch(): atrapa CUALQUIER error que haya ocurrido en la cadena de arriba (fallo de red, respuesta.ok falso, JSON mal formado, etc.).
                .catch((error) => {
                    console.error("Algo salió mal:", error);
                });

            // RESUMEN del flujo:
            // fetch(URL)              -> pide los datos al servidor
            //   .then(respuesta)      -> llega la respuesta (aún no son los datos finales)
            //   .then(datos)          -> ya tenemos los datos listos para usar
            //   .catch(error)         -> si algo falló en cualquier paso anterior, cae aquí
        </script>
    </body>
</html>
