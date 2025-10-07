<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>El Ahorcado de la Familia</title>
    <!-- Carga de Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Fuente Inter y Estilos Base -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(180deg, #585353 0%, #4b4949 100%); /* Fondo con tonos cálidos/familiares */
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh; /* Usamos height en lugar de min-height para mejor control */
            overflow-y: auto; /* Permitir scroll en el body si es estrictamente necesario */
            padding: 0.5rem; /* Reducir padding exterior */
            color: #1f2937;
        }

        /* Estilo del contenedor principal (simula un pergamino) */
        .game-container {
            background-color: #fffbeb; /* Color de pergamino más claro */
            box-shadow: 0 15px 30px -5px rgba(0, 0, 0, 0.4);
            border: 8px solid #b45309; /* Borde de madera más cálido */
            /* SOLUCIÓN: Ajustar la altura máxima al 100% del viewport menos el padding del body */
            max-height: calc(100vh - 1rem); 
            overflow-y: auto; /* Permite scroll interno si el contenido es demasiado alto */
        }
        
        /* Contenedor del ahorcado (para el patíbulo) */
        #gallows-container {
            position: relative;
            width: 150px;
            height: 200px;
            margin: 0 auto 20px;
            /* Base del patíbulo */
            border-bottom: 5px solid #7c2d12; 
        }

        /* Partes del Patíbulo */
        .gallows-part {
            background-color: #7c2d12; /* Marrón oscuro para la madera */
            position: absolute;
            display: none; /* Oculto por defecto */
        }

        /* 1. Base vertical */
        .gallows-part:nth-child(1) { top: 0; left: 20px; width: 5px; height: 100%; }
        /* 2. Barra horizontal superior */
        .gallows-part:nth-child(2) { top: 0; left: 20px; width: 100px; height: 5px; }
        /* 3. Cuerda (pequeña vertical) */
        .gallows-part:nth-child(3) { top: 5px; left: 120px; width: 5px; height: 20px; } 

        /* Partes del Cuerpo */
        .hangman-part {
            position: absolute;
            background-color: #38bdf8; /* Color de túnica/figura - Azul claro */
            border: 2px solid #0ea5e9;
            border-radius: 50%; /* Usado para la cabeza */
            display: none;
        }

        /* 4. Cabeza */
        .hangman-part:nth-child(4) { top: 25px; left: 105px; width: 30px; height: 30px; }
        /* 5. Cuerpo */
        .hangman-part:nth-child(5) { top: 55px; left: 118px; width: 5px; height: 60px; border-radius: 0; }
        /* 6. Brazo izquierdo */
        .hangman-part:nth-child(6) { top: 60px; left: 100px; width: 25px; height: 5px; transform: rotate(45deg); border-radius: 0; }
        /* 7. Brazo derecho */
        .hangman-part:nth-child(7) { top: 60px; left: 116px; width: 25px; height: 5px; transform: rotate(-45deg); border-radius: 0; }
        /* 8. Pierna izquierda */
        .hangman-part:nth-child(8) { top: 110px; left: 100px; width: 25px; height: 5px; transform: rotate(-45deg); border-radius: 0; }
        /* 9. Pierna derecha */
        .hangman-part:nth-child(9) { top: 110px; left: 116px; width: 25px; height: 5px; transform: rotate(45deg); border-radius: 0; }
        
        .letter-button:disabled {
            cursor: not-allowed;
            opacity: 0.6;
            background-color: #e5e5e5 !important;
            color: #737373 !important;
        }

        /* Diseño de los guiones de la palabra */
        #wordDisplay span {
            font-size: 2.5rem;
            margin: 0 0.25rem;
            padding: 0.2rem 0.5rem;
            border-bottom: 3px solid #78350f; /* Color de la línea */
        }
        
        @media (max-width: 640px) {
            #wordDisplay span {
                font-size: 1.8rem;
                margin: 0 0.1rem;
                padding: 0.1rem 0.3rem;
            }
        }

    </style>
