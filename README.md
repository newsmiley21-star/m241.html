<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Plateforme Officielle</title> 
    
    <!-- Favicon et Icônes -->
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
        :root { --gabon-green: #00853f; --gabon-yellow: #fcd116; --gabon-blue: #3a75c4; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; background: #f8fafc; }
        .glass { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px); }
        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .echoppe-card { background: linear-gradient(135deg, #00853f 0%, #005f2d 100%); }
        .btn-pay { @apply bg-[#fcd116] text-slate-900 font-black py-4 rounded-2xl shadow-lg active:scale-95 transition-all w-full flex justify-center items-center gap-2; }
    </style>
</head>
<body class="text-slate-900 overflow-x-hidden">

    <!-- NAVIGATION -->
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
                    <p id="user-name-nav" class="text-xs font-bold text-slate-400">Chargement...</p>
                </div>
                <div class="w-10 h-10 rounded-full echoppe-card flex items-center justify-center text-white font-bold border-2 border-white shadow-sm" id="user-initials">?</div>
            </div>
        </div>
    </nav>

    <!-- 1. ACCUEIL -->
    <div id="view-home" class="view active max-w-7xl mx-auto p-4 md:p-8">
        <div class="mb-10">
            <h1 class="text-5xl font-black italic uppercase leading-none mb-4">Le Marché <span class="text-emerald-600">Digital</span></h1>
            <p class="text-slate-400 font-semibold max-w-md">Soutenez nos planteurs. Achetez local, payez en toute sécurité avec EchoppePay.</p>
        </div>
        <div class="grid grid-cols-1 lg:grid-cols-4 gap-8">
            <div class="lg:col-span-1">
                <div class="bg-white rounded-[32px] p-6 shadow-sm border border-slate-100 sticky top-28">
                    <h3 class="font-black uppercase text-xs text-slate-400 mb-6 tracking-widest">Provinces du Gabon</h3>
                    <div id="filter-provinces" class="flex flex-col gap-2">
                        <button onclick="setFilter('Tous')" class="province-btn text-left text-[11px] font-bold p-3 rounded-2xl bg-emerald-600 text-white shadow-lg shadow-emerald-100">Tous le Gabon</button>
                    </div>
                </div>
            </div>
            <div class="lg:col-span-3">
                <div id="market-grid" class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6"></div>
            </div>
        </div>
    </div>

    <!-- 2. AUTHENTIFICATION -->
    <div id="view-auth" class="view max-w-md mx-auto p-6 mt-10">
        <div class="bg-white rounded-[40px] p-10 shadow-2xl border border-slate-50">
            <h2 class="text-3xl font-black mb-8 uppercase italic leading-tight">Rejoindre <br><span class="text-emerald-600">La Communauté</span></h2>
            <div class="space-y-4">
                <input type="text" id="auth-name" placeholder="Nom complet" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold">
                <input type="email" id="auth-email" placeholder="Email" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold">
                <input type="password" id="auth-pass" placeholder="Mot de passe" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold">
                <select id="auth-role" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold text-slate-600">
                    <option value="buyer">Je suis un Acheteur</option>
                    <option value="producer">Je suis un Producteur / Vendeur</option>
                </select>
                <button onclick="handleAuth()" class="w-full bg-[#00853f] text-white font-black py-5 rounded-2xl shadow-xl uppercase tracking-widest mt-4">Créer mon compte</button>
            </div>
        </div>
    </div>

    <!-- 3. PROFIL -->
    <div id="view-profile" class="view max-w-2xl mx-auto p-4 md:p-8">
        <div class="echoppe-card rounded-[40px] p-8 text-white mb-8 relative overflow-hidden shadow-2xl">
            <div class="relative z-10">
                <div class="flex justify-between items-start mb-10">
                    <div>
                        <p class="text-[10px] font-black opacity-70 uppercase tracking-widest mb-1">Mon Solde EchoppePay</p>
                        <h2 id="p-wallet-balance" class="text-5xl font-black italic">0 FCFA</h2>
                    </div>
                    <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-12 h-12 brightness-200">
                </div>
                <div class="flex gap-3">
                    <button onclick="openRechargeModal()" class="bg-white text-emerald-900 font-black px-8 py-4 rounded-2xl text-xs uppercase shadow-lg">Recharger</button>
                    <button id="admin-access-btn" onclick="navigateTo('admin')" class="hidden bg-yellow-400 text-slate-900 font-black px-8 py-4 rounded-2xl text-xs uppercase shadow-lg">Console Admin</button>
                    <button onclick="handleLogout()" class="bg-emerald-800/50 text-white font-black px-8 py-4 rounded-2xl text-xs uppercase">Déconnexion</button>
                </div>
            </div>
            <i class="fa-solid fa-wallet absolute -right-6 -bottom-6 text-white/10 text-[12rem]"></i>
        </div>

        <div id="view-producer" class="hidden bg-emerald-50 rounded-[40px] p-8 border-2 border-dashed border-emerald-200 mb-8">
            <h3 class="font-black uppercase text-xl mb-6 italic text-emerald-900">Publier un produit</h3>
            <div class="space-y-4">
                <div class="h-40 rounded-3xl bg-white flex flex-col items-center justify-center border-2 border-dashed border-slate-200 cursor-pointer overflow-hidden" onclick="document.getElementById('prod-img-input').click()">
                    <div id="upload-preview" class="text-center">
                        <i class="fa-solid fa-camera text-3xl text-slate-200 mb-2"></i>
                        <p class="text-[9px] font-black text-slate-400 uppercase">Photo du produit</p>
                    </div>
                    <input type="file" id="prod-img-input" class="hidden" accept="image/*">
                </div>
                <input type="text" id="prod-name" placeholder="Nom du produit" class="w-full p-4 bg-white rounded-xl border-none font-bold">
                <input type="number" id="prod-price" placeholder="Prix (FCFA)" class="w-full p-4 bg-white rounded-xl border-none font-bold">
                <select id="prod-province" class="w-full p-4 bg-white rounded-xl border-none font-black text-emerald-600 uppercase text-xs"></select>
                <button onclick="handlePublish()" id="btn-publish" class="w-full bg-slate-900 text-white font-black py-4 rounded-xl uppercase text-xs">Mettre en vente</button>
            </div>
        </div>
        
        <div class="bg-white rounded-[32px] p-8 shadow-sm border border-slate-100">
            <h3 class="font-black uppercase text-xs text-slate-400 tracking-widest mb-6 italic">Mon Historique</h3>
            <div id="wallet-history" class="space-y-4"></div>
        </div>
    </div>

    <!-- 4. CONSOLE ADMIN (ACCÈS ADMINISTRATEUR) -->
    <div id="view-admin" class="view max-w-4xl mx-auto p-4 md:p-8">
        <div class="mb-10 flex justify-between items-end">
            <div>
                <h1 class="text-4xl font-black italic uppercase">Console <span class="text-emerald-600">Admin</span></h1>
                <p class="text-slate-400 font-bold text-xs uppercase tracking-widest mt-2">Gestion des recharges EchoppePay</p>
            </div>
            <button onclick="navigateTo('profile')" class="bg-slate-100 p-4 rounded-2xl text-xs font-black uppercase">Retour Profil</button>
        </div>

        <div class="bg-white rounded-[40px] p-8 shadow-xl border border-slate-100">
            <div class="flex items-center justify-between mb-8 pb-6 border-b border-slate-50">
                <h3 class="font-black text-slate-800 uppercase text-sm tracking-tighter">Demandes en attente de vérification</h3>
                <span id="pending-count" class="bg-emerald-100 text-emerald-700 text-[10px] font-black px-3 py-1 rounded-full">0 En attente</span>
            </div>
            
            <div id="admin-recharge-list" class="space-y-4">
                <!-- Les demandes de recharge s'affichent ici -->
                <p class="text-center py-20 text-slate-300 font-bold italic">Aucune demande en attente.</p>
            </div>
        </div>
    </div>

    <!-- 5. CHECKOUT -->
    <div id="view-checkout" class="view max-w-lg mx-auto p-4 md:p-8">
        <div class="bg-white rounded-[40px] p-10 shadow-2xl border border-slate-100 mt-6">
            <h2 class="text-3xl font-black mb-8 uppercase italic">Ma <span class="text-emerald-600">Commande</span></h2>
            <div id="checkout-item" class="flex gap-5 p-5 bg-slate-50 rounded-[24px] mb-8 border border-slate-100"></div>
            <div class="bg-emerald-900 rounded-[32px] p-8 text-white">
                <div class="flex justify-between items-center mb-6">
                    <span class="font-bold text-emerald-200">Total</span>
                    <span id="checkout-total" class="text-3xl font-black">0 FCFA</span>
                </div>
                <button onclick="processOrder()" id="btn-confirm-order" class="btn-pay">PAYER MAINTENANT</button>
            </div>
        </div>
    </div>

    <!-- MODAL RECHARGE -->
    <div id="modal-recharge" class="hidden fixed inset-0 z-[2000] bg-slate-900/90 flex items-center justify-center p-4 backdrop-blur-sm">
        <div class="bg-white rounded-[40px] p-10 max-w-md w-full animate-fadeIn">
            <h3 class="font-black uppercase italic text-2xl mb-6">Recharger <span class="text-emerald-600">EchoppePay</span></h3>
            <div class="space-y-4">
                <div class="p-4 bg-emerald-50 rounded-2xl border border-emerald-100">
                    <p class="text-[10px] font-black text-emerald-800 uppercase mb-2">Envoyez le montant au :</p>
                    <p class="font-black text-xs">Airtel : <span class="text-emerald-700">074 00 00 00</span></p>
                    <p class="font-black text-xs">Moov : <span class="text-blue-700">066 00 00 00</span></p>
                </div>
                <input type="number" id="recharge-amount" placeholder="Montant transféré" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-black">
                <input type="text" id="recharge-id" placeholder="ID de Transaction (ID Airtel/Moov)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold">
                <button onclick="submitRechargeRequest()" id="btn-submit-recharge" class="w-full bg-emerald-600 text-white font-black py-5 rounded-2xl uppercase">Envoyer la preuve</button>
                <button onclick="closeRechargeModal()" class="w-full text-slate-400 font-bold text-[10px] uppercase">Fermer</button>
            </div>
        </div>
    </div>

    <!-- TOAST -->
    <div id="toast" class="hidden fixed bottom-10 left-1/2 -translate-x-1/2 z-[3000] bg-slate-900 text-white px-8 py-4 rounded-2xl shadow-2xl flex items-center gap-3">
        <p id="toast-msg" class="text-[10px] font-black uppercase tracking-widest"></p>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, createUserWithEmailAndPassword, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, serverTimestamp, updateDoc, increment, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'echoppe241-prod-v1';

        let userProfile = null;
        let activeFilter = 'Tous';
        let currentCheckoutItem = null;
        let tempProdImg = null;

        const ADMIN_EMAIL = "admin@echoppe241.ga"; // Modifiez avec votre email admin
        const provinces = ["Estuaire", "Haut-Ogooué", "Moyen-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Ogooué-Maritime", "Woleu-Ntem"];

        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(view => view.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            if(v === 'admin') loadAdminData();
        };

        // AUTH
        window.handleAuth = async () => {
            const name = document.getElementById('auth-name').value;
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;
            const role = document.getElementById('auth-role').value;
            if(!name || !email || pass.length < 6) return showToast("Vérifiez les champs");
            try {
                const cred = await createUserWithEmailAndPassword(auth, email, pass);
                const profile = { fullName: name, role: role, email: email, walletBalance: 0, uid: cred.user.uid, isAdmin: email === ADMIN_EMAIL };
                await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), profile);
                navigateTo('home');
            } catch (e) { showToast(e.message); }
        };

        onAuthStateChanged(auth, u => {
            if (u && !u.isAnonymous) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), s => {
                    if (s.exists()) { userProfile = s.data(); updateUI(); loadHistory(); }
                });
            } else if (!u) { signInAnonymously(auth); }
            updateUI();
        });

        // MARCHÉ
        onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), s => {
            const grid = document.getElementById('market-grid'); grid.innerHTML = "";
            s.docs.forEach(d => {
                const p = d.data();
                if(activeFilter !== 'Tous' && p.province !== activeFilter) return;
                const card = document.createElement('div');
                card.className = "bg-white rounded-[32px] overflow-hidden shadow-sm border border-slate-100 p-4";
                card.innerHTML = `
                    <img src="${p.image}" class="w-full h-48 object-cover rounded-2xl mb-4">
                    <h3 class="font-black text-lg">${p.name}</h3>
                    <p class="text-[9px] font-black text-emerald-600 uppercase mb-4">${p.province}</p>
                    <div class="flex justify-between items-center">
                        <span class="font-black">${p.price.toLocaleString()} F</span>
                        <button onclick="openCheckout('${d.id}')" class="bg-slate-900 text-white p-3 rounded-xl text-[10px] font-black uppercase">Acheter</button>
                    </div>
                `;
                grid.appendChild(card);
            });
        });

        // RECHARGE (UTILISATEUR)
        window.submitRechargeRequest = async () => {
            const amount = parseInt(document.getElementById('recharge-amount').value);
            const txnId = document.getElementById('recharge-id').value;
            if(!amount || !txnId) return showToast("Informations manquantes");
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                    userId: userProfile.uid, userName: userProfile.fullName, amount, txnId, status: 'pending', createdAt: serverTimestamp()
                });
                showToast("Preuve envoyée !"); closeRechargeModal();
            } catch (e) { showToast("Erreur d'envoi"); }
        };

        // --- CONSOLE ADMIN (LOGIQUE RESERVÉE) ---
        const loadAdminData = () => {
            if(!userProfile.isAdmin) return navigateTo('home');
            
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), s => {
                const list = document.getElementById('admin-recharge-list');
                const pending = s.docs.filter(d => d.data().status === 'pending');
                document.getElementById('pending-count').innerText = `${pending.length} En attente`;
                
                if(pending.length === 0) {
                    list.innerHTML = `<p class="text-center py-20 text-slate-300 font-bold italic">Aucune recharge à valider.</p>`;
                    return;
                }

                list.innerHTML = "";
                pending.forEach(d => {
                    const t = d.data();
                    const item = document.createElement('div');
                    item.className = "p-6 bg-slate-50 rounded-[28px] border border-slate-100 flex justify-between items-center animate-fadeIn";
                    item.innerHTML = `
                        <div>
                            <p class="text-[9px] font-black text-emerald-600 uppercase tracking-widest mb-1">${t.userName}</p>
                            <p class="text-xl font-black text-slate-800">${t.amount.toLocaleString()} FCFA</p>
                            <p class="text-[10px] font-bold text-slate-400 mt-1 italic">ID : ${t.txnId}</p>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="rejectRecharge('${d.id}')" class="bg-white border border-slate-200 text-slate-400 p-4 rounded-2xl hover:bg-red-50 hover:text-red-500 transition-all">
                                <i class="fa-solid fa-xmark"></i>
                            </button>
                            <button onclick="approveRecharge('${d.id}', '${t.userId}', ${t.amount})" class="bg-emerald-600 text-white px-6 py-4 rounded-2xl font-black text-xs uppercase shadow-lg shadow-emerald-100 hover:bg-emerald-700 active:scale-95 transition-all">
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
                // 1. Créditer le compte de l'utilisateur
                await updateDoc(doc(db, 'artifacts', appId, 'users', userId), {
                    walletBalance: increment(amount)
                });
                // 2. Marquer comme validé
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', docId), {
                    status: 'completed',
                    validatedAt: serverTimestamp()
                });
                showToast("Compte crédité avec succès !");
            } catch (e) { showToast("Erreur lors de la validation"); }
        };

        window.rejectRecharge = async (id) => {
            if(confirm("Supprimer cette demande frauduleuse ?")) {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', id));
                showToast("Demande rejetée");
            }
        };

        // HELPERS UI
        function updateUI() {
            const isLogged = userProfile && !auth.currentUser.isAnonymous;
            document.getElementById('user-profile').classList.toggle('hidden', !isLogged);
            document.getElementById('btn-login-trigger').classList.toggle('hidden', isLogged);
            if(isLogged) {
                document.getElementById('user-initials').innerText = userProfile.fullName[0];
                document.getElementById('user-name-nav').innerText = userProfile.fullName;
                document.getElementById('user-wallet-nav').innerText = `${userProfile.walletBalance.toLocaleString()} F`;
                document.getElementById('p-wallet-balance').innerText = `${userProfile.walletBalance.toLocaleString()} FCFA`;
                document.getElementById('view-producer').classList.toggle('hidden', userProfile.role !== 'producer');
                document.getElementById('admin-access-btn').classList.toggle('hidden', !userProfile.isAdmin);
            }
        }

        const loadHistory = () => {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), s => {
                const hist = document.getElementById('wallet-history'); hist.innerHTML = "";
                const mine = s.docs.filter(d => d.data().userId === userProfile.uid).sort((a,b) => b.data().createdAt?.seconds - a.data().createdAt?.seconds);
                mine.forEach(d => {
                    const t = d.data();
                    const item = document.createElement('div');
                    item.className = "flex justify-between p-3 bg-slate-50 rounded-xl text-[10px]";
                    item.innerHTML = `<span class="font-bold">${t.txnId}</span> <span class="font-black ${t.status === 'pending' ? 'text-orange-400' : 'text-emerald-600'}">${t.amount} F (${t.status})</span>`;
                    hist.appendChild(item);
                });
            });
        };

        window.openRechargeModal = () => document.getElementById('modal-recharge').classList.remove('hidden');
        window.closeRechargeModal = () => document.getElementById('modal-recharge').classList.add('hidden');
        window.handleLogout = () => signOut(auth).then(() => location.reload());
        function showToast(m) { const t = document.getElementById('toast'); document.getElementById('toast-msg').innerText = m; t.classList.remove('hidden'); setTimeout(() => t.classList.add('hidden'), 3000); }

        // Init Provinces
        const pSelect = document.getElementById('prod-province');
        provinces.forEach(p => {
            const opt = document.createElement('option'); opt.value = p; opt.innerText = p; pSelect.appendChild(opt);
            const btn = document.createElement('button');
            btn.className = "province-btn text-left text-[11px] font-bold p-3 rounded-2xl text-slate-500 border border-transparent";
            btn.innerText = p; btn.onclick = () => { activeFilter = p; showToast(p); };
            document.getElementById('filter-provinces').appendChild(btn);
        });

        // Image Handling
        document.getElementById('prod-img-input').addEventListener('change', e => {
            const reader = new FileReader();
            reader.onload = r => { tempProdImg = r.target.result; document.getElementById('upload-preview').innerHTML = `<img src="${tempProdImg}" class="w-full h-full object-cover">`; };
            reader.readAsDataURL(e.target.files[0]);
        });

        window.handlePublish = async () => {
            const name = document.getElementById('prod-name').value;
            const price = parseInt(document.getElementById('prod-price').value);
            const prov = document.getElementById('prod-province').value;
            if(!name || !price || !tempProdImg) return showToast("Complets les champs");
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), { name, price, province: prov, image: tempProdImg, ownerId: userProfile.uid });
            showToast("Produit publié !"); navigateTo('home');
        };

        window.openCheckout = async (id) => {
            const s = await getDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', id));
            currentCheckoutItem = { ...s.data(), id: s.id };
            document.getElementById('checkout-item').innerHTML = `<img src="${currentCheckoutItem.image}" class="w-16 h-16 rounded-xl object-cover"><p class="font-black">${currentCheckoutItem.name}</p>`;
            document.getElementById('checkout-total').innerText = `${currentCheckoutItem.price.toLocaleString()} FCFA`;
            navigateTo('checkout');
        };

        window.processOrder = async () => {
            if(userProfile.walletBalance < currentCheckoutItem.price) return showToast("Solde insuffisant");
            await updateDoc(doc(db, 'artifacts', appId, 'users', userProfile.uid), { walletBalance: increment(-currentCheckoutItem.price) });
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), { productName: currentCheckoutItem.name, buyerId: userProfile.uid, status: 'paid' });
            showToast("Paiement validé !"); navigateTo('home');
        };

    </script>
</body>
</html>
