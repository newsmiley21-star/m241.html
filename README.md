<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Le Marché 100% Gabonais</title> 
    
    <!-- Favicon -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <meta name="theme-color" content="#00853f">
    
    <!-- Styles & Fonts -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { --gabon-green: #00853f; --gabon-yellow: #fcd116; --gabon-blue: #3a75c4; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(255, 255, 255, 0.85); backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px); }
        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .echoppe-card { background: linear-gradient(135deg, #00853f 0%, #005f2d 100%); }
        .btn-gabon { background: var(--gabon-green); color: white; transition: all 0.3s; }
        .btn-gabon:active { transform: scale(0.95); }
        ::-webkit-scrollbar { display: none; }
    </style>
</head>
<body class="bg-[#f8fafc] text-slate-900">

    <!-- NAVIGATION -->
    <nav class="sticky top-0 z-[100] glass border-b border-slate-100 px-4 h-20 flex justify-between items-center">
        <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 h-10 object-contain">
            <span class="font-extrabold text-xl tracking-tighter uppercase italic">Echoppe<span class="text-emerald-600">241</span></span>
        </div>
        
        <div class="flex items-center gap-4">
            <button id="btn-login-nav" onclick="navigateTo('auth')" class="bg-emerald-600 text-white px-5 py-2.5 rounded-2xl text-[10px] font-black uppercase shadow-md">Connexion</button>
            <div id="user-profile-nav" class="hidden flex items-center gap-3 cursor-pointer" onclick="navigateTo('profile')">
                <div class="text-right hidden sm:block">
                    <p id="user-wallet-nav" class="text-[10px] font-black text-emerald-600 uppercase">0 FCFA</p>
                    <p id="user-name-nav" class="text-xs font-bold text-slate-400 italic truncate max-w-[100px]">Chargement...</p>
                </div>
                <div class="relative">
                    <img id="user-avatar-nav" src="https://ui-avatars.com/api/?background=00853f&color=fff&name=User" class="w-10 h-10 rounded-full border-2 border-white shadow-sm object-cover">
                    <div id="admin-badge" class="hidden absolute -top-1 -right-1 bg-yellow-400 w-4 h-4 rounded-full border-2 border-white flex items-center justify-center">
                        <i class="fa-solid fa-crown text-[6px] text-slate-900"></i>
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <!-- CONTENT -->
    <main class="min-h-screen pb-24">
        
        <!-- ACCUEIL : MARCHÉ -->
        <div id="view-home" class="view active max-w-7xl mx-auto p-4 md:p-8">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-end gap-6 mb-10">
                <div>
                    <h1 class="text-4xl md:text-6xl font-black italic uppercase leading-none">Le Terroir <br><span class="text-emerald-600">Gabonais</span></h1>
                    <p class="text-slate-400 font-semibold mt-2">Achetez et vendez les meilleurs produits du Gabon.</p>
                </div>
                <div class="flex gap-2 w-full md:w-auto overflow-x-auto pb-2">
                    <button onclick="filterCategory('all')" class="whitespace-nowrap bg-white border border-slate-200 px-6 py-3 rounded-2xl font-black text-[10px] uppercase shadow-sm">Tout</button>
                    <button onclick="filterCategory('Agriculture')" class="whitespace-nowrap bg-white border border-slate-200 px-6 py-3 rounded-2xl font-black text-[10px] uppercase shadow-sm">Agriculture</button>
                    <button onclick="filterCategory('Pêche')" class="whitespace-nowrap bg-white border border-slate-200 px-6 py-3 rounded-2xl font-black text-[10px] uppercase shadow-sm">Pêche</button>
                    <button onclick="filterCategory('Artisanat')" class="whitespace-nowrap bg-white border border-slate-200 px-6 py-3 rounded-2xl font-black text-[10px] uppercase shadow-sm">Artisanat</button>
                </div>
            </div>

            <div id="market-grid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 md:gap-8">
                <!-- Les produits s'affichent ici -->
            </div>
        </div>

        <!-- PUBLIER -->
        <div id="view-publish" class="view max-w-2xl mx-auto p-4 mt-6">
            <div class="bg-white rounded-[40px] p-8 shadow-2xl border border-slate-50">
                <h2 class="text-3xl font-black uppercase italic mb-8">Vendre un <span class="text-emerald-600">produit</span></h2>
                <div class="space-y-6">
                    <div id="upload-zone" onclick="document.getElementById('prod-img').click()" class="w-full h-48 bg-slate-50 rounded-[32px] border-4 border-dashed border-slate-100 flex flex-col items-center justify-center cursor-pointer overflow-hidden">
                        <i class="fa-solid fa-camera text-3xl text-slate-200 mb-2"></i>
                        <p class="text-[10px] font-black text-slate-300 uppercase">Ajouter une photo</p>
                        <input type="file" id="prod-img" class="hidden" accept="image/*">
                    </div>
                    <input type="text" id="prod-title" placeholder="Nom du produit" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold">
                    <div class="grid grid-cols-2 gap-4">
                        <input type="number" id="prod-price" placeholder="Prix (FCFA)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold">
                        <select id="prod-cat" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold outline-none">
                            <option value="Agriculture">Agriculture</option>
                            <option value="Pêche">Pêche</option>
                            <option value="Artisanat">Artisanat</option>
                            <option value="Élevage">Élevage</option>
                        </select>
                    </div>
                    <select id="prod-prov" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold outline-none">
                        <option value="Estuaire">Estuaire (Libreville)</option>
                        <option value="Ogooué-Maritime">Ogooué-Maritime (POG)</option>
                        <option value="Haut-Ogooué">Haut-Ogooué (Franceville)</option>
                        <option value="Woleu-Ntem">Woleu-Ntem (Oyem)</option>
                    </select>
                    <button onclick="handlePublish()" id="btn-publish" class="w-full bg-emerald-600 text-white font-black py-5 rounded-3xl shadow-xl uppercase tracking-widest">Mettre en vente</button>
                </div>
            </div>
        </div>

        <!-- AUTH -->
        <div id="view-auth" class="view max-w-md mx-auto p-6 mt-10">
            <div class="bg-white rounded-[40px] p-10 shadow-2xl">
                <h2 id="auth-title" class="text-3xl font-black uppercase italic mb-8 text-center">Connexion</h2>
                <div class="space-y-4">
                    <div id="auth-name-container" class="hidden">
                        <input type="text" id="auth-name" placeholder="Nom complet" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold">
                    </div>
                    <input type="email" id="auth-email" placeholder="Email" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold">
                    <input type="password" id="auth-pass" placeholder="Mot de passe" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold">
                    <button onclick="handleAuthSubmit()" id="btn-auth-submit" class="w-full bg-[#00853f] text-white font-black py-5 rounded-2xl shadow-xl uppercase tracking-widest">Accéder</button>
                    <button onclick="toggleAuthMode()" id="btn-toggle-mode" class="w-full text-[10px] font-black uppercase text-slate-400 mt-4 underline">Pas de compte ? Créer ici</button>
                </div>
            </div>
        </div>

        <!-- PROFIL -->
        <div id="view-profile" class="view max-w-2xl mx-auto p-4 md:p-8">
            <div class="echoppe-card rounded-[40px] p-8 text-white mb-8 relative overflow-hidden shadow-2xl">
                <div class="relative z-10">
                    <p class="text-[10px] font-black opacity-70 uppercase tracking-widest mb-1">Mon Solde EchoppePay</p>
                    <h2 id="p-wallet-balance" class="text-5xl font-black italic tracking-tighter mb-8">0 FCFA</h2>
                    <div class="flex flex-wrap gap-3">
                        <button onclick="openRechargeModal()" class="bg-white text-emerald-900 font-black px-8 py-4 rounded-2xl text-[10px] uppercase shadow-lg">Recharger</button>
                        <button id="btn-admin-view" onclick="navigateTo('admin')" class="hidden bg-yellow-400 text-slate-900 font-black px-8 py-4 rounded-2xl text-[10px] uppercase">Console Admin</button>
                        <button onclick="handleLogout()" class="bg-emerald-800/50 text-white font-black px-8 py-4 rounded-2xl text-[10px] uppercase">Déconnexion</button>
                    </div>
                </div>
            </div>

            <div class="bg-white rounded-[32px] p-8 shadow-sm border border-slate-100">
                <h3 class="font-black uppercase text-xs text-slate-400 tracking-widest mb-6">Activités Récentes</h3>
                <div id="recharge-history" class="space-y-4">
                    <!-- Transactions ici -->
                </div>
            </div>
        </div>

        <!-- CONSOLE ADMIN -->
        <div id="view-admin" class="view max-w-4xl mx-auto p-4 md:p-8">
            <div class="flex justify-between items-end mb-10">
                <div>
                    <h1 class="text-4xl font-black italic uppercase tracking-tighter">Console <span class="text-emerald-600">Admin</span></h1>
                    <p class="text-[10px] font-black uppercase text-slate-400 mt-1">Validation des transferts mobiles</p>
                </div>
                <div class="text-right">
                    <p class="text-[9px] font-black uppercase text-slate-400">En attente</p>
                    <h4 id="admin-total-sum" class="text-2xl font-black text-emerald-600">0 FCFA</h4>
                </div>
            </div>

            <div class="bg-white rounded-[40px] p-8 shadow-xl border border-slate-100">
                <div id="admin-request-list" class="space-y-4">
                    <!-- Demandes de recharge admin -->
                </div>
            </div>
        </div>

    </main>

    <!-- TAB BAR -->
    <nav class="fixed bottom-0 left-0 right-0 glass border-t border-slate-100 h-20 flex items-center justify-around px-6 z-[1000]">
        <button onclick="navigateTo('home')" class="flex flex-col items-center gap-1">
            <i class="fa-solid fa-house text-xl text-slate-400"></i>
            <span class="text-[8px] font-black uppercase">Accueil</span>
        </button>
        <button onclick="navigateTo('publish')" class="flex flex-col items-center -translate-y-6">
            <div class="bg-emerald-600 w-14 h-14 rounded-full flex items-center justify-center text-white shadow-xl border-4 border-white">
                <i class="fa-solid fa-plus text-xl"></i>
            </div>
            <span class="text-[8px] font-black uppercase mt-1 text-emerald-600">Vendre</span>
        </button>
        <button onclick="navigateTo('profile')" class="flex flex-col items-center gap-1">
            <i class="fa-solid fa-wallet text-xl text-slate-400"></i>
            <span class="text-[8px] font-black uppercase">Compte</span>
        </button>
    </nav>

    <!-- MODAL RECHARGE -->
    <div id="modal-recharge" class="hidden fixed inset-0 z-[2000] bg-slate-900/90 flex items-center justify-center p-4 backdrop-blur-md">
        <div class="bg-white rounded-[40px] p-8 max-w-md w-full relative">
            <h3 class="font-black uppercase italic text-2xl text-center mb-6 leading-tight">Créditer mon <br><span class="text-emerald-600">Portefeuille</span></h3>
            <div class="space-y-5">
                <div class="p-5 bg-emerald-50 rounded-2xl border border-emerald-100">
                    <p class="text-[10px] font-black text-emerald-800 uppercase mb-3">Numéros de transfert :</p>
                    <div class="flex justify-between items-center mb-2">
                        <span class="text-xs font-bold">Airtel Money</span>
                        <span class="font-black text-sm">077 73 60 65</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-xs font-bold">Moov Money</span>
                        <span class="font-black text-sm">066 45 71 72</span>
                    </div>
                </div>
                <input type="number" id="recharge-amount" placeholder="Montant versé (FCFA)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-black text-center text-lg">
                <input type="text" id="recharge-txn-id" placeholder="ID Transaction (Ex: MP23...)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold uppercase text-center">
                <button onclick="submitRecharge()" class="w-full bg-emerald-600 text-white font-black py-5 rounded-2xl uppercase shadow-lg tracking-widest">Soumettre la preuve</button>
                <button onclick="closeRechargeModal()" class="w-full text-slate-400 font-bold text-[10px] uppercase tracking-widest">Retour</button>
            </div>
        </div>
    </div>

    <!-- TOAST -->
    <div id="toast" class="hidden fixed top-24 left-1/2 -translate-x-1/2 z-[3000] bg-slate-900 text-white px-8 py-4 rounded-3xl shadow-2xl flex items-center gap-4 transition-all">
        <p id="toast-msg" class="text-[10px] font-black uppercase tracking-widest"></p>
    </div>

    <!-- Firebase Script -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, serverTimestamp, updateDoc, increment, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // CONFIGURATION OFFICIELLE
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            databaseURL: "https://communautedugabon-default-rtdb.firebaseio.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f",
            measurementId: "G-T7415FPS91"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = "echoppe241-v1";
        const ADMIN_EMAIL = "admin@echoppe241.ga";

        let userProfile = null;
        let authMode = 'login';
        let currentImgBase64 = null;

        // NAVIGATION
        window.navigateTo = (view) => {
            if(view === 'admin' && (!userProfile || !userProfile.isAdmin)) return showToast("Accès refusé", "error");
            if((view === 'publish' || view === 'profile') && (!userProfile || auth.currentUser.isAnonymous)) {
                navigateTo('auth');
                return;
            }
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(`view-${view}`).classList.add('active');
            if(view === 'admin') initAdminListener();
            window.scrollTo(0,0);
        };

        // AUTHENTICATION
        window.toggleAuthMode = () => {
            authMode = authMode === 'login' ? 'signup' : 'login';
            document.getElementById('auth-name-container').classList.toggle('hidden', authMode === 'login');
            document.getElementById('auth-title').innerText = authMode === 'login' ? 'Connexion' : 'Rejoindre Echoppe241';
        };

        window.handleAuthSubmit = async () => {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;
            if(!email || pass.length < 6) return showToast("Veuillez vérifier vos accès", "error");

            try {
                if(authMode === 'login') {
                    await signInWithEmailAndPassword(auth, email, pass);
                } else {
                    const name = document.getElementById('auth-name').value;
                    if(!name) return showToast("Entrez votre nom", "error");
                    const cred = await createUserWithEmailAndPassword(auth, email, pass);
                    const profile = {
                        uid: cred.user.uid,
                        fullName: name,
                        email: email,
                        walletBalance: 0,
                        isAdmin: email === ADMIN_EMAIL,
                        createdAt: serverTimestamp()
                    };
                    await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), profile);
                }
                navigateTo('home');
            } catch (e) { showToast("Erreur: " + e.message, "error"); }
        };

        onAuthStateChanged(auth, (user) => {
            if (user && !user.isAnonymous) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', user.uid), (snap) => {
                    if (snap.exists()) {
                        userProfile = snap.data();
                        updateUI();
                        loadMyRecharges();
                    }
                });
            } else {
                if(!user) signInAnonymously(auth);
                userProfile = null;
                updateUI();
            }
        });

        function updateUI() {
            const isLogged = userProfile && !auth.currentUser.isAnonymous;
            document.getElementById('btn-login-nav').classList.toggle('hidden', isLogged);
            document.getElementById('user-profile-nav').classList.toggle('hidden', !isLogged);

            if (isLogged) {
                document.getElementById('user-name-nav').innerText = userProfile.fullName;
                document.getElementById('user-wallet-nav').innerText = `${userProfile.walletBalance.toLocaleString()} FCFA`;
                document.getElementById('p-wallet-balance').innerText = `${userProfile.walletBalance.toLocaleString()} FCFA`;
                document.getElementById('btn-admin-view').classList.toggle('hidden', !userProfile.isAdmin);
                document.getElementById('admin-badge').classList.toggle('hidden', !userProfile.isAdmin);
            }
        }

        // MARCHÉ & PRODUITS
        function initMarket() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const grid = document.getElementById('market-grid');
                if(snap.empty) {
                    grid.innerHTML = `<div class="col-span-full py-20 text-center opacity-30 font-black uppercase text-xs">Aucun produit en vente</div>`;
                    return;
                }
                const prods = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderProducts(prods);
            });
        }
        initMarket();

        function renderProducts(prods) {
            const grid = document.getElementById('market-grid');
            grid.innerHTML = prods.map(p => `
                <div class="bg-white rounded-[32px] overflow-hidden shadow-sm border border-slate-100 group">
                    <div class="h-40 md:h-56 overflow-hidden relative">
                        <img src="${p.image || 'https://via.placeholder.com/400'}" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500">
                        <div class="absolute top-3 left-3 bg-white/90 backdrop-blur px-3 py-1 rounded-full text-[8px] font-black uppercase tracking-tighter">${p.category}</div>
                    </div>
                    <div class="p-4 md:p-6">
                        <h3 class="font-bold text-sm md:text-base truncate mb-1 uppercase tracking-tight">${p.title}</h3>
                        <p class="text-[9px] font-bold text-slate-400 uppercase mb-4"><i class="fa-solid fa-location-dot mr-1 text-emerald-600"></i>${p.province}</p>
                        <div class="flex justify-between items-center">
                            <span class="text-emerald-600 font-black text-sm md:text-lg">${p.price.toLocaleString()} <small class="text-[10px]">FCFA</small></span>
                            <button onclick="contactSeller('${p.sellerName}')" class="bg-slate-100 w-10 h-10 rounded-full flex items-center justify-center text-slate-900 shadow-sm"><i class="fa-solid fa-cart-shopping text-xs"></i></button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // PUBLICATION
        document.getElementById('prod-img').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = (re) => {
                currentImgBase64 = re.target.result;
                document.getElementById('upload-zone').innerHTML = `<img src="${currentImgBase64}" class="w-full h-full object-cover">`;
            };
            reader.readAsDataURL(file);
        });

        window.handlePublish = async () => {
            const title = document.getElementById('prod-title').value;
            const price = parseInt(document.getElementById('prod-price').value);
            const category = document.getElementById('prod-cat').value;
            const province = document.getElementById('prod-prov').value;

            if(!title || !price || !currentImgBase64) return showToast("Infos manquantes", "error");

            try {
                document.getElementById('btn-publish').disabled = true;
                document.getElementById('btn-publish').innerText = "Publication...";
                
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    title, price, category, province,
                    image: currentImgBase64,
                    sellerId: userProfile.uid,
                    sellerName: userProfile.fullName,
                    createdAt: serverTimestamp()
                });

                showToast("Produit ajouté au marché !");
                navigateTo('home');
                // Reset form
                document.getElementById('prod-title').value = '';
                document.getElementById('prod-price').value = '';
                currentImgBase64 = null;
                document.getElementById('upload-zone').innerHTML = `<i class="fa-solid fa-camera text-3xl text-slate-200 mb-2"></i><p class="text-[10px] font-black text-slate-300 uppercase">Ajouter une photo</p>`;
            } catch (e) { showToast("Erreur lors de l'ajout", "error"); }
            finally { document.getElementById('btn-publish').disabled = false; document.getElementById('btn-publish').innerText = "Mettre en vente"; }
        };

        // ECHOPPEPAY : RECHARGE
        window.openRechargeModal = () => document.getElementById('modal-recharge').classList.remove('hidden');
        window.closeRechargeModal = () => document.getElementById('modal-recharge').classList.add('hidden');

        window.submitRecharge = async () => {
            const amount = parseInt(document.getElementById('recharge-amount').value);
            const txnId = document.getElementById('recharge-txn-id').value.trim();
            if(!amount || !txnId) return showToast("Veuillez remplir le montant et l'ID", "error");

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                    userId: userProfile.uid,
                    userName: userProfile.fullName,
                    amount, txnId,
                    status: 'pending',
                    timestamp: serverTimestamp()
                });
                showToast("Demande envoyée ! En attente de validation.");
                closeRechargeModal();
            } catch (e) { showToast("Erreur d'envoi", "error"); }
        };

        function loadMyRecharges() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), (snap) => {
                const list = document.getElementById('recharge-history');
                const mine = snap.docs.map(d => d.data()).filter(d => d.userId === userProfile.uid);
                if(mine.length === 0) {
                    list.innerHTML = `<p class="text-center py-6 text-slate-300 font-bold text-[10px] italic">Aucun mouvement.</p>`;
                    return;
                }
                list.innerHTML = mine.map(t => `
                    <div class="flex justify-between items-center p-4 bg-slate-50 rounded-2xl border border-slate-100">
                        <div>
                            <p class="text-[9px] font-black text-slate-400 uppercase">ID: ${t.txnId}</p>
                            <p class="font-bold text-slate-900">${t.amount.toLocaleString()} FCFA</p>
                        </div>
                        <div class="text-[9px] font-black uppercase ${t.status === 'completed' ? 'text-emerald-600' : 'text-orange-500'}">
                            ${t.status === 'completed' ? 'Validé' : 'Vérification...'}
                        </div>
                    </div>
                `).join('');
            });
        }

        // ADMIN : VALIDATION
        function initAdminListener() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), (snap) => {
                const list = document.getElementById('admin-request-list');
                const pending = snap.docs
                    .map(d => ({id: d.id, ...d.data()}))
                    .filter(d => d.status === 'pending');

                const total = pending.reduce((acc, curr) => acc + curr.amount, 0);
                document.getElementById('admin-total-sum').innerText = `${total.toLocaleString()} FCFA`;

                if(pending.length === 0) {
                    list.innerHTML = `<div class="text-center py-20 opacity-30 font-black text-xs uppercase">Rien à valider</div>`;
                    return;
                }

                list.innerHTML = pending.map(t => `
                    <div class="p-6 bg-slate-50 rounded-3xl flex flex-col md:flex-row justify-between items-center gap-6 border border-slate-200">
                        <div class="text-center md:text-left">
                            <p class="text-emerald-800 font-black uppercase text-[9px] tracking-widest">${t.userName}</p>
                            <h4 class="text-3xl font-black mt-1">${t.amount.toLocaleString()} FCFA</h4>
                            <p class="text-[10px] font-mono font-bold text-slate-500 mt-2 bg-white px-3 py-1 rounded-lg inline-block border">ID TXN: ${t.txnId}</p>
                        </div>
                        <div class="flex gap-2 w-full md:w-auto">
                            <button onclick="approveRecharge('${t.id}', '${t.userId}', ${t.amount})" class="w-full bg-emerald-600 text-white px-8 py-4 rounded-2xl font-black text-[10px] shadow-lg uppercase">Valider le dépôt</button>
                        </div>
                    </div>
                `).join('');
            });
        }

        window.approveRecharge = async (docId, uid, amount) => {
            if(!confirm(`Créditer ${amount} FCFA au portefeuille de l'utilisateur ?`)) return;
            try {
                await updateDoc(doc(db, 'artifacts', appId, 'users', uid), { walletBalance: increment(amount) });
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', docId), { status: 'completed' });
                showToast("Dépôt validé !");
            } catch (e) { showToast("Erreur admin", "error"); }
        };

        // UTILS
        function showToast(msg, type = 'success') {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            t.className = `fixed top-24 left-1/2 -translate-x-1/2 z-[3000] text-white px-8 py-4 rounded-3xl shadow-2xl transition-all ${type === 'error' ? 'bg-red-600' : 'bg-slate-900'}`;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3000);
        }

        window.contactSeller = (name) => showToast(`Contact de ${name} via WhatsApp...`);
        window.handleLogout = () => signOut(auth).then(() => location.reload());

    </script>
</body>
</html>
