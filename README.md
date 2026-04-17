<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Le Terroir Gabonais en un Clic</title>
    
    <!-- PWA / App Mobile Configuration -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --gabon-green: #00853f;
            --gabon-yellow: #fcd116;
            --gabon-blue: #3a75c4;
            --deep-slate: #0f172a;
        }
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background-color: #fdfdfd; 
            color: var(--deep-slate);
            -webkit-tap-highlight-color: transparent;
        }
        
        .brand-gradient {
            background: linear-gradient(135deg, var(--gabon-green), #10b981);
        }

        .glass {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .product-card {
            transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        .product-card:active { transform: scale(0.96); }

        .btn-primary {
            background: var(--deep-slate);
            color: white;
            transition: all 0.3s ease;
        }
        .btn-primary:hover { background: var(--gabon-green); }

        .shimmer {
            background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
            background-size: 200% 100%;
            animation: loading 1.5s infinite;
        }
        @keyframes loading { 0% { background-position: 200% 0; } 100% { background-position: -200% 0; } }

        .hide-scroll::-webkit-scrollbar { display: none; }
        .hide-scroll { -ms-overflow-style: none; scrollbar-width: none; }

        .province-badge {
            background: rgba(255, 255, 255, 0.9);
            padding: 4px 10px;
            border-radius: 100px;
            font-size: 9px;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body class="selection:bg-emerald-100">

    <!-- Toast de Notification -->
    <div id="toast" class="fixed top-6 left-1/2 -translate-x-1/2 z-[3000] hidden animate-bounce">
        <div class="bg-slate-900 text-white px-6 py-3 rounded-2xl shadow-2xl flex items-center gap-3 border-b-2 border-amber-400">
            <span id="toast-msg" class="text-[10px] font-extrabold uppercase tracking-wider">Succès</span>
        </div>
    </div>

    <!-- Header Navigation -->
    <header class="sticky top-0 z-[1000] glass border-b">
        <div class="h-[3px] flex">
            <div class="flex-1 bg-[var(--gabon-green)]"></div>
            <div class="flex-1 bg-[var(--gabon-yellow)]"></div>
            <div class="flex-1 bg-[var(--gabon-blue)]"></div>
        </div>
        <div class="max-w-7xl mx-auto px-4 md:px-6 py-4 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="location.reload()">
                <div class="bg-slate-900 p-1.5 rounded-lg shadow-lg">
                    <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Echoppe241" class="h-6 w-auto">
                </div>
                <div>
                    <h1 class="font-extrabold text-xl tracking-tighter">ECHOPPE<span class="text-amber-500">241</span></h1>
                </div>
            </div>

            <div class="flex items-center gap-2">
                <button id="btn-auth" onclick="toggleAuthModal()" class="btn-primary px-5 py-2.5 rounded-xl font-bold text-[10px] uppercase tracking-wide">Connexion</button>
                <div id="user-menu" class="hidden flex items-center gap-3 bg-slate-50 p-1 rounded-2xl border">
                    <div class="w-9 h-9 rounded-xl bg-emerald-600 text-white flex items-center justify-center font-bold text-xs" id="u-initials">?</div>
                    <button onclick="handleLogout()" class="pr-3 pl-1 text-slate-400 hover:text-red-500"><i class="fa-solid fa-power-off"></i></button>
                </div>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 md:px-6 py-6 min-h-screen">
        
        <!-- Marketplace View -->
        <div id="view-buyer">
            <div class="relative rounded-[2.5rem] overflow-hidden bg-slate-900 aspect-[16/9] md:aspect-auto md:min-h-[350px] mb-10 flex items-center shadow-2xl">
                <div class="absolute inset-0 opacity-50 bg-cover bg-center" style="background-image: url('https://images.unsplash.com/photo-1542838132-92c53300491e?auto=format&fit=crop&w=1200&q=80');"></div>
                <div class="relative z-10 p-8 md:p-12 text-white max-w-xl">
                    <span class="bg-amber-500 text-slate-900 px-3 py-1 rounded-full text-[9px] font-black uppercase mb-4 inline-block">Bientôt Demain</span>
                    <h2 class="text-3xl md:text-5xl font-extrabold leading-none mb-4 italic">Directement des <span class="text-emerald-400">9 Provinces</span>.</h2>
                    <p class="text-slate-300 text-sm md:text-base font-medium opacity-90">Soutenez nos agriculteurs. Qualité garantie, prix direct producteur.</p>
                </div>
            </div>

            <div class="flex flex-col md:flex-row gap-4 mb-8">
                <div class="relative flex-grow">
                    <i class="fa-solid fa-search absolute left-4 top-1/2 -translate-y-1/2 text-slate-400"></i>
                    <input type="text" id="market-search" placeholder="Rechercher (Manioc, Banane, Huile...)" class="w-full bg-white border border-slate-200 p-4 pl-12 rounded-2xl focus:border-emerald-500 outline-none font-semibold text-sm shadow-sm transition-all">
                </div>
                <div class="flex gap-2 overflow-x-auto pb-2 hide-scroll">
                    <button onclick="filterProvince('all')" class="bg-slate-900 text-white px-5 py-3 rounded-2xl text-[10px] font-bold uppercase whitespace-nowrap shadow-md">Toutes Provinces</button>
                    <button onclick="filterProvince('Estuaire')" class="bg-white border px-5 py-3 rounded-2xl text-[10px] font-bold uppercase whitespace-nowrap">Estuaire</button>
                    <button onclick="filterProvince('Woleu-Ntem')" class="bg-white border px-5 py-3 rounded-2xl text-[10px] font-bold uppercase whitespace-nowrap">Woleu-Ntem</button>
                    <button onclick="filterProvince('Ngounié')" class="bg-white border px-5 py-3 rounded-2xl text-[10px] font-bold uppercase whitespace-nowrap">Ngounié</button>
                </div>
            </div>

            <div id="products-container" class="grid grid-cols-2 lg:grid-cols-4 gap-4 md:gap-8">
                <!-- Skeletons -->
                <div class="shimmer h-64 rounded-3xl"></div>
                <div class="shimmer h-64 rounded-3xl"></div>
                <div class="shimmer h-64 rounded-3xl"></div>
                <div class="shimmer h-64 rounded-3xl"></div>
            </div>
        </div>

        <!-- Dashboard Producteur -->
        <div id="view-producer" class="hidden space-y-8 animate-in slide-in-from-right-10">
            <div class="brand-gradient text-white p-8 md:p-12 rounded-[2.5rem] shadow-2xl flex flex-col md:flex-row justify-between items-center gap-6">
                <div>
                    <h2 class="text-3xl font-extrabold italic uppercase tracking-tighter">Ma Boutique <span class="text-slate-900">Echoppe</span></h2>
                    <p class="text-emerald-100 text-xs font-bold uppercase tracking-widest mt-2">Mise en ligne de vos récoltes</p>
                </div>
                <button onclick="togglePublishModal()" class="bg-white text-slate-900 px-8 py-4 rounded-2xl font-black text-[10px] uppercase tracking-widest shadow-xl hover:scale-105 transition">
                    <i class="fa-solid fa-plus-circle mr-2"></i> Publier un produit
                </button>
            </div>

            <div class="bg-white rounded-[2rem] p-6 border shadow-sm">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="font-extrabold uppercase italic tracking-tighter text-slate-500">Mon Inventaire en ligne</h3>
                </div>
                <div id="my-products" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Mes produits -->
                </div>
            </div>
        </div>

    </main>

    <!-- Modal Authentification -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 md:p-10 shadow-2xl">
            <div class="text-center mb-8">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Logo" class="h-10 mx-auto mb-4">
                <h2 id="auth-title" class="text-2xl font-extrabold uppercase italic tracking-tighter">Connexion</h2>
            </div>

            <div id="login-form" class="space-y-4">
                <input type="email" id="l-email" placeholder="Email" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 outline-none focus:border-emerald-500 font-bold">
                <input type="password" id="l-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 outline-none focus:border-emerald-500 font-bold">
                <button onclick="handleLogin()" class="w-full brand-gradient text-white py-4 rounded-xl font-bold uppercase text-[10px] tracking-widest shadow-lg">Entrer dans la Place</button>
                <p class="text-center text-[10px] font-extrabold text-slate-400 uppercase pt-4">Nouveau membre ? <button onclick="switchAuth('reg')" class="text-emerald-600 underline">Créer un compte</button></p>
            </div>

            <div id="reg-form" class="space-y-4 hidden">
                <input type="text" id="r-name" placeholder="Nom ou Nom de la Coopérative" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 outline-none font-bold">
                <input type="email" id="r-email" placeholder="Email" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 outline-none font-bold">
                <input type="password" id="r-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-100 outline-none font-bold">
                <div class="grid grid-cols-2 gap-3">
                    <button id="role-buyer" onclick="setRegRole('buyer')" class="p-3 rounded-xl border-2 border-emerald-500 bg-emerald-50 text-emerald-700 font-bold text-[10px] uppercase">Acheteur</button>
                    <button id="role-producer" onclick="setRegRole('producer')" class="p-3 rounded-xl border-2 border-slate-100 font-bold text-[10px] uppercase">Producteur</button>
                </div>
                <button onclick="handleRegister()" class="w-full bg-slate-900 text-white py-4 rounded-xl font-bold uppercase text-[10px] tracking-widest shadow-lg">S'inscrire</button>
                <p class="text-center text-[10px] font-extrabold text-slate-400 uppercase pt-4">Déjà inscrit ? <button onclick="switchAuth('login')" class="text-slate-900 underline">Se connecter</button></p>
            </div>
        </div>
    </div>

    <!-- Modal Publication Produit -->
    <div id="publish-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-lg rounded-[2.5rem] p-8 md:p-10 shadow-2xl relative overflow-y-auto max-h-[90vh]">
            <button onclick="togglePublishModal()" class="absolute top-6 right-6 text-slate-400 text-2xl hover:text-slate-900">&times;</button>
            <h3 class="text-2xl font-extrabold mb-6 uppercase italic tracking-tighter">Publier un produit</h3>
            
            <form id="product-form" class="space-y-4">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Nom du produit</label>
                        <input type="text" id="p-name" placeholder="ex: Manioc d'Oyem" class="w-full bg-slate-50 p-4 rounded-xl border outline-none font-bold" required>
                    </div>
                    <div>
                        <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Province</label>
                        <select id="p-province" class="w-full bg-slate-50 p-4 rounded-xl border outline-none font-bold" required>
                            <option>Estuaire</option><option>Haut-Ogooué</option><option>Moyen-Ogooué</option><option>Ngounié</option>
                            <option>Nyanga</option><option>Ogooué-Ivindo</option><option>Ogooué-Lolo</option><option>Ogooué-Maritime</option><option>Woleu-Ntem</option>
                        </select>
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Prix (FCFA)</label>
                        <input type="number" id="p-price" placeholder="Prix" class="w-full bg-slate-50 p-4 rounded-xl border outline-none font-bold" required>
                    </div>
                    <div>
                        <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Unité</label>
                        <input type="text" id="p-unit" placeholder="ex: Sac, Kg, Litre" class="w-full bg-slate-50 p-4 rounded-xl border outline-none font-bold" required>
                    </div>
                </div>
                <textarea id="p-desc" placeholder="Détails sur la récolte..." class="w-full bg-slate-50 p-4 rounded-xl border outline-none font-bold h-24"></textarea>
                <button type="submit" class="w-full bg-slate-900 text-white py-4 rounded-xl font-bold uppercase text-[10px] tracking-widest shadow-xl">Publier maintenant</button>
            </form>
        </div>
    </div>

    <!-- Modal Chat -->
    <div id="chat-modal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm z-[2000] hidden flex items-end md:items-center justify-center p-0 md:p-4">
        <div class="bg-white w-full max-w-md h-[80vh] md:h-[600px] rounded-t-[2rem] md:rounded-[2rem] shadow-2xl flex flex-col">
            <div class="bg-slate-900 text-white p-5 flex justify-between items-center rounded-t-[2rem]">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-emerald-500 rounded-xl flex items-center justify-center font-bold" id="chat-icon">E</div>
                    <div>
                        <h4 id="chat-target-name" class="font-extrabold text-sm uppercase tracking-tight">Vendeur</h4>
                        <p class="text-[8px] font-bold text-slate-400 uppercase tracking-widest">Canal de négociation</p>
                    </div>
                </div>
                <button onclick="closeChat()" class="text-slate-400 text-3xl">&times;</button>
            </div>
            <div id="chat-messages" class="flex-grow p-4 overflow-y-auto flex flex-col gap-3 bg-slate-50"></div>
            <form id="chat-form" class="p-4 bg-white border-t flex gap-2">
                <input type="text" id="chat-input" placeholder="Ecrivez votre message..." class="flex-grow bg-slate-100 p-4 rounded-xl outline-none font-semibold text-sm">
                <button type="submit" class="bg-slate-900 text-white w-12 rounded-xl flex items-center justify-center shadow-lg"><i class="fa-solid fa-paper-plane"></i></button>
            </form>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, serverTimestamp, where, limit } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Configuration Firebase (Utilisation des identifiants officiels du projet)
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe241-v1-prod';

        let currentUser = null;
        let regRole = 'buyer';
        let currentRoomId = null;
        let currentFilter = 'all';

        // --- Gestion de l'Auth ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                // Essayer de récupérer le profil utilisateur
                const uDoc = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid));
                if (uDoc.exists()) {
                    currentUser = { uid: user.uid, ...uDoc.data() };
                } else if (user.isAnonymous) {
                    currentUser = { uid: user.uid, role: 'guest', fullName: 'Invité' };
                }
                updateView();
                syncMarket();
            } else {
                signInAnonymously(auth).catch(e => console.error(e));
            }
        });

        // --- Actions Globales ---
        window.handleLogin = async () => {
            const email = document.getElementById('l-email').value;
            const pass = document.getElementById('l-pass').value;
            if(!email || !pass) return alertMsg("Identifiants manquants");

            try {
                await signInWithEmailAndPassword(auth, email, pass);
                toggleAuthModal();
                showToast("Bienvenue sur Echoppe241 !");
            } catch (e) { alertMsg("Accès refusé. Vérifiez vos identifiants."); }
        };

        window.handleRegister = async () => {
            const email = document.getElementById('r-email').value;
            const pass = document.getElementById('r-pass').value;
            const name = document.getElementById('r-name').value;
            if(!email || !pass || !name) return alertMsg("Champs obligatoires manquants.");

            try {
                const cred = await createUserWithEmailAndPassword(auth, email, pass);
                const profile = { fullName: name, role: regRole, email: email, createdAt: serverTimestamp() };
                await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), profile);
                toggleAuthModal();
                showToast("Compte créé avec succès !");
            } catch (e) { alertMsg("Erreur lors de l'inscription."); }
        };

        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // --- Synchronisation Données ---
        function syncMarket() {
            const productsRef = collection(db, 'artifacts', appId, 'public', 'data', 'products');
            onSnapshot(productsRef, (snap) => {
                const all = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                renderMarket(all);
                if(currentUser?.role === 'producer') {
                    renderProducerItems(all.filter(p => p.ownerId === currentUser.uid));
                }
            });
        }

        function renderMarket(products) {
            const container = document.getElementById('products-container');
            let filtered = products;
            if(currentFilter !== 'all') filtered = products.filter(p => p.province === currentFilter);

            container.innerHTML = filtered.length ? '' : '<div class="col-span-full py-16 text-center text-slate-400 font-bold">Aucun produit trouvé dans cette province.</div>';
            
            filtered.sort((a,b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0)).forEach(p => {
                const card = document.createElement('div');
                card.className = "product-card bg-white p-4 rounded-[2rem] border border-slate-100 shadow-sm relative group cursor-pointer";
                card.onclick = () => openChat(p.ownerId, p.ownerName);
                card.innerHTML = `
                    <div class="h-40 bg-slate-50 rounded-2xl mb-4 flex items-center justify-center relative overflow-hidden">
                        <span class="province-badge absolute top-3 left-3">${p.province}</span>
                        <i class="fa-solid fa-basket-shopping text-3xl text-slate-200"></i>
                    </div>
                    <div class="flex justify-between items-start mb-1">
                        <h4 class="font-bold text-xs uppercase truncate pr-4">${p.name}</h4>
                    </div>
                    <p class="text-[10px] text-emerald-600 font-bold mb-4">${p.price} FCFA / ${p.unit}</p>
                    <div class="flex justify-between items-center border-t pt-3">
                        <div class="flex items-center gap-2">
                            <div class="w-6 h-6 rounded-lg bg-slate-900 text-white flex items-center justify-center text-[8px] font-bold">${p.ownerName.charAt(0)}</div>
                            <span class="text-[8px] font-black uppercase text-slate-400 truncate max-w-[60px]">${p.ownerName}</span>
                        </div>
                        <div class="bg-emerald-100 text-emerald-700 w-8 h-8 rounded-lg flex items-center justify-center"><i class="fa-solid fa-comment-dots text-xs"></i></div>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function renderProducerItems(items) {
            const box = document.getElementById('my-products');
            box.innerHTML = items.length ? '' : '<p class="text-slate-400 text-xs py-10 text-center border-2 border-dashed rounded-2xl">Vous n\'avez pas encore de produits en vente.</p>';
            items.forEach(p => {
                const item = document.createElement('div');
                item.className = "flex items-center gap-4 bg-slate-50 p-4 rounded-2xl border";
                item.innerHTML = `
                    <div class="w-12 h-12 bg-white rounded-xl flex items-center justify-center text-emerald-600 shadow-sm"><i class="fa-solid fa-box"></i></div>
                    <div class="flex-grow">
                        <h5 class="font-bold text-[10px] uppercase">${p.name}</h5>
                        <p class="text-[9px] font-bold text-slate-400">${p.price} FCFA - ${p.province}</p>
                    </div>
                    <div class="bg-emerald-100 text-emerald-700 text-[8px] font-black px-2 py-1 rounded">Vente Active</div>
                `;
                box.appendChild(item);
            });
        }

        // --- Messagerie ---
        window.openChat = (targetId, targetName) => {
            if(currentUser.role === 'guest') return toggleAuthModal();
            if(currentUser.uid === targetId) return;

            currentRoomId = [currentUser.uid, targetId].sort().join('_');
            document.getElementById('chat-target-name').innerText = targetName;
            document.getElementById('chat-icon').innerText = targetName.charAt(0);
            document.getElementById('chat-modal').classList.remove('hidden');

            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'chats', currentRoomId, 'messages'), limit(50));
            onSnapshot(q, (snap) => {
                const box = document.getElementById('chat-messages');
                box.innerHTML = '';
                const msgs = snap.docs.map(d => d.data()).sort((a,b) => (a.timestamp?.seconds || 0) - (b.timestamp?.seconds || 0));
                msgs.forEach(m => {
                    const el = document.createElement('div');
                    el.className = `max-w-[85%] p-3 rounded-2xl text-xs font-semibold ${m.senderId === currentUser.uid ? 'bg-slate-900 text-white self-end rounded-br-none' : 'bg-white border text-slate-800 self-start rounded-bl-none'}`;
                    el.innerText = m.text;
                    box.appendChild(el);
                });
                box.scrollTop = box.scrollHeight;
            });
        };

        document.getElementById('chat-form').onsubmit = async (e) => {
            e.preventDefault();
            const input = document.getElementById('chat-input');
            if(!input.value || !currentRoomId) return;
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', currentRoomId, 'messages'), {
                    text: input.value, senderId: currentUser.uid, timestamp: serverTimestamp()
                });
                input.value = '';
            } catch (e) { alertMsg("Erreur d'envoi."); }
        };

        // --- Publication ---
        document.getElementById('product-form').onsubmit = async (e) => {
            e.preventDefault();
            const data = {
                name: document.getElementById('p-name').value,
                province: document.getElementById('p-province').value,
                price: parseInt(document.getElementById('p-price').value),
                unit: document.getElementById('p-unit').value,
                description: document.getElementById('p-desc').value,
                ownerId: currentUser.uid,
                ownerName: currentUser.fullName,
                createdAt: serverTimestamp()
            };

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), data);
                togglePublishModal();
                showToast("Produit publié avec succès !");
                document.getElementById('product-form').reset();
            } catch (e) { alertMsg("Erreur de publication."); }
        };

        // --- UI Helpers ---
        function updateView() {
            const isGuest = currentUser?.role === 'guest';
            document.getElementById('btn-auth').classList.toggle('hidden', !isGuest);
            document.getElementById('user-menu').classList.toggle('hidden', isGuest);
            
            if(!isGuest) {
                document.getElementById('u-initials').innerText = currentUser.fullName.substring(0,1);
                document.getElementById('view-buyer').classList.toggle('hidden', currentUser.role === 'producer');
                document.getElementById('view-producer').classList.toggle('hidden', currentUser.role !== 'producer');
            }
        }

        window.toggleAuthModal = () => document.getElementById('auth-modal').classList.toggle('hidden');
        window.switchAuth = (m) => {
            document.getElementById('login-form').classList.toggle('hidden', m === 'reg');
            document.getElementById('reg-form').classList.toggle('hidden', m === 'login');
            document.getElementById('auth-title').innerText = m === 'reg' ? "Inscription" : "Connexion";
        };
        window.setRegRole = (r) => {
            regRole = r;
            document.getElementById('role-buyer').className = r === 'buyer' ? "p-3 rounded-xl border-2 border-emerald-500 bg-emerald-50 text-emerald-700 font-bold text-[10px] uppercase" : "p-3 rounded-xl border-2 border-slate-100 font-bold text-[10px] uppercase";
            document.getElementById('role-producer').className = r === 'producer' ? "p-3 rounded-xl border-2 border-emerald-500 bg-emerald-50 text-emerald-700 font-bold text-[10px] uppercase" : "p-3 rounded-xl border-2 border-slate-100 font-bold text-[10px] uppercase";
        };
        window.togglePublishModal = () => document.getElementById('publish-modal').classList.toggle('hidden');
        window.closeChat = () => document.getElementById('chat-modal').classList.add('hidden');
        window.filterProvince = (p) => {
            currentFilter = p;
            syncMarket();
        };

        function showToast(msg) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        function alertMsg(msg) {
            document.getElementById('toast-msg').innerText = msg;
            document.getElementById('toast').classList.remove('hidden');
            setTimeout(() => document.getElementById('toast').classList.add('hidden'), 4000);
        }

    </script>
</body>
</html>
