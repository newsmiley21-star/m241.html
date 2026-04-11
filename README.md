<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>echoppe241 | Votre boutique de confiance</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --brand-blue: #0056b3;
            --brand-orange: #ff8c00;
        }
        .bg-brand-blue { background-color: var(--brand-blue); }
        .text-brand-blue { color: var(--brand-blue); }
        .bg-brand-orange { background-color: var(--brand-orange); }
        .text-brand-orange { color: var(--brand-orange); }
        .border-brand-orange { border-color: var(--brand-orange); }
        
        body {
            font-family: 'Inter', sans-serif;
            scroll-behavior: smooth;
        }

        .product-card:hover {
            transform: translateY(-5px);
            transition: all 0.3s ease;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }

        .nav-link {
            position: relative;
        }
        .nav-link::after {
            content: '';
            position: absolute;
            width: 0;
            height: 2px;
            bottom: -2px;
            left: 0;
            background-color: var(--brand-orange);
            transition: width 0.3s ease;
        }
        .nav-link:hover::after {
            width: 100%;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">

    <!-- Navigation -->
    <nav class="bg-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-20">
                <div class="flex items-center">
                    <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="echoppe241 Logo" class="h-12 w-auto">
                </div>
                
                <!-- Desktop Menu -->
                <div class="hidden md:flex items-center space-x-8">
                    <a href="#" class="nav-link font-medium hover:text-brand-blue">Accueil</a>
                    <a href="#categories" class="nav-link font-medium hover:text-brand-blue">Catégories</a>
                    <a href="#promotions" class="nav-link font-medium hover:text-brand-blue">Promotions</a>
                    <a href="#contact" class="nav-link font-medium hover:text-brand-blue">Contact</a>
                </div>

                <div class="flex items-center space-x-5">
                    <button class="text-gray-600 hover:text-brand-orange transition">
                        <i class="fa-solid fa-magnifying-glass text-xl"></i>
                    </button>
                    <button class="text-gray-600 hover:text-brand-orange transition relative">
                        <i class="fa-solid fa-cart-shopping text-xl"></i>
                        <span class="absolute -top-2 -right-2 bg-brand-orange text-white text-xs rounded-full h-5 w-5 flex items-center justify-center">0</span>
                    </button>
                    <button class="md:hidden text-gray-600" id="mobile-menu-button">
                        <i class="fa-solid fa-bars text-xl"></i>
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <header class="relative bg-brand-blue overflow-hidden">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-20 md:py-32 flex flex-col md:flex-row items-center">
            <div class="md:w-1/2 text-white z-10">
                <h1 class="text-4xl md:text-6xl font-extrabold leading-tight mb-6">
                    La qualité au meilleur prix avec <span class="text-brand-orange">echoppe241</span>
                </h1>
                <p class="text-lg md:text-xl mb-8 text-blue-100">
                    Découvrez notre sélection exclusive de produits importés et locaux, livrés directement chez vous en un clic.
                </p>
                <div class="flex flex-col sm:flex-row space-y-4 sm:space-y-0 sm:space-x-4">
                    <a href="#boutique" class="bg-brand-orange hover:bg-orange-600 text-white px-8 py-4 rounded-lg font-bold text-center transition shadow-lg">
                        Acheter maintenant
                    </a>
                    <a href="#" class="border-2 border-white hover:bg-white hover:text-brand-blue text-white px-8 py-4 rounded-lg font-bold text-center transition">
                        Nos collections
                    </a>
                </div>
            </div>
            <div class="md:w-1/2 mt-12 md:mt-0 flex justify-center">
                <div class="relative">
                    <div class="absolute inset-0 bg-brand-orange rounded-full blur-3xl opacity-20"></div>
                    <!-- Illustration Placeholder -->
                    <div class="relative bg-white/10 p-4 rounded-2xl backdrop-blur-sm border border-white/20">
                        <img src="https://images.unsplash.com/photo-1607082348824-0a96f2a4b9da?auto=format&fit=crop&w=800&q=80" alt="Shopping" class="rounded-xl shadow-2xl w-full max-w-md">
                    </div>
                </div>
            </div>
        </div>
    </header>

    <!-- Features -->
    <section class="py-12 bg-white border-b">
        <div class="max-w-7xl mx-auto px-4 grid grid-cols-1 md:grid-cols-3 gap-8">
            <div class="flex items-center space-x-4 p-6 rounded-xl hover:bg-gray-50 transition">
                <div class="bg-blue-100 p-4 rounded-full text-brand-blue">
                    <i class="fa-solid fa-truck-fast text-2xl"></i>
                </div>
                <div>
                    <h3 class="font-bold text-lg">Livraison Rapide</h3>
                    <p class="text-sm text-gray-500">Partout au Gabon sous 24h/48h.</p>
                </div>
            </div>
            <div class="flex items-center space-x-4 p-6 rounded-xl hover:bg-gray-50 transition">
                <div class="bg-orange-100 p-4 rounded-full text-brand-orange">
                    <i class="fa-solid fa-shield-halved text-2xl"></i>
                </div>
                <div>
                    <h3 class="font-bold text-lg">Paiement Sécurisé</h3>
                    <p class="text-sm text-gray-500">Airtel Money, Moov Money ou Cash.</p>
                </div>
            </div>
            <div class="flex items-center space-x-4 p-6 rounded-xl hover:bg-gray-50 transition">
                <div class="bg-green-100 p-4 rounded-full text-green-600">
                    <i class="fa-solid fa-headset text-2xl"></i>
                </div>
                <div>
                    <h3 class="font-bold text-lg">Support Client</h3>
                    <p class="text-sm text-gray-500">Une équipe à votre écoute 7j/7.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Popular Products -->
    <section id="boutique" class="py-20 bg-gray-50">
        <div class="max-w-7xl mx-auto px-4">
            <div class="flex justify-between items-end mb-12">
                <div>
                    <h2 class="text-3xl font-bold text-gray-900">Articles en vedette</h2>
                    <p class="text-gray-600 mt-2">Découvrez ce que nos clients adorent en ce moment.</p>
                </div>
                <a href="#" class="text-brand-blue font-bold hover:underline">Voir tout →</a>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
                <!-- Product 1 -->
                <div class="product-card bg-white rounded-2xl overflow-hidden border border-gray-100">
                    <div class="relative h-64 bg-gray-200">
                        <img src="https://images.unsplash.com/photo-1523275335684-37898b6baf30?auto=format&fit=crop&w=500&q=80" alt="Montre" class="w-full h-full object-cover">
                        <span class="absolute top-4 left-4 bg-brand-orange text-white text-xs font-bold px-3 py-1 rounded-full">-15%</span>
                    </div>
                    <div class="p-6">
                        <p class="text-xs text-brand-blue font-semibold uppercase tracking-wider mb-2">Électronique</p>
                        <h3 class="font-bold text-gray-900 mb-2">Smart Watch Pro Series</h3>
                        <div class="flex items-center mb-4">
                            <span class="text-brand-orange font-bold text-xl">45.000 FCFA</span>
                            <span class="text-gray-400 line-through ml-3 text-sm">52.000 FCFA</span>
                        </div>
                        <button class="w-full bg-gray-100 hover:bg-brand-blue hover:text-white text-gray-900 py-3 rounded-lg font-bold transition flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-cart-plus"></i>
                            <span>Ajouter</span>
                        </button>
                    </div>
                </div>

                <!-- Product 2 -->
                <div class="product-card bg-white rounded-2xl overflow-hidden border border-gray-100">
                    <div class="relative h-64 bg-gray-200">
                        <img src="https://images.unsplash.com/photo-1542291026-7eec264c27ff?auto=format&fit=crop&w=500&q=80" alt="Chaussures" class="w-full h-full object-cover">
                    </div>
                    <div class="p-6">
                        <p class="text-xs text-brand-blue font-semibold uppercase tracking-wider mb-2">Mode</p>
                        <h3 class="font-bold text-gray-900 mb-2">Baskets Sport Max Air</h3>
                        <div class="flex items-center mb-4">
                            <span class="text-brand-orange font-bold text-xl">32.000 FCFA</span>
                        </div>
                        <button class="w-full bg-gray-100 hover:bg-brand-blue hover:text-white text-gray-900 py-3 rounded-lg font-bold transition flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-cart-plus"></i>
                            <span>Ajouter</span>
                        </button>
                    </div>
                </div>

                <!-- Product 3 -->
                <div class="product-card bg-white rounded-2xl overflow-hidden border border-gray-100">
                    <div class="relative h-64 bg-gray-200">
                        <img src="https://images.unsplash.com/photo-1505740420928-5e560c06d30e?auto=format&fit=crop&w=500&q=80" alt="Audio" class="w-full h-full object-cover">
                    </div>
                    <div class="p-6">
                        <p class="text-xs text-brand-blue font-semibold uppercase tracking-wider mb-2">Accessoires</p>
                        <h3 class="font-bold text-gray-900 mb-2">Casque Audio Premium</h3>
                        <div class="flex items-center mb-4">
                            <span class="text-brand-orange font-bold text-xl">18.500 FCFA</span>
                        </div>
                        <button class="w-full bg-gray-100 hover:bg-brand-blue hover:text-white text-gray-900 py-3 rounded-lg font-bold transition flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-cart-plus"></i>
                            <span>Ajouter</span>
                        </button>
                    </div>
                </div>

                <!-- Product 4 -->
                <div class="product-card bg-white rounded-2xl overflow-hidden border border-gray-100">
                    <div class="relative h-64 bg-gray-200">
                        <img src="https://images.unsplash.com/photo-1526170315870-ef6d82f583ad?auto=format&fit=crop&w=500&q=80" alt="Appareil Photo" class="w-full h-full object-cover">
                        <span class="absolute top-4 left-4 bg-red-500 text-white text-xs font-bold px-3 py-1 rounded-full">RUPTURE</span>
                    </div>
                    <div class="p-6">
                        <p class="text-xs text-brand-blue font-semibold uppercase tracking-wider mb-2">Électronique</p>
                        <h3 class="font-bold text-gray-900 mb-2">Appareil Photo Vintage</h3>
                        <div class="flex items-center mb-4">
                            <span class="text-brand-orange font-bold text-xl">125.000 FCFA</span>
                        </div>
                        <button disabled class="w-full bg-gray-100 text-gray-400 py-3 rounded-lg font-bold cursor-not-allowed">
                            Indisponible
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-900 text-white pt-16 pb-8">
        <div class="max-w-7xl mx-auto px-4">
            <div class="grid grid-cols-1 md:grid-cols-4 gap-12 mb-12 border-b border-gray-800 pb-12">
                <div class="col-span-1 md:col-span-1">
                    <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="echoppe241 Logo" class="h-12 w-auto mb-6 brightness-0 invert">
                    <p class="text-gray-400 text-sm leading-relaxed">
                        echoppe241 est votre destination numéro 1 au Gabon pour des produits authentiques et une expérience de shopping inégalée.
                    </p>
                    <div class="flex space-x-4 mt-6">
                        <a href="#" class="h-10 w-10 bg-gray-800 rounded-full flex items-center justify-center hover:bg-brand-orange transition"><i class="fa-brands fa-facebook-f"></i></a>
                        <a href="#" class="h-10 w-10 bg-gray-800 rounded-full flex items-center justify-center hover:bg-brand-orange transition"><i class="fa-brands fa-instagram"></i></a>
                        <a href="#" class="h-10 w-10 bg-gray-800 rounded-full flex items-center justify-center hover:bg-brand-orange transition"><i class="fa-brands fa-whatsapp"></i></a>
                    </div>
                </div>
                
                <div>
                    <h4 class="font-bold text-lg mb-6">Liens Rapides</h4>
                    <ul class="space-y-4 text-gray-400">
                        <li><a href="#" class="hover:text-white transition">Accueil</a></li>
                        <li><a href="#" class="hover:text-white transition">Nos Produits</a></li>
                        <li><a href="#" class="hover:text-white transition">À propos de nous</a></li>
                        <li><a href="#" class="hover:text-white transition">Blog</a></li>
                    </ul>
                </div>

                <div>
                    <h4 class="font-bold text-lg mb-6">Assistance</h4>
                    <ul class="space-y-4 text-gray-400">
                        <li><a href="#" class="hover:text-white transition">Suivre une commande</a></li>
                        <li><a href="#" class="hover:text-white transition">Conditions de livraison</a></li>
                        <li><a href="#" class="hover:text-white transition">Retours & Remboursements</a></li>
                        <li><a href="#" class="hover:text-white transition">FAQ</a></li>
                    </ul>
                </div>

                <div id="contact">
                    <h4 class="font-bold text-lg mb-6">Newsletter</h4>
                    <p class="text-gray-400 text-sm mb-4">Recevez nos meilleures offres directement dans votre boîte mail.</p>
                    <form class="flex flex-col space-y-2">
                        <input type="email" placeholder="Votre email" class="bg-gray-800 border-0 rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-orange text-white">
                        <button class="bg-brand-orange hover:bg-orange-600 text-white font-bold py-3 rounded-lg transition">S'abonner</button>
                    </form>
                </div>
            </div>
            
            <div class="flex flex-col md:flex-row justify-between items-center text-sm text-gray-500">
                <p>&copy; 2024 echoppe241. Tous droits réservés.</p>
                <div class="flex space-x-6 mt-4 md:mt-0">
                    <span>Libreville, Gabon</span>
                    <a href="#" class="hover:text-white">Mentions légales</a>
                </div>
            </div>
        </div>
    </footer>

    <script>
        // Simple mobile menu toggle logic
        const mobileMenuButton = document.getElementById('mobile-menu-button');
        mobileMenuButton.addEventListener('click', () => {
            // Dans une version complète, on ajouterait ici l'ouverture d'un drawer
            console.log("Menu mobile cliqué");
        });
    </script>
</body>
</html>
