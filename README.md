# STAR-WARS-

                                                              HTML

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>

    <title>Star Wars</title>
</head>

<body
    class="min-h-screen text-yellow-300 bg-cover bg-center bg-fixed"
   style="background-image: url('https://www.shutterstock.com/image-illustration/abstract-glowing-light-rays-background-600nw-2357414009.jpg');"
>
    <div class="container mx-auto p-6">

        <div class="rounded-3xl p-6 backdrop-blur-sm">

            <h1
                class="text-7xl md:text-8xl font-extrabold text-center tracking-wider mb-6"
                style="
                    color: #f7d354;
                    text-shadow:
                    3px 3px 0 #c08f14,
                    6px 6px 10px rgba(19, 110, 213, 0.8);
                "
            >
                STAR WARS
            </h1>

            <p class="text-center text-sm text-slate-300 mb-8 tracking-wide">
                PERSONAJES DE LA GALAXIA
            </p>

            <div id="character-list" class="grid grid-cols-1 md:grid-cols-4 gap-6">
            </div>

            <div class="flex justify-center gap-4 mt-8">

                <button
                    id="prev-btn"
                    class="bg-yellow-400 px-5 py-2 rounded-xl shadow font-bold text-black cursor-pointer hover:bg-yellow-500"
                >
                    Anterior
                </button>

                <button
                    id="next-btn"
                    class="bg-yellow-400 px-5 py-2 rounded-xl shadow font-bold text-black cursor-pointer hover:bg-yellow-500"
                >
                    Siguiente
                </button>

            </div>

            <!-- COMPARACIÓN -->
            <div id="comparison" class="mt-10">
            </div>

        </div>

    </div>

    <script src="index.js"></script>

</body>
</html>

                                                              JAVASCRIPT


const characterListDiv = document.getElementById("character-list");
const prevBtn = document.getElementById("prev-btn");
const nextBtn = document.getElementById("next-btn");
const comparisonDiv = document.getElementById("comparison");

let characters = [];
let selectedCharacters = [];

let currentPage = 1;
const charactersPerPage = 8;

// Cargar personajes
async function cargarCharacters() {
    try {
        const response = await fetch(
            "https://akabab.github.io/starwars-api/api/all.json"
        );

        characters = await response.json();

        mostrarCharacters();

    } catch (error) {
        console.error("Error al cargar personajes:", error);
    }
}

// Mostrar personajes
function mostrarCharacters() {

    characterListDiv.innerHTML = "";

    const start = (currentPage - 1) * charactersPerPage;
    const end = start + charactersPerPage;

    const charactersToShow = characters.slice(start, end);

    charactersToShow.forEach((character) => {

        const card = document.createElement("div");

        card.className =
            "bg-white rounded-2xl shadow-md p-4 hover:scale-105 hover:shadow-xl transition duration-300";

        card.innerHTML = `
            <img
                src="${character.image}"
                alt="${character.name}"
                class="w-full h-64 object-cover rounded-lg"
                onerror="this.src='https://placehold.co/300x400?text=Sin+Imagen'"
            >

            <h2 class="mt-3 text-lg font-bold text-center text-black">
                ${character.name}
            </h2>

            <p class="text-center text-sm text-gray-600">
                Altura: ${character.height ?? "N/A"} cm
            </p>

            <p class="text-center text-sm text-gray-600">
                Peso: ${character.mass ?? "N/A"} kg
            </p>

            <button
                onclick="seleccionarPersonaje(${character.id})"
                class="mt-4 w-full bg-yellow-400 text-black py-2 rounded-lg font-bold hover:bg-yellow-500"
            >
                Seleccionar
            </button>
        `;

        characterListDiv.appendChild(card);
    });

    prevBtn.disabled = currentPage === 1;
    nextBtn.disabled = end >= characters.length;
}

// Seleccionar personaje
window.seleccionarPersonaje = function(id) {

    const personaje = characters.find(
        character => character.id === id
    );

    if (!personaje) return;

    if (selectedCharacters.some(
        character => character.id === id
    )) {
        alert("Este personaje ya fue seleccionado");
        return;
    }

    if (selectedCharacters.length >= 2) {
        alert("Solo puedes seleccionar 2 personajes");
        return;
    }

    selectedCharacters.push(personaje);

    if (selectedCharacters.length === 2) {
        mostrarComparacion();
    }
};

// Mostrar comparación
function mostrarComparacion() {
    console.log(selectedCharacters[0]);
    console.log(selectedCharacters[1]);

    const p1 = selectedCharacters[0];
    const p2 = selectedCharacters[1];

    comparisonDiv.innerHTML = `
    <div class="bg-black border border-yellow-400 rounded-2xl shadow-md p-6 mt-8">

        <h2 class="text-3xl font-bold text-center text-yellow-400 mb-6">
            COMPARACIÓN DE PERSONAJES
        </h2>

        <table class="w-full text-center border border-yellow-400 text-white">

            <thead>
                <tr class="bg-yellow-400 text-black">
                    <th class="p-3">Dato</th>
                    <th class="p-3">${p1.name}</th>
                    <th class="p-3">${p2.name}</th>
                </tr>
            </thead>

            <tbody>

                <tr class="border-b border-gray-700">
                    <td class="p-3">Altura</td>
                    <td>${p1.height ?? "N/A"} cm</td>
                    <td>${p2.height ?? "N/A"} cm</td>
                </tr>

                <tr class="border-b border-gray-700">
                    <td class="p-3">Peso</td>
                    <td>${p1.mass ?? "N/A"} kg</td>
                    <td>${p2.mass ?? "N/A"} kg</td>
                </tr>

                <tr class="border-b border-gray-700">
                    <td class="p-3">Género</td>
                    <td>${p1.gender ?? "N/A"}</td>
                    <td>${p2.gender ?? "N/A"}</td>
                </tr>
                
           <tr class="border-b border-gray-700">
    <td class="p-3">Color de cabello</td>
    <td>${p1.hairColor ?? "N/A"}</td>
    <td>${p2.hairColor ?? "N/A"}</td>
</tr>

<tr class="border-b border-gray-700">
    <td class="p-3">Año de nacimiento</td>
    <td>${p1.born ?? "N/A"}</td>
    <td>${p2.born ?? "N/A"}</td>
</tr>

            </tbody>

        </table>

        <div class="text-center mt-6">
            <button
                onclick="reiniciarComparacion()"
                class="bg-red-500 text-white px-5 py-2 rounded-lg hover:bg-red-600"
            >
                Nueva Comparación
            </button>
        </div>

    </div>
    `;
}

// Reiniciar comparación
window.reiniciarComparacion = function() {
    selectedCharacters = [];
    comparisonDiv.innerHTML = "";
};

// Botón anterior
prevBtn.addEventListener("click", () => {
    if (currentPage > 1) {
        currentPage--;
        mostrarCharacters();
    }
});

// Botón siguiente
nextBtn.addEventListener("click", () => {
    const totalPages = Math.ceil(
        characters.length / charactersPerPage
    );

    if (currentPage < totalPages) {
        currentPage++;
        mostrarCharacters();
    }
});

// Iniciar aplicación
cargarCharacters();
