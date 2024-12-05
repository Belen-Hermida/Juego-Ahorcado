Este proyecto es un **juego de ahorcado** interactivo en el que los jugadores deben adivinar una palabra oculta. Al comienzo, se elige una palabra aleatoria de un conjunto predefinido, y el jugador tiene 6 intentos para acertar las letras. Cada vez que el jugador selecciona una letra, el juego muestra si fue correcta o incorrecta, actualizando la visualización de la palabra y la imagen del ahorcado según los intentos restantes. Si el jugador adivina todas las letras de la palabra o se queda sin intentos, el juego termina, mostrando un mensaje de victoria o derrota. La implementación en **JavaScript** incluye la selección aleatoria de palabras, el manejo de los intentos, y la actualización en tiempo real del estado del juego (aciertos y errores). Además, al seleccionar una letra, se deshabilita el botón correspondiente para evitar que se seleccione nuevamente. En cuanto al **CSS**, se usaron estilos responsivos para asegurar que el juego se vea bien en pantallas de laptop y dispositivos móviles. Se emplearon animaciones de rebote y sacudida para resaltar los aciertos y errores. 

Para el cuerpo de ahorcado.html se ocupo: 
```html
<div class="container">
    <h1 class="Titulo">¡Juego del Ahorcado!</h1>
    <h1 id="final" class="zoom-in"></h1>
    <h3 id="acierto"></h3>
    <div class="flex-row no-wrap">
        <h2 class="palabra" id="palabra"></h2>
        <picture>
            <img src="img_1.png" alt="" id="img1" style="display:none;">
            <img src="img_2.png" alt="" id="img2" style="display:none;">
            <img src="img_3.png" alt="" id="img3" style="display:none;">
            <img src="img_4.png" alt="" id="img4" style="display:none;">
            <img src="img_5.png" alt="" id="img5" style="display:none;">
            <img src="img_6.png" alt="" id="img6" style="display:none;">
            <img src="img_7.png" alt="" id="img7" style="display:none;">
        </picture>
    </div>
    <div class="flex-row" id="oportunidades">
        <div class="col">
            <h3>Intentos restantes: <span id="intentos">6</span></h3>
        </div>
        <div class="col">
            <button onclick="iniciar()" id="reset">Elegir otra palabra</button>
            <button onclick="pista()" id="pista">Dame una pista!!!</button>
            <div id="ayu_pista" class="fade-in"></div>
        </div>
    </div>
    <div class="flex-row">
        <div class="col">
            <div class="flex-row" id="abecedario"></div>
        </div>
        <div class="col"></div>
    </div>
</div>
```
Se hizo uso de JavaScript para las funciones para la creacion del juego del ahorcado, entre las funciones estan, el generador de palabras el cual se encarga de dar alguna palabra al asar la cual sera la palabra a adivinar, el generador de los espacios para cada letra, el generador del abecedario,el controlador de la cantidad de intentos que tiene el jugador, el que dara una pista en caso que el jugador la pida, entre otras funciones

