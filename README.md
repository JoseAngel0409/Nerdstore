
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NerdStore - Tienda de Juegos y Música</title>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- METADATOS PWA -->
    <meta name="theme-color" content="#6366F1">
    <meta name="description" content="Tu tienda geek de videojuegos y música alternativa">
    <link rel="manifest" href="manifest.json">
    <link rel="icon" href="icons/icon-192x192.png">
    <link rel="apple-touch-icon" href="icons/icon-192x192.png">

    <style>
        /* Tus estilos CSS existentes... */
        body {
            background: linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 50%, #0a0a0a 100%);
            color: #e2e8f0;
            min-height: 100vh;
        }

        .desktop-sidebar {
            width: 250px;
            position: fixed;
            z-index: 10;
            top: 0;
            left: 0;
            height: 100%;
            background: linear-gradient(to bottom, #1e293b, #0f172a);
            padding-top: 20px;
            box-shadow: 2px 0 10px rgba(0,0,0,0.3);
            transition: all 0.3s ease;
            overflow-x: hidden;
        }
        
        .desktop-sidebar a, .desktop-sidebar button {
            padding: 16px 20px;
            text-decoration: none;
            font-size: 16px;
            color: #cbd5e1;
            display: flex;
            align-items: center;
            transition: all 0.3s ease;
            width: 100%;
            border: none;
            background: transparent;
            text-align: left;
            cursor: pointer;
        }
        
        .desktop-sidebar a:hover, .desktop-sidebar button:hover {
            background-color: rgba(99, 102, 241, 0.2);
            color: #818cf8;
        }
        
        .menu-icon {
            min-width: 24px;
            text-align: center;
            font-size: 18px;
            transition: all 0.3s ease;
        }
        
        .sidebar-collapsed .desktop-sidebar {
            width: 70px;
        }
        
        .sidebar-collapsed .desktop-sidebar a,
        .sidebar-collapsed .desktop-sidebar button {
            padding: 16px 0;
            justify-content: center;
        }
        
        .sidebar-collapsed .desktop-sidebar span:not(.menu-icon) {
            display: none;
        }
        
        .sidebar-collapsed .menu-icon {
            margin-right: 0;
            font-size: 20px;
        }
        
        .sidebar-toggle {
            position: fixed;
            bottom: 20px;
            left: 20px;
            z-index: 20;
            background-color: #6366F1;
            color: white;
            border: none;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 2px 10px rgba(0,0,0,0.4);
            transition: all 0.3s ease;
        }
        
        .hidden {
            display: none;
        }

        .product-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .header-card {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .star {
            color: #fbbf24;
            font-size: 1.2rem;
        }

        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(30, 41, 59, 0.5);
        }

        ::-webkit-scrollbar-thumb {
            background: #4f46e5;
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: #6366f1;
        }
        
        .loading-spinner {
            border: 3px solid #f3f3f3;
            border-top: 3px solid #6366f1;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .fade-in {
            animation: fadeIn 0.6s ease-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .install-toast {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #1e293b;
            border: 1px solid #334155;
            border-radius: 12px;
            padding: 16px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.5);
            z-index: 1000;
            max-width: 320px;
            animation: slideIn 0.3s ease-out;
        }

        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .install-toast.hidden {
            display: none;
        }
    </style>
        <script async src="https://www.googletagmanager.com/gtag/js?id=G-ABC123DEF"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', '375953757');
    </script>
</head>
<body class="min-h-screen">
    <!-- Desktop Sidebar -->
    <div class="desktop-sidebar">
        <a href="#" onclick="showCatalog('all')">
            <span class="menu-icon mr-3">🏠</span>
            <span>Inicio</span>
        </a>
        <a href="#" onclick="showCatalog('game')">
            <span class="menu-icon mr-3">🎮</span>
            <span>Juegos</span>
        </a>
        <a href="#" onclick="showCatalog('music')">
            <span class="menu-icon mr-3">🎵</span>
            <span>Música</span>
        </a>
        <a href="profile.html" id="profileSidebarButton" class="hidden">
            <span class="menu-icon mr-3">👤</span>
            <span>Mi Perfil</span>
        </a>
        <a href="#" onclick="showRecommendations()">
            <span class="menu-icon mr-3">🤖</span>
            <span>Recomendaciones IA</span>
        </a>
        <a href="auth.html" id="loginSidebarButton">
            <span class="menu-icon mr-3">🔐</span>
            <span>Iniciar Sesión</span>
        </a>
    </div>

    <!-- Toggle Button -->
    <button class="sidebar-toggle" onclick="toggleSidebar()">≡</button>

    <div class="main-content" style="margin-left: 250px; transition: margin-left 0.3s ease;">
        <div class="container mx-auto p-8 max-w-6xl">
            <header class="header-card rounded-3xl shadow-xl p-6 mb-8 flex flex-col md:flex-row justify-between items-center">
                <div class="flex items-center justify-between w-full md:w-auto mb-4 md:mb-0">
                    <button onclick="showCatalog('all')" class="text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-pink-400 focus:outline-none hover:scale-105 transition duration-300">
                        NerdStore 
                    </button>
                </div>
                <div class="flex space-x-4">
                    <button id="viewCartButton" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition duration-300 ease-in-out transform hover:scale-105">
                        Ver Carrito (<span id="cartItemCount">0</span>)
                    </button>
                    <!-- Install PWA Button - VISIBLE POR DEFECTO PARA TESTING -->
                    <button id="installButton" class="bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition duration-300 ease-in-out transform hover:scale-105">
                        📲 Instalar App
                    </button>
                </div>
            </header>

            <main id="productGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 mb-12">
                <!-- Los productos se cargarán aquí -->
            </main>

            <!-- Sección de Recomendaciones IA -->
            <section id="recommendationsSection" class="mb-12 hidden">
                <div class="flex items-center justify-between mb-6">
                    <h2 class="text-3xl font-bold text-white">🤖 Recomendaciones Personalizadas</h2>
                    <button onclick="refreshRecommendations()" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg transition duration-300">
                        Actualizar
                    </button>
                </div>
                <div id="aiRecommendations" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                    <!-- Las recomendaciones se cargarán aquí -->
                </div>
            </section>

            <!-- Cart Modal -->
            <div id="cartModal" class="fixed inset-0 bg-gray-900 bg-opacity-90 flex items-center justify-center z-50 hidden">
                <div class="bg-gray-800 rounded-3xl shadow-2xl p-8 w-full max-w-2xl relative transform transition-all duration-300 scale-95 opacity-0" id="cartModalContent">
                    <button id="closeCartModal" class="absolute top-5 right-5 text-gray-400 hover:text-gray-200 text-3xl font-bold">&times;</button>
                    <h2 class="text-3xl font-extrabold text-white mb-6 text-center">Tu Carrito de Compras</h2>
                    <div id="cartItems" class="space-y-4 max-h-80 overflow-y-auto pr-2 mb-6">
                        <!-- Cart items will be injected here by JavaScript -->
                        <p id="emptyCartMessage" class="text-center text-gray-400 text-lg">Tu carrito está tristemente vacío.</p>
                    </div>
                    <div class="border-t border-gray-700 pt-6 flex justify-between items-center">
                        <p class="text-2xl font-bold text-white">Total: <span id="cartTotal">$0.00</span></p>
                        <button id="checkoutButton" class="bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition duration-300 ease-in-out transform hover:scale-105">
                            Proceder al Pago
                        </button>
                    </div>
                </div>
            </div>

<div class="flex-1">
    <h3 class="font-bold text-white text-sm mb-1">Instalar NerdStore</h3>
    <p class="text-gray-300 text-xs mb-3">Instala nuestra app para una experiencia mejorada</p>
    <div class="flex space-x-2">
        <button id="installToastButton" onclick="window.location.href='PWA.html'" class="bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-medium py-2 px-4 rounded-lg transition duration-300">
            Instalar
        </button>
        <button id="dismissToast" class="bg-gray-600 hover:bg-gray-500 text-gray-200 text-xs font-medium py-2 px-4 rounded-lg transition duration-300">
            Más tarde
        </button>
    </div>
</div>

            <!-- Details Modal -->
            <div id="detailsModal" class="fixed inset-0 bg-gray-900 bg-opacity-90 flex items-center justify-center z-50 hidden">
                <div class="bg-gray-800 rounded-3xl shadow-2xl p-8 w-full max-w-2xl relative transform transition-all duration-300 scale-95 opacity-0" id="detailsModalContent">
                    <button id="closeDetailsModal" class="absolute top-5 right-5 text-gray-400 hover:text-gray-200 text-3xl font-bold">&times;</button>
                    <h2 class="text-3xl font-extrabold text-white mb-4 text-center" id="detailsTitle"></h2>
                    <img id="detailsImage" src="" alt="" class="w-64 h-64 object-cover rounded-lg mx-auto mb-6 shadow-md">
                    <div class="star-rating flex justify-center mb-4" id="detailsRating"></div>
                    <p class="text-gray-300 text-lg text-center mb-6" id="detailsDescription"></p>
                    <div class="text-center mb-6">
                        <p class="text-xl text-gray-500 line-through" id="detailsOldPrice"></p>
                        <p class="text-4xl font-extrabold text-indigo-400" id="detailsPrice"></p>
                    </div>
                    
                    <!-- FORMULARIO PARA ESCRIBIR RESEÑA -->
                    <div id="reviewFormContainer" class="mb-6 p-4 bg-gray-700 rounded-xl hidden">
                        <h3 class="text-xl font-bold text-white mb-4">Escribe tu reseña</h3>
                        <div class="mb-4">
                            <label class="block text-gray-300 text-sm font-bold mb-2">Calificación:</label>
                            <div class="flex space-x-1" id="reviewStars">
                                <span class="text-2xl cursor-pointer review-star" data-rating="1">☆</span>
                                <span class="text-2xl cursor-pointer review-star" data-rating="2">☆</span>
                                <span class="text-2xl cursor-pointer review-star" data-rating="3">☆</span>
                                <span class="text-2xl cursor-pointer review-star" data-rating="4">☆</span>
                                <span class="text-2xl cursor-pointer review-star" data-rating="5">☆</span>
                            </div>
                            <p class="text-gray-400 text-sm mt-1" id="selectedRating">Selecciona una calificación</p>
                        </div>
                        <div class="mb-4">
                            <label for="reviewText" class="block text-gray-300 text-sm font-bold mb-2">Tu reseña:</label>
                            <textarea id="reviewText" class="w-full bg-gray-600 text-white rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-indigo-500" rows="4" placeholder="Comparte tu experiencia con este producto..."></textarea>
                        </div>
                        <div class="flex space-x-4">
                            <button id="submitReviewButton" class="flex-1 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-lg transition duration-300">
                                Publicar Reseña
                            </button>
                            <button id="cancelReviewButton" class="bg-gray-600 hover:bg-gray-500 text-white font-bold py-2 px-4 rounded-lg transition duration-300">
                                Cancelar
                            </button>
                        </div>
                    </div>

                    <h3 class="text-2xl font-bold text-white mb-4 text-center">Reseñas de "Clientes Felices"</h3>
                    
                    <!-- BOTÓN PARA ESCRIBIR RESEÑA -->
                    <div class="text-center mb-4">
                        <button id="writeReviewButton" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-6 rounded-full transition duration-300">
                            ✍️ Escribir Reseña
                        </button>
                    </div>

                    <div id="detailsReviews" class="space-y-4 max-h-48 overflow-y-auto pr-2 mb-6">
                        <!-- Reviews will be injected here -->
                    </div>

                    <!-- BOTONES MODIFICADOS -->
                    <div class="flex space-x-4">
                        <button id="addToCartDetailsButton" class="flex-1 bg-purple-600 hover:bg-purple-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition duration-300 ease-in-out transform hover:scale-105">
                            Agregar al Carrito
                        </button>
                        <button id="addToFavoritesDetailsButton" class="bg-yellow-600 hover:bg-yellow-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition duration-300 ease-in-out transform hover:scale-105">
                            ⭐ Favorito
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <footer class="bg-gray-900 text-gray-400 text-center p-6 mt-12 rounded-t-3xl shadow-inner">
            <p>&copy; 2025 NerdStore. Todos los derechos reservados.</p>
            <p class="text-sm text-gray-500 mt-2">El mejor sitio para comprar.</p>
        </footer>
    </div>

    <!-- Toast de instalación -->
    <div id="installToast" class="install-toast hidden">
        <div class="flex items-start space-x-3">
            <div class="flex-shrink-0">
                <div class="w-10 h-10 bg-indigo-600 rounded-full flex items-center justify-center">
                    <span class="text-white text-lg">📱</span>
                </div>
            </div>



    <script>
     // ===== SISTEMA PWA MEJORADO =====
        let deferredPrompt;
        const installButton = document.getElementById('installButton');
        const installToast = document.getElementById('installToast');
        const installToastButton = document.getElementById('installToastButton');
        const dismissToast = document.getElementById('dismissToast');

        // Detectar si se puede instalar
        window.addEventListener('beforeinstallprompt', (e) => {
            console.log('🚀 beforeinstallprompt event fired');
            
            e.preventDefault();
            deferredPrompt = e;
            
            // El botón ya está visible, así que solo agregamos funcionalidad
            installButton.addEventListener('click', async () => {
                if (deferredPrompt) {
                    await triggerInstall();
                }
            });

            // Mostrar toast después de 5 segundos
            setTimeout(() => {
                if (deferredPrompt && !isAppInstalled()) {
                    showInstallToast();
                }
            }, 5000);

            // Agregar evento al botón del toast
            installToastButton.addEventListener('click', async () => {
                if (deferredPrompt) {
                    await triggerInstall();
                    hideInstallToast();
                }
            });
        });

        // Función para disparar la instalación
        async function triggerInstall() {
            if (deferredPrompt) {
                deferredPrompt.prompt();
                
                const { outcome } = await deferredPrompt.userChoice;
                
                console.log(`User response to the install prompt: ${outcome}`);
                
                if (outcome === 'accepted') {
                    console.log('✅ PWA installed successfully');
                    installButton.classList.add('hidden');
                    hideInstallToast();
                    showNotification('¡NerdStore instalada! 🎉 Ahora puedes acceder desde tu pantalla de inicio.');
                }
                
                deferredPrompt = null;
            } else {
                // Si no hay evento beforeinstallprompt, mostrar instrucciones
                showNotification('Para instalar: En Chrome, haz clic en los 3 puntos → "Instalar NerdStore". En Safari, comparte → "Agregar a pantalla de inicio"', 'info');
            }
        }

        // Mostrar toast de instalación
        function showInstallToast() {
            if (!isAppInstalled()) {
                installToast.classList.remove('hidden');
                
                setTimeout(() => {
                    hideInstallToast();
                }, 15000);
            }
        }

        // Ocultar toast de instalación
        function hideInstallToast() {
            installToast.classList.add('hidden');
        }

        // Verificar si la app ya está instalada
        function isAppInstalled() {
            return window.matchMedia('(display-mode: standalone)').matches || 
                   window.navigator.standalone === true;
        }

        // Ocultar elementos si ya está instalada
        window.addEventListener('appinstalled', () => {
            console.log('✅ PWA was installed');
            installButton.classList.add('hidden');
            hideInstallToast();
        });

        // También ocultar si ya está en modo standalone (ya instalada)
        if (isAppInstalled()) {
            console.log('📱 Ya está en modo standalone (instalada)');
            installButton.classList.add('hidden');
            hideInstallToast();
        }

        // Evento para descartar el toast
        dismissToast.addEventListener('click', hideInstallToast);

        // Registrar Service Worker
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                // Intentar registrar el service worker
                navigator.serviceWorker.register('/sw.js')
                    .then(registration => {
                        console.log('✅ Service Worker registrado: ', registration);
                        
                        // Verificar actualizaciones periódicamente
                        setInterval(() => {
                            registration.update();
                        }, 60 * 60 * 1000); // Cada hora
                    })
                    .catch(registrationError => {
                        console.log('❌ Error registrando Service Worker: ', registrationError);
                        
                        // Intentar con ruta relativa si falla la absoluta
                        navigator.serviceWorker.register('./sw.js')
                            .then(registration => {
                                console.log('✅ Service Worker registrado con ruta relativa');
                            })
                            .catch(error => {
                                console.log('❌ Error con ruta relativa también: ', error);
                            });
                    });
            });
        }

        // ===== FUNCIÓN DE NOTIFICACIÓN =====
        function showNotification(message, type = 'success') {
            // Crear notificación
            const notification = document.createElement('div');
            notification.className = `fixed top-4 right-4 p-4 rounded-lg shadow-lg z-50 transform transition-transform duration-300 ${
                type === 'success' ? 'bg-green-600 text-white' : 'bg-red-600 text-white'
            }`;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            // Remover después de 3 segundos
            setTimeout(() => {
                notification.style.transform = 'translateX(100%)';
                setTimeout(() => {
                    if (document.body.contains(notification)) {
                        document.body.removeChild(notification);
                    }
                }, 300);
            }, 3000);
        }

        // ===== DEBUG PWA =====
        function debugPWA() {
            console.log('=== DEBUG PWA ===');
            console.log('Install button:', document.getElementById('installButton'));
            console.log('Service Worker support:', 'serviceWorker' in navigator);
            console.log('BeforeInstallPrompt event:', !!deferredPrompt);
            console.log('Display mode:', window.matchMedia('(display-mode: standalone)').matches);
            console.log('Is app installed:', isAppInstalled());
        }

        // ===== SISTEMA DE GESTIÓN DE USUARIOS =====
        class UserManager {
            constructor() {
                this.currentUser = this.getCurrentUserFromStorage();
                this.users = this.getUsersFromStorage();
            }

            getCurrentUserFromStorage() {
                const user = localStorage.getItem('nerdstore_current_user');
                return user ? JSON.parse(user) : null;
            }

            getUsersFromStorage() {
                const users = localStorage.getItem('nerdstore_users');
                return users ? JSON.parse(users) : [];
            }

            saveUsersToStorage() {
                localStorage.setItem('nerdstore_users', JSON.stringify(this.users));
            }

            saveCurrentUserToStorage(user) {
                if (user) {
                    localStorage.setItem('nerdstore_current_user', JSON.stringify(user));
                } else {
                    localStorage.removeItem('nerdstore_current_user');
                }
            }

            registerUser(name, email, password) {
                if (this.users.find(user => user.email === email)) {
                    return { success: false, message: 'Este correo electrónico ya está registrado' };
                }

                const newUser = {
                    id: Date.now().toString(),
                    name: name,
                    email: email,
                    password: password,
                    createdAt: new Date().toISOString(),
                    purchases: [],
                    favoriteGames: [],
                    favoriteMusic: [],
                    reviews: []
                };

                this.users.push(newUser);
                this.saveUsersToStorage();
                
                return { success: true, message: 'Usuario registrado exitosamente', user: newUser };
            }

            loginUser(email, password) {
                const user = this.users.find(user => user.email === email && user.password === password);
                
                if (user) {
                    this.currentUser = user;
                    this.saveCurrentUserToStorage(user);
                    return { success: true, message: 'Inicio de sesión exitoso', user: user };
                } else {
                    return { success: false, message: 'Correo electrónico o contraseña incorrectos' };
                }
            }

            logout() {
                this.currentUser = null;
                this.saveCurrentUserToStorage(null);
            }

            isLoggedIn() {
                return this.currentUser !== null;
            }

            getCurrentUser() {
                return this.currentUser;
            }

            addToFavorites(userId, product, type) {
                const user = this.users.find(u => u.id === userId);
                if (user) {
                    if (!user.favoriteGames) user.favoriteGames = [];
                    if (!user.favoriteMusic) user.favoriteMusic = [];
                    
                    const favoritesArray = type === 'game' ? user.favoriteGames : user.favoriteMusic;
                    
                    if (!favoritesArray.find(fav => fav.id === product.id)) {
                        favoritesArray.push(product);
                        this.saveUsersToStorage();
                        return true;
                    }
                }
                return false;
            }

            getUserProfile(userId) {
                const user = this.users.find(u => u.id === userId);
                if (!user) return null;

                if (!user.favoriteGames) user.favoriteGames = [];
                if (!user.favoriteMusic) user.favoriteMusic = [];
                if (!user.reviews) user.reviews = [];
                if (!user.purchases) user.purchases = [];

                return user;
            }

            saveUserProfile(userProfile) {
                const userIndex = this.users.findIndex(u => u.id === userProfile.id);
                if (userIndex !== -1) {
                    this.users[userIndex] = userProfile;
                    this.saveUsersToStorage();
                    return true;
                }
                return false;
            }

            addReview(userId, productId, productName, rating, text) {
                const user = this.users.find(u => u.id === userId);
                if (user) {
                    if (!user.reviews) user.reviews = [];
                    
                    const newReview = {
                        id: Date.now().toString(),
                        productId: productId,
                        product: productName,
                        rating: rating,
                        text: text,
                        date: new Date().toISOString(),
                        user: user.name
                    };
                    
                    user.reviews.push(newReview);
                    this.saveUsersToStorage();
                    return { success: true, review: newReview };
                }
                return { success: false, message: 'Usuario no encontrado' };
            }

            deleteReview(userId, reviewId) {
                const user = this.users.find(u => u.id === userId);
                if (user && user.reviews) {
                    const initialLength = user.reviews.length;
                    user.reviews = user.reviews.filter(review => review.id !== reviewId);
                    
                    if (user.reviews.length < initialLength) {
                        this.saveUsersToStorage();
                        return { success: true };
                    }
                }
                return { success: false, message: 'Reseña no encontrada' };
            }

            addPurchase(userId, cartItems, total) {
                const user = this.users.find(u => u.id === userId);
                if (user) {
                    if (!user.purchases) user.purchases = [];
                    
                    const purchase = {
                        id: Date.now().toString(),
                        date: new Date().toISOString(),
                        items: JSON.parse(JSON.stringify(cartItems)),
                        total: total
                    };
                    user.purchases.push(purchase);
                    this.saveUsersToStorage();
                    return { success: true };
                }
                return { success: false };
            }
        }

        // INICIALIZAR COMPONENTES
        const userManager = new UserManager();
        
        // Elementos del DOM
        const productGrid = document.getElementById('productGrid');
        const cartModal = document.getElementById('cartModal');
        const cartModalContent = document.getElementById('cartModalContent');
        const closeCartModal = document.getElementById('closeCartModal');
        const viewCartButton = document.getElementById('viewCartButton');
        const cartItemsContainer = document.getElementById('cartItems');
        const cartTotalSpan = document.getElementById('cartTotal');
        const cartItemCountSpan = document.getElementById('cartItemCount');
        const emptyCartMessage = document.getElementById('emptyCartMessage');
        const checkoutButton = document.getElementById('checkoutButton');

        const writeReviewButton = document.getElementById('writeReviewButton');
        const reviewFormContainer = document.getElementById('reviewFormContainer');
        const reviewStars = document.querySelectorAll('.review-star');
        const reviewText = document.getElementById('reviewText');
        const submitReviewButton = document.getElementById('submitReviewButton');
        const cancelReviewButton = document.getElementById('cancelReviewButton');
        const selectedRating = document.getElementById('selectedRating');

        const detailsModal = document.getElementById('detailsModal');
        const detailsModalContent = document.getElementById('detailsModalContent');
        const closeDetailsModal = document.getElementById('closeDetailsModal');
        const detailsTitle = document.getElementById('detailsTitle');
        const detailsDescription = document.getElementById('detailsDescription');
        const detailsPrice = document.getElementById('detailsPrice');
        const detailsOldPrice = document.getElementById('detailsOldPrice');
        const detailsImage = document.getElementById('detailsImage');
        const detailsRating = document.getElementById('detailsRating');
        const detailsReviews = document.getElementById('detailsReviews');
        const addToCartDetailsButton = document.getElementById('addToCartDetailsButton');
        const addToFavoritesDetailsButton = document.getElementById('addToFavoritesDetailsButton');

        const profileSidebarButton = document.getElementById('profileSidebarButton');
        const loginSidebarButton = document.getElementById('loginSidebarButton');

        // Variables globales
        let cart = [];
        let currentReviewRating = 0;
        let currentReviewProductId = null;
        let currentReviewProductName = null;

        // FUNCIONES PRINCIPALES
        function updateUserInterface() {
            const currentUser = userManager.getCurrentUser();
            
            if (currentUser) {
                profileSidebarButton.classList.remove('hidden');
            } else {
                profileSidebarButton.classList.add('hidden');
            }
        }

        function toggleSidebar() {
            document.body.classList.toggle('sidebar-collapsed');
            const mainContent = document.querySelector('.main-content');
            if (document.body.classList.contains('sidebar-collapsed')) {
                mainContent.style.marginLeft = '70px';
            } else {
                mainContent.style.marginLeft = '250px';
            }
        }

        function renderProducts(productsToRender) {
            productGrid.innerHTML = '';
            productsToRender.forEach(product => {
                const productCard = document.createElement('div');
                productCard.className = 'product-card rounded-2xl shadow-lg p-6 flex flex-col items-center text-center transform transition duration-300 hover:scale-105 hover:shadow-xl';
                
                let priceDisplay = `<p class="text-3xl font-extrabold text-indigo-400 mb-4">$${product.price.toFixed(2)}</p>`;
                if (product.oldPrice) {
                    priceDisplay = `
                        <p class="text-xl text-gray-500 line-through">$${product.oldPrice.toFixed(2)}</p>
                        <p class="text-3xl font-extrabold text-indigo-400 mb-4">$${product.price.toFixed(2)}</p>
                    `;
                }

                productCard.innerHTML = `
                    <img src="${product.image}" alt="${product.name}" class="w-40 h-40 object-cover rounded-lg mb-4 shadow-md">
                    <h2 class="text-2xl font-bold text-white mb-2">${product.name}</h2>
                    <p class="text-gray-300 mb-4">${product.description.substring(0, 60)}...</p>
                    ${priceDisplay}
                    <div class="flex space-x-4">
                        <button class="add-to-cart-btn bg-purple-600 hover:bg-purple-700 text-white font-semibold py-2 px-5 rounded-full shadow-md transition duration-300" data-id="${product.id}">
                            Agregar
                        </button>
                        <button class="view-details-btn bg-gray-700 hover:bg-gray-600 text-gray-200 font-semibold py-2 px-5 rounded-full shadow-md transition duration-300" data-id="${product.id}">
                            Detalles
                        </button>
                        <button class="favorite-btn bg-yellow-600 hover:bg-yellow-700 text-white font-semibold py-2 px-3 rounded-full shadow-md transition duration-300" data-id="${product.id}" data-type="${product.type}">
                            ⭐
                        </button>
                    </div>
                `;
                productGrid.appendChild(productCard);
            });
        }

        function updateCartDisplay() {
            cartItemsContainer.innerHTML = '';
            let total = 0;

            if (cart.length === 0) {
                emptyCartMessage.style.display = 'block';
            } else {
                emptyCartMessage.style.display = 'none';
                cart.forEach(item => {
                    const itemElement = document.createElement('div');
                    itemElement.className = 'flex items-center justify-between bg-gray-700 p-4 rounded-xl shadow-sm';
                    itemElement.innerHTML = `
                        <img src="${item.image}" alt="${item.name}" class="w-16 h-16 object-cover rounded-md mr-4">
                        <div class="flex-1">
                            <span class="font-semibold text-white block">${item.name}</span>
                            <span class="text-gray-300 text-sm">$${item.price.toFixed(2)} c/u</span>
                        </div>
                        <div class="flex items-center space-x-2">
                            <button class="quantity-btn bg-gray-600 hover:bg-gray-500 text-gray-200 font-bold py-1 px-2 rounded-full transition duration-300" data-id="${item.id}" data-action="decrease">-</button>
                            <span class="font-bold text-lg text-white">${item.quantity}</span>
                            <button class="quantity-btn bg-gray-600 hover:bg-gray-500 text-gray-200 font-bold py-1 px-2 rounded-full transition duration-300" data-id="${item.id}" data-action="increase">+</button>
                        </div>
                        <span class="font-bold text-white ml-4">$${(item.price * item.quantity).toFixed(2)}</span>
                        <button class="remove-from-cart-btn bg-red-700 hover:bg-red-600 text-white font-bold py-1 px-3 rounded-full transition duration-300 ml-4" data-id="${item.id}">
                            Eliminar
                        </button>
                    `;
                    cartItemsContainer.appendChild(itemElement);
                    total += item.price * item.quantity;
                });
            }
            cartTotalSpan.textContent = `$${total.toFixed(2)}`;
            const itemCount = cart.reduce((sum, item) => sum + item.quantity, 0);
            cartItemCountSpan.textContent = itemCount;
        }

        function showCatalog(type) {
            let filteredProducts = [];
            if (type === 'game') {
                filteredProducts = allProducts.filter(p => p.type === 'game');
            } else if (type === 'music') {
                filteredProducts = allProducts.filter(p => p.type === 'music');
            } else {
                filteredProducts = allProducts;
            }
            renderProducts(filteredProducts);
        }

        function resetReviewForm() {
            currentReviewRating = 0;
            reviewText.value = '';
            selectedRating.textContent = 'Selecciona una calificación';
            
            reviewStars.forEach(star => {
                star.textContent = '☆';
                star.classList.remove('text-yellow-400');
            });
            
            reviewFormContainer.classList.add('hidden');
            writeReviewButton.classList.remove('hidden');
        }

        function displayReviews(product) {
            const reviewsContainer = document.getElementById('detailsReviews');
            reviewsContainer.innerHTML = '';

            const currentUser = userManager.getCurrentUser();
            let allReviews = [...product.reviews];

            if (currentUser && currentUser.reviews) {
                const userReviewsForProduct = currentUser.reviews.filter(review => 
                    review.productId === product.id
                );
                allReviews = [...userReviewsForProduct, ...allReviews];
            }

            if (allReviews.length > 0) {
                allReviews.forEach(review => {
                    const reviewElement = document.createElement('div');
                    reviewElement.className = 'bg-gray-700 p-4 rounded-lg shadow-sm';
                    
                    let stars = '';
                    for (let i = 0; i < 5; i++) {
                        stars += i < review.rating ? '⭐' : '☆';
                    }
                    
                    reviewElement.innerHTML = `
                        <div class="flex justify-between items-start mb-2">
                            <p class="font-semibold text-white">${review.user}</p>
                            <div class="text-yellow-400 text-sm">${stars}</div>
                        </div>
                        <p class="text-gray-300 text-sm mb-2">${review.text}</p>
                        <p class="text-gray-500 text-xs">${new Date(review.date).toLocaleDateString('es-ES')}</p>
                        ${review.user === currentUser?.name ? 
                            `<button onclick="deleteReview('${review.id}')" class="text-red-400 hover:text-red-300 text-xs mt-2">Eliminar</button>` : 
                            ''
                        }
                    `;
                    reviewsContainer.appendChild(reviewElement);
                });
            } else {
                reviewsContainer.innerHTML = '<p class="text-center text-gray-400">Sé el primero en reseñar este producto. ¡Anímate!</p>';
            }
        }

        function deleteReview(reviewId) {
            if (confirm('¿Estás seguro de que quieres eliminar esta reseña?')) {
                const currentUser = userManager.getCurrentUser();
                if (currentUser) {
                    const result = userManager.deleteReview(currentUser.id, reviewId);
                    
                    if (result.success) {
                        const product = allProducts.find(p => p.id === currentReviewProductId);
                        if (product) {
                            displayReviews(product);
                        }
                        alert('✅ Reseña eliminada correctamente');
                    } else {
                        alert('❌ Error al eliminar la reseña');
                    }
                }
            }
        }

        // EVENT LISTENERS
        productGrid.addEventListener('click', (e) => {
            const target = e.target;
            
            if (target.classList.contains('add-to-cart-btn')) {
                const productId = target.dataset.id;
                const productToAdd = allProducts.find(p => p.id === productId);
                const existingItem = cart.find(item => item.id === productId);

                if (existingItem) {
                    existingItem.quantity++;
                } else {
                    cart.push({ ...productToAdd, quantity: 1 });
                }
                updateCartDisplay();
                alert(`¡Éxito! "${productToAdd.name}" ha sido añadido a tu carrito. ¡Que lo "disfrutes"!`);
            }
            
            else if (target.classList.contains('view-details-btn')) {
                const id = target.dataset.id;
                const product = allProducts.find(p => p.id === id);
                if (product) {
                    detailsTitle.textContent = product.name;
                    detailsDescription.textContent = product.description;
                    detailsImage.src = product.image;

                    if (product.oldPrice) {
                        detailsOldPrice.textContent = `$${product.oldPrice.toFixed(2)}`;
                        detailsOldPrice.style.display = 'block';
                    } else {
                        detailsOldPrice.style.display = 'none';
                    }
                    detailsPrice.textContent = `$${product.price.toFixed(2)}`;

                    detailsRating.innerHTML = '';
                    for (let i = 0; i < 5; i++) {
                        const star = document.createElement('span');
                        star.className = 'star';
                        star.innerHTML = i < product.rating ? '&#9733;' : '&#9734;';
                        detailsRating.appendChild(star);
                    }

                    resetReviewForm();
                    currentReviewProductId = product.id;
                    currentReviewProductName = product.name;
                    displayReviews(product);

                    addToCartDetailsButton.dataset.id = product.id;
                    addToCartDetailsButton.dataset.name = product.name;
                    addToCartDetailsButton.dataset.price = product.price;

                    addToFavoritesDetailsButton.dataset.id = product.id;
                    addToFavoritesDetailsButton.dataset.name = product.name;
                    addToFavoritesDetailsButton.dataset.type = product.type;

                    detailsModal.classList.remove('hidden');
                    setTimeout(() => {
                        detailsModalContent.classList.remove('scale-95', 'opacity-0');
                        detailsModalContent.classList.add('scale-100', 'opacity-100');
                    }, 50);
                }
            }
            
            else if (target.classList.contains('favorite-btn')) {
                const productId = target.dataset.id;
                const productType = target.dataset.type;
                const productToAdd = allProducts.find(p => p.id === productId);
                const currentUser = userManager.getCurrentUser();
                
                if (currentUser) {
                    const success = userManager.addToFavorites(currentUser.id, productToAdd, productType);
                    if (success) {
                        alert(`"${productToAdd.name}" agregado a favoritos! 🎉`);
                    } else {
                        alert(`"${productToAdd.name}" ya está en tus favoritos ⭐`);
                    }
                } else {
                    alert('⚠️ Inicia sesión para agregar productos a favoritos');
                    window.location.href = 'auth.html';
                }
            }
        });

        cartItemsContainer.addEventListener('click', (e) => {
            if (e.target.classList.contains('remove-from-cart-btn')) {
                const idToRemove = e.target.dataset.id;
                const removedItem = cart.find(item => item.id === idToRemove);
                cart = cart.filter(item => item.id !== idToRemove);
                alert(`¡Adiós! "${removedItem.name}" ha sido retirado de tu lista de deseos. ¡Esperamos que no te arrepientas!`);
                updateCartDisplay();
            } else if (e.target.classList.contains('quantity-btn')) {
                const productId = e.target.dataset.id;
                const action = e.target.dataset.action;
                const item = cart.find(i => i.id === productId);

                if (item) {
                    if (action === 'increase') {
                        item.quantity++;
                    } else if (action === 'decrease') {
                        if (item.quantity > 1) {
                            item.quantity--;
                        } else {
                            const removedItem = cart.find(i => i.id === productId);
                            cart = cart.filter(i => i.id !== productId);
                            alert(`¡Adiós! "${removedItem.name}" ha sido retirado de tu lista de deseos. ¡Esperamos que no te arrepientas!`);
                        }
                    }
                    updateCartDisplay();
                }
            }
        });

        viewCartButton.addEventListener('click', () => {
            cartModal.classList.remove('hidden');
            setTimeout(() => {
                cartModalContent.classList.remove('scale-95', 'opacity-0');
                cartModalContent.classList.add('scale-100', 'opacity-100');
            }, 50);
        });

        closeCartModal.addEventListener('click', () => {
            cartModalContent.classList.remove('scale-100', 'opacity-100');
            cartModalContent.classList.add('scale-95', 'opacity-0');
            setTimeout(() => {
                cartModal.classList.add('hidden');
            }, 300);
        });

        closeDetailsModal.addEventListener('click', () => {
            detailsModalContent.classList.remove('scale-100', 'opacity-100');
            detailsModalContent.classList.add('scale-95', 'opacity-0');
            setTimeout(() => {
                detailsModal.classList.add('hidden');
            }, 300);
        });

        addToCartDetailsButton.addEventListener('click', (e) => {
            const productId = e.target.dataset.id;
            const productToAdd = allProducts.find(p => p.id === productId);
            const existingItem = cart.find(item => item.id === productId);

            if (existingItem) {
                existingItem.quantity++;
            } else {
                cart.push({ ...productToAdd, quantity: 1 });
            }
            updateCartDisplay();
            alert(`¡Éxito! "${productToAdd.name}" ha sido añadido a tu carrito de la fortuna. ¡Que lo "disfrutes"!`);
            closeDetailsModal.click();
        });

        addToFavoritesDetailsButton.addEventListener('click', (e) => {
            const productId = e.target.dataset.id;
            const productName = e.target.dataset.name;
            const productType = e.target.dataset.type;
            const productToAdd = allProducts.find(p => p.id === productId);
            const currentUser = userManager.getCurrentUser();
            
            if (currentUser) {
                const success = userManager.addToFavorites(currentUser.id, productToAdd, productType);
                if (success) {
                    alert(`"${productName}" agregado a favoritos! 🎉`);
                } else {
                    alert(`"${productName}" ya está en tus favoritos ⭐`);
                }
            } else {
                alert('⚠️ Inicia sesión para agregar productos a favoritos');
                window.location.href = 'auth.html';
            }
        });

        // CHECKOUT MEJORADO - CON VERIFICACIÓN
        checkoutButton.addEventListener('click', () => {
            console.log('🛒 Botón de checkout clickeado');
            
            if (cart.length === 0) {
                alert('¡Tu carrito está vacío! No puedes pagar el aire, ¿o sí?');
                return;
            }
            
            console.log('📦 Productos en carrito:', cart);
            
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            console.log('💰 Total calculado:', total);
            
            const currentUser = userManager.getCurrentUser();
            if (currentUser) {
                console.log('👤 Usuario logueado:', currentUser.name);
                const purchaseResult = userManager.addPurchase(currentUser.id, cart, total);
                if (purchaseResult.success) {
                    console.log('✅ Compra guardada en historial');
                }
            } else {
                console.log('👤 Usuario no logueado - compra como invitado');
            }
            
            try {
                sessionStorage.setItem('checkoutCart', JSON.stringify(cart));
                sessionStorage.setItem('cartTotal', total.toFixed(2));
                console.log('💾 Datos guardados en sessionStorage');
            } catch (error) {
                console.error('❌ Error guardando en sessionStorage:', error);
                alert('Error al procesar la compra. Intenta nuevamente.');
                return;
            }
            
            console.log('🔗 Intentando redirección a payment.html...');
            
            // Redirección con manejo de errores
            setTimeout(() => {
                try {
                    window.location.href = 'payment.html';
                } catch (error) {
                    console.error('❌ Error en redirección:', error);
                    alert('Error al cargar la página de pago. Verifica que payment.html exista.');
                }
            }, 100);
        });

        // ===== SISTEMA DE RECOMENDACIONES CON IA =====
        class AIRecommender {
            constructor() {
                this.userPreferences = this.getUserPreferences();
            }

            getUserPreferences() {
                const user = userManager.getCurrentUser();
                if (!user) {
                    return this.getDefaultPreferences();
                }

                return {
                    favoriteGenres: user.favoriteGames?.map(game => game.genre).concat(user.favoriteMusic?.map(music => music.genre)) || [],
                    favoriteArtists: user.favoriteMusic?.map(music => music.artist) || [],
                    purchaseHistory: user.purchases || [],
                    ratingPattern: this.analyzeRatingPattern(user),
                    priceRange: user.preferences?.priceRange || { min: 0, max: 100 }
                };
            }

            getDefaultPreferences() {
                return {
                    favoriteGenres: ['RPG', 'Action', 'Indie', 'Rock', 'Alternative'],
                    favoriteArtists: [],
                    purchaseHistory: [],
                    ratingPattern: { average: 4, preference: 'high-rated' },
                    priceRange: { min: 0, max: 100 }
                };
            }

            analyzeRatingPattern(user) {
                if (!user.reviews || user.reviews.length === 0) {
                    return { average: 4, preference: 'high-rated' };
                }

                const avgRating = user.reviews.reduce((sum, review) => sum + review.rating, 0) / user.reviews.length;
                return {
                    average: avgRating,
                    preference: avgRating > 4 ? 'high-rated' : avgRating < 3 ? 'low-rated' : 'mixed'
                };
            }

            getRecommendations(products, count = 6) {
                const preferences = this.userPreferences;
                
                const scoredProducts = products.map(product => ({
                    product,
                    score: this.calculateMatchScore(product, preferences)
                }));

                return scoredProducts
                    .filter(item => item.score > 0.3)
                    .sort((a, b) => b.score - a.score)
                    .slice(0, count)
                    .map(item => item.product);
            }

            calculateMatchScore(product, preferences) {
                let score = 0;
                
                // Match por género
                if (preferences.favoriteGenres.includes(product.genre)) {
                    score += 0.4;
                }
                
                // Match por artista (solo música)
                if (product.type === 'music' && preferences.favoriteArtists.includes(product.artist)) {
                    score += 0.3;
                }
                
                // Match por precio
                if (product.price >= preferences.priceRange.min && product.price <= preferences.priceRange.max) {
                    score += 0.2;
                }
                
                // Bonus por alta calificación si el usuario prefiere productos bien calificados
                if (preferences.ratingPattern.preference === 'high-rated' && product.rating >= 4) {
                    score += 0.1;
                }
                
                return Math.min(score, 1);
            }
        }

        // ===== FUNCIONES PARA LA INTERFAZ DE RECOMENDACIONES =====
        function showRecommendations() {
            // Mostrar sección de recomendaciones
            const recommendationsSection = document.getElementById('recommendationsSection');
            const productGrid = document.getElementById('productGrid');
            
            if (recommendationsSection) {
                recommendationsSection.classList.remove('hidden');
                productGrid.classList.add('hidden');
            }
            
            // Crear spinner de carga
            const aiRecommendations = document.getElementById('aiRecommendations');
            if (aiRecommendations) {
                aiRecommendations.innerHTML = `
                    <div class="col-span-full text-center py-12">
                        <div class="loading-spinner"></div>
                        <p class="text-gray-400 mt-4">🤖 Analizando tus preferencias...</p>
                    </div>
                `;
            }
            
            // Simular carga de recomendaciones
            setTimeout(() => {
                const aiRecommender = new AIRecommender();
                const recommendations = aiRecommender.getRecommendations(allProducts);
                renderAIRecommendations(recommendations);
            }, 1500);
        }

        function renderAIRecommendations(recommendations) {
            const aiRecommendations = document.getElementById('aiRecommendations');
            if (!aiRecommendations) return;
            
            aiRecommendations.innerHTML = '';
            
            if (recommendations.length === 0) {
                aiRecommendations.innerHTML = `
                    <div class="col-span-full text-center py-12">
                        <p class="text-gray-400 text-xl">Inicia sesión para obtener recomendaciones personalizadas</p>
                        <button onclick="window.location.href='auth.html'" class="mt-4 bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-2 px-6 rounded-lg transition duration-300">
                            Iniciar Sesión
                        </button>
                    </div>
                `;
                return;
            }
            
            recommendations.forEach((product, index) => {
                const productCard = document.createElement('div');
                productCard.className = 'product-card rounded-2xl shadow-lg p-6 flex flex-col items-center text-center transform transition duration-300 hover:scale-105 hover:shadow-xl fade-in';
                productCard.style.animationDelay = `${index * 0.1}s`;
                
                let priceDisplay = `<p class="text-3xl font-extrabold text-indigo-400 mb-4">$${product.price.toFixed(2)}</p>`;
                if (product.oldPrice) {
                    priceDisplay = `
                        <p class="text-xl text-gray-500 line-through">$${product.oldPrice.toFixed(2)}</p>
                        <p class="text-3xl font-extrabold text-indigo-400 mb-4">$${product.price.toFixed(2)}</p>
                    `;
                }

                productCard.innerHTML = `
                    <div class="absolute top-3 left-3 bg-gradient-to-r from-purple-600 to-pink-600 text-white text-xs font-bold px-2 py-1 rounded-full">
                        🤖 IA
                    </div>
                    <img src="${product.image}" alt="${product.name}" class="w-32 h-32 object-cover rounded-lg mb-4 shadow-md" loading="lazy">
                    <h2 class="text-xl font-bold text-white mb-2">${product.name}</h2>
                    <p class="text-gray-300 text-sm mb-4">${product.description.substring(0, 50)}...</p>
                    ${priceDisplay}
                    <div class="flex space-x-2">
                        <button class="add-to-cart-btn bg-purple-600 hover:bg-purple-700 text-white font-semibold py-2 px-4 rounded-full shadow-md transition duration-300 text-sm" data-id="${product.id}">
                            Agregar
                        </button>
                        <button class="view-details-btn bg-gray-700 hover:bg-gray-600 text-gray-200 font-semibold py-2 px-4 rounded-full shadow-md transition duration-300 text-sm" data-id="${product.id}">
                            Ver
                        </button>
                    </div>
                `;
                aiRecommendations.appendChild(productCard);
            });
        }

        function refreshRecommendations() {
            showRecommendations();
            showNotification('🔄 Recomendaciones actualizadas');
        }

        // ===== MODIFICAR showCatalog PARA OCULTAR RECOMENDACIONES =====
        const originalShowCatalog = showCatalog;
        showCatalog = function(type) {
            // Ocultar sección de recomendaciones
            const recommendationsSection = document.getElementById('recommendationsSection');
            if (recommendationsSection) {
                recommendationsSection.classList.add('hidden');
            }
            
            // Mostrar catálogo normal
            const productGrid = document.getElementById('productGrid');
            if (productGrid) {
                productGrid.classList.remove('hidden');
            }
            
            // Llamar a la función original
            originalShowCatalog(type);
        };

        // ===== AGREGAR EVENT LISTENER PARA LOS BOTONES DE RECOMENDACIONES =====
        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('add-to-cart-btn') || 
                e.target.classList.contains('view-details-btn') || 
                e.target.classList.contains('favorite-btn')) {
                
                const target = e.target;
                const productId = target.dataset.id;
                const product = allProducts.find(p => p.id === productId);
                
                if (!product) return;
                
                if (target.classList.contains('add-to-cart-btn')) {
                    const existingItem = cart.find(item => item.id === productId);
                    if (existingItem) {
                        existingItem.quantity++;
                    } else {
                        cart.push({ ...product, quantity: 1 });
                    }
                    updateCartDisplay();
                    showNotification(`¡Éxito! "${product.name}" ha sido añadido a tu carrito. ¡Que lo "disfrutes"!`);
                }
                else if (target.classList.contains('view-details-btn')) {
                    // Tu código existente para mostrar detalles
                    detailsTitle.textContent = product.name;
                    detailsDescription.textContent = product.description;
                    detailsImage.src = product.image;

                    if (product.oldPrice) {
                        detailsOldPrice.textContent = `$${product.oldPrice.toFixed(2)}`;
                        detailsOldPrice.style.display = 'block';
                    } else {
                        detailsOldPrice.style.display = 'none';
                    }
                    detailsPrice.textContent = `$${product.price.toFixed(2)}`;

                    detailsRating.innerHTML = '';
                    for (let i = 0; i < 5; i++) {
                        const star = document.createElement('span');
                        star.className = 'star';
                        star.innerHTML = i < product.rating ? '&#9733;' : '&#9734;';
                        detailsRating.appendChild(star);
                    }

                    resetReviewForm();
                    currentReviewProductId = product.id;
                    currentReviewProductName = product.name;
                    displayReviews(product);

                    addToCartDetailsButton.dataset.id = product.id;
                    addToCartDetailsButton.dataset.name = product.name;
                    addToCartDetailsButton.dataset.price = product.price;

                    addToFavoritesDetailsButton.dataset.id = product.id;
                    addToFavoritesDetailsButton.dataset.name = product.name;
                    addToFavoritesDetailsButton.dataset.type = product.type;

                    detailsModal.classList.remove('hidden');
                    setTimeout(() => {
                        detailsModalContent.classList.remove('scale-95', 'opacity-0');
                        detailsModalContent.classList.add('scale-100', 'opacity-100');
                    }, 50);
                }
                else if (target.classList.contains('favorite-btn')) {
                    const productType = target.dataset.type;
                    const currentUser = userManager.getCurrentUser();
                    
                    if (currentUser) {
                        const success = userManager.addToFavorites(currentUser.id, product, productType);
                        if (success) {
                            showNotification(`"${product.name}" agregado a favoritos! 🎉`);
                        } else {
                            showNotification(`"${product.name}" ya está en tus favoritos ⭐`);
                        }
                    } else {
                        showNotification('⚠️ Inicia sesión para agregar productos a favoritos', 'error');
                        setTimeout(() => {
                            window.location.href = 'auth.html';
                        }, 2000);
                    }
                }
            }
        });

        // SISTEMA DE RESEÑAS
        writeReviewButton.addEventListener('click', () => {
            const currentUser = userManager.getCurrentUser();
            if (!currentUser) {
                alert('⚠️ Inicia sesión para escribir una reseña');
                window.location.href = 'auth.html';
                return;
            }
            reviewFormContainer.classList.remove('hidden');
            writeReviewButton.classList.add('hidden');
        });

        reviewStars.forEach(star => {
            star.addEventListener('click', (e) => {
                const rating = parseInt(e.target.dataset.rating);
                currentReviewRating = rating;
                selectedRating.textContent = `Calificación: ${rating}/5 estrellas`;
                
                reviewStars.forEach((s, index) => {
                    if (index < rating) {
                        s.textContent = '⭐';
                        s.classList.add('text-yellow-400');
                    } else {
                        s.textContent = '☆';
                        s.classList.remove('text-yellow-400');
                    }
                });
            });
        });

        cancelReviewButton.addEventListener('click', resetReviewForm);

        submitReviewButton.addEventListener('click', () => {
            const currentUser = userManager.getCurrentUser();
            
            if (!currentUser) {
                alert('⚠️ Debes iniciar sesión para escribir reseñas');
                return;
            }
            
            if (currentReviewRating === 0) {
                alert('⚠️ Por favor selecciona una calificación');
                return;
            }
            
            if (!reviewText.value.trim()) {
                alert('⚠️ Por favor escribe tu reseña');
                return;
            }
            
            const result = userManager.addReview(
                currentUser.id,
                currentReviewProductId,
                currentReviewProductName,
                currentReviewRating,
                reviewText.value.trim()
            );
            
            if (result.success) {
                alert('✅ ¡Tu reseña ha sido publicada!');
                resetReviewForm();
                
                const product = allProducts.find(p => p.id === currentReviewProductId);
                if (product) {
                    displayReviews(product);
                }
            } else {
                alert('❌ Error al publicar la reseña');
            }
        });

        // DATOS DE PRODUCTOS
        const allProducts = [
            { 
                id: "1", 
                name: "Silent Hill 2 Remake", 
                description: "James Sunderland, quien recibe una carta de su difunta esposa, Mary, pidiéndole que la encuentre en su lugar especial en Silent Hill.", 
                price: 59.99, 
                oldPrice: 69.99,
                image: "https://cdn.hobbyconsolas.com/sites/navi.axelspringer.es/public/media/image/2022/10/silent-hill-2-remake-2848323.jpg?tf=3840x", 
                type: "game",
                genre: "Horror",
                artist: null,
                rating: 5,
                reviews: [
                    { user: "GamerPro77", text: "¡Increíble! Los gráficos son de otro mundo y la historia te atrapa desde el primer minuto. ¡Totalmente recomendado!" },
                    { user: "Silencioso", text: "Brillante, absolutamente brillante. Me ha encantado, yo no jugue a los silent hill originales pero gracias a este remake he podido vivir lo que muchos decian que era una obra maestra y vaya que si lo es." }
                ]
            },
            { 
                id: "2", 
                name: "private music - Deftones", 
                description: "Private Music es el décimo álbum de estudio de la banda estadounidense de metal alternativo Deftones.", 
                price: 12.50, 
                oldPrice: 15.00,
                image: "https://static.stereogum.com/uploads/2025/07/Deftones-1752104528-1000x1000.jpg", 
                type: "music",
                genre: "Alternative Metal", 
                artist: "Deftones",
                rating: 4,
                reviews: [
                    { user: "Lloron67", text: "Private Music de Deftones es una experiencia auditiva sublime. La fusión de sonidos atmosféricos con los característicos riffs pesados de la banda crea un viaje emocional intenso. Tracks como 'Ethereal Dreams' y 'Midnight Transmission' muestran una evolución madura de su sonido, manteniendo la esencia que amamos pero explorando territorios más experimentales. La producción es impecable, cada detalle está cuidadosamente elaborado. Un disco que crece con cada escucha." },
                    { user: "MelodyLover", text: "Este álbum demuestra por qué Deftones sigue siendo relevante después de todos estos años. Private Music logra el equilibrio perfecto entre agresividad y melodía, con letras introspectivas que resuenan profundamente. La voz de Chino Moreno oscila entre susurros hipnóticos y gritos catárticos, especialmente en 'Silent Screams'. La base rítmica de Abe Cunningham y Sergio Vega es sólida como siempre. Un trabajo coherente que confirma su lugar en el altar del alternative metal." }
                ]
            },
            { 
                id: "3", 
                name: "METAL GEAR SOLID Δ: SNAKE EATER", 
                description: "Ambientada en la Guerra Fría en 1964, donde el agente de élite Naked Snake debe infiltrarse en la jungla soviética para detener una amenaza nuclear.", 
                price: 39.99, 
                oldPrice: null,
                image: "https://arabgamerz.com/wp-content/uploads/2025/08/Metal-Gear-Solid-Delta-Snake-Eater-Review.webp", 
                type: "game",
                genre: "Action",
                artist: null,
                rating: 5,
                reviews: [
                    { user: "S0L0MlLL0", text: "Este juego es increíble, en su momento fue una obra maestra y ahora, lo sigue siendo" },
                    { user: "NecesitoVida", text: "Juegazo… me encanta:)) fiel al original. Espero que saquen otros remakes de la saga." }
                ]
            },
            { 
                id: "4", 
                name: "In Rainbows - Radiohead", 
                description: "In Rainbows es el séptimo álbum de estudio de la banda británica de rock alternativo Radiohead.", 
                price: 18.00, 
                oldPrice: 20.00,
                image: "https://m.media-amazon.com/images/I/A1MwaIeBpwL._UF1000,1000_QL80_.jpg", 
                type: "music",
                genre: "Alternative Rock",
                artist: "Radiohead",
                rating: 5,
                reviews: [
                    { user: "JoseJoseLover", text: "Este álbum demuestra la madurez creativa de la banda. Desde los beats hipnóticos de '15 Step' hasta la melancolía sublime de 'Videotape', In Rainbows fluye con una coherencia impresionante. Yorke explora vulnerabilidad y conexión humana con una profundidad conmovedora. Un disco que mejora con cada escucha." },
                    { user: "Bufarracas_Tristes", text: "Este álbum demuestra la madurez creativa de la banda. Desde los beats hipnóticos de '15 Step' hasta la melancolía sublime de 'Videotape', In Rainbows fluye con una coherencia impresionante. Yorke explora vulnerabilidad y conexión humana con una profundidad conmovedora. Un disco que mejora con cada escucha." }
                ]
            },
            { 
                id: "5", 
                name: "Persona 4 Golden", 
                description: "El protagonista y sus amigos deben investigar los crímenes, fortalecer sus vínculos sociales y combatir a sus sombras interiores para desvelar la verdad.", 
                price: 49.99, 
                oldPrice: null,
                image: "https://media.vandal.net/t200/121003/persona-4-golden-202312518264012_1.jpg", 
                type: "game",
                genre: "RPG",
                artist: null,
                rating: 4,
                reviews: [
                    { user: "Aguaruto", text: "TE AMO PERSONA 4 GOLDEN. Personajes que se sienten reales por lo bien que te llevas con ellos, de verdad los quieres y formas parte del grupo de amigos, la relación con tu familia, Soundtrack, historia" },
                    { user: "LaCabraLoca", text: "Otro post-depresion después de terminar este Persona 4 Golden, me ha encantado, sobre todo sus personajes, 100% recomendado." }
                ]
            },
            { 
                id: "6", 
                name: "D>E>A>T>H>M>E>T>A>L - Panchiko", 
                description: "D>E>A>T>H>M>E>T>A>L es el EP debut de la banda británica de indie rock Panchiko. Fue grabado y autoeditado por la banda entre 1999 y 2000", 
                rating: 5,
                price: 15.00, 
                genre: "Indie Rock",
                artist: "Panchiko",
                oldPrice: null,
                image: "https://i.scdn.co/image/ab67616d0000b273e045aa197ada995407bf92fc", 
                type: "music",
                reviews: [
                    { user: "TengoHambre", text: "D>E>A>T>H>M>E>T>A>L es una joya oculta redescubierta. Panchiko mezcla dream pop con toques de shoegaze y electrónica lo-fi de forma ingeniosa. Canciones como 'Laputa' y 'Strawberry Genes' tienen una melancolía nostálgica que te atrapa. La calidad de grabación imperfecta le añade carácter y autenticidad al disco." },
                    { user: "PechurinaConPapa", text: "Este álbum se ha convertido en un fenómeno de culto por una razón. La fusión de letras oscuras con melodías etéreas crea una atmósfera única. Aunque técnicamente no es metal, el título irónico encapsula perfectamente su estética de los 2000. Una obra que demuestra cómo lo accidental puede convertirse en arte puro" }
                ]
            },
   {
        id: "7",
        name: "Resident evil 4 Remake",
        description: "Leon S. Kennedy es enviado a una aislada aldea europea para rescatar a Ashley Graham, la hija del presidente de los Estados Unidos. ",
        price: 59.99,
        oldPrice: 69.99,
        image: "https://i.3djuegos.com/juegos/18541/resident_evil_4_remake/fotos/ficha/resident_evil_4_remake-5789986.jpg",
        type: "game",
        rating: 5,
        reviews: [
            { user: "Bumblebee312", text: "Juegazo de los god, recrea cosas del re4 original, y ademas aumentaron las mecánicas, lo recomiendo si quieres pasar horas de estres y entretenimiento." },
            { user: "Jhon107", text: "Brillante, absolutamente brillante. Me ha encantado, yo no jugue a los silent hill originales pero gracias a este remake he podido vivir lo que muchos decian que era una obra maestra y vaya que si lo es." }
        ]
    },
  {
    id: "8",
    name: "La nube en el jardin - Ed Maverick",
    description: "Es un álbum conceptual que se presenta como una única obra sonora, explorando el duelo y la nostalgia tras una ruptura con un estilo nostálgico y reflexivo.",
    price: 12.50,
    oldPrice: 15.00,
    image: "https://akamai.sscdn.co/uploadfile/letras/albuns/6/4/c/0/2434541731329949.jpg",
    type: "music",
    rating: 4,
    reviews: [
      { user: "David", text: "Se siente como estar solo en tu cuarto de madrugada. Es un disco tranquilo, con un aire nostálgico que te atrapa sin darte cuenta. Me gustó porque parece que Ed canta desde un lugar muy honesto." },
      { user: "Jose_Angel", text: "El álbum tiene un sonido muy relajado, casi como un suspiro. No es para todos los momentos, pero cuando lo escuchas con calma transmite paz y melancolía. Sientes que más que un disco, es una conversación íntima con Ed." }
    ]
  },
  {
    id: "9",
    name: "Final Fantasy VII Rebirth",
    description: "Continúa la aventura de Cloud Strife y sus compañeros de AVALANCHE tras escapar de Midgar, emprendiendo un viaje por el mundo para encontrar al legendario Sephiroth.",
    price: 39.99,
    oldPrice: null,
    image: "https://upload.wikimedia.org/wikipedia/en/7/75/Boxart_for_Final_Fantasy_VII_Rebirth.png",
    type: "game",
    rating: 5,
    reviews: [
      { user: "Victambo_Cerdivil", text: "Una gozada de remake aunque casi por momentos es una historia alternativa, no puedo decir mas que si te gusta final fantasy este tienes que jugarlo" },
      { user: "OmarSSJ333", text: "Top 5 mejores juegos que he jugado. De las cosas que más deseo en esta vida es poder vivir para disfrutar de la 3 parte." }
    ]
  },
  {
    id: "10",
    name: "I´M PART - Easykid",
    description: "I'm Part nace de una búsqueda personal a que Easykid se ha enfrentado en los últimos años, la cual se centra en sentirse parte de algo auténtico que se aleje de las reglas estipuladas por la industria musical.",
    price: 18.00,
    oldPrice: 20.00,
    image: "https://i.scdn.co/image/ab67616d0000b27304e2de01118e868ade8e64d1",
    type: "music",
    rating: 5,
    reviews: [
      { user: "PeterCico", text: "Escuchar IM PART fue como entrar a un viaje nocturno: beats electrónicos, voces procesadas y una atmósfera que mezcla melancolía con movimiento. Es un disco que suena moderno y muy personal." },
      { user: "Wickidist", text: "El álbum me atrapó porque se siente vulnerable y a la vez lleno de estilo. Easykid logra que cada canción tenga un aire íntimo, como si contara partes de su vida en clave musical." }
    ]
  },
  {
    id: "11",
    name: "Devil May Cry 4",
    description: "El joven cazador de demonios Nero es testigo de cómo Dante, el protagonista de la saga, asesina al líder de la Orden de la Espada, una organización religiosa que venera al padre de Dante, Sparda.",
    price: 49.99,
    oldPrice: null,
    image: "https://imgproxy.eneba.games/aK305nlPMVT2y-0XcrWZFFPI9P6qF5yOTB0k5RSsuGM/rs:fit:350/ar:1/czM6Ly9wcm9kdWN0/cy5lbmViYS5nYW1l/cy9wcm9kdWN0cy84/TGpDZUoyWExlZ0hP/SWhBLS1zNnB0WkUx/MGtwVUlNUHI0UXdr/bWlaV2VjLmpwZw",
    type: "game",
    rating: 4,
    reviews: [
      { user: "Pol543", text: "Porque buscas una reseña mala? Compralo y disfrutaaaaaa" },
      { user: "LanaRocks", text: "El mejor devil may cry a mi parecer." }
    ]
  },
  {
    id: "12",
    name: "❤ - Devon Hendryx",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 15.00,
    oldPrice: null,
    image: "https://m.media-amazon.com/images/I/51PkFPS9U4L._SX354_SY354_BL0_QL100__UXNaN_FMjpg_QL85_.jpg",
    type: "music",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
  {
    id: "13",
    name: "Gears of War 2",
    description: "El joven cazador de demonios Nero es testigo de cómo Dante, el protagonista de la saga, asesina al líder de la Orden de la Espada, una organización religiosa que venera al padre de Dante, Sparda.",
    price: 49.99,
    oldPrice: null,
    image: "https://cdn.gearsofwar.com/gearsofwar/sites/9/2020/02/gears-of-war-2-2000x2000-5e5574b29a4d0.jpg",
    type: "game",
    rating: 4,
    reviews: [
      { user: "Pol543", text: "Porque buscas una reseña mala? Compralo y disfrutaaaaaa" },
      { user: "LanaRocks", text: "El mejor devil may cry a mi parecer." }
    ]
  },
  {
    id: "14",
    name: "Is This It - The Strokes",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 35.00,
    oldPrice: null,
    image: "https://i.scdn.co/image/ab67616d0000b27313f2466b83507515291acce4",
    type: "music",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
  {
    id: "15",
    name: "Dead Space Reamke",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 55.00,
    oldPrice: null,
    image: "https://static.wikia.nocookie.net/deadspace/images/1/1d/Deadspaceremake_poster1.jpg/revision/latest?cb=20221003152557",
    type: "game",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
  {
    id: "16",
    name: "Cigarettes After Sex - Cigarettes After Sex ",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 25.00,
    oldPrice: null,
    image: "https://cdn-images.dzcdn.net/images/cover/2db20377876da16feb8ec9652e835a81/0x1900-000000-80-0-0.jpg",
    type: "music",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
    {
    id: "17",
    name: "Bioshock ",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 45.00,
    oldPrice: null,
    image: "https://upload.wikimedia.org/wikipedia/en/6/6d/BioShock_cover.jpg",
    type: "game",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
    {
    id: "18",
    name: "Souvlaki - Slowdive ",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 35.00,
    oldPrice: null,
    image: "https://i.scdn.co/image/ab67616d0000b27353da07db25e2543cce38abfe",
    type: "music",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
  {
    id: "19",
    name: "Metal Gear Solid V: The Phantom Pain",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 25.00,
    oldPrice: null,
    image: "https://store-images.s-microsoft.com/image/apps.24111.65806558541457305.a0ff0982-eced-4bfd-bb78-5ba7a73376c4.8173048f-456f-4928-bfcc-268f5002a41c",
    type: "game",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
  {
    id: "20",
    name: "LSD And The Search For God - LSD And The Search For God ",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 15.00,
    oldPrice: null,
    image: "data:image/jpeg;base64,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",
    type: "music",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
{
    id: "21",
    name: "KINGDOM HEARTS HD 1.5 + 2.5 ReMIX",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 45.00,
    oldPrice: null,
    image: "https://cdn1.epicgames.com/4158b699dd70447a981fee752d970a3e/offer/EGS_KINGDOMHEARTSHD1525ReMIX_SquareEnix_S6-1200x1600-132fc1f63bf40a41cddbff3bab7acc52.jpg",
    type: "game",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
  {
    id: "22",
    name: "Slipknot - Slipknot",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 35.00,
    oldPrice: null,
    image: "data:image/jpeg;base64,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",
    type: "music",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
  {
    id: "23",
    name: "Resident Evil 2 Remake",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 45.00,
    oldPrice: null,
    image: "https://image.api.playstation.com/vulcan/ap/rnd/202206/0608/9cJzlUE7sOpwvrCmOzxubniq.jpg",
    type: "game",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },
  {
    id: "24",
    name: "American Football - American Football",
    description: "Es una obra cruda, caótica y profundamente personal. Mezcla rap alternativo, ruidos industriales y pasajes melódicos con una producción lo-fi que potencia su carácter visceral.",
    price: 15.00,
    oldPrice: null,
    image: "https://m.media-amazon.com/images/I/71C2ZkbB0nL._UF894,1000_QL80_.jpg",
    type: "music",
    rating: 5,
    reviews: [
      { user: "TengoHambre", text: "El álbum suena caótico y lleno de ideas raras, pero justo eso me gustó. Se siente muy crudo, como si Devon estuviera desahogándose sin filtros, mezclando rap con sonidos extraños que no esperas." },
      { user: "PechurinaConPapa", text: "Escuchar ❤ fue raro al principio, pero poco a poco entendí la vibra: es intenso, personal y hasta incómodo a veces. No es un disco fácil, pero si entras en su mundo te deja una experiencia única." }
    ]
  },

        ];

        // INICIALIZACIÓN
        document.addEventListener('DOMContentLoaded', () => {
            debugPWA();
            updateUserInterface();
            showCatalog('all');
            updateCartDisplay();
            
            const currentUser = userManager.getCurrentUser();
            if (currentUser) {
                console.log('Usuario logueado:', currentUser.name);
            }
        });
    </script>
</body>
</html>
