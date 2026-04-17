<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 Pro | Le Gabon Digital</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --gabon-green: #00853f; 
            --gabon-yellow: #fcd116; 
            --gabon-blue: #3a75c4; 
            --slate-900: #0f172a; 
        }
        
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background-color: #f8fafc; 
            color: var(--slate-900); 
            -webkit-tap-highlight-color: transparent; 
        }

        .glass { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); }
        .btn-action { transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1); }
        .btn-action:active { transform: scale(0.95); }
        
        .hide-scroll::-webkit-scrollbar { display: none; }
        
        .card-shadow { box-shadow: 0 10px 30px -5px rgba(0,0,0,0.05); }
        
        .input-field {
            @apply w-full bg-slate-50 border-2 border-slate-100 rounded-2xl p-4 outline-none focus:border-emerald-500 transition-all font-semibold;
        }

        .tab-active { @apply border-b-4 border-emerald-600 text-emerald-600; }

        @keyframes slideIn { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        .animate-slide { animation: slideIn 0.4s ease-out forwards; }
    </style>
</head>
<body class="overflow-x-hidden">

    <!-- Toast de Notification -->
    <div id="toast" class="fixed top-6 left-1/2 -translate-x-1/2 z-[9999] hidden animate-slide">
        <div class="bg-slate-900 text-white px-8 py-4 rounded-full shadow-2xl flex items-center gap-3">
            <div class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></div>
            <span id="toast-msg" class="text-xs font-bold uppercase tracking-widest">Message</span>
        </div>
    </div>

    <!-- Navigation -->
    <nav class="sticky top-0 z-[1000] glass border-b border-slate-100">
        <div class="max-w-7xl mx-auto px-6 h-20 flex justify-between items-center">
            <div class="flex items-center gap-2 cursor-pointer" onclick="showView('home')">
                <div class="w-10 h-10 brand-gradient rounded-xl flex items-center justify-center text-white shadow-lg">
                    <i class="fa-solid fa-store"></i>
                </div>
                <span class="font-black text-xl tracking-tighter italic">ECHOPPE<span class="text-emerald-600">241</span></span>
            </div>

            <div class="flex items-center gap-4">
                <button onclick="showView('cart')" class="relative p-3 text-slate-500 hover:text-emerald-600 transition-colors">
                    <i class="fa-solid fa-basket-shopping text-xl"></i>
                    <span id="cart-count" class="absolute top-1 right-1 bg-red-500 text-white text-[10px] font-black w-5 h-5 rounded-full flex items-center justify-center border-2 border-white">0</span>
                </button>
                <div id="nav-auth" class="flex items-center gap-3">
                    <button onclick="toggleModal('modal-auth')" class="bg-slate-900 text-white px-6 py-2.5 rounded-full text-[10px] font-black uppercase tracking-widest btn-action">Connexion</button>
                </div>
                <div id="nav-user" class="hidden flex items-center gap-3">
                    <button onclick="showView('profile')" class="flex items-center gap-2 group">
                        <img id="nav-avatar" src="https://ui-avatars.com/api/?name=User" class="w-10 h-10 rounded-full border-2 border-white shadow-md object-cover">
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-6 py-8">
        
        <!-- VUE : ACCUEIL -->
        <section id="view-home" class="view">
            <div class="mb-12 bg-emerald-900 rounded-[3rem] p-12 text-white relative overflow-hidden shadow-2xl">
                <div class="relative z-10 max-w-xl">
                    <h1 class="text-5xl font-black italic uppercase leading-none mb-4">Le Gabon à <br> votre porte.</h1>
                    <p class="text-emerald-100 font-medium opacity-80 mb-8">Découvrez les saveurs et l'artisanat de nos 9 provinces dans une plateforme sécurisée.</p>
                    <div class="flex gap-4">
                        <button onclick="scrollToMarket()" class="bg-white text-emerald-900 px-8 py-4 rounded-full font-black uppercase text-[10px] tracking-widest shadow-xl btn-action">Faire mes courses</button>
                        <button onclick="toggleModal('modal-support')" class="bg-emerald-800 text-white px-8 py-4 rounded-full font-black uppercase text-[10px] tracking-widest btn-action">Besoin d'aide ?</button>
                    </div>
                </div>
                <div class="absolute -right-20 -bottom-20 w-96 h-96 bg-emerald-800 rounded-full opacity-50 blur-3xl"></div>
            </div>

            <!-- Filtres -->
            <div class="flex gap-3 overflow-x-auto pb-8 hide-scroll" id="filter-container">
                <button onclick="filterMarket('all')" class="filter-btn active bg-white border-2 border-slate-100 px-8 py-3.5 rounded-2xl text-[10px] font-black uppercase tracking-widest transition-all shadow-sm">Tout</button>
                <!-- Provinces Injectées -->
            </div>

            <div id="market-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 xl:grid-cols-5 gap-8">
                <!-- Produits Injectés -->
            </div>
        </section>

        <!-- VUE : PROFIL -->
        <section id="view-profile" class="view hidden animate-slide">
            <div class="max-w-4xl mx-auto">
                <div class="bg-white rounded-[3rem] p-10 card-shadow border border-slate-50 mb-8">
                    <div class="flex flex-col md:flex-row items-center gap-8 mb-10">
                        <div class="relative group">
                            <img id="profile-pic" src="https://ui-avatars.com/api/?name=U" class="w-40 h-40 rounded-[2.5rem] object-cover border-4 border-slate-50 shadow-xl">
                            <label class="absolute bottom-2 right-2 w-10 h-10 bg-emerald-600 text-white rounded-xl flex items-center justify-center cursor-pointer shadow-lg hover:bg-emerald-500 transition-colors">
                                <i class="fa-solid fa-camera"></i>
                                <input type="file" id="input-avatar" class="hidden" accept="image/*">
                            </label>
                        </div>
                        <div class="text-center md:text-left flex-1">
                            <div class="flex flex-wrap items-center gap-3 justify-center md:justify-start mb-2">
                                <h2 id="profile-name" class="text-4xl font-black tracking-tighter">Utilisateur</h2>
                                <span id="profile-role-badge" class="bg-emerald-100 text-emerald-700 px-4 py-1 rounded-full text-[10px] font-black uppercase">Client</span>
                            </div>
                            <p id="profile-bio" class="text-slate-400 font-medium mb-4">Bienvenue sur votre espace Echoppe241.</p>
                            <div class="flex flex-wrap gap-4 justify-center md:justify-start">
                                <span class="flex items-center gap-2 text-[10px] font-bold text-slate-500 uppercase tracking-wider bg-slate-50 px-4 py-2 rounded-xl"><i class="fa-solid fa-phone"></i> <span id="profile-phone">-</span></span>
                                <span class="flex items-center gap-2 text-[10px] font-bold text-slate-500 uppercase tracking-wider bg-slate-50 px-4 py-2 rounded-xl"><i class="fa-solid fa-location-dot"></i> <span id="profile-location">-</span></span>
                            </div>
                        </div>
                        <button onclick="handleLogout()" class="text-red-500 font-black uppercase text-[10px] tracking-widest hover:underline px-6">Déconnexion</button>
                    </div>

                    <!-- Tabs Profil -->
                    <div class="flex border-b border-slate-100 mb-8">
                        <button onclick="switchProfileTab('settings')" id="tab-settings" class="px-8 py-4 font-black uppercase text-[10px] tracking-widest tab-active">Paramètres</button>
                        <button onclick="switchProfileTab('orders')" id="tab-orders" class="px-8 py-4 font-black uppercase text-[10px] tracking-widest text-slate-400">Mes Commandes</button>
                        <button onclick="switchProfileTab('inventory')" id="tab-inventory" class="hidden px-8 py-4 font-black uppercase text-[10px] tracking-widest text-slate-400">Ma Boutique</button>
                    </div>

                    <!-- Contenu Tabs -->
                    <div id="profile-settings" class="space-y-6">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <label class="text-[10px] font-black uppercase text-slate-400 mb-2 block ml-2">Bio / Description</label>
                                <textarea id="edit-bio" class="input-field h-32 resize-none" placeholder="Décrivez-vous..."></textarea>
                            </div>
                            <div class="space-y-4">
                                <div>
                                    <label class="text-[10px] font-black uppercase text-slate-400 mb-2 block ml-2">Téléphone</label>
                                    <input type="tel" id="edit-phone" class="input-field" placeholder="07x xx xx xx">
                                </div>
                                <div>
                                    <label class="text-[10px] font-black uppercase text-slate-400 mb-2 block ml-2">Ville</label>
                                    <input type="text" id="edit-city" class="input-field" placeholder="Libreville, Port-Gentil...">
                                </div>
                            </div>
                        </div>
                        <button onclick="saveProfile()" class="bg-emerald-600 text-white w-full py-5 rounded-[2rem] font-black uppercase text-xs tracking-widest shadow-xl btn-action">Mettre à jour mon profil</button>
                    </div>

                    <div id="profile-orders" class="hidden space-y-4">
                        <!-- Commandes Injectées -->
                    </div>

                    <div id="profile-inventory" class="hidden space-y-4">
                        <button onclick="toggleModal('modal-publish')" class="w-full border-2 border-dashed border-slate-200 p-8 rounded-[2.5rem] text-slate-400 font-black uppercase text-xs hover:border-emerald-400 hover:text-emerald-600 transition-all mb-8">
                            + Ajouter un nouveau produit
                        </button>
                        <div id="my-products-grid" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <!-- Produits du vendeur -->
                        </div>
                    </div>
                </div>

                <!-- ZONE ADMIN (Visible uniquement pour emails administratifs) -->
                <div id="admin-zone" class="hidden animate-slide">
                    <div class="bg-red-50 border-2 border-red-100 rounded-[3rem] p-10 mb-8">
                        <h3 class="text-2xl font-black text-red-900 uppercase italic mb-6">Panel Administration</h3>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                            <div class="bg-white p-6 rounded-3xl shadow-sm">
                                <p class="text-[10px] font-black text-slate-400 uppercase mb-1">Ventes Totales</p>
                                <h4 id="admin-total-sales" class="text-2xl font-black">0 FCFA</h4>
                            </div>
                            <div class="bg-white p-6 rounded-3xl shadow-sm">
                                <p class="text-[10px] font-black text-slate-400 uppercase mb-1">Utilisateurs</p>
                                <h4 id="admin-user-count" class="text-2xl font-black">0</h4>
                            </div>
                            <div class="bg-white p-6 rounded-3xl shadow-sm">
                                <p class="text-[10px] font-black text-slate-400 uppercase mb-1">Produits Actifs</p>
                                <h4 id="admin-prod-count" class="text-2xl font-black">0</h4>
                            </div>
                        </div>
                        <div id="admin-all-orders" class="space-y-4">
                            <!-- Toutes les commandes pour suivi -->
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- VUE : PANIER -->
        <section id="view-cart" class="view hidden animate-slide">
            <div class="max-w-3xl mx-auto">
                <h2 class="text-4xl font-black italic uppercase tracking-tighter mb-10">Mon Panier</h2>
                <div id="cart-items" class="space-y-6 mb-12">
                    <!-- Items Panier -->
                </div>
                <div id="cart-summary" class="hidden bg-white p-10 rounded-[3rem] card-shadow border border-slate-50">
                    <div class="flex justify-between items-center mb-8">
                        <span class="text-[10px] font-black uppercase text-slate-400">Total à payer</span>
                        <h4 id="cart-total" class="text-4xl font-black">0 FCFA</h4>
                    </div>
                    <div class="space-y-6">
                        <input type="tel" id="cart-pay-phone" class="input-field text-2xl text-center py-6" placeholder="Numéro Mobile Money">
                        <button onclick="checkoutAll()" class="bg-slate-900 text-white w-full py-6 rounded-[2rem] font-black uppercase text-xs tracking-[0.3em] shadow-2xl btn-action">Valider ma commande</button>
                    </div>
                </div>
                <div id="cart-empty" class="text-center py-20">
                    <i class="fa-solid fa-basket-shopping text-6xl text-slate-100 mb-6"></i>
                    <p class="text-slate-400 font-bold">Votre panier est tristement vide.</p>
                </div>
            </div>
        </section>
    </main>

    <!-- MODALE : AUTHENTIFICATION -->
    <div id="modal-auth" class="fixed inset-0 bg-slate-900/80 backdrop-blur-xl z-[2000] hidden flex items-center justify-center p-6">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 animate-slide">
            <div class="flex justify-between items-center mb-8">
                <h3 id="auth-title" class="text-3xl font-black italic uppercase tracking-tighter">Bienvenue</h3>
                <button onclick="toggleModal('modal-auth')" class="text-slate-400 p-2"><i class="fa-solid fa-xmark text-xl"></i></button>
            </div>

            <div id="auth-form" class="space-y-5">
                <div class="flex gap-2 p-1.5 bg-slate-50 rounded-2xl mb-4">
                    <button onclick="setAuthRole('buyer')" id="auth-role-buyer" class="flex-1 py-3 rounded-xl text-[10px] font-black uppercase transition-all bg-white shadow-sm border border-slate-100">Client</button>
                    <button onclick="setAuthRole('producer')" id="auth-role-producer" class="flex-1 py-3 rounded-xl text-[10px] font-black uppercase transition-all text-slate-400">Producteur</button>
                </div>
                <input type="text" id="auth-fullname" placeholder="Nom Complet / Nom Boutique" class="input-field">
                <input type="email" id="auth-email" placeholder="Email" class="input-field">
                <input type="password" id="auth-password" placeholder="Mot de passe" class="input-field">
                <button onclick="handleAuth()" class="bg-emerald-600 text-white w-full py-5 rounded-2xl font-black uppercase text-xs tracking-widest shadow-xl btn-action mt-4">C'est parti !</button>
                <p class="text-center text-[10px] font-bold text-slate-400 mt-4">En continuant, vous acceptez nos conditions d'utilisation.</p>
            </div>
        </div>
    </div>

    <!-- MODALE : PUBLICATION PRODUIT -->
    <div id="modal-publish" class="fixed inset-0 bg-slate-900/80 backdrop-blur-xl z-[2000] hidden flex items-center justify-center p-6">
        <div class="bg-white w-full max-w-lg rounded-[3.5rem] p-12 animate-slide overflow-y-auto max-h-[90vh]">
            <div class="flex justify-between items-center mb-10">
                <h3 class="text-3xl font-black italic uppercase tracking-tighter">Vendre un article</h3>
                <button onclick="toggleModal('modal-publish')" class="text-slate-400 p-2"><i class="fa-solid fa-xmark text-xl"></i></button>
            </div>
            <div class="space-y-5">
                <div class="relative group">
                    <div id="product-preview" class="w-full h-48 bg-slate-50 rounded-3xl border-2 border-dashed border-slate-200 flex flex-col items-center justify-center text-slate-400 gap-2 overflow-hidden">
                        <i class="fa-solid fa-cloud-arrow-up text-3xl"></i>
                        <span class="text-[10px] font-black uppercase">Photo du produit</span>
                    </div>
                    <input type="file" id="input-prod-img" class="absolute inset-0 opacity-0 cursor-pointer" accept="image/*">
                </div>
                <input type="text" id="p-name" placeholder="Nom du produit" class="input-field">
                <div class="grid grid-cols-2 gap-4">
                    <input type="number" id="p-price" placeholder="Prix (FCFA)" class="input-field">
                    <select id="p-province" class="input-field appearance-none">
                        <option value="">Province</option>
                        <option>Estuaire</option><option>Woleu-Ntem</option><option>Ogooué-Maritime</option>
                        <option>Haut-Ogooué</option><option>Ngounié</option><option>Nyanga</option>
                        <option>Ogooué-Ivindo</option><option>Ogooué-Lolo</option><option>Moyen-Ogooué</option>
                    </select>
                </div>
                <button onclick="submitProduct()" class="bg-slate-900 text-white w-full py-6 rounded-3xl font-black uppercase text-xs tracking-widest shadow-2xl btn-action mt-6">Publier l'annonce</button>
            </div>
        </div>
    </div>

    <!-- MODALE : MESSAGERIE SUPPORT -->
    <div id="modal-support" class="fixed bottom-6 right-6 z-[2000] w-[350px] hidden animate-slide">
        <div class="bg-white rounded-[2.5rem] shadow-2xl border border-slate-100 flex flex-col h-[500px]">
            <div class="brand-gradient p-6 rounded-t-[2.5rem] flex justify-between items-center text-white">
                <div>
                    <h4 class="font-black uppercase text-[10px] tracking-widest">Support Echoppe241</h4>
                    <p class="text-[9px] opacity-70">En ligne pour vous aider</p>
                </div>
                <button onclick="toggleModal('modal-support')" class="text-white opacity-60"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div id="chat-messages" class="flex-1 overflow-y-auto p-6 space-y-4 text-[11px] font-medium text-slate-600">
                <div class="bg-slate-50 p-4 rounded-2xl rounded-tl-none border border-slate-100">Bonjour ! Comment pouvons-nous vous aider aujourd'hui ? Posez vos questions sur vos commandes ici.</div>
            </div>
            <div class="p-4 border-t border-slate-50 flex gap-2">
                <input type="text" id="chat-input" placeholder="Votre message..." class="flex-1 bg-slate-50 rounded-full px-4 text-[11px] outline-none font-bold">
                <button onclick="sendMessage()" class="w-10 h-10 brand-gradient text-white rounded-full flex items-center justify-center btn-action"><i class="fa-solid fa-paper-plane text-[10px]"></i></button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, where, deleteDoc, updateDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = JSON.parse('{"apiKey": "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg", "authDomain": "communautedugabon.firebaseapp.com", "projectId": "communautedugabon", "storageBucket": "communautedugabon.firebasestorage.app", "messagingSenderId": "647862371022", "appId": "1:647862371022:web:b209bfc8eb81accb1fc69f"}');

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe-241-pro-v1';

        let userProfile = null;
        let cart = [];
        let authRole = 'buyer';
        let currentProducts = [];
        let activeFilter = 'all';
        let tempAvatar = null;
        let tempProdImg = null;

        // AUTHENTICATION
        window.setAuthRole = (role) => {
            authRole = role;
            document.getElementById('auth-role-buyer').className = role === 'buyer' ? 'flex-1 py-3 rounded-xl text-[10px] font-black uppercase transition-all bg-white shadow-sm border border-slate-100' : 'flex-1 py-3 rounded-xl text-[10px] font-black uppercase transition-all text-slate-400';
            document.getElementById('auth-role-producer').className = role === 'producer' ? 'flex-1 py-3 rounded-xl text-[10px] font-black uppercase transition-all bg-white shadow-sm border border-slate-100' : 'flex-1 py-3 rounded-xl text-[10px] font-black uppercase transition-all text-slate-400';
        };

        window.handleAuth = async () => {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-password').value;
            const name = document.getElementById('auth-fullname').value;

            try {
                // Tente de s'inscrire ou de se connecter
                let userCred;
                try {
                    userCred = await createUserWithEmailAndPassword(auth, email, pass);
                    // Création du profil Firestore
                    await setDoc(doc(db, 'artifacts', appId, 'users', userCred.user.uid), {
                        fullName: name,
                        email: email,
                        role: authRole,
                        avatar: `https://ui-avatars.com/api/?name=${encodeURIComponent(name)}&background=random`,
                        bio: "Nouveau membre d'Echoppe241",
                        phone: "",
                        city: "",
                        isAdmin: email.includes('administratif'),
                        createdAt: serverTimestamp()
                    });
                } catch (e) {
                    userCred = await signInWithEmailAndPassword(auth, email, pass);
                }
                toggleModal('modal-auth');
                showToast("Connexion réussie !");
            } catch (err) {
                showToast("Erreur : " + err.message);
            }
        };

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                const uDoc = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid));
                if (uDoc.exists()) {
                    userProfile = { uid: user.uid, ...uDoc.data() };
                    updateNav();
                    initProfileUI();
                    syncData();
                }
            } else {
                userProfile = null;
                updateNav();
            }
        });

        // DATA SYNC
        function syncData() {
            // Marché public
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                currentProducts = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderMarket();
                renderMyProducts();
                if (userProfile?.isAdmin) updateAdminStats();
            });

            // Commandes perso
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), (snap) => {
                const orders = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderOrders(orders);
                if (userProfile?.isAdmin) renderAdminOrders(orders);
            });
            
            // Messages support
            const chatQuery = query(collection(db, 'artifacts', appId, 'public', 'data', 'messages'));
            onSnapshot(chatQuery, (snap) => {
                const msgs = snap.docs.map(d => d.data()).sort((a,b) => a.at - b.at);
                renderMessages(msgs);
            });
        }

        // MARKET RENDER
        window.filterMarket = (p) => {
            activeFilter = p;
            document.querySelectorAll('.filter-btn').forEach(b => {
                b.classList.toggle('active', b.innerText.toLowerCase() === p.toLowerCase() || (p === 'all' && b.innerText === 'Tout'));
                if (b.classList.contains('active')) {
                    b.classList.replace('border-slate-100', 'border-emerald-600');
                    b.classList.add('text-emerald-600');
                } else {
                    b.classList.replace('border-emerald-600', 'border-slate-100');
                    b.classList.remove('text-emerald-600');
                }
            });
            renderMarket();
        };

        function renderMarket() {
            const grid = document.getElementById('market-grid');
            grid.innerHTML = '';
            
            const filtered = currentProducts.filter(p => activeFilter === 'all' || p.province === activeFilter);
            
            filtered.forEach(p => {
                const card = document.createElement('div');
                card.className = "group bg-white rounded-[2.5rem] p-3 card-shadow border border-transparent hover:border-emerald-500 transition-all animate-slide";
                card.innerHTML = `
                    <div class="relative aspect-[4/5] rounded-[2rem] overflow-hidden mb-4">
                        <img src="${p.image}" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-700">
                        <div class="absolute top-4 left-4 bg-white/90 backdrop-blur px-4 py-1.5 rounded-full text-[9px] font-black uppercase tracking-widest text-emerald-800 shadow-sm">${p.province}</div>
                    </div>
                    <div class="px-2 pb-2">
                        <p class="text-[9px] font-black text-slate-400 uppercase tracking-widest mb-1">${p.ownerName}</p>
                        <h4 class="font-black text-sm uppercase truncate mb-3">${p.name}</h4>
                        <div class="flex justify-between items-center">
                            <span class="text-xl font-black text-emerald-600">${p.price.toLocaleString()}<span class="text-xs ml-1">F</span></span>
                            <button onclick="addToCart('${p.id}')" class="w-10 h-10 rounded-2xl bg-slate-900 text-white flex items-center justify-center btn-action shadow-lg">
                                <i class="fa-solid fa-cart-plus text-xs"></i>
                            </button>
                        </div>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        // CART LOGIC
        window.addToCart = (id) => {
            const prod = currentProducts.find(p => p.id === id);
            cart.push(prod);
            updateCartUI();
            showToast(`${prod.name} ajouté au panier !`);
        };

        window.removeFromCart = (index) => {
            cart.splice(index, 1);
            updateCartUI();
        };

        function updateCartUI() {
            document.getElementById('cart-count').innerText = cart.length;
            const container = document.getElementById('cart-items');
            const empty = document.getElementById('cart-empty');
            const summary = document.getElementById('cart-summary');
            
            container.innerHTML = '';
            if (cart.length === 0) {
                empty.classList.remove('hidden');
                summary.classList.add('hidden');
                return;
            }
            
            empty.classList.add('hidden');
            summary.classList.remove('hidden');
            
            let total = 0;
            cart.forEach((item, idx) => {
                total += item.price;
                const div = document.createElement('div');
                div.className = "bg-white p-6 rounded-3xl flex items-center justify-between card-shadow border border-slate-50";
                div.innerHTML = `
                    <div class="flex items-center gap-6">
                        <img src="${item.image}" class="w-20 h-20 rounded-2xl object-cover shadow-lg">
                        <div>
                            <h5 class="font-black uppercase text-sm">${item.name}</h5>
                            <p class="text-emerald-600 font-black">${item.price.toLocaleString()} FCFA</p>
                        </div>
                    </div>
                    <button onclick="removeFromCart(${idx})" class="text-red-400 p-2"><i class="fa-solid fa-trash-can"></i></button>
                `;
                container.appendChild(div);
            });
            document.getElementById('cart-total').innerText = `${total.toLocaleString()} FCFA`;
        }

        window.checkoutAll = async () => {
            if (!userProfile) return toggleModal('modal-auth');
            const phone = document.getElementById('cart-pay-phone').value;
            if (!phone) return showToast("Numéro Mobile Money obligatoire");

            try {
                const total = cart.reduce((sum, item) => sum + item.price, 0);
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), {
                    buyerId: userProfile.uid,
                    buyerName: userProfile.fullName,
                    buyerPhone: phone,
                    items: cart.map(i => ({ name: i.name, sellerId: i.ownerId, price: i.price })),
                    total: total,
                    status: 'En attente',
                    at: serverTimestamp()
                });
                
                cart = [];
                updateCartUI();
                showToast("Commande envoyée à l'administration !");
                showView('home');
            } catch (e) {
                showToast("Erreur commande");
            }
        };

        // PROFILE & SETTINGS
        window.saveProfile = async () => {
            const updates = {
                bio: document.getElementById('edit-bio').value,
                phone: document.getElementById('edit-phone').value,
                city: document.getElementById('edit-city').value
            };
            if (tempAvatar) updates.avatar = tempAvatar;

            await updateDoc(doc(db, 'artifacts', appId, 'users', userProfile.uid), updates);
            showToast("Profil mis à jour !");
            location.reload();
        };

        // PRODUCT MANAGEMENT
        window.submitProduct = async () => {
            const name = document.getElementById('p-name').value;
            const price = parseInt(document.getElementById('p-price').value);
            const prov = document.getElementById('p-province').value;
            
            if (!name || !price || !prov || !tempProdImg) return showToast("Veuillez tout remplir (photo comprise)");

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                name, price, province: prov,
                image: tempProdImg,
                ownerId: userProfile.uid,
                ownerName: userProfile.fullName,
                at: serverTimestamp()
            });
            
            showToast("Produit publié !");
            toggleModal('modal-publish');
            document.getElementById('product-preview').innerHTML = '<i class="fa-solid fa-cloud-arrow-up text-3xl"></i>';
            tempProdImg = null;
        };

        window.deleteProduct = async (id) => {
            if (confirm("Supprimer cet article définitivement ?")) {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', id));
                showToast("Produit retiré.");
            }
        };

        // ADMIN PANEL
        function updateAdminStats() {
            document.getElementById('admin-total-sales').innerText = "Vérification...";
            document.getElementById('admin-user-count').innerText = "...";
            document.getElementById('admin-prod-count').innerText = currentProducts.length;
        }

        // IMAGE HANDLERS (Base64 for prototype)
        document.getElementById('input-avatar').addEventListener('change', (e) => {
            const file = e.target.files[0];
            const reader = new FileReader();
            reader.onload = (re) => {
                tempAvatar = re.target.result;
                document.getElementById('profile-pic').src = tempAvatar;
            };
            reader.readAsDataURL(file);
        });

        document.getElementById('input-prod-img').addEventListener('change', (e) => {
            const file = e.target.files[0];
            const reader = new FileReader();
            reader.onload = (re) => {
                tempProdImg = re.target.result;
                document.getElementById('product-preview').innerHTML = `<img src="${tempProdImg}" class="w-full h-full object-cover">`;
            };
            reader.readAsDataURL(file);
        });

        // UI NAVIGATION
        window.showView = (v) => {
            document.querySelectorAll('.view').forEach(view => view.classList.add('hidden'));
            document.getElementById(`view-${v}`).classList.remove('hidden');
            window.scrollTo(0,0);
        };

        window.toggleModal = (id) => document.getElementById(id).classList.toggle('hidden');

        function updateNav() {
            const isLogged = !!userProfile;
            document.getElementById('nav-auth').classList.toggle('hidden', isLogged);
            document.getElementById('nav-user').classList.toggle('hidden', !isLogged);
            if (isLogged) {
                document.getElementById('nav-avatar').src = userProfile.avatar;
                if (userProfile.isAdmin) document.getElementById('admin-zone').classList.remove('hidden');
            }
        }

        function initProfileUI() {
            document.getElementById('profile-pic').src = userProfile.avatar;
            document.getElementById('profile-name').innerText = userProfile.fullName;
            document.getElementById('profile-role-badge').innerText = userProfile.role === 'producer' ? 'Vendeur' : 'Acheteur';
            document.getElementById('profile-bio').innerText = userProfile.bio;
            document.getElementById('profile-phone').innerText = userProfile.phone || '-';
            document.getElementById('profile-location').innerText = userProfile.city || '-';
            document.getElementById('edit-bio').value = userProfile.bio;
            document.getElementById('edit-phone').value = userProfile.phone;
            document.getElementById('edit-city').value = userProfile.city;
            
            if (userProfile.role === 'producer') document.getElementById('tab-inventory').classList.remove('hidden');
        }

        window.switchProfileTab = (t) => {
            ['settings', 'orders', 'inventory'].forEach(id => {
                const el = document.getElementById(`profile-${id}`);
                const tab = document.getElementById(`tab-${id}`);
                if (el) el.classList.add('hidden');
                if (tab) tab.className = "px-8 py-4 font-black uppercase text-[10px] tracking-widest text-slate-400";
            });
            document.getElementById(`profile-${t}`).classList.remove('hidden');
            document.getElementById(`tab-${t}`).className = "px-8 py-4 font-black uppercase text-[10px] tracking-widest tab-active";
        };

        function renderMyProducts() {
            if (!userProfile || userProfile.role !== 'producer') return;
            const container = document.getElementById('my-products-grid');
            container.innerHTML = '';
            const mine = currentProducts.filter(p => p.ownerId === userProfile.uid);
            
            mine.forEach(p => {
                const div = document.createElement('div');
                div.className = "flex items-center gap-4 bg-slate-50 p-4 rounded-3xl";
                div.innerHTML = `
                    <img src="${p.image}" class="w-16 h-16 rounded-2xl object-cover">
                    <div class="flex-1">
                        <p class="font-black text-xs uppercase">${p.name}</p>
                        <p class="text-[10px] font-bold text-emerald-600">${p.price.toLocaleString()} F</p>
                    </div>
                    <button onclick="deleteProduct('${p.id}')" class="text-red-400 p-3"><i class="fa-solid fa-trash"></i></button>
                `;
                container.appendChild(div);
            });
        }

        function renderOrders(orders) {
            const container = document.getElementById('profile-orders');
            container.innerHTML = '';
            const mine = orders.filter(o => o.buyerId === userProfile?.uid);
            
            if (mine.length === 0) {
                container.innerHTML = '<p class="text-center py-10 text-slate-400 font-bold">Aucune commande passée.</p>';
                return;
            }

            mine.forEach(o => {
                const div = document.createElement('div');
                div.className = "bg-white p-6 rounded-3xl card-shadow border border-slate-50";
                div.innerHTML = `
                    <div class="flex justify-between items-center mb-4">
                        <span class="text-[9px] font-black uppercase tracking-widest text-slate-400">Commande #${o.id.slice(-5)}</span>
                        <span class="px-3 py-1 bg-amber-100 text-amber-700 rounded-full text-[8px] font-black uppercase">${o.status}</span>
                    </div>
                    <p class="font-black text-lg mb-2">${o.total.toLocaleString()} FCFA</p>
                    <p class="text-[10px] text-slate-400">${o.items.length} article(s)</p>
                `;
                container.appendChild(div);
            });
        }

        // SUPPORT CHAT
        window.sendMessage = async () => {
            const input = document.getElementById('chat-input');
            const txt = input.value.trim();
            if (!txt || !userProfile) return;

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'messages'), {
                uid: userProfile.uid,
                name: userProfile.fullName,
                text: txt,
                at: Date.now()
            });
            input.value = '';
        };

        function renderMessages(msgs) {
            const chat = document.getElementById('chat-messages');
            chat.innerHTML = '';
            msgs.forEach(m => {
                const div = document.createElement('div');
                const isMe = m.uid === userProfile?.uid;
                div.className = isMe ? 'bg-emerald-50 p-4 rounded-2xl rounded-tr-none border border-emerald-100 ml-auto max-w-[80%]' : 'bg-slate-50 p-4 rounded-2xl rounded-tl-none border border-slate-100 mr-auto max-w-[80%]';
                div.innerHTML = `<p class="text-[8px] font-black uppercase opacity-40 mb-1">${m.name}</p><p>${m.text}</p>`;
                chat.appendChild(div);
            });
            chat.scrollTop = chat.scrollHeight;
        }

        window.handleLogout = () => signOut(auth).then(() => location.reload());
        window.scrollToMarket = () => document.getElementById('market-grid').scrollIntoView({ behavior: 'smooth' });
        
        function showToast(m) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = m;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        // Init Provinces Filters
        const provinces = ["Estuaire", "Woleu-Ntem", "Ogooué-Maritime", "Haut-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Moyen-Ogooué"];
        const filterContainer = document.getElementById('filter-container');
        provinces.forEach(p => {
            const btn = document.createElement('button');
            btn.className = "filter-btn whitespace-nowrap bg-white border-2 border-slate-100 px-8 py-3.5 rounded-2xl text-[10px] font-black uppercase tracking-widest transition-all shadow-sm";
            btn.innerText = p;
            btn.onclick = () => filterMarket(p);
            filterContainer.appendChild(btn);
        });

    </script>
</body>
</html>
