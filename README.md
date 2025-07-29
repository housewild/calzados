<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calzado Exclusivo - Estilo y Comodidad en Ecuador</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Inter font -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- Tone.js for sound effects -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.min.js"></script>

    <!-- Firebase SDKs -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Your web app's Firebase configuration (provided by the user)
        const firebaseConfig = {
            apiKey: "AIzaSyDAVIBECvAvSM9XnX3FEj3PE1iGIOXF-WQ",
            authDomain: "housewild-def17.firebaseapp.com",
            projectId: "housewild-def17",
            storageBucket: "housewild-def17.firebasestorage.app",
            messagingSenderId: "871819895049",
            appId: "1:871819895049:web:63964b85b0e75bd214f3d8",
            measurementId: "G-0QFR4STXMS"
        };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);

        // Global variables for Canvas environment (important for Firestore paths)
        // Check if __app_id is defined in the Canvas environment, otherwise use a default
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';

        // Sign in anonymously to allow writing to Firestore (as per security rules)
        async function authenticateAndInitializeFirestore() {
            try {
                // Use __initial_auth_token if available (Canvas environment)
                if (typeof __initial_auth_token !== 'undefined') {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
                console.log("Firebase authenticated successfully.");
            } catch (error) {
                console.error("Error during Firebase authentication:", error);
            }
        }

        // Call the authentication function when the script loads
        authenticateAndInitializeFirestore();

        // Make db, auth, and appId globally accessible for the main script
        window.db = db;
        window.auth = auth;
        window.appId = appId;
        window.addDoc = addDoc;
        window.collection = collection;
    </script>

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8f8f8;
        }
        .whatsapp-button {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: #25D366;
            color: white;
            border-radius: 50%;
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            z-index: 1000;
            transition: transform 0.3s ease, box-shadow 0.3s ease; /* Added box-shadow transition */
            animation: pulse-whatsapp 2s infinite; /* Initial pulse animation */
        }
        .whatsapp-button:hover {
            transform: scale(1.1) translateY(-5px); /* Slightly larger and moves up */
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3); /* More prominent shadow on hover */
            animation: none; /* Stop pulse on hover */
        }

        @keyframes pulse-whatsapp {
            0% {
                transform: scale(1);
                box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            }
            50% {
                transform: scale(1.05);
                box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
            }
            100% {
                transform: scale(1);
                box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            }
        }

        .sale-popup {
            position: fixed;
            bottom: 20px;
            left: 20px;
            background-color: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 15px 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
            z-index: 999;
            display: none; /* Hidden by default */
            animation: fadeInOut 8s infinite;
        }

        @keyframes fadeInOut {
            0% { opacity: 0; transform: translateX(-100%); }
            10% { opacity: 1; transform: translateX(0); }
            90% { opacity: 1; transform: translateX(0); }
            100% { opacity: 0; transform: translateX(-100%); }
        }

        /* Custom scrollbar for product list */
        .scrollable-products::-webkit-scrollbar {
            width: 8px;
        }

        .scrollable-products::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }

        .scrollable-products::-webkit-scrollbar-thumb {
            background: #888;
            border-radius: 10px;
        }

        .scrollable-products::-webkit-scrollbar-thumb:hover {
            background: #555;
        }

        /* Modal specific styles */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1001;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }
        .modal-overlay.show {
            opacity: 1;
            visibility: visible;
        }
        .modal-content {
            background-color: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
            max-width: 500px;
            width: 90%;
            transform: translateY(-20px);
            transition: transform 0.3s ease;
            position: relative;
        }
        .modal-overlay.show .modal-content {
            transform: translateY(0);
        }
        .modal-close-button {
            position: absolute;
            top: 15px;
            right: 15px;
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: #4a5568;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">

    <!-- WhatsApp Button -->
    <a href="https://wa.me/593985932083?text=Hola!%20Estoy%20interesado/a%20en%20su%20calzado%20y%20me%20gustaría%20más%20información." target="_blank" class="whatsapp-button" id="whatsappButton">
        <i class="fab fa-whatsapp"></i>
    </a>

    <!-- Sale Notification Pop-up -->
    <div id="salePopup" class="sale-popup">
        <p><i class="fas fa-shopping-cart text-yellow-400 mr-2"></i><span id="popupText"></span></p>
    </div>

    <!-- Header / Hero Section -->
    <header class="relative bg-gradient-to-r from-blue-600 to-purple-600 text-white py-20 px-4 overflow-hidden rounded-b-xl shadow-lg">
        <div class="container mx-auto flex flex-col md:flex-row items-center justify-between relative z-10">
            <div class="md:w-1/2 text-center md:text-left mb-8 md:mb-0">
                <h1 class="text-5xl md:text-6xl font-extrabold leading-tight mb-4 drop-shadow-lg">
                    Calzado Exclusivo <br> <span class="text-yellow-300">Estilo y Comodidad</span>
                </h1>
                <p class="text-xl md:text-2xl mb-8 opacity-90">
                    Descubre la colección premium que tus pies merecen. Envíos rápidos a todo Ecuador.
                </p>
                <a href="#productos" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-3 px-8 rounded-full text-lg shadow-lg transform transition duration-300 hover:scale-105">
                    ¡Explorar Colección Ahora!
                </a>
            </div>
            <div class="md:w-1/2 flex justify-center md:justify-end">
                <!-- Updated hero image -->
                <img src="https://i.ibb.co/fY64WcqG/Full-Size-Render.jpg" alt="Calzado de estilo moderno" class="w-full max-w-md rounded-xl shadow-2xl transform rotate-3 hover:rotate-0 transition-transform duration-500">
            </div>
        </div>
        <!-- Background elements for visual appeal -->
        <!-- Updated background image -->
        <div class="absolute top-0 left-0 w-full h-full bg-cover bg-center opacity-20" style="background-image: url('https://i.ibb.co/HpCj3Rs6/IMG-2694.jpg');"></div>
    </header>

    <!-- Urgency Section (Countdown Timer) -->
    <section class="bg-red-600 text-white py-8 px-4 text-center shadow-inner rounded-xl mx-auto max-w-6xl -mt-10 relative z-20">
        <div class="container mx-auto">
            <h2 class="text-3xl font-bold mb-4 animate-pulse">
                🔥 ¡OFERTA POR TIEMPO LIMITADO! 🔥
            </h2>
            <p class="text-xl mb-4">
                Obtén un <span class="text-yellow-300 font-extrabold text-2xl">20% de descuento</span> en toda nuestra colección.
            </p>
            <div id="countdown" class="flex justify-center items-center space-x-4 text-4xl font-extrabold bg-red-700 p-4 rounded-lg shadow-md">
                <div><span id="days">00</span><br><span class="text-sm">Días</span></div>
                <div>:</div>
                <div><span id="hours">00</span><br><span class="text-sm">Horas</span></div>
                <div>:</div>
                <div><span id="minutes">00</span><br><span class="text-sm">Minutos</span></div>
                <div>:</div>
                <div><span id="seconds">00</span><br><span class="text-sm">Segundos</span></div>
            </div>
            <p class="text-lg mt-4">¡No dejes pasar esta oportunidad!</p>
        </div>
    </section>

    <!-- Benefits Section -->
    <section class="py-16 px-4 bg-white">
        <div class="container mx-auto text-center">
            <h2 class="text-4xl font-bold mb-12 text-gray-900">¿Por qué elegir nuestro calzado?</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
                <div class="bg-blue-50 p-6 rounded-xl shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <i class="fas fa-shoe-prints text-blue-600 text-5xl mb-4"></i>
                    <h3 class="text-xl font-semibold mb-2">Comodidad Superior</h3>
                    <p class="text-gray-700">Diseñados para el confort de tus pies durante todo el día, sin sacrificar el estilo.</p>
                </div>
                <div class="bg-purple-50 p-6 rounded-xl shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <i class="fas fa-gem text-purple-600 text-5xl mb-4"></i>
                    <h3 class="text-xl font-semibold mb-2">Estilo Vanguardista</h3>
                    <p class="text-gray-700">Las últimas tendencias en moda para que siempre luzcas a la vanguardia.</p>
                </div>
                <div class="bg-green-50 p-6 rounded-xl shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <i class="fas fa-award text-green-600 text-5xl mb-4"></i>
                    <h3 class="text-xl font-semibold mb-2">Calidad Garantizada</h3>
                    <p class="text-gray-700">Fabricados con materiales duraderos que aseguran una larga vida útil a tu calzado.</p>
                </div>
                <div class="bg-yellow-50 p-6 rounded-xl shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <i class="fas fa-truck-fast text-yellow-600 text-5xl mb-4"></i>
                    <h3 class="text-xl font-semibold mb-2">Envíos Rápidos a Ecuador</h3>
                    <p class="text-gray-700">Recibe tus zapatos directamente en la puerta de tu casa, en cualquier ciudad de Ecuador.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Product Video Section -->
    <section class="py-16 px-4 bg-gray-200">
        <div class="container mx-auto text-center">
            <h2 class="text-4xl font-bold mb-12 text-gray-900">Mira nuestros zapatos en acción</h2>
            <div class="relative w-full max-w-3xl mx-auto rounded-xl shadow-2xl overflow-hidden">
                <!-- Replace with your actual video embed code (e.g., YouTube, Vimeo) or a local video -->
                <!-- For a local video, use <video controls src="path/to/your/video.mp4" class="w-full h-auto"></video> -->
                <iframe class="w-full aspect-video" src="https://www.youtube.com/embed/dQw4w9WgXcQ?controls=0" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                <!-- Example of a placeholder image for the video if no video is available -->
                <!-- <img src="https://placehold.co/800x450/333333/FFFFFF?text=Video+de+Producto" alt="Video de presentación de calzado" class="w-full h-auto object-cover"> -->
            </div>
            <p class="text-lg text-gray-700 mt-8">Descubre la calidad y el diseño en cada detalle.</p>
        </div>
    </section>

    <!-- Size Guide Section -->
    <section class="py-16 px-4 bg-white">
        <div class="container mx-auto text-center">
            <h2 class="text-4xl font-bold mb-12 text-gray-900">Encuentra tu talla perfecta</h2>
            <div class="max-w-4xl mx-auto bg-gray-50 p-8 rounded-xl shadow-lg border border-gray-200">
                <p class="text-lg text-gray-700 mb-6">Para asegurar el ajuste ideal, sigue nuestra guía de tallas. Mide tu pie y compara con nuestra tabla.</p>
                <div class="overflow-x-auto mb-6">
                    <table class="min-w-full bg-white border border-gray-300 rounded-lg">
                        <thead>
                            <tr class="bg-blue-600 text-white">
                                <th class="py-3 px-4 border-b border-gray-300 rounded-tl-lg">Talla EUR</th>
                                <th class="py-3 px-4 border-b border-gray-300">Talla US (Hombre)</th>
                                <th class="py-3 px-4 border-b border-gray-300">Talla US (Mujer)</th>
                                <th class="py-3 px-4 border-b border-gray-300 rounded-tr-lg">Largo del Pie (cm)</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr class="hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">36</td>
                                <td class="py-3 px-4 border-b border-gray-200">4</td>
                                <td class="py-3 px-4 border-b border-gray-200">5</td>
                                <td class="py-3 px-4 border-b border-gray-200">22.5</td>
                            </tr>
                            <tr class="bg-gray-50 hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">37</td>
                                <td class="py-3 px-4 border-b border-gray-200">5</td>
                                <td class="py-3 px-4 border-b border-gray-200">6</td>
                                <td class="py-3 px-4 border-b border-gray-200">23.5</td>
                            </tr>
                            <tr class="hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">38</td>
                                <td class="py-3 px-4 border-b border-gray-200">6</td>
                                <td class="py-3 px-4 border-b border-gray-200">7</td>
                                <td class="py-3 px-4 border-b border-gray-200">24.5</td>
                            </tr>
                            <tr class="bg-gray-50 hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">39</td>
                                <td class="py-3 px-4 border-b border-gray-200">7</td>
                                <td class="py-3 px-4 border-b border-gray-200">8</td>
                                <td class="py-3 px-4 border-b border-gray-200">25.5</td>
                            </tr>
                            <tr class="hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">40</td>
                                <td class="py-3 px-4 border-b border-gray-200">7.5</td>
                                <td class="py-3 px-4 border-b border-gray-200">9</td>
                                <td class="py-3 px-4 border-b border-gray-200">26.0</td>
                            </tr>
                            <tr class="bg-gray-50 hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">41</td>
                                <td class="py-3 px-4 border-b border-gray-200">8</td>
                                <td class="py-3 px-4 border-b border-gray-200">10</td>
                                <td class="py-3 px-4 border-b border-gray-200">26.5</td>
                            </tr>
                            <tr class="hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">42</td>
                                <td class="py-3 px-4 border-b border-gray-200">9</td>
                                <td class="py-3 px-4 border-b border-gray-200">11</td>
                                <td class="py-3 px-4 border-b border-gray-200">27.5</td>
                            </tr>
                            <tr class="bg-gray-50 hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">43</td>
                                <td class="py-3 px-4 border-b border-gray-200">10</td>
                                <td class="py-3 px-4 border-b border-gray-200">12</td>
                                <td class="py-3 px-4 border-b border-gray-200">28.5</td>
                            </tr>
                            <tr class="hover:bg-gray-100">
                                <td class="py-3 px-4 border-b border-gray-200">44</td>
                                <td class="py-3 px-4 border-b border-gray-200">11</td>
                                <td class="py-3 px-4 border-b border-gray-200">13</td>
                                <td class="py-3 px-4 border-b border-gray-200">29.5</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <p class="text-sm text-gray-600">¿Dudas con tu talla? Contáctanos por WhatsApp para asesoría personalizada.</p>
            </div>
        </div>
    </section>

    <!-- Featured Products Section -->
    <section id="productos" class="py-16 px-4 bg-gray-100">
        <div class="container mx-auto text-center">
            <h2 class="text-4xl font-bold mb-12 text-gray-900">Nuestra Colección Destacada</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 scrollable-products max-h-[600px] overflow-y-auto p-4 rounded-xl bg-white shadow-lg">
                <!-- Product Card 1 -->
                <div class="bg-white rounded-xl shadow-lg overflow-hidden transform transition duration-300 hover:scale-105 hover:shadow-2xl">
                    <!-- Replace this placeholder with your actual product image -->
                    <img src="https://placehold.co/400x300/FF5733/FFFFFF?text=Zapatilla+Urbana" alt="Zapatilla Urbana ConfortPlus" class="w-full h-64 object-cover">
                    <div class="p-6">
                        <h3 class="text-2xl font-semibold mb-2 text-gray-900">Zapatilla Urbana "ConfortPlus"</h3>
                        <p class="text-gray-600 mb-4">Ideal para el día a día, combina estilo y máxima comodidad.</p>
                        <p class="text-3xl font-bold text-blue-600 mb-4">$49.99</p>
                        <button class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-full w-full transition duration-300 transform hover:scale-105 order-button" data-product-name="Zapatilla Urbana 'ConfortPlus'">
                            Comprar Ahora
                        </button>
                    </div>
                </div>
                <!-- Product Card 2 -->
                <div class="bg-white rounded-xl shadow-lg overflow-hidden transform transition duration-300 hover:scale-105 hover:shadow-2xl">
                    <!-- Replace this placeholder with your actual product image -->
                    <img src="https://placehold.co/400x300/33FF57/FFFFFF?text=Botin+Elegancia" alt="Botín Casual Elegancia" class="w-full h-64 object-cover">
                    <div class="p-6">
                        <h3 class="text-2xl font-semibold mb-2 text-gray-900">Botín Casual "Elegancia"</h3>
                        <p class="text-gray-600 mb-4">Perfecto para un look sofisticado y versátil.</p>
                        <p class="text-3xl font-bold text-blue-600 mb-4">$79.99</p>
                        <button class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-full w-full transition duration-300 transform hover:scale-105 order-button" data-product-name="Botín Casual 'Elegancia'">
                            Comprar Ahora
                        </button>
                    </div>
                </div>
                <!-- Product Card 3 -->
                <div class="bg-white rounded-xl shadow-lg overflow-hidden transform transition duration-300 hover:scale-105 hover:shadow-2xl">
                    <!-- Replace this placeholder with your actual product image -->
                    <img src="https://placehold.co/400x300/3357FF/FFFFFF?text=Sandalia+Verano" alt="Sandalia Verano Frescura" class="w-full h-64 object-cover">
                    <div class="p-6">
                        <h3 class="text-2xl font-semibold mb-2 text-gray-900">Sandalia "Verano Frescura"</h3>
                        <p class="text-gray-600 mb-4">Mantente fresco y cómodo en los días más cálidos.</p>
                        <p class="text-3xl font-bold text-blue-600 mb-4">$34.99</p>
                        <button class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-full w-full transition duration-300 transform hover:scale-105 order-button" data-product-name="Sandalia 'Verano Frescura'">
                            Comprar Ahora
                        </button>
                    </div>
                </div>
                <!-- Product Card 4 -->
                <div class="bg-white rounded-xl shadow-lg overflow-hidden transform transition duration-300 hover:scale-105 hover:shadow-2xl">
                    <!-- Replace this placeholder with your actual product image -->
                    <img src="https://placehold.co/400x300/FF33A1/FFFFFF?text=Zapatilla+Deportiva" alt="Zapatilla Deportiva Agilidad" class="w-full h-64 object-cover">
                    <div class="p-6">
                        <h3 class="text-2xl font-semibold mb-2 text-gray-900">Zapatilla Deportiva "Agilidad"</h3>
                        <p class="text-gray-600 mb-4">Rendimiento y estilo para tus entrenamientos.</p>
                        <p class="text-3xl font-bold text-blue-600 mb-4">$65.00</p>
                        <button class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-full w-full transition duration-300 transform hover:scale-105 order-button" data-product-name="Zapatilla Deportiva 'Agilidad'">
                            Comprar Ahora
                        </button>
                    </div>
                </div>
                <!-- Product Card 5 -->
                <div class="bg-white rounded-xl shadow-lg overflow-hidden transform transition duration-300 hover:scale-105 hover:shadow-2xl">
                    <!-- Replace this placeholder with your actual product image -->
                    <img src="https://placehold.co/400x300/33A1FF/FFFFFF?text=Mocasines+Clasicos" alt="Mocasines Clásicos" class="w-full h-64 object-cover">
                    <div class="p-6">
                        <h3 class="text-2xl font-semibold mb-2 text-gray-900">Mocasines "Clásicos"</h3>
                        <p class="text-gray-600 mb-4">La elegancia atemporal para cualquier ocasión formal.</p>
                        <p class="text-3xl font-bold text-blue-600 mb-4">$89.99</p>
                        <button class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-full w-full transition duration-300 transform hover:scale-105 order-button" data-product-name="Mocasines 'Clásicos'">
                            Comprar Ahora
                        </button>
                    </div>
                </div>
                <!-- Product Card 6 -->
                <div class="bg-white rounded-xl shadow-lg overflow-hidden transform transition duration-300 hover:scale-105 hover:shadow-2xl">
                    <!-- Replace this placeholder with your actual product image -->
                    <img src="https://placehold.co/400x300/A133FF/FFFFFF?text=Botas+Aventura" alt="Botas de Aventura" class="w-full h-64 object-cover">
                    <div class="p-6">
                        <h3 class="text-2xl font-semibold mb-2 text-gray-900">Botas "Aventura Extrema"</h3>
                        <p class="text-gray-600 mb-4">Resistencia y confort para tus exploraciones.</p>
                        <p class="text-3xl font-bold text-blue-600 mb-4">$110.00</p>
                        <button class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-full w-full transition duration-300 transform hover:scale-105 order-button" data-product-name="Botas 'Aventura Extrema'">
                            Comprar Ahora
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Instagram/UGC Gallery Section -->
    <section class="py-16 px-4 bg-gray-200">
        <div class="container mx-auto text-center">
            <h2 class="text-4xl font-bold mb-12 text-gray-900">#NuestrosClientesFelices</h2>
            <p class="text-lg text-gray-700 mb-8">Inspírate con nuestros clientes luciendo su calzado favorito. ¡Comparte tu estilo con nosotros!</p>
            <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
                <!-- Replace with actual Instagram/UGC images -->
                <div class="rounded-xl overflow-hidden shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <img src="https://placehold.co/300x300/FFD700/000000?text=Cliente+1" alt="Cliente usando calzado 1" class="w-full h-full object-cover">
                </div>
                <div class="rounded-xl overflow-hidden shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <img src="https://placehold.co/300x300/ADD8E6/000000?text=Cliente+2" alt="Cliente usando calzado 2" class="w-full h-full object-cover">
                </div>
                <div class="rounded-xl overflow-hidden shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <img src="https://placehold.co/300x300/90EE90/000000?text=Cliente+3" alt="Cliente usando calzado 3" class="w-full h-full object-cover">
                </div>
                <div class="rounded-xl overflow-hidden shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <img src="https://placehold.co/300x300/FFA07A/000000?text=Cliente+4" alt="Cliente usando calzado 4" class="w-full h-full object-cover">
                </div>
                <div class="rounded-xl overflow-hidden shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <img src="https://placehold.co/300x300/DDA0DD/000000?text=Cliente+5" alt="Cliente usando calzado 5" class="w-full h-full object-cover">
                </div>
                <div class="rounded-xl overflow-hidden shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <img src="https://placehold.co/300x300/87CEEB/000000?text=Cliente+6" alt="Cliente usando calzado 6" class="w-full h-full object-cover">
                </div>
                <div class="rounded-xl overflow-hidden shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <img src="https://placehold.co/300x300/FF6347/000000?text=Cliente+7" alt="Cliente usando calzado 7" class="w-full h-full object-cover">
                </div>
                <div class="rounded-xl overflow-hidden shadow-md transform transition duration-300 hover:scale-105 hover:shadow-xl">
                    <img src="https://placehold.co/300x300/7B68EE/000000?text=Cliente+8" alt="Cliente usando calzado 8" class="w-full h-full object-cover">
                </div>
            </div>
            <a href="#" class="inline-block mt-10 bg-pink-600 hover:bg-pink-700 text-white font-bold py-3 px-8 rounded-full text-lg shadow-lg transform transition duration-300 hover:scale-105">
                Síguenos en Instagram <i class="fab fa-instagram ml-2"></i>
            </a>
        </div>
    </section>

    <!-- Testimonials Section -->
    <section class="py-16 px-4 bg-white">
        <div class="container mx-auto text-center">
            <h2 class="text-4xl font-bold mb-12 text-gray-900">Lo que dicen nuestros clientes</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                <!-- Testimonial 1 -->
                <div class="bg-gray-50 p-6 rounded-xl shadow-md border border-gray-200">
                    <!-- Replace this placeholder with your actual customer image -->
                    <img src="https://placehold.co/80x80/FFD700/000000?text=JP" alt="Foto de Juan Pérez" class="w-20 h-20 rounded-full mx-auto mb-4 object-cover">
                    <p class="text-lg italic text-gray-700 mb-4">"¡Me encantaron mis nuevas zapatillas! La calidad es increíble y llegaron rapidísimo a Quito. ¡Definitivamente volveré a comprar!"</p>
                    <p class="font-semibold text-gray-900">- Juan P., Quito</p>
                </div>
                <!-- Testimonial 2 -->
                <div class="bg-gray-50 p-6 rounded-xl shadow-md border border-gray-200">
                    <!-- Replace this placeholder with your actual customer image -->
                    <img src="https://placehold.co/80x80/ADD8E6/000000?text=MR" alt="Foto de María Rodríguez" class="w-20 h-20 rounded-full mx-auto mb-4 object-cover">
                    <p class="text-lg italic text-gray-700 mb-4">"Los botines son hermosos y súper cómodos. El envío a Guayaquil fue muy eficiente. ¡Recomendados 100%!"</p>
                    <p class="font-semibold text-gray-900">- María R., Guayaquil</p>
                </div>
                <!-- Testimonial 3 -->
                <div class="bg-gray-50 p-6 rounded-xl shadow-md border border-gray-200">
                    <!-- Replace this placeholder with your actual customer image -->
                    <img src="https://placehold.co/80x80/90EE90/000000?text=LC" alt="Foto de Luis Castro" class="w-20 h-20 rounded-full mx-auto mb-4 object-cover">
                    <p class="text-lg italic text-gray-700 mb-4">"Excelente servicio al cliente y los zapatos son de primera. La mejor compra que he hecho en línea en Cuenca."</p>
                    <p class="font-semibold text-gray-900">- Luis C., Cuenca</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Trust and Guarantee Section -->
    <section class="py-16 px-4 bg-blue-700 text-white">
        <div class="container mx-auto text-center">
            <h2 class="text-4xl font-bold mb-12">Compra con Total Confianza</h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                <div class="p-6 rounded-xl bg-blue-800 shadow-md">
                    <i class="fas fa-undo-alt text-yellow-300 text-5xl mb-4"></i>
                    <h3 class="text-xl font-semibold mb-2">Garantía de Satisfacción</h3>
                    <p class="text-blue-100">Si no estás 100% satisfecho, te ofrecemos un cambio o devolución.</p>
                </div>
                <div class="p-6 rounded-xl bg-blue-800 shadow-md">
                    <i class="fas fa-lock text-yellow-300 text-5xl mb-4"></i>
                    <h3 class="text-xl font-semibold mb-2">Pagos Seguros</h3>
                    <p class="text-blue-100">Tus transacciones están protegidas con la última tecnología de encriptación.</p>
                    <div class="flex justify-center mt-4 space-x-4">
                        <!-- Replace these placeholders with actual payment logos -->
                        <img src="https://placehold.co/60x40/FFFFFF/000000?text=Visa" alt="Logo Visa" class="h-10">
                        <img src="https://placehold.co/60x40/FFFFFF/000000?text=MC" alt="Logo Mastercard" class="h-10">
                        <img src="https://placehold.co/60x40/FFFFFF/000000?text=PayP" alt="Logo PayPal" class="h-10">
                    </div>
                </div>
                <div class="p-6 rounded-xl bg-blue-800 shadow-md">
                    <i class="fas fa-box-open text-yellow-300 text-5xl mb-4"></i>
                    <h3 class="text-xl font-semibold mb-2">Envíos a Todo Ecuador</h3>
                    <p class="text-blue-100">Llevamos tus zapatos a cualquier rincón del país de forma rápida y segura.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Newsletter Subscription Section -->
    <section class="py-16 px-4 bg-purple-700 text-white text-center">
        <div class="container mx-auto max-w-2xl">
            <h2 class="text-4xl font-bold mb-6">¡No te pierdas nuestras ofertas!</h2>
            <p class="text-xl mb-8 opacity-90">Suscríbete a nuestro newsletter y sé el primero en enterarte de nuevos lanzamientos y descuentos exclusivos.</p>
            <form class="flex flex-col md:flex-row gap-4 justify-center">
                <input type="email" placeholder="Ingresa tu correo electrónico" class="p-3 rounded-full border-2 border-white bg-white bg-opacity-20 text-white placeholder-white focus:outline-none focus:ring-2 focus:ring-yellow-400 flex-grow md:max-w-md">
                <button type="submit" class="bg-yellow-400 hover:bg-yellow-500 text-purple-900 font-bold py-3 px-8 rounded-full shadow-lg transform transition duration-300 hover:scale-105">
                    Suscribirme
                </button>
            </form>
            <p class="text-sm mt-4 opacity-70">Prometemos no enviarte spam. Solo lo mejor de nuestro calzado.</p>
        </div>
    </section>

    <!-- FAQ Section -->
    <section class="py-16 px-4 bg-gray-100">
        <div class="container mx-auto">
            <h2 class="text-4xl font-bold text-center mb-12 text-gray-900">Preguntas Frecuentes</h2>
            <div class="max-w-3xl mx-auto space-y-4">
                <div class="bg-white p-6 rounded-xl shadow-md">
                    <h3 class="text-xl font-semibold mb-2 text-gray-900">¿Cuáles son los tiempos de envío?</h3>
                    <p class="text-gray-700">Los envíos dentro de Ecuador toman de 2 a 5 días hábiles, dependiendo de tu ubicación. Procesamos los pedidos en 24 horas.</p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-md">
                    <h3 class="text-xl font-semibold mb-2 text-gray-900">¿Ofrecen cambios de talla o devoluciones?</h3>
                    <p class="text-gray-700">Sí, ofrecemos cambios de talla y devoluciones dentro de los 15 días posteriores a la compra, siempre y cuando el calzado no haya sido usado y esté en su empaque original.</p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-md">
                    <h3 class="text-xl font-semibold mb-2 text-gray-900">¿Cómo puedo contactar a servicio al cliente?</h3>
                    <p class="text-gray-700">Puedes contactarnos a través del botón de WhatsApp flotante en la página, o enviando un correo a info@tumarca.com. Estamos disponibles de Lunes a Viernes, de 9 AM a 6 PM.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Final Call to Action -->
    <section class="bg-gradient-to-r from-purple-600 to-blue-600 text-white py-20 px-4 text-center rounded-t-xl shadow-lg">
        <div class="container mx-auto">
            <h2 class="text-4xl md:text-5xl font-bold mb-6 drop-shadow-lg">
                ¡No Esperes Más! Encuentra Tu Par Ideal
            </h2>
            <p class="text-xl md:text-2xl mb-10 opacity-90">
                El estilo y la comodidad que tus pies merecen están a solo un clic.
            </p>
            <a href="#productos" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-4 px-10 rounded-full text-xl shadow-lg transform transition duration-300 hover:scale-105">
                ¡Comprar Ahora!
            </a>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-900 text-gray-400 py-10 px-4">
        <div class="container mx-auto text-center md:flex md:justify-between md:items-center">
            <p class="mb-4 md:mb-0">&copy; 2025 Tu Marca de Calzado. Todos los derechos reservados.</p>
            <div class="flex justify-center space-x-6 mb-4 md:mb-0">
                <a href="#" class="hover:text-white transition-colors duration-300"><i class="fab fa-facebook-f text-xl"></i></a>
                <a href="#" class="hover:text-white transition-colors duration-300"><i class="fab fa-instagram text-xl"></i></a>
                <a href="#" class="hover:text-white transition-colors duration-300"><i class="fab fa-tiktok text-xl"></i></a>
            </div>
            <div class="flex flex-col md:flex-row md:space-x-6">
                <a href="#" class="hover:text-white transition-colors duration-300 mb-2 md:mb-0">Política de Privacidad</a>
                <a href="#" class="hover:text-white transition-colors duration-300">Términos y Condiciones</a>
            </div>
        </div>
    </footer>

    <!-- Order Form Modal -->
    <div id="orderModal" class="modal-overlay">
        <div class="modal-content">
            <button class="modal-close-button" id="closeModal">&times;</button>
            <h2 class="text-3xl font-bold mb-6 text-gray-900 text-center">Completa tu Pedido</h2>
            <p class="text-lg text-gray-700 mb-4 text-center">Estás a punto de pedir: <span id="modalProductName" class="font-semibold text-blue-600"></span></p>

            <form id="orderForm" class="space-y-4">
                <div>
                    <label for="customerName" class="block text-gray-700 text-sm font-bold mb-2">Nombre Completo:</label>
                    <input type="text" id="customerName" name="customerName" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Tu Nombre" required>
                </div>
                <div>
                    <label for="customerID" class="block text-gray-700 text-sm font-bold mb-2">Cédula:</label>
                    <input type="text" id="customerID" name="customerID" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Número de Cédula" required>
                </div>
                <div>
                    <label for="customerCity" class="block text-gray-700 text-sm font-bold mb-2">Ciudad:</label>
                    <input type="text" id="customerCity" name="customerCity" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Tu Ciudad" required>
                </div>
                <div>
                    <label for="customerAddress" class="block text-gray-700 text-sm font-bold mb-2">Dirección de Envío:</label>
                    <input type="text" id="customerAddress" name="customerAddress" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Calle, Número, Sector" required>
                </div>
                <div>
                    <label for="customerPhone" class="block text-gray-700 text-sm font-bold mb-2">Número de Teléfono:</label>
                    <input type="tel" id="customerPhone" name="customerPhone" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Ej: 0985932083" required>
                </div>
                <input type="hidden" id="hiddenProductName" name="productName">
                <button type="submit" class="bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-full w-full transition duration-300 transform hover:scale-105">
                    Enviar Pedido
                </button>
            </form>
        </div>
    </div>

    <script>
        // Ensure Firebase instances are available globally after initialization
        // These will be populated by the module script in the head
        let db;
        let auth;
        let appId;
        let addDoc;
        let collection;

        // Wait for the DOM to be fully loaded and Firebase to be initialized
        window.addEventListener('load', async () => {
            // Access global variables set by the module script
            db = window.db;
            auth = window.auth;
            appId = window.appId;
            addDoc = window.addDoc;
            collection = window.collection;

            // Countdown Timer Logic
            const countdown = document.getElementById('countdown');
            const daysSpan = document.getElementById('days');
            const hoursSpan = document.getElementById('hours');
            const minutesSpan = document.getElementById('minutes');
            const secondsSpan = document.getElementById('seconds');

            // Set the date we're counting down to (e.g., 3 days from now)
            const endDate = new Date();
            endDate.setDate(endDate.getDate() + 3); // Offer ends in 3 days
            endDate.setHours(23, 59, 59, 999); // Set to end of the day

            function updateCountdown() {
                const now = new Date().getTime();
                const distance = endDate.getTime() - now;

                if (distance < 0) {
                    clearInterval(countdownInterval);
                    countdown.innerHTML = "<span class='text-yellow-300'>¡Oferta Finalizada!</span>";
                    return;
                }

                const days = Math.floor(distance / (1000 * 60 * 60 * 24));
                const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((distance % (1000 * 60)) / 1000);

                daysSpan.textContent = String(days).padStart(2, '0');
                hoursSpan.textContent = String(hours).padStart(2, '0');
                minutesSpan.textContent = String(minutes).padStart(2, '0');
                secondsSpan.textContent = String(seconds).padStart(2, '0');
            }

            const countdownInterval = setInterval(updateCountdown, 1000);
            updateCountdown(); // Initial call to display immediately

            // Sale Notification Pop-up Logic
            const salePopup = document.getElementById('salePopup');
            const popupText = document.getElementById('popupText');

            const customers = [
                { name: "Sofía R.", city: "Cuenca", product: "Zapatillas Deportivas 'Agilidad'" },
                { name: "Carlos M.", city: "Quito", product: "Botín Casual 'Elegancia'" },
                { name: "Ana L.", city: "Guayaquil", product: "Zapatilla Urbana 'ConfortPlus'" },
                { name: "Pedro S.", city: "Ambato", product: "Mocasines 'Clásicos'" },
                { name: "Gabriela F.", city: "Manta", product: "Sandalia 'Verano Frescura'" }
            ];

            let currentCustomerIndex = 0;

            function showSalePopup() {
                const customer = customers[currentCustomerIndex];
                popupText.textContent = `¡${customer.name} de ${customer.city} acaba de comprar unas ${customer.product}!`;
                salePopup.style.display = 'block';

                currentCustomerIndex = (currentCustomerIndex + 1) % customers.length;
            }

            // Cycle through pop-ups every 8 seconds (matches animation duration)
            setInterval(showSalePopup, 8000);
            showSalePopup(); // Show initial pop-up immediately

            // WhatsApp Button Sound and Animation
            const whatsappButton = document.getElementById('whatsappButton');
            const synth = new Tone.Synth().toDestination(); // Initialize Tone.js synth

            whatsappButton.addEventListener('click', () => {
                // Play a simple 'pop' sound
                synth.triggerAttackRelease("C5", "8n"); // C5 note for an 8th note duration

                // Add a temporary bounce effect on click
                whatsappButton.style.animation = 'none'; // Remove existing animation
                void whatsappButton.offsetWidth; // Trigger reflow
                whatsappButton.style.animation = 'bounce-click 0.5s ease-out'; // Apply new animation

                // Remove the temporary animation after it finishes to allow hover/pulse to resume
                setTimeout(() => {
                    whatsappButton.style.animation = ''; // Clear the click animation
                    // Reapply the pulse animation if not hovered
                    if (!whatsappButton.matches(':hover')) {
                        whatsappButton.style.animation = 'pulse-whatsapp 2s infinite';
                    }
                }, 500); // Duration of bounce-click animation
            });

            // Define bounce-click animation
            const styleSheet = document.styleSheets[0];
            const bounceClickKeyframes = `
                @keyframes bounce-click {
                    0% { transform: scale(1) translateY(0); }
                    25% { transform: scale(1.15) translateY(-10px); }
                    50% { transform: scale(1.05) translateY(0); }
                    75% { transform: scale(1.1) translateY(-5px); }
                    100% { transform: scale(1) translateY(0); }
                }
            `;
            styleSheet.insertRule(bounceClickKeyframes, styleSheet.cssRules.length);

            // Order Form Modal Logic
            const orderButtons = document.querySelectorAll('.order-button');
            const orderModal = document.getElementById('orderModal');
            const closeModalButton = document.getElementById('closeModal');
            const modalProductName = document.getElementById('modalProductName');
            const hiddenProductName = document.getElementById('hiddenProductName');
            const orderForm = document.getElementById('orderForm');
            const customerNameInput = document.getElementById('customerName');
            const customerIDInput = document.getElementById('customerID');
            const customerCityInput = document.getElementById('customerCity');
            const customerAddressInput = document.getElementById('customerAddress');
            const customerPhoneInput = document.getElementById('customerPhone');

            // Show modal when "Comprar Ahora" button is clicked
            orderButtons.forEach(button => {
                button.addEventListener('click', () => {
                    const productName = button.dataset.productName;
                    modalProductName.textContent = productName;
                    hiddenProductName.value = productName; // Set hidden input value
                    orderModal.classList.add('show');
                });
            });

            // Hide modal when close button or overlay is clicked
            closeModalButton.addEventListener('click', () => {
                orderModal.classList.remove('show');
                orderForm.reset(); // Clear form fields
            });

            orderModal.addEventListener('click', (e) => {
                if (e.target === orderModal) { // Only close if clicking on the overlay, not the content
                    orderModal.classList.remove('show');
                    orderForm.reset(); // Clear form fields
                }
            });

            // Handle form submission
            orderForm.addEventListener('submit', async (e) => { // Added 'async' keyword
                e.preventDefault(); // Prevent default form submission

                const name = customerNameInput.value;
                const id = customerIDInput.value;
                const city = customerCityInput.value;
                const address = customerAddressInput.value;
                const phone = customerPhoneInput.value;
                const product = hiddenProductName.value;
                const whatsappNumber = '593985932083'; // Your WhatsApp number

                // Prepare order data for Firestore
                const orderData = {
                    producto: product,
                    nombre: name,
                    cedula: id,
                    ciudad: city,
                    direccion: address,
                    telefono: phone,
                    fecha_pedido: new Date().toISOString() // Timestamp for the order
                };

                try {
                    // Save order to Firestore
                    // Collection path: /artifacts/{appId}/public/data/pedidos_calzado
                    await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'pedidos_calzado'), orderData);
                    console.log("Pedido guardado en Firestore:", orderData);

                    // Construct the WhatsApp message (no bolding, correct order)
                    const message = `¡Hola! Quiero hacer un pedido.\n\n` +
                                    `Producto: ${product}\n` +
                                    `Nombre: ${name}\n` +
                                    `Cédula: ${id}\n` +
                                    `Ciudad: ${city}\n` +
                                    `Dirección: ${address}\n` +
                                    `Teléfono: ${phone}\n\n` +
                                    `Por favor, confirma mi pedido.`;

                    const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`;

                    // Open WhatsApp chat
                    window.open(whatsappUrl, '_blank');

                    // Optionally, close modal and clear form after submission
                    orderModal.classList.remove('show');
                    orderForm.reset();

                } catch (error) {
                    console.error("Error al guardar el pedido en Firestore:", error);
                    // You could add a user-friendly message here, e.g., "Hubo un error al procesar tu pedido. Por favor, inténtalo de nuevo."
                }
            });
        });
    </script>
</body>
</html>
