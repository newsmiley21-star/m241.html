<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Business & Livraison</title>
    
    <!-- Meta tags pour l'installation mobile (PWA) -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">
    <meta name="theme-color" content="#00853f">
    
    <!-- Icônes pour navigateurs et mobile -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    
    <!-- Manifeste Web pour définir l'icône d'installation -->
    <link rel="manifest" href='data:application/json,{"name":"Echoppe241","short_name":"Echoppe241","start_url":"/","display":"standalone","background_color":"#ffffff","theme_color":"#00853f","icons":[{"src":"https://i.ibb.co/RkhcRdCs/echoppe241-logo.png","sizes":"512x512","type":"image/png"}]}'>

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { --gabon-green: #00853f; --gabon-yellow: #fcd116; --gabon-blue: #3a75c4; --deep-slate: #0f172a; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; color: var(--deep-slate); overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        .brand-gradient { background: linear-gradient(135deg, var(--gabon-green), #10b981); }
        .glass { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-bottom: 1px solid rgba(0,0,0,0.05); }
        .product-card { transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); border: 1px solid #f1f5f9; }
        .product-card:active { transform: scale(0.98); }
        .delivery-option.active { border-color: var(--gabon-green); background-color: #f0fdf4; border-width: 2px; }
        .hide-scroll::-webkit-scrollbar { display: none; }
        
        @keyframes slideIn { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        .animate-slide { animation: slideIn 0.4s ease-out forwards; }
        
        .loading-shimmer { background: linear-gradient(90deg, #f0f0f0 25%, #f8f8f8 50%, #f0f0f0 75%); background-size: 200% 100%; animation: shimmer 1.5s infinite; }
        @keyframes shimmer { 0% { background-position: -200% 0; } 100% { background-position: 200% 0; } }
    </style>
</head>
<body>

    <!-- Système de Notification (Toast) -->
    <div id="toast" class="fixed top-6 left-1/2 -translate-x-1/2 z-[3000] hidden">
        <div class="bg-slate-900 text-white px-6 py-4 rounded-2xl shadow-2xl flex items-center gap-3 border-b-2 border-amber-400 min-w-[280px]">
            <i class="fa-solid fa-circle-check text-amber-400"></i>
            <span id="toast-msg" class="text-[11px] font-extrabold uppercase tracking-wider italic">Action effectuée</span>
        </div>
    </div>

    <!-- Modale Authentification -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 animate-slide shadow-2xl">
            <div class="flex justify-between items-center mb-8">
                <h3 class="text-2xl font-black italic uppercase tracking-tighter">Accès Echoppe<span class="text-amber-500">241</span></h3>
                <button onclick="toggleAuthModal()" class="text-slate-400 p-2"><i class="fa-solid fa-xmark text-xl"></i></button>
            </div>
            
            <div class="space-y-5">
                <div>
                    <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Nom Complet ou Boutique</label>
                    <input type="text" id="auth-name" placeholder="Ex: Marché de Nzeng-Ayong" class="w-full bg-slate-50 p-4 rounded-2xl border border-slate-100 font-bold focus:ring-2 ring-emerald-500/20 focus:outline-none">
                </div>
                
                <div class="grid grid-cols-2 gap-3">
                    <button onclick="setRole('buyer')" id="role-buyer" class="role-btn active p-5 border rounded-2xl flex flex-col items-center gap-2 transition-all group">
                        <i class="fa-solid fa-basket-shopping text-xl group-[.active]:text-emerald-600"></i>
                        <span class="text-[9px] font-black uppercase">Consommateur</span>
                    </button>
                    <button onclick="setRole('producer')" id="role-producer" class="role-btn p-5 border rounded-2xl flex flex-col items-center gap-2 transition-all group">
                        <i class="fa-solid fa-wheat-awn text-xl group-[.active]:text-emerald-600"></i>
                        <span class="text-[9px] font-black uppercase">Producteur</span>
                    </button>
                </div>

                <button id="btn-auth-confirm" onclick="handleAuthSubmit()" class="w-full brand-gradient text-white py-5 rounded-2xl font-black uppercase text-[10px] tracking-widest shadow-xl mt-4 active:scale-95 transition-transform">Valider mon profil</button>
            </div>
        </div>
    </div>

    <!-- Modale Mise en vente -->
    <div id="publish-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 animate-slide overflow-y-auto max-h-[95vh] shadow-2xl">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-2xl font-black italic uppercase tracking-tighter">Publier un produit</h3>
                <button onclick="togglePublishModal()" class="text-slate-400 p-2"><i class="fa-solid fa-xmark text-xl"></i></button>
            </div>
            <div class="space-y-4">
                <input type="text" id="p-name" placeholder="Nom du produit (Ex: Atanga de Mouila)" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 font-bold italic">
                <div class="relative">
                    <input type="number" id="p-price" placeholder="Prix" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 font-bold">
                    <span class="absolute right-4 top-1/2 -translate-y-1/2 font-black text-[10px] text-slate-400">FCFA</span>
                </div>
                <select id="p-province" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 font-bold appearance-none">
                    <option value="">Sélectionner la Province</option>
                    <option>Estuaire</option><option>Haut-Ogooué</option><option>Moyen-Ogooué</option>
                    <option>Ngounié</option><option>Nyanga</option><option>Ogooué-Ivindo</option>
                    <option>Ogooué-Lolo</option><option>Ogooué-Maritime</option><option>Woleu-Ntem</option>
                </select>
                <input type="text" id="p-image" placeholder="Lien image (URL)" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 font-bold">
                <p class="text-[8px] text-slate-400 font-bold uppercase text-center">Note: Les prix incluent la commission Smile Services</p>
                <button id="btn-publish-confirm" onclick="submitProduct()" class="w-full brand-gradient text-white py-5 rounded-2xl font-black uppercase text-[10px] tracking-widest shadow-xl active:scale-95 transition-transform">Lancer la vente</button>
            </div>
        </div>
    </div>

    <!-- En-tête -->
    <header class="sticky top-0 z-[1000] glass">
        <div class="h-[4px] flex">
            <div class="flex-1 bg-[var(--gabon-green)]"></div>
            <div class="flex-1 bg-[var(--gabon-yellow)]"></div>
            <div class="flex-1 bg-[var(--gabon-blue)]"></div>
        </div>
        <div class="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Echoppe241" class="h-9 w-auto">
                <h1 class="font-extrabold text-xl tracking-tighter uppercase">Echoppe<span class="text-amber-500">241</span></h1>
            </div>
            <div class="flex items-center gap-4">
                <div id="user-profile" class="hidden flex items-center gap-3 bg-white p-1 rounded-2xl border shadow-sm">
                    <div id="user-initials" class="w-9 h-9 rounded-xl brand-gradient text-white flex items-center justify-center font-black text-xs uppercase shadow-inner">?</div>
                    <button onclick="handleLogout()" class="pr-3 pl-1 text-slate-300 hover:text-red-500 transition-colors"><i class="fa-solid fa-power-off"></i></button>
                </div>
                <button id="btn-login-trigger" onclick="toggleAuthModal()" class="bg-slate-900 text-white px-6 py-3 rounded-xl font-bold text-[10px] uppercase tracking-wider hover:bg-black transition-colors">Connexion</button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-8">
        
        <!-- DASHBOARD / ACCUEIL -->
        <div id="view-home">
            <!-- Vue Producteur -->
            <section id="view-producer" class="hidden mb-12 space-y-6 animate-slide">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
                    <div class="bg-white p-7 rounded-[2.5rem] border border-slate-100 shadow-sm flex items-center gap-5">
                        <div class="w-14 h-14 rounded-2xl bg-emerald-50 text-emerald-600 flex items-center justify-center text-xl shadow-inner"><i class="fa-solid fa-sack-dollar"></i></div>
                        <div>
                            <p class="text-[10px] font-black uppercase text-slate-400 mb-0.5 tracking-tight">Ventes Encaissées</p>
                            <h4 id="stat-sales" class="text-2xl font-black tracking-tighter">0 FCFA</h4>
                        </div>
                    </div>
                    <div class="bg-white p-7 rounded-[2.5rem] border border-slate-100 shadow-sm flex items-center gap-5">
                        <div class="w-14 h-14 rounded-2xl bg-blue-50 text-blue-600 flex items-center justify-center text-xl shadow-inner"><i class="fa-solid fa-truck-ramp-box"></i></div>
                        <div>
                            <p class="text-[10px] font-black uppercase text-slate-400 mb-0.5 tracking-tight">Commandes Smile</p>
                            <h4 id="stat-count" class="text-2xl font-black tracking-tighter">0</h4>
                        </div>
                    </div>
                    <button onclick="togglePublishModal()" class="brand-gradient p-7 rounded-[2.5rem] text-white shadow-xl flex items-center justify-between group active:scale-95 transition-all">
                        <div class="text-left">
                            <p class="text-[10px] font-black uppercase opacity-70 mb-0.5 tracking-widest">Gérer Stock</p>
                            <h4 class="text-2xl font-black italic tracking-tighter">Nouveau Produit</h4>
                        </div>
                        <i class="fa-solid fa-circle-plus text-3xl group-hover:rotate-90 transition-transform"></i>
                    </button>
                </div>

                <div class="bg-white rounded-[2.5rem] p-7 border border-slate-100 shadow-sm">
                    <h3 class="text-[11px] font-black uppercase text-slate-400 mb-6 flex items-center gap-3">
                        <span class="w-3 h-3 rounded-full bg-amber-400 animate-pulse"></span>
                        Flux Logistique Smile Services
                    </h3>
                    <div id="delivery-tracking-list" class="grid gap-4 md:grid-cols-2 lg:grid-cols-3">
                        <!-- Commandes en attente -->
                    </div>
                </div>
            </section>

            <!-- Marché Public -->
            <section id="view-buyer">
                <div class="flex flex-col md:flex-row gap-6 mb-10 items-start md:items-center justify-between">
                    <div>
                        <h2 class="text-4xl font-black uppercase italic tracking-tighter leading-none mb-2">Terroir Gabonais</h2>
                        <div class="flex items-center gap-2">
                            <span class="w-4 h-[2px] bg-emerald-600"></span>
                            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-widest">Circuit court : Plantation ➜ Assiette</p>
                        </div>
                    </div>
                    <div class="flex gap-2 overflow-x-auto pb-3 hide-scroll w-full md:w-auto" id="province-filters">
                        <button onclick="setFilter('all')" id="filter-all" class="bg-slate-900 text-white px-6 py-4 rounded-2xl text-[10px] font-black uppercase transition-all whitespace-nowrap shadow-lg shadow-slate-900/20">Toutes Provinces</button>
                    </div>
                </div>

                <div id="market-grid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-6">
                    <!-- Shimmers (Chargement) -->
                    <div class="loading-shimmer h-72 rounded-[2.5rem]"></div>
                    <div class="loading-shimmer h-72 rounded-[2.5rem]"></div>
                    <div class="loading-shimmer h-72 rounded-[2.5rem]"></div>
                    <div class="loading-shimmer h-72 rounded-[2.5rem]"></div>
                </div>
            </section>
        </div>

        <!-- TUNNEL DE PAIEMENT MOBILE -->
        <div id="view-checkout" class="hidden max-w-2xl mx-auto py-6 animate-slide">
            <div class="bg-white rounded-[4rem] p-10 shadow-2xl border border-slate-50">
                <button onclick="navigateTo('home')" class="text-slate-400 text-[10px] font-black uppercase mb-10 flex items-center gap-3 group">
                    <i class="fa-solid fa-chevron-left group-hover:-translate-x-1 transition-transform"></i> Retour au marché
                </button>
                
                <h2 class="text-4xl font-black uppercase italic tracking-tighter mb-8 leading-none">Votre Commande</h2>
                
                <div id="checkout-item" class="bg-slate-50 p-6 rounded-[2.5rem] flex items-center gap-6 mb-10 border border-slate-100 shadow-inner"></div>

                <div class="space-y-10">
                    <div class="space-y-4">
                        <label class="text-[11px] font-black uppercase text-slate-900 ml-1">Mode de Réception (Smile)</label>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <button onclick="setDeliveryMode('pickup')" id="opt-pickup" class="delivery-option active p-6 border rounded-3xl flex items-center gap-5 transition-all">
                                <div class="w-12 h-12 rounded-2xl bg-slate-100 flex items-center justify-center text-xl text-slate-400"><i class="fa-solid fa-shop"></i></div>
                                <div class="text-left">
                                    <span class="block text-[10px] font-black uppercase">Retrait Sur Place</span>
                                    <span class="text-[9px] font-bold text-slate-400">Gratuit</span>
                                </div>
                            </button>
                            <button onclick="setDeliveryMode('delivery')" id="opt-delivery" class="delivery-option p-6 border rounded-3xl flex items-center gap-5 transition-all">
                                <div class="w-12 h-12 rounded-2xl bg-emerald-50 flex items-center justify-center text-xl text-emerald-600"><i class="fa-solid fa-moped"></i></div>
                                <div class="text-left">
                                    <span class="block text-[10px] font-black uppercase">Livraison à domicile</span>
                                    <span class="text-[9px] font-bold text-emerald-600">+1 500 FCFA</span>
                                </div>
                            </button>
                        </div>
                    </div>

                    <div id="delivery-address-box" class="hidden space-y-3">
                        <label class="text-[11px] font-black uppercase text-slate-900 ml-1">Adresse de livraison (Libreville/POG/etc.)</label>
                        <textarea id="pay-address" rows="2" placeholder="Ex: Akanda, Cité Shell, Villa 42" class="w-full bg-slate-50 p-6 rounded-3xl border border-slate-200 font-bold focus:ring-2 ring-emerald-500/20 focus:outline-none resize-none"></textarea>
                    </div>

                    <div class="p-8 border-2 border-slate-900 bg-slate-900 text-white rounded-[3rem] shadow-2xl relative overflow-hidden group">
                        <div class="absolute -right-10 -top-10 w-40 h-40 bg-emerald-500/10 rounded-full blur-3xl"></div>
                        <div class="flex justify-between items-center relative z-10">
                            <div class="space-y-1">
                                <span class="text-[11px] font-black uppercase opacity-60">Total Global</span>
                                <p class="text-[9px] uppercase tracking-[0.2em] font-bold text-emerald-400">Paiement Mobile 100% Sécurisé</p>
                            </div>
                            <span id="checkout-total" class="font-black text-3xl tracking-tighter">0 FCFA</span>
                        </div>
                    </div>

                    <div class="space-y-4">
                        <label class="text-[11px] font-black uppercase text-slate-900 ml-1">Numéro Airtel Money / Moov Money</label>
                        <input type="tel" id="pay-phone" placeholder="074 00 00 00" class="w-full bg-slate-50 p-6 rounded-3xl border border-slate-200 font-black text-2xl focus:ring-2 ring-emerald-500/20 focus:outline-none tracking-widest text-center">
                    </div>

                    <button id="btn-process-order" onclick="processOrder()" class="w-full brand-gradient text-white py-7 rounded-[2.5rem] font-black uppercase text-[12px] tracking-widest shadow-2xl active:scale-95 hover:brightness-110 transition-all flex items-center justify-center gap-4">
                        <i class="fa-solid fa-lock text-sm"></i>
                        <span>Confirmer le Paiement</span>
                    </button>
                </div>
            </div>
        </div>
    </main>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, serverTimestamp, orderBy } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe-241-prod-v1';

        let userProfile = null;
        let selectedRole = 'buyer';
        let allProducts = [];
        let activeFilter = 'all';
        let deliveryMode = 'pickup';
        let currentCheckoutItem = null;
        const DELIVERY_FEE = 1500;

        // --- AUTH & INITIALISATION ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                try {
                    const uDoc = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid));
                    userProfile = uDoc.exists() ? { uid: user.uid, ...uDoc.data() } : { uid: user.uid, role: 'buyer', fullName: 'Invité' };
                    updateUI();
                    syncCoreData();
                } catch (e) { console.error("Erreur Profil:", e); }
            } else {
                signInAnonymously(auth);
            }
        });

        window.toggleAuthModal = () => document.getElementById('auth-modal').classList.toggle('hidden');
        window.setRole = (r) => {
            selectedRole = r;
            document.querySelectorAll('.role-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(`role-${r}`).classList.add('active');
        };

        window.handleAuthSubmit = async () => {
            const name = document.getElementById('auth-name').value.trim();
            if (!name) return showToast("Votre nom est requis");
            
            const btn = document.getElementById('btn-auth-confirm');
            btn.disabled = true;
            btn.innerText = "Traitement...";

            try {
                await setDoc(doc(db, 'artifacts', appId, 'users', auth.currentUser.uid), {
                    fullName: name,
                    role: selectedRole,
                    createdAt: serverTimestamp()
                });
                location.reload();
            } catch (e) { 
                showToast("Erreur de connexion"); 
                btn.disabled = false;
                btn.innerText = "Valider mon profil";
            }
        };

        // --- GESTION PRODUITS ---
        window.togglePublishModal = () => {
            if(!userProfile || userProfile.fullName === 'Invité') return toggleAuthModal();
            document.getElementById('publish-modal').classList.toggle('hidden');
        };

        window.submitProduct = async () => {
            const name = document.getElementById('p-name').value.trim();
            const price = parseInt(document.getElementById('p-price').value);
            const prov = document.getElementById('p-province').value;
            const img = document.getElementById('p-image').value.trim() || 'https://images.unsplash.com/photo-1542838132-92c53300491e?auto=format&fit=crop&q=80&w=400';

            if(!name || isNaN(price) || !prov) return showToast("Veuillez remplir tous les champs");

            const btn = document.getElementById('btn-publish-confirm');
            btn.disabled = true;
            btn.innerText = "En cours...";

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name, price, province: prov, image: img,
                    ownerId: userProfile.uid,
                    ownerName: userProfile.fullName,
                    status: 'active',
                    createdAt: serverTimestamp()
                });
                showToast("Félicitations ! Produit en vente.");
                togglePublishModal();
            } catch (e) { showToast("Erreur lors de la publication"); }
            btn.disabled = false;
            btn.innerText = "Lancer la vente";
        };

        // --- SYNCHRONISATION TEMPS REEL ---
        function syncCoreData() {
            // Produits du marché
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                allProducts = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                renderMarket();
            }, (err) => showToast("Erreur réseau - Retentez"));

            // Commandes si producteur
            if(userProfile?.role === 'producer') {
                onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), (snap) => {
                    const orders = snap.docs.map(d => ({id: d.id, ...d.data()}))
                                        .filter(o => o.sellerId === userProfile.uid);
                    renderProducerView(orders);
                });
            }
        }

        // --- AFFICHAGES UI ---
        function renderMarket() {
            const grid = document.getElementById('market-grid');
            grid.innerHTML = '';
            
            const filtered = allProducts.filter(p => (activeFilter === 'all' || p.province === activeFilter) && p.status === 'active');
            
            if(filtered.length === 0) {
                grid.innerHTML = `<div class="col-span-full py-20 text-center space-y-4">
                    <i class="fa-solid fa-store-slash text-4xl text-slate-200"></i>
                    <p class="text-slate-400 font-bold uppercase text-[10px] tracking-widest">Rupture de stock dans cette zone</p>
                </div>`;
                return;
            }

            filtered.forEach(p => {
                const card = document.createElement('div');
                card.className = "product-card bg-white p-4 rounded-[2.5rem] shadow-sm flex flex-col cursor-pointer group";
                card.onclick = () => navigateTo('checkout', p);
                card.innerHTML = `
                    <div class="h-48 bg-slate-50 rounded-[2rem] mb-4 overflow-hidden relative shadow-inner">
                        <img src="${p.image}" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500">
                        <div class="absolute top-3 right-3 bg-emerald-500 text-white px-3 py-1.5 rounded-xl text-[8px] font-black uppercase shadow-lg border border-white/20">
                            <i class="fa-solid fa-circle-check mr-1"></i> Vérifié
                        </div>
                    </div>
                    <div class="px-2 flex-1">
                        <div class="flex items-center gap-1.5 mb-1.5">
                            <span class="w-1.5 h-1.5 rounded-full bg-emerald-500"></span>
                            <p class="text-[8px] font-black text-slate-400 uppercase tracking-widest">${p.province}</p>
                        </div>
                        <h4 class="font-black text-[13px] uppercase leading-tight truncate mb-3">${p.name}</h4>
                    </div>
                    <div class="flex items-center justify-between px-2 pb-1">
                        <div>
                            <p class="text-[15px] font-black text-slate-900 leading-none">${p.price.toLocaleString('fr-FR')} <span class="text-[9px]">F</span></p>
                            <p class="text-[7px] font-bold text-slate-400 uppercase mt-1">Smile Ready</p>
                        </div>
                        <div class="w-10 h-10 rounded-2xl bg-slate-900 text-white flex items-center justify-center shadow-lg group-hover:brand-gradient transition-all"><i class="fa-solid fa-cart-plus"></i></div>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        function renderProducerView(orders) {
            const list = document.getElementById('delivery-tracking-list');
            list.innerHTML = '';
            
            const total = orders.reduce((s, o) => s + o.total, 0);
            document.getElementById('stat-sales').innerText = `${total.toLocaleString('fr-FR')} FCFA`;
            document.getElementById('stat-count').innerText = orders.length;

            if(orders.length === 0) {
                list.innerHTML = `<div class="col-span-full py-10 text-center text-[10px] font-bold text-slate-300 uppercase tracking-widest italic">Aucun flux de commande reçu</div>`;
                return;
            }

            orders.forEach(o => {
                const div = document.createElement('div');
                div.className = "bg-slate-50 p-6 rounded-[2.5rem] border border-slate-100 flex flex-col gap-4 hover:border-emerald-200 transition-colors relative overflow-hidden";
                div.innerHTML = `
                    <div class="flex justify-between items-start">
                        <div class="flex items-center gap-4">
                            <div class="w-12 h-12 rounded-2xl brand-gradient text-white flex items-center justify-center shadow-lg"><i class="fa-solid fa-box-open"></i></div>
                            <div>
                                <h5 class="text-[11px] font-black uppercase tracking-tight">${o.productName}</h5>
                                <p class="text-[9px] font-bold text-emerald-600">${o.total.toLocaleString('fr-FR')} F Reçus</p>
                            </div>
                        </div>
                        <span class="text-[7px] font-black uppercase bg-emerald-100 text-emerald-700 px-3 py-1 rounded-full border border-emerald-200">Payé</span>
                    </div>
                    <div class="pt-4 border-t border-slate-200/60">
                        <p class="text-[9px] font-black text-slate-800 flex items-center gap-2 mb-1">
                            <i class="fa-solid fa-location-dot text-slate-400"></i>
                            ${o.deliveryMode === 'delivery' ? o.address : 'Retrait Direct à la Boutique'}
                        </p>
                        <p class="text-[9px] font-bold text-slate-400 italic">Contact: ${o.buyerPhone}</p>
                    </div>
                `;
                list.appendChild(div);
            });
        }

        // --- NAVIGATION & TUNNEL PAIEMENT ---
        window.navigateTo = (v, data = null) => {
            document.getElementById('view-home').classList.toggle('hidden', v !== 'home');
            document.getElementById('view-checkout').classList.toggle('hidden', v !== 'checkout');
            if(v === 'checkout') {
                currentCheckoutItem = data;
                renderCheckout();
            }
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };

        function renderCheckout() {
            const box = document.getElementById('checkout-item');
            box.innerHTML = `
                <img src="${currentCheckoutItem.image}" class="w-24 h-24 rounded-3xl object-cover shadow-lg border-2 border-white">
                <div>
                    <p class="text-[9px] font-black text-emerald-600 uppercase mb-1 tracking-widest">${currentCheckoutItem.province}</p>
                    <h5 class="font-black text-xl uppercase leading-none mb-2">${currentCheckoutItem.name}</h5>
                    <div class="flex items-center gap-2">
                        <i class="fa-solid fa-user-tie text-[10px] text-slate-300"></i>
                        <p class="text-[10px] font-bold text-slate-400">${currentCheckoutItem.ownerName}</p>
                    </div>
                </div>
            `;
            setDeliveryMode('pickup');
        }

        window.setDeliveryMode = (m) => {
            deliveryMode = m;
            document.getElementById('opt-pickup').classList.toggle('active', m === 'pickup');
            document.getElementById('opt-delivery').classList.toggle('active', m === 'delivery');
            document.getElementById('delivery-address-box').classList.toggle('hidden', m === 'pickup');
            
            const total = currentCheckoutItem.price + (m === 'delivery' ? DELIVERY_FEE : 0);
            document.getElementById('checkout-total').innerText = `${total.toLocaleString('fr-FR')} FCFA`;
        };

        window.processOrder = async () => {
            const phone = document.getElementById('pay-phone').value.trim();
            const address = document.getElementById('pay-address').value.trim();
            const btn = document.getElementById('btn-process-order');
            
            if(!phone || phone.length < 8) return showToast("Numéro Mobile Invalide");
            if(deliveryMode === 'delivery' && !address) return showToast("Lieu de livraison requis");

            btn.disabled = true;
            btn.innerHTML = `<i class="fa-solid fa-spinner animate-spin"></i> Transaction Smile...`;

            try {
                const total = currentCheckoutItem.price + (deliveryMode === 'delivery' ? DELIVERY_FEE : 0);
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), {
                    productId: currentCheckoutItem.id,
                    productName: currentCheckoutItem.name,
                    sellerId: currentCheckoutItem.ownerId,
                    buyerId: userProfile.uid,
                    buyerPhone: phone,
                    total: total,
                    deliveryMode: deliveryMode,
                    address: address || 'Retrait Coopérative',
                    status: 'paid',
                    createdAt: serverTimestamp()
                });
                showToast("Commande payée ! Merci pour votre confiance.");
                navigateTo('home');
            } catch (e) { showToast("Paiement échoué"); }
            
            btn.disabled = false;
            btn.innerHTML = `<i class="fa-solid fa-lock text-sm"></i> <span>Confirmer le Paiement</span>`;
        };

        // --- FILTRES PROVINCES ---
        const provinces = ["Estuaire", "Haut-Ogooué", "Moyen-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Ogooué-Maritime", "Woleu-Ntem"];
        const filterBox = document.getElementById('province-filters');
        provinces.forEach(p => {
            const btn = document.createElement('button');
            btn.className = "bg-white border border-slate-100 px-6 py-4 rounded-2xl text-[10px] font-black uppercase whitespace-nowrap shadow-sm hover:border-slate-300 transition-all";
            btn.innerText = p;
            btn.onclick = () => {
                document.querySelectorAll('#province-filters button').forEach(b => {
                    b.className = "bg-white border border-slate-100 px-6 py-4 rounded-2xl text-[10px] font-black uppercase whitespace-nowrap shadow-sm";
                });
                btn.className = "bg-slate-900 text-white px-6 py-4 rounded-2xl text-[10px] font-black uppercase whitespace-nowrap shadow-lg shadow-slate-900/20";
                activeFilter = p;
                renderMarket();
            };
            filterBox.appendChild(btn);
        });

        window.setFilter = (f) => {
            activeFilter = f;
            renderMarket();
        };

        // --- UTILITAIRES ---
        function updateUI() {
            const isGuest = !userProfile || userProfile.fullName === 'Invité';
            document.getElementById('btn-login-trigger').classList.toggle('hidden', !isGuest);
            document.getElementById('user-profile').classList.toggle('hidden', isGuest);
            if(userProfile) {
                document.getElementById('user-initials').innerText = userProfile.fullName[0];
                document.getElementById('view-producer').classList.toggle('hidden', userProfile.role !== 'producer');
            }
        }

        function showToast(m) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = m;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 4000);
        }

        window.handleLogout = () => signOut(auth).then(() => location.reload());

    </script>
</body>
</html>
