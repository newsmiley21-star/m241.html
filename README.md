<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Plateforme Officielle</title> 
    
    <!-- Identité Visuelle & PWA -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#00853f">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --gabon-green: #00853f;
            --gabon-yellow: #fcd116;
            --gabon-blue: #3a75c4;
        }
        body { font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px); }
        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .echoppe-card { background: linear-gradient(135deg, #00853f 0%, #005f2d 100%); }
        .btn-pay { @apply bg-[#fcd116] text-slate-900 font-black py-4 rounded-2xl shadow-lg active:scale-95 transition-all w-full flex justify-center items-center gap-2; }
    </style>
</head>
<body class="bg-[#f8fafc] text-slate-900 overflow-x-hidden">

    <!-- NAVIGATION SUPERIEURE -->
    <nav class="sticky top-0 z-[100] glass border-b border-slate-100 px-4 h-20 flex justify-between items-center">
        <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 h-10 object-contain" alt="Logo">
            <span class="font-extrabold text-xl tracking-tighter uppercase italic">Echoppe<span class="text-emerald-600">241</span></span>
        </div>
        
        <div class="flex items-center gap-4">
            <div id="btn-login-trigger" onclick="navigateTo('auth')" class="cursor-pointer bg-slate-100 p-3 rounded-full hover:bg-slate-200 transition-all">
                <i class="fa-solid fa-user text-slate-500"></i>
            </div>
            <div id="user-profile" class="hidden flex items-center gap-3 cursor-pointer" onclick="navigateTo('profile')">
                <div class="text-right hidden md:block">
                    <p id="user-wallet-nav" class="text-[10px] font-black text-emerald-600 uppercase">0 FCFA</p>
                    <p id="user-name-nav" class="text-xs font-bold text-slate-400 italic">Chargement...</p>
                </div>
                <div class="relative">
                    <img id="user-avatar-nav" src="https://ui-avatars.com/api/?name=User&background=00853f&color=fff" class="w-10 h-10 rounded-full border-2 border-white shadow-sm object-cover">
                    <div id="admin-badge" class="hidden absolute -top-1 -right-1 bg-yellow-400 w-4 h-4 rounded-full border-2 border-white flex items-center justify-center">
                        <i class="fa-solid fa-crown text-[6px] text-slate-900"></i>
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <!-- 1. VUE ACCUEIL -->
    <div id="view-home" class="view active max-w-7xl mx-auto p-4 md:p-8">
        <div class="mb-10">
            <h1 class="text-5xl font-black italic uppercase leading-none mb-4">Le Marché <span class="text-emerald-600">Digital</span></h1>
            <p class="text-slate-400 font-semibold max-w-md">Soutenez nos planteurs. Achetez local, payez en toute sécurité avec EchoppePay.</p>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-4 gap-8">
            <!-- Sidebar Filtres -->
            <div class="lg:col-span-1">
                <div class="bg-white rounded-[32px] p-6 shadow-sm border border-slate-100 sticky top-28">
                    <h3 class="font-black uppercase text-xs text-slate-400 mb-6 tracking-widest">Provinces du Gabon</h3>
                    <div id="filter-provinces" class="flex flex-col gap-2">
                        <button onclick="setFilter('Tous')" class="province-btn text-left text-[11px] font-bold p-3 rounded-2xl bg-emerald-600 text-white shadow-lg shadow-emerald-100">Tous le Gabon</button>
                    </div>
                </div>
            </div>

            <!-- Grille Produits -->
            <div class="lg:col-span-3">
                <div id="market-grid" class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6">
                    <!-- Injection JS -->
                </div>
            </div>
        </div>
    </div>

    <!-- 2. VUE AUTH -->
    <div id="view-auth" class="view max-w-md mx-auto p-6 mt-10">
        <div class="bg-white rounded-[40px] p-10 shadow-2xl border border-slate-50">
            <h2 class="text-3xl font-black mb-8 uppercase italic leading-tight">Rejoindre <br><span class="text-emerald-600">La Communauté</span></h2>
            <div class="space-y-4">
                <input type="text" id="auth-name" placeholder="Nom complet" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold focus:ring-2 ring-emerald-500 outline-none transition-all">
                <input type="email" id="auth-email" placeholder="Adresse Email" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold focus:ring-2 ring-emerald-500 outline-none transition-all">
                <input type="password" id="auth-pass" placeholder="Mot de passe" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold focus:ring-2 ring-emerald-500 outline-none transition-all">
                
                <div class="pt-4">
                    <p class="text-[10px] font-black uppercase text-slate-400 mb-3 px-2 tracking-widest">Mon Statut</p>
                    <div class="grid grid-cols-2 gap-3">
                        <button onclick="selectRole('buyer')" id="role-buyer" class="role-btn p-4 rounded-2xl border-2 border-emerald-600 bg-emerald-50 text-emerald-700 font-black text-[10px] uppercase">Acheteur</button>
                        <button onclick="selectRole('producer')" id="role-producer" class="role-btn p-4 rounded-2xl border-2 border-slate-100 text-slate-400 font-black text-[10px] uppercase">Producteur</button>
                    </div>
                </div>

                <button onclick="handleAuth()" class="w-full bg-[#00853f] text-white font-black py-5 rounded-2xl shadow-xl uppercase tracking-widest mt-4 hover:bg-emerald-700 transition-all">Créer mon compte</button>
            </div>
        </div>
    </div>

    <!-- 3. VUE PROFIL -->
    <div id="view-profile" class="view max-w-2xl mx-auto p-4 md:p-8 pb-32">
        <!-- Carte EchoppePay -->
        <div class="echoppe-card rounded-[40px] p-8 text-white mb-8 relative overflow-hidden shadow-2xl">
            <div class="relative z-10">
                <div class="flex justify-between items-start mb-10">
                    <div>
                        <p class="text-[10px] font-black opacity-70 uppercase tracking-widest mb-1">Mon Solde EchoppePay</p>
                        <h2 id="p-wallet-balance" class="text-5xl font-black italic tracking-tighter">0 FCFA</h2>
                    </div>
                    <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-12 h-12 brightness-200">
                </div>
                <div class="flex flex-wrap gap-3">
                    <button onclick="openRechargeModal()" class="bg-white text-emerald-900 font-black px-8 py-4 rounded-2xl text-[10px] uppercase shadow-lg active:scale-95 transition-all">Recharger</button>
                    <button id="btn-admin-panel" onclick="navigateTo('admin')" class="hidden bg-yellow-400 text-slate-900 font-black px-8 py-4 rounded-2xl text-[10px] uppercase shadow-lg active:scale-95 transition-all">Console Admin</button>
                    <button onclick="handleLogout()" class="bg-emerald-800/50 text-white font-black px-8 py-4 rounded-2xl text-[10px] uppercase active:scale-95 transition-all">Déconnexion</button>
                </div>
            </div>
            <i class="fa-solid fa-wallet absolute -right-6 -bottom-6 text-white/10 text-[12rem]"></i>
        </div>

        <!-- Section Vendeur -->
        <div id="view-producer" class="hidden bg-emerald-50 rounded-[40px] p-8 border-2 border-dashed border-emerald-200 mb-8">
            <h3 class="font-black uppercase text-xl mb-6 italic text-emerald-900">Publier un produit</h3>
            <div class="space-y-4">
                <div class="h-48 rounded-3xl bg-white flex flex-col items-center justify-center border-2 border-dashed border-slate-200 cursor-pointer overflow-hidden transition-all hover:border-emerald-400 group" onclick="document.getElementById('prod-img-input').click()">
                    <div id="upload-preview" class="text-center group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-camera text-4xl text-slate-200 mb-2"></i>
                        <p class="text-[10px] font-black text-slate-400 uppercase">Ajouter une photo</p>
                    </div>
                    <input type="file" id="prod-img-input" class="hidden" accept="image/*">
                </div>
                <input type="text" id="prod-name" placeholder="Nom du produit" class="w-full p-4 bg-white rounded-xl border-none font-bold">
                <div class="grid grid-cols-2 gap-4">
                    <input type="number" id="prod-price" placeholder="Prix (FCFA)" class="w-full p-4 bg-white rounded-xl border-none font-bold">
                    <select id="prod-province" class="w-full p-4 bg-white rounded-xl border-none font-black text-emerald-600 uppercase text-xs"></select>
                </div>
                <button onclick="handlePublish()" id="btn-publish" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl uppercase text-xs tracking-widest shadow-lg">Mettre en vente</button>
            </div>
        </div>

        <!-- Historique des Recharges -->
        <div class="bg-white rounded-[32px] p-8 shadow-sm border border-slate-100">
            <h3 class="font-black uppercase text-xs text-slate-400 tracking-widest mb-6 italic">Mes Recharges EchoppePay</h3>
            <div id="recharge-history" class="space-y-4">
                <!-- Injection JS -->
                <p class="text-center py-6 text-slate-300 font-bold italic text-xs">Aucune recharge effectuée.</p>
            </div>
        </div>
    </div>

    <!-- 4. VUE ADMIN (EXCLUSIF ADMIN) -->
    <div id="view-admin" class="view max-w-4xl mx-auto p-4 md:p-8">
        <div class="flex justify-between items-end mb-10">
            <div>
                <h1 class="text-4xl font-black italic uppercase">Console <span class="text-emerald-600">Admin</span></h1>
                <p class="text-slate-400 font-bold text-[10px] uppercase tracking-widest mt-2">Validation des recharges clients</p>
            </div>
            <button onclick="navigateTo('profile')" class="bg-slate-100 px-6 py-3 rounded-xl text-[10px] font-black uppercase">Fermer</button>
        </div>

        <div class="bg-white rounded-[40px] p-8 shadow-xl border border-slate-100">
            <div class="flex items-center justify-between mb-8 border-b border-slate-50 pb-6">
                <h3 class="font-black text-slate-800 uppercase text-xs">Demandes en attente</h3>
                <span id="pending-count" class="bg-orange-100 text-orange-600 text-[10px] font-black px-4 py-1 rounded-full">0 Requêtes</span>
            </div>

            <div id="admin-request-list" class="space-y-4">
                <!-- Les demandes apparaissent ici -->
            </div>
        </div>
    </div>

    <!-- 5. VUE CHECKOUT (PAIEMENT) -->
    <div id="view-checkout" class="view max-w-lg mx-auto p-4 md:p-8">
        <div class="bg-white rounded-[40px] p-10 shadow-2xl border border-slate-100 mt-6 relative overflow-hidden">
            <h2 class="text-3xl font-black mb-8 uppercase italic">Ma <span class="text-emerald-600">Commande</span></h2>
            
            <div id="checkout-item" class="flex gap-5 p-5 bg-slate-50 rounded-[28px] mb-8 border border-slate-100">
                <!-- Injection JS -->
            </div>

            <div class="bg-emerald-900 rounded-[32px] p-8 text-white">
                <div class="flex justify-between items-center mb-6">
                    <span class="font-bold text-emerald-200">Total à payer</span>
                    <span id="checkout-total" class="text-3xl font-black italic">0 FCFA</span>
                </div>
                
                <button onclick="processOrder()" id="btn-confirm-order" class="btn-pay">
                    PAYER AVEC ECHOPPEPAY
                </button>
                
                <p class="text-[9px] text-center mt-4 text-emerald-300 opacity-60 font-medium uppercase tracking-widest">Transaction sécurisée par EchoppePay</p>
            </div>
        </div>
    </div>

    <!-- MODAL RECHARGE -->
    <div id="modal-recharge" class="hidden fixed inset-0 z-[2000] bg-slate-900/90 flex items-center justify-center p-4 backdrop-blur-sm">
        <div class="bg-white rounded-[40px] p-10 max-w-md w-full animate-fadeIn shadow-2xl">
            <div class="text-center mb-8">
                <div class="w-16 h-16 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center mx-auto mb-4">
                    <i class="fa-solid fa-mobile-screen-button text-2xl"></i>
                </div>
                <h3 class="font-black uppercase italic text-2xl">Recharger <br><span class="text-emerald-600">EchoppePay</span></h3>
            </div>

            <div class="space-y-5">
                <div class="p-5 bg-emerald-50 rounded-2xl border-2 border-emerald-100 space-y-2">
                    <p class="text-[10px] font-black text-emerald-800 uppercase tracking-widest">Numéros Marchands :</p>
                    <div class="flex justify-between items-center font-black text-sm">
                        <span>Airtel Money</span>
                        <span class="text-emerald-600">074 00 00 00</span>
                    </div>
                    <div class="flex justify-between items-center font-black text-sm">
                        <span>Moov Money</span>
                        <span class="text-blue-600">066 00 00 00</span>
                    </div>
                </div>

                <div class="space-y-4">
                    <input type="number" id="recharge-amount" placeholder="Montant transféré (FCFA)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-black text-lg focus:ring-2 ring-emerald-500">
                    <input type="text" id="recharge-txn-id" placeholder="ID de Transaction (ID Airtel/Moov)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold uppercase focus:ring-2 ring-emerald-500">
                </div>

                <button onclick="submitRechargeRequest()" id="btn-submit-recharge" class="w-full bg-emerald-600 text-white font-black py-5 rounded-2xl uppercase shadow-lg shadow-emerald-100">
                    Envoyer la preuve
                </button>
                <button onclick="closeRechargeModal()" class="w-full text-slate-400 font-bold text-[10px] uppercase tracking-widest mt-2 hover:text-slate-600 transition-all">Retour</button>
            </div>
        </div>
    </div>

    <!-- TOAST -->
    <div id="toast" class="hidden fixed bottom-10 left-1/2 -translate-x-1/2 z-[3000] bg-slate-900 text-white px-8 py-4 rounded-3xl shadow-2xl flex items-center gap-4 animate-fadeIn border border-slate-700">
        <div id="toast-icon" class="w-6 h-6 rounded-full bg-emerald-500 flex items-center justify-center text-[10px]">
            <i class="fa-solid fa-check"></i>
        </div>
        <p id="toast-msg" class="text-[10px] font-black uppercase tracking-widest"></p>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, createUserWithEmailAndPassword, signInAnonymously, signOut, signInWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, serverTimestamp, updateDoc, increment, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // CONFIGURATION
        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'echoppe241-prod-full';

        // ETAT GLOBAL
        let userProfile = null;
        let activeFilter = 'Tous';
        let currentCheckoutItem = null;
        let tempProdImg = null;
        let selectedRole = 'buyer';
        
        const ADMIN_EMAIL = "admin@echoppe241.ga"; // Email qui a les droits admin
        const provinces = ["Estuaire", "Haut-Ogooué", "Moyen-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Ogooué-Maritime", "Woleu-Ntem"];

        // NAVIGATION
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(view => view.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            window.scrollTo(0,0);
            if(v === 'admin') loadAdminDashboard();
        };

        // AUTHENTIFICATION
        window.selectRole = (r) => {
            selectedRole = r;
            document.querySelectorAll('.role-btn').forEach(b => {
                b.classList.remove('border-emerald-600', 'bg-emerald-50', 'text-emerald-700');
                b.classList.add('border-slate-100', 'text-slate-400');
            });
            document.getElementById(`role-${r}`).classList.add('border-emerald-600', 'bg-emerald-50', 'text-emerald-700');
        };

        window.handleAuth = async () => {
            const name = document.getElementById('auth-name').value;
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;

            if(!name || !email || pass.length < 6) return showToast("Veuillez remplir correctement les champs", "error");

            try {
                const cred = await createUserWithEmailAndPassword(auth, email, pass);
                const profile = {
                    uid: cred.user.uid,
                    fullName: name,
                    email: email,
                    role: selectedRole,
                    walletBalance: 0,
                    isAdmin: email === ADMIN_EMAIL,
                    createdAt: serverTimestamp()
                };
                await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), profile);
                showToast("Bienvenue sur Echoppe241 !");
                navigateTo('home');
            } catch (e) { showToast("Email déjà utilisé ou erreur réseau", "error"); }
        };

        onAuthStateChanged(auth, u => {
            if (u && !u.isAnonymous) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), s => {
                    if (s.exists()) {
                        userProfile = s.data();
                        updateUI();
                        loadRechargeHistory();
                    }
                });
            } else if (!u) {
                signInAnonymously(auth);
            }
        });

        // MARCHÉ & PRODUITS
        onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), s => {
            const grid = document.getElementById('market-grid');
            grid.innerHTML = "";
            s.docs.forEach(d => {
                const p = d.data();
                if(activeFilter !== 'Tous' && p.province !== activeFilter) return;

                const card = document.createElement('div');
                card.className = "bg-white rounded-[32px] overflow-hidden shadow-sm border border-slate-100 p-4 hover:shadow-xl transition-all group";
                card.innerHTML = `
                    <div class="relative overflow-hidden rounded-2xl mb-4 h-48">
                        <img src="${p.image}" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500">
                        <div class="absolute top-3 left-3 bg-white/90 backdrop-blur-md px-3 py-1 rounded-full text-[8px] font-black uppercase text-emerald-700 shadow-sm">
                            ${p.province}
                        </div>
                    </div>
                    <div class="px-2">
                        <h3 class="font-black text-lg text-slate-800 line-clamp-1">${p.name}</h3>
                        <p class="text-[9px] font-bold text-slate-400 mb-4 italic">Vendeur vérifié</p>
                        <div class="flex justify-between items-center bg-slate-50 p-3 rounded-2xl border border-slate-100">
                            <span class="font-black text-emerald-700">${p.price.toLocaleString()} F</span>
                            <button onclick="openCheckout('${d.id}')" class="bg-slate-900 text-white px-4 py-2 rounded-xl text-[9px] font-black uppercase hover:bg-emerald-600 transition-colors">Acheter</button>
                        </div>
                    </div>
                `;
                grid.appendChild(card);
            });
        });

        // --- SYSTEME ECHOPPEPAY (CLIENT) ---
        window.openRechargeModal = () => document.getElementById('modal-recharge').classList.remove('hidden');
        window.closeRechargeModal = () => document.getElementById('modal-recharge').classList.add('hidden');

        window.submitRechargeRequest = async () => {
            const amount = parseInt(document.getElementById('recharge-amount').value);
            const txnId = document.getElementById('recharge-txn-id').value.trim();

            if(!amount || amount < 500 || !txnId) return showToast("Montant (min 500) ou ID manquant", "error");

            try {
                const btn = document.getElementById('btn-submit-recharge');
                btn.disabled = true; btn.innerText = "Traitement...";

                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                    userId: userProfile.uid,
                    userName: userProfile.fullName,
                    amount: amount,
                    txnId: txnId,
                    status: 'pending',
                    createdAt: serverTimestamp()
                });

                showToast("Demande envoyée ! En attente de validation.");
                closeRechargeModal();
                btn.disabled = false; btn.innerText = "Envoyer la preuve";
                document.getElementById('recharge-amount').value = "";
                document.getElementById('recharge-txn-id').value = "";
            } catch (e) { showToast("Erreur de connexion", "error"); }
        };

        const loadRechargeHistory = () => {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), s => {
                const list = document.getElementById('recharge-history');
                const mine = s.docs
                    .filter(d => d.data().userId === userProfile.uid)
                    .sort((a,b) => (b.data().createdAt?.seconds || 0) - (a.data().createdAt?.seconds || 0));

                if(mine.length === 0) return;
                list.innerHTML = "";
                mine.forEach(d => {
                    const t = d.data();
                    const statusColor = t.status === 'pending' ? 'bg-orange-100 text-orange-600' : 'bg-emerald-100 text-emerald-600';
                    const item = document.createElement('div');
                    item.className = "flex justify-between items-center p-4 bg-slate-50 rounded-2xl border border-slate-100";
                    item.innerHTML = `
                        <div>
                            <p class="text-[10px] font-black text-slate-800 uppercase tracking-tighter">ID: ${t.txnId}</p>
                            <p class="text-[8px] font-bold text-slate-400 italic">${new Date(t.createdAt?.seconds*1000).toLocaleDateString()}</p>
                        </div>
                        <div class="text-right">
                            <p class="font-black text-xs">${t.amount.toLocaleString()} F</p>
                            <span class="text-[7px] font-black uppercase px-2 py-0.5 rounded-full ${statusColor}">${t.status}</span>
                        </div>
                    `;
                    list.appendChild(item);
                });
            });
        };

        // --- CONSOLE ADMIN (LOGIQUE RESERVÉE) ---
        const loadAdminDashboard = () => {
            if(!userProfile.isAdmin) return navigateTo('home');

            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), s => {
                const list = document.getElementById('admin-request-list');
                const pending = s.docs.filter(d => d.data().status === 'pending');
                document.getElementById('pending-count').innerText = `${pending.length} Requêtes`;

                if(pending.length === 0) {
                    list.innerHTML = `<p class="text-center py-16 text-slate-300 font-bold italic text-xs">Aucune demande en attente de vérification.</p>`;
                    return;
                }

                list.innerHTML = "";
                pending.forEach(d => {
                    const t = d.data();
                    const item = document.createElement('div');
                    item.className = "p-6 bg-slate-50 rounded-[32px] border border-slate-100 flex justify-between items-center animate-fadeIn";
                    item.innerHTML = `
                        <div>
                            <div class="flex items-center gap-2 mb-1">
                                <p class="text-[10px] font-black text-emerald-700 uppercase">${t.userName}</p>
                                <span class="w-1 h-1 rounded-full bg-slate-300"></span>
                                <p class="text-[9px] font-bold text-slate-400 uppercase italic">ID: ${t.txnId}</p>
                            </div>
                            <p class="text-2xl font-black text-slate-800 tracking-tighter">${t.amount.toLocaleString()} FCFA</p>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="rejectRecharge('${d.id}')" class="bg-white border border-slate-200 text-slate-400 w-12 h-12 rounded-2xl flex items-center justify-center hover:bg-red-50 hover:text-red-500 transition-all">
                                <i class="fa-solid fa-xmark"></i>
                            </button>
                            <button onclick="approveRecharge('${d.id}', '${t.userId}', ${t.amount})" class="bg-emerald-600 text-white px-6 h-12 rounded-2xl font-black text-[10px] uppercase shadow-lg shadow-emerald-100 hover:bg-emerald-700 active:scale-95 transition-all">
                                VALIDER LE CRÉDIT
                            </button>
                        </div>
                    `;
                    list.appendChild(item);
                });
            });
        };

        window.approveRecharge = async (docId, userId, amount) => {
            try {
                // 1. Créditer le compte utilisateur
                await updateDoc(doc(db, 'artifacts', appId, 'users', userId), {
                    walletBalance: increment(amount)
                });
                // 2. Marquer la demande comme complétée
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', docId), {
                    status: 'completed',
                    validatedAt: serverTimestamp()
                });
                showToast("Compte crédité avec succès !");
            } catch (e) { showToast("Erreur lors de la validation", "error"); }
        };

        window.rejectRecharge = async (id) => {
            if(confirm("Confirmer le rejet de cette demande ? (Elle sera supprimée)")) {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', id));
                showToast("Demande supprimée");
            }
        };

        // UI HELPERS
        function updateUI() {
            const isLogged = userProfile && !auth.currentUser.isAnonymous;
            document.getElementById('user-profile').classList.toggle('hidden', !isLogged);
            document.getElementById('btn-login-trigger').classList.toggle('hidden', isLogged);
            
            if(isLogged) {
                document.getElementById('user-name-nav').innerText = userProfile.fullName;
                document.getElementById('user-avatar-nav').src = `https://ui-avatars.com/api/?name=${userProfile.fullName}&background=00853f&color=fff`;
                document.getElementById('user-wallet-nav').innerText = `${userProfile.walletBalance.toLocaleString()} F`;
                document.getElementById('p-wallet-balance').innerText = `${userProfile.walletBalance.toLocaleString()} FCFA`;
                document.getElementById('view-producer').classList.toggle('hidden', userProfile.role !== 'producer');
                document.getElementById('btn-admin-panel').classList.toggle('hidden', !userProfile.isAdmin);
                document.getElementById('admin-badge').classList.toggle('hidden', !userProfile.isAdmin);
            }
        }

        window.setFilter = (p) => {
            activeFilter = p;
            document.querySelectorAll('.province-btn').forEach(b => {
                b.classList.remove('bg-emerald-600', 'text-white', 'shadow-lg');
                b.classList.add('text-slate-500');
            });
            event.target.classList.add('bg-emerald-600', 'text-white', 'shadow-lg');
            showToast(`Région : ${p}`);
        };

        function showToast(m, type = "success") {
            const t = document.getElementById('toast');
            const icon = document.getElementById('toast-icon');
            document.getElementById('toast-msg').innerText = m;
            
            if(type === "error") {
                icon.className = "w-6 h-6 rounded-full bg-red-500 flex items-center justify-center text-[10px]";
                icon.innerHTML = '<i class="fa-solid fa-xmark"></i>';
            } else {
                icon.className = "w-6 h-6 rounded-full bg-emerald-500 flex items-center justify-center text-[10px]";
                icon.innerHTML = '<i class="fa-solid fa-check"></i>';
            }
            
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        // IMAGE HANDLER
        document.getElementById('prod-img-input').addEventListener('change', e => {
            const file = e.target.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = re => {
                tempProdImg = re.target.result;
                document.getElementById('upload-preview').innerHTML = `<img src="${tempProdImg}" class="w-full h-full object-cover">`;
            };
            reader.readAsDataURL(file);
        });

        // ACTIONS PRODUITS
        window.handlePublish = async () => {
            const name = document.getElementById('prod-name').value;
            const price = parseInt(document.getElementById('prod-price').value);
            const prov = document.getElementById('prod-province').value;

            if(!name || !price || !tempProdImg) return showToast("Informations manquantes", "error");

            try {
                const btn = document.getElementById('btn-publish');
                btn.disabled = true; btn.innerText = "Mise en ligne...";
                
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name, price, province: prov, image: tempProdImg, ownerId: userProfile.uid, createdAt: serverTimestamp()
                });

                showToast("Produit publié avec succès !");
                navigateTo('home');
                btn.disabled = false; btn.innerText = "Mettre en vente";
            } catch (e) { showToast("Erreur de publication", "error"); }
        };

        window.openCheckout = async (id) => {
            if(auth.currentUser.isAnonymous) return navigateTo('auth');
            const s = await getDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', id));
            if(!s.exists()) return;
            currentCheckoutItem = { ...s.data(), id: s.id };
            
            document.getElementById('checkout-item').innerHTML = `
                <img src="${currentCheckoutItem.image}" class="w-20 h-20 rounded-2xl object-cover shadow-sm">
                <div>
                    <p class="font-black text-slate-800 text-lg">${currentCheckoutItem.name}</p>
                    <p class="text-[9px] font-bold text-emerald-600 uppercase tracking-widest">${currentCheckoutItem.province}</p>
                </div>
            `;
            document.getElementById('checkout-total').innerText = `${currentCheckoutItem.price.toLocaleString()} FCFA`;
            navigateTo('checkout');
        };

        window.processOrder = async () => {
            if(userProfile.walletBalance < currentCheckoutItem.price) {
                showToast("Solde EchoppePay insuffisant !", "error");
                return openRechargeModal();
            }

            const btn = document.getElementById('btn-confirm-order');
            btn.disabled = true; btn.innerText = "Paiement en cours...";

            try {
                // Déduction du solde
                await updateDoc(doc(db, 'artifacts', appId, 'users', userProfile.uid), {
                    walletBalance: increment(-currentCheckoutItem.price)
                });
                
                // Enregistrement de la commande
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), {
                    productId: currentCheckoutItem.id,
                    productName: currentCheckoutItem.name,
                    buyerId: userProfile.uid,
                    amount: currentCheckoutItem.price,
                    status: 'paid',
                    createdAt: serverTimestamp()
                });

                showToast("Paiement réussi ! Livraison en cours.");
                navigateTo('home');
            } catch (e) { showToast("Erreur de paiement", "error"); }
            btn.disabled = false; btn.innerText = "PAYER AVEC ECHOPPEPAY";
        };

        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // INITIALISATION DES LISTES
        const pSelect = document.getElementById('prod-province');
        provinces.forEach(p => {
            const opt = document.createElement('option');
            opt.value = p; opt.innerText = p;
            pSelect.appendChild(opt);

            const btn = document.createElement('button');
            btn.className = "province-btn text-left text-[11px] font-bold p-4 rounded-2xl text-slate-500 border border-transparent hover:bg-slate-50 transition-all";
            btn.innerText = p;
            btn.onclick = () => setFilter(p);
            document.getElementById('filter-provinces').appendChild(btn);
        });

    </script>
</body>
</html>