```javascript

var palabras = [["conejo", "Un animal"], ["piña", "Es una fruta"], ["zapato", "Los utilizas en los pies"], ["pantalon", "Es una prenda de vestir"], ["telefono", "Tecnologia"], ["tequila", "Bebida alcholica"], ["pajaro", "ave"], ["perfume", "lo usamos para oler rico"], ["jugo", "Bebida"], ["flores", "En primavera hay muchas"], ["chamarra", "sirve para quitar el frio"], ["silla", "La ocupas para sentarte"], ["amor", "Sentimiento"]];

// Palabra a adivinar
var palabra = "";
var num;
var adivinar = [];
var esp = document.getElementById("palabra");
var conta = 6;
var buttons = document.getElementsByClassName('letra');
var btniniciar = document.getElementById("reset");

function generadorPalabras() {
    num = Math.floor(Math.random() * palabras.length);
    palabra = palabras[num][0].toUpperCase();
    console.log(palabra);
}

function rayitas(length) {
    adivinar = Array(length).fill("__");
    esp.innerHTML = adivinar.join("  ");
}

function generarAbecedario(a, z) {
    var abecedarioDiv = document.getElementById("abecedario");
    abecedarioDiv.innerHTML = ""; // Asegurarse de que el contenedor esté
    var i = a.charCodeAt(0), j = z.charCodeAt(0);
    var letra = "";
    for (; i <= j; i++) {
        letra = String.fromCharCode(i).toUpperCase();
        abecedarioDiv.innerHTML += "<button value='" + letra + "' onclick='intento(\"" + letra + "\")' class='letra' id='" + letra + "'>" + letra + "</button>";
    }
    // Añadir la letra Ñ
    abecedarioDiv.innerHTML += "<button value='Ñ' onclick='intento(\"Ñ\")' class='letra' id='Ñ'>Ñ</button>";
}

function intento(letra) {
    const boton = document.getElementById(letra);
    boton.disabled = true; // Deshabilitar el botón
    if (palabra.includes(letra)) {
        for (var i = 0; i < palabra.length; i++) {
            if (palabra[i] === letra) adivinar[i] = letra;
        }
        esp.innerHTML = adivinar.join("  ");
        document.getElementById("acierto").innerHTML = "¡Bien hecho!";
        document.getElementById("acierto").classList.add("acierto", "verde");
    } else {
        conta--;
        document.getElementById("intentos").innerHTML = conta;
        document.getElementById("acierto").innerHTML = "Fallaste, ¡intenta otra vez!";
        document.getElementById("acierto").classList.add("acierto", "rojo");
        document.getElementById("img" + (7 - conta)).style.display = "block";
        if (conta < 6) {
            document.getElementById("img" + (6 - conta)).style.display = "none";
        }
    }
    compruebaFinal();
    setTimeout(function () {
        document.getElementById("acierto").className = "";
    }, 800);
}


function pista() {
    const pistaElemento = document.getElementById("ayu_pista");
    pistaElemento.innerHTML = palabras[num][1]; // Asignar la pista
    pistaElemento.style.opacity = "1"; // Asegurar visibilidad (por si usas animaciones)
    pistaElemento.style.transform = "translateY(0)"; // Resetear transformaciones (si las usas)
    pistaElemento.style.display = "block"; // Asegurarse de que se muestre
}

function compruebaFinal() {
    if (adivinar.indexOf("__") === -1) {
        document.getElementById("final").innerHTML = "Lo lograste, Felicidades!!!";
        document.getElementById("final").classList.add("zoom-in");
        document.getElementById("palabra").classList.add("encuadre");
        for (var i = 0; i < buttons.length; i++) {
            buttons[i].disabled = true;
        }
        document.getElementById("reset").innerHTML = "Empezar";
        btniniciar.onclick = function () { location.reload(); };
    } else if (conta === 0) {
        document.getElementById("final").innerHTML = "Lo siento, perdiste. La palabra era " + palabra;
        document.getElementById("final").classList.add("zoom-in");
        for (var i = 0; i < buttons.length; i++) {
            buttons[i].disabled = true;
        }
        document.getElementById("reset").innerHTML = "Empezar";
        btniniciar.onclick = function () { location.reload(); };
    }
}

function iniciar() {
    generadorPalabras();
    rayitas(palabra.length);
    generarAbecedario("a", "z");
    conta = 6;
    document.getElementById("intentos").innerHTML = conta;

    // Ocultar imágenes y pista
    for (var i = 1; i <= 7; i++) {
        document.getElementById("img" + i).style.display = "none";
    }
    document.getElementById("img1").style.display = "block";
    const pistaElemento = document.getElementById("ayu_pista");
    pistaElemento.style.display = "none";
    pistaElemento.style.opacity = "0";
    pistaElemento.style.transform = "translateY(-10px)";
}

window.onload = iniciar();

```

Por ultimo se hizo uso de estilos.css para poder darle mas color a la pagina, asi como cierto movimiento al titulo cuando el cursor esta sobre él, tambien se le dio color a los botones que tenemos y a el abecedario, entre otras funciones, dando como resultado:

![image](https://github.com/user-attachments/assets/7d133096-814d-4a10-a200-5495408a6a10)