</head>
<body class="selection:bg-amber-200">

    <!-- Contenedor Principal del Juego -->
    <div class="game-container w-full max-w-xl p-4 sm:p-6 rounded-xl text-center">
        
        <!-- Encabezado y Título -->
        <h1 class="text-3xl font-black text-red-700 mb-2 drop-shadow-lg">
            EL AHORCADO: DESAFÍOS FAMILIARES
        </h1>
        <p class="text-gray-700 text-sm font-semibold mb-6">
            Adivina el error o problema que afecta la armonía familiar. ¡Tienes 9 intentos!
        </p>

        <!-- Área de Dibujo del Ahorcado (Patíbulo) -->
        <div id="gallows-container">
            <!-- 1. Base vertical -->
            <div class="gallows-part"></div> 
            <!-- 2. Barra horizontal superior -->
            <div class="gallows-part"></div> 
            <!-- 3. Cuerda -->
            <div class="gallows-part"></div> 
            
            <!-- 4. Cabeza -->
            <div class="hangman-part"></div>
            <!-- 5. Cuerpo -->
            <div class="hangman-part"></div>
            <!-- 6. Brazo izquierdo -->
            <div class="hangman-part"></div>
            <!-- 7. Brazo derecho -->
            <div class="hangman-part"></div>
            <!-- 8. Pierna izquierda -->
            <div class="hangman-part"></div>
            <!-- 9. Pierna derecha -->
            <div class="hangman-part"></div>
        </div>

        <!-- Mensaje de Estado -->
        <div id="message" class="text-xl font-bold h-8 mb-4">
            ¡Adivina una letra!
        </div>

        <!-- Área de la Palabra Oculta -->
        <div id="wordDisplay" class="font-mono tracking-widest uppercase mb-4 flex justify-center flex-wrap">
            <!-- Los guiones se generan aquí -->
        </div>

        <!-- Contador de Intentos -->
        <div class="text-lg font-bold text-gray-700 mb-4">
            Intentos fallidos restantes: <span id="attemptsLeft" class="text-red-600">9</span>
        </div>

        <!-- Teclado de Letras -->
        <div id="keyboard" class="grid grid-cols-7 gap-1">
            <!-- Botones generados por JavaScript -->
        </div>

        <!-- Botón de Reinicio -->
        <button 
            id="resetButton"
            class="mt-4 bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition duration-300 transform hover:scale-105"
            onclick="initGame()"
        >
            Nueva Palabra
        </button>

        <!-- Área para mostrar la palabra correcta al perder -->
        <p id="correctWord" class="mt-4 text-xl font-bold text-green-700 hidden">
            La palabra era: <span class="uppercase"></span>
        </p>

    </div>

    <script>
        // --- Diccionario de Problemas Familiares (en mayúsculas y sin acentos/ñ) ---
        const BIBLE_WORDS = [
            "IRRESPETO", "PEREZA", "INCOMUNICACION", "EGOISMO", "IRA", 
            "ORGULLO", "CHISME", "DISCUSION", "SOLEDAD", "MENTIRA", 
            "RENCOR", "DISCORDIA", "AGRESION", "ADICCION", "ENVIDIA",
            "CRITICA", "SILENCIO", "DESCONFIANZA", "INFIDELIDAD", "APATIA"
        ];
        
        const MAX_ATTEMPTS = 9; // Máximo de partes a dibujar: 3 patíbulo + 6 cuerpo

        // --- Variables de Estado del Juego ---
        let currentWord = "";
        let guessedLetters = [];
        let wrongGuesses = 0;
        let wordState = []; // Array para mostrar el estado actual de la palabra (guiones o letras)
        let gameActive = false;

        // Referencias del DOM
        const wordDisplay = document.getElementById('wordDisplay');
        const attemptsLeft = document.getElementById('attemptsLeft');
        const keyboard = document.getElementById('keyboard');
        const message = document.getElementById('message');
        const hangmanParts = document.querySelectorAll('.gallows-part, .hangman-part');
        const correctWordDisplay = document.getElementById('correctWord');


        // --- Lógica del Juego ---
        
        // 1. Inicializa el juego
        function initGame() {
            // Resetear estado
            currentWord = BIBLE_WORDS[Math.floor(Math.random() * BIBLE_WORDS.length)];
            guessedLetters = [];
            wrongGuesses = 0;
            wordState = Array(currentWord.length).fill('_');
            gameActive = true;
            message.textContent = "¡Adivina una letra!";
            correctWordDisplay.classList.add('hidden');

            updateDisplay();
            createKeyboard();
            drawHangman();
        }

        // 2. Actualiza la visualización de la palabra y los intentos
        function updateDisplay() {
            // Mostrar los guiones y las letras adivinadas
            wordDisplay.innerHTML = wordState.map(letter => 
                `<span>${letter === '_' ? '_' : letter}</span>`
            ).join('');

            attemptsLeft.textContent = MAX_ATTEMPTS - wrongGuesses;
        }

        // 3. Crea el teclado de botones de A-Z
        function createKeyboard() {
            keyboard.innerHTML = '';
            for (let i = 0; i < 26; i++) {
                const letter = String.fromCharCode(65 + i);
                const button = document.createElement('button');
                
                button.textContent = letter;
                // Usamos colores de peligro/alerta para la temática de problemas
                button.classList.add(
                    'letter-button',
                    'bg-red-400', 'hover:bg-red-500', 'text-white', 
                    'font-bold', 'py-2', 'rounded-lg', 'shadow-md', 
                    'transition', 'duration-150', 'text-xl'
                );
                button.onclick = () => handleGuess(letter, button);
                
                keyboard.appendChild(button);
            }
        }

        // 4. Dibuja el patíbulo y la figura del ahorcado
        function drawHangman() {
            // Ocultar todas las partes
            hangmanParts.forEach(part => part.style.display = 'none');
            
            // Mostrar las partes según los intentos fallidos
            for (let i = 0; i < wrongGuesses; i++) {
                if (hangmanParts[i]) {
                    hangmanParts[i].style.display = 'block';
                }
            }
        }

        // 5. Maneja la adivinanza de una letra
        function handleGuess(letter, button) {
            if (!gameActive || guessedLetters.includes(letter)) {
                return;
            }

            guessedLetters.push(letter);
            button.disabled = true;

            const isCorrect = currentWord.includes(letter);

            if (isCorrect) {
                // Letra Correcta (Usamos color de solución/armonía)
                button.classList.remove('bg-red-400', 'hover:bg-red-500');
                button.classList.add('bg-green-500', 'hover:bg-green-500');
                message.textContent = "¡Correcto! Sigue así.";
                
                // Revelar la letra en la palabra
                for (let i = 0; i < currentWord.length; i++) {
                    if (currentWord[i] === letter) {
                        wordState[i] = letter;
                    }
                }
                
                // Chequear victoria
                if (!wordState.includes('_')) {
                    endGame(true);
                }
            } else {
                // Letra Incorrecta (Usamos color de error/problema)
                button.classList.remove('bg-red-400', 'hover:bg-red-500');
                button.classList.add('bg-gray-500', 'hover:bg-gray-500');
                wrongGuesses++;
                message.textContent = "¡Incorrecto! Prueba otra letra.";

                // Chequear derrota
                if (wrongGuesses >= MAX_ATTEMPTS) {
                    endGame(false);
                }
            }

            updateDisplay();
            drawHangman();
        }

        // 6. Termina el juego (victoria o derrota)
        function endGame(win) {
            gameActive = false;
            
            // Deshabilitar todos los botones
            document.querySelectorAll('.letter-button').forEach(btn => btn.disabled = true);

            if (win) {
                message.textContent = "🎉 ¡Felicidades! ¡Has adivinado el problema! 🎉";
                message.classList.remove('text-red-600');
                message.classList.add('text-green-600');
            } else {
                message.textContent = "😔 ¡Fallaste! ¡El problema se concretó! 😔";
                message.classList.remove('text-green-600');
                message.classList.add('text-red-600');
                
                // Mostrar la palabra correcta
                correctWordDisplay.querySelector('span').textContent = currentWord;
                correctWordDisplay.classList.remove('hidden');
            }
        }


        // Iniciar el juego al cargar la ventana
        window.onload = initGame;
    </script>
     <footer> jhon arias </footer>
</body>
</html>
