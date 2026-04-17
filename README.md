<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <title>Echoppe241 | Officiel</title> 
    
    <!-- Styles & Fonts -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --primary-blue: #3a75c4;
            --accent-yellow: #fcd116;
            --subtle-green: #00853f;
            --white: #ffffff;
            --dark: #0f172a;
        }

        body { 
            font-family: 'Inter', sans-serif; 
            background-color: var(--white); 
            color: var(--dark); 
            margin: 0; 
            overflow-x: hidden; 
            -webkit-tap-highlight-color: transparent;
        }

        h1, h2, h3, .brand-font { font-family: 'Syne', sans-serif; text-transform: lowercase; }

        /* Navigation Views */
        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* UI Elements */
        .card-neo { background: var(--white); border-radius: 24px; border: 1px solid #f1f5f9; box-shadow: 0 4px 12px rgba(58, 117, 196, 0.05); }
        .btn-blue { background: var(--primary-blue); color: var(--white); padding: 18px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 11px; width: 100%; transition: 0.2s; border: none; cursor: pointer; }
        .btn-blue:active { transform: scale(0.98); opacity: 0.9; }
        .btn-blue:disabled { opacity: 0.5; cursor: not-allowed; }
        
        .input-custom { background: #f8fafc; border-radius: 16px; padding: 16px; width: 100%; font-size: 14px; border: 2px solid transparent; outline: none; transition: 0.3s; }
        .input-custom:focus { border-color: var(--primary-blue); background: var(--white); }

        #toast { position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%); z-index: 10000; background: var(--dark); color: var(--white); padding: 12px 24px; border-radius: 50px; font-size: 11px; font-weight: 700; display: none; text-align: center; min-width: 200px; box-shadow: 0 10px 25px rgba(0,0,0,0.2); }

        .nav-item { color: #cbd5e1; transition: 0.3s; text-align: center; flex: 1; cursor: pointer; }
        .nav-item.active { color: var(--primary-blue); }

        .modal-full { position: fixed; inset: 0; background: var(--white); z-index: 9000; overflow-y: auto; padding: 32px 24px; display: none; }
        
        #splash-screen { position: fixed; inset: 0; background: var(--white); z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; transition: opacity 0.5s ease; }
        
        /* Chat UI */
        .chat-bubble { max-width: 85%; padding: 12px 16px; border-radius: 20px; font-size: 14px; margin-bottom: 8px; line-height: 1.5; position: relative; word-wrap: break-word; }
        .chat-me { background: var(--primary-blue); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .chat-them { background: #f1f5f9; color: var(--dark); align-self: flex-start; border-bottom-left-radius: 4px; }
        .chat-time { font-size: 8px; opacity: 0.5; margin-top: 4px; display: block; text-align: right; }

        .order-bill-card { background: white; border: 2px solid var(--primary-blue); border-radius: 24px; padding: 20px; width: 260px; margin: 12px 0; box-shadow: 0 10px 20px rgba(58, 117, 196, 0.1); }
        .status-pill { font-weight: 900; font-size: 9px; text-transform: uppercase; padding: 6px 12px; border-radius: 50px; display: inline-flex; items-center; gap: 6px; margin-top: 12px; }
        .status-waiting { background: #fef3c7; color: #d97706; }
        .status-paid { background: #dcfce7; color: #16a34a; }
        .status-pending { background: #f1f5f9; color: #64748b; }

        .role-badge { font-size: 8px; padding: 4px 10px; border-radius: 50px; font-weight: 900; text-transform: uppercase; letter-spacing: 0.5px; }
        .role-admin { background: #fee2e2; color: #ef4444; }
        .role-seller { background: #dcfce7; color: #16a34a; }
        .role-buyer { background: #e0f2fe; color: #0284c7; }

        .photo-preview { width: 100%; height: 200px; border-radius: 24px; object-fit: cover; background: #f8fafc; display: flex; align-items: center; justify-content: center; border: 2px dashed #e2e8f0; cursor: pointer; overflow: hidden; }
        
        /* Hide scrollbars */
        ::-webkit-scrollbar { display: none; }
    </style>
</head>
<body class="pb-24">

    <div id="toast">Action confirmée !</div>

    <!-- SPLASH SCREEN -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 h-24 mb-6">
        <div class="flex gap-2">
            <div class="w-2 h-2 rounded-full bg-[#3a75c4] animate-bounce"></div>
            <div class="w-2 h-2 rounded-full bg-[#fcd116] animate-bounce [animation-delay:0.2s]"></div>
            <div class="w-2 h-2 rounded-full bg-[#00853f] animate-bounce [animation-delay:0.4s]"></div>
        </div>
    </div>

    <!-- AUTHENTICATION -->
    <div id="view-auth" class="view active min-h-screen flex items-center px-6">
        <div class="w-full max-w-sm mx-auto">
            <div class="text-center mb-12">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 mx-auto mb-6">
                <h1 class="text-4xl font-black text-blue-900 tracking-tighter">Bienvenue.</h1>
                <p class="text-slate-400 text-sm mt-2 font-medium">Le commerce gabonais 100% sécurisé.</p>
            </div>

            <div id="login-form" class="space-y-4">
                <input type="email" id="login-email" class="input-custom" placeholder="Email">
                <input type="password" id="login-pass" class="input-custom" placeholder="Mot de passe">
                <button onclick="handleLogin()" class="btn-blue shadow-xl shadow-blue-100 mt-2">Se connecter</button>
                <p class="text-center text-xs text-slate-500 font-medium">Nouveau ici ? <span onclick="toggleAuth('register')" class="text-blue-600 font-bold cursor-pointer">Créer un compte</span></p>
            </div>

            <div id="register-form" class="space-y-4 hidden">
                <input type="text" id="reg-name" class="input-custom" placeholder="Nom complet">
                <input type="email" id="reg-email" class="input-custom" placeholder="Email">
                <input type="password" id="reg-pass" class="input-custom" placeholder="Mot de passe">
                <div class="flex gap-2 p-1.5 bg-slate-100 rounded-2xl">
                    <button onclick="setRegRole('buyer')" id="role-btn-buyer" class="flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm text-blue-600 uppercase transition">Acheteur</button>
                    <button onclick="setRegRole('seller')" id="role-btn-seller" class="flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase transition">Vendeur</button>
                </div>
                <button onclick="handleRegister()" class="btn-blue bg-[#00853f] mt-2">S'inscrire</button>
                <p class="text-center text-xs text-slate-500 font-medium">Déjà un compte ? <span onclick="toggleAuth('login')" class="text-blue-600 font-bold cursor-pointer">Connexion</span></p>
            </div>
        </div>
    </div>

    <!-- APP CONTENT -->
    <div id="app-content" style="display:none">
        <!-- Header -->
        <header class="fixed top-0 inset-x-0 h-20 px-6 flex items-center justify-between bg-white/90 backdrop-blur-xl border-b border-slate-50 z-[500]">
            <div class="flex items-center gap-2">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-9 h-9">
                <h1 class="text-2xl font-black text-blue-900 italic">echoppe<span class="text-[#fcd116]">241</span></h1>
            </div>
            <div class="flex items-center gap-3">
                <div class="text-right">
                    <p id="header-balance" class="text-sm font-black text-blue-600">0 F</p>
                    <p class="text-[8px] font-black text-slate-400 uppercase tracking-widest">EchoppePay</p>
                </div>
                <img id="header-avatar" onclick="navigateTo('profile')" class="w-11 h-11 rounded-2xl bg-slate-100 border-2 border-white shadow-sm object-cover cursor-pointer">
            </div>
        </header>

        <main class="max-w-xl mx-auto px-6 pt-24 pb-28">
            <!-- ACCUEIL -->
            <div id="view-home" class="view active">
                <div class="mb-10">
                    <h2 class="text-3xl font-black text-blue-900 leading-tight">Le Marché<br><span class="text-[#fcd116]">Libre & Sûr.</span></h2>
                </div>
                <div id="product-list" class="grid grid-cols-2 gap-4">
                    <!-- Produits injectés -->
                </div>
            </div>

            <!-- MESSAGERIE -->
            <div id="view-messages" class="view">
                <h2 class="text-3xl font-black text-blue-900 mb-8">Discussions.</h2>
                <div id="conversations-list" class="space-y-3">
                    <!-- Chats injectés -->
                </div>
            </div>

            <!-- PROFIL & ADMIN -->
            <div id="view-profile" class="view">
                <div class="card-neo p-8 text-center mb-8">
                    <div class="relative w-24 h-24 mx-auto mb-6">
                        <img id="p-avatar" class="w-24 h-24 rounded-[32px] object-cover border-4 border-slate-50 shadow-lg">
                        <div id="admin-indicator" class="hidden absolute -top-1 -right-1 w-5 h-5 bg-red-500 rounded-full border-2 border-white"></div>
                    </div>
                    <h3 id="p-name" class="font-black text-2xl text-blue-900">...</h3>
                    <div id="p-role-tag" class="role-badge my-4 inline-block">...</div>
                    
                    <div class="bg-blue-600 p-6 rounded-[32px] text-white mt-4 shadow-xl shadow-blue-100">
                        <p class="text-[9px] font-bold opacity-70 uppercase tracking-widest mb-1">Portefeuille Digital</p>
                        <p id="p-balance" class="text-2xl font-black">0 FCFA</p>
                    </div>
                </div>

                <!-- SECTION ADMINISTRATION -->
                <div id="admin-panel" class="hidden mb-10 space-y-6">
                    <div class="flex items-center gap-3 mb-2">
                        <div class="w-2 h-2 rounded-full bg-red-500 animate-pulse"></div>
                        <h4 class="text-[10px] font-black text-red-500 uppercase tracking-widest">Commandes en attente de validation</h4>
                    </div>
                    <div id="admin-orders-queue" class="space-y-3">
                        <!-- Commandes à valider -->
                    </div>
                </div>

                <div class="space-y-3">
                    <button onclick="openModal('recharge-modal')" class="w-full p-5 bg-white border border-slate-100 rounded-2xl flex items-center justify-between group active:bg-slate-50 transition">
                        <span class="text-xs font-black uppercase text-slate-700">Recharger EchoppePay</span>
                        <i class="fa-solid fa-plus-circle text-blue-600 text-lg"></i>
                    </button>
                    <button id="btn-sell" onclick="openModal('publish-modal')" class="hidden w-full p-5 bg-white border border-slate-100 rounded-2xl flex items-center justify-between group active:bg-slate-50 transition">
                        <span class="text-xs font-black uppercase text-slate-700">Publier un article</span>
                        <i class="fa-solid fa-camera text-blue-600 text-lg"></i>
                    </button>
                    <button onclick="handleLogout()" class="w-full p-6 text-red-500 font-black text-[10px] uppercase tracking-widest">Déconnexion</button>
                </div>
            </div>
        </main>

        <!-- Navigation Bar -->
        <nav class="fixed bottom-0 inset-x-0 h-22 bg-white/95 backdrop-blur-md border-t border-slate-50 flex items-center justify-around z-[500] pb-6 pt-2">
            <button onclick="navigateTo('home')" id="nav-home" class="nav-item active"><i class="fa-solid fa-house-chimney text-xl"></i><p class="text-[8px] font-black mt-1">ACCUEIL</p></button>
            <button onclick="navigateTo('messages')" id="nav-messages" class="nav-item"><i class="fa-solid fa-comment-dots text-xl"></i><p class="text-[8px] font-black mt-1">CHAT</p></button>
            <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item"><i class="fa-solid fa-user-ninja text-xl"></i><p class="text-[8px] font-black mt-1">PROFIL</p></button>
        </nav>
    </div>

    <!-- MODALS -->
    
    <!-- Modal Chat -->
    <div id="chat-modal" class="modal-full !p-0">
        <div class="flex flex-col h-full bg-white">
            <div class="p-6 border-b border-slate-50 flex items-center justify-between bg-white sticky top-0 z-[100]">
                <div class="flex items-center gap-4">
                    <button onclick="closeModal('chat-modal')" class="w-11 h-11 bg-slate-50 rounded-2xl flex items-center justify-center text-slate-600"><i class="fa-solid fa-chevron-left"></i></button>
                    <div>
                        <h3 id="chat-title" class="font-black text-blue-900 text-lg leading-none mb-1">...</h3>
                        <p class="text-[9px] text-green-500 uppercase font-black tracking-widest">En ligne</p>
                    </div>
                </div>
                <div id="seller-tools" class="hidden">
                    <button onclick="openModal('bill-modal')" class="h-11 px-5 bg-blue-600 text-white rounded-2xl text-[10px] font-black uppercase flex items-center gap-2 shadow-lg shadow-blue-100"><i class="fa-solid fa-file-invoice-dollar"></i> Créer Bon</button>
                </div>
            </div>
            <div id="chat-messages" class="flex-1 overflow-y-auto p-6 flex flex-col bg-slate-50/30"></div>
            <div class="p-4 border-t border-slate-50 bg-white flex gap-2">
                <input type="text" id="chat-input" class="input-custom !py-4 flex-1" placeholder="Ecrivez votre message...">
                <button onclick="sendChatMessage()" class="w-14 h-14 bg-blue-600 text-white rounded-2xl flex items-center justify-center shadow-xl shadow-blue-100"><i class="fa-solid fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <!-- Modal Bon de Commande -->
    <div id="bill-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-10">
                <h2 class="text-3xl font-black text-blue-900">vendre.</h2>
                <button onclick="closeModal('bill-modal')" class="w-11 h-11 bg-slate-100 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>
            <div class="space-y-5">
                <div class="p-5 bg-blue-50 rounded-3xl mb-4">
                    <p class="text-[10px] font-black text-blue-700 uppercase mb-1">Sécurité Echoppe241</p>
                    <p class="text-xs text-blue-600/80 leading-relaxed font-medium">L'argent du client sera bloqué par l'administration jusqu'à la livraison effective.</p>
                </div>
                <input type="text" id="bill-item" class="input-custom" placeholder="Nom de l'article">
                <input type="number" id="bill-amount" class="input-custom" placeholder="Montant final (FCFA)">
                <button onclick="sendOrderBill()" class="btn-blue mt-4">Envoyer le bon au client</button>
            </div>
        </div>
    </div>

    <!-- Modal Publication Article -->
    <div id="publish-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-10">
                <h2 class="text-3xl font-black text-blue-900">exposer.</h2>
                <button onclick="closeModal('publish-modal')" class="w-11 h-11 bg-slate-100 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>
            <div class="space-y-5">
                <div onclick="document.getElementById('pub-file').click()" class="photo-preview" id="pub-preview-container">
                    <div class="text-center">
                        <i class="fa-solid fa-camera text-4xl text-slate-200 mb-3"></i>
                        <p class="text-[9px] font-black text-slate-400 uppercase tracking-widest">Ajouter une photo</p>
                    </div>
                    <img id="pub-preview-img" class="hidden absolute inset-0 w-full h-full object-cover">
                </div>
                <input type="file" id="pub-file" accept="image/*" class="hidden" onchange="previewImage(this)">
                
                <input type="text" id="pub-name" class="input-custom" placeholder="Titre de l'annonce">
                <input type="number" id="pub-price" class="input-custom" placeholder="Prix (FCFA)">
                <textarea id="pub-desc" class="input-custom h-32" placeholder="Description de l'article..."></textarea>
                <button onclick="publishProduct()" id="btn-pub-submit" class="btn-blue mt-4">Publier maintenant</button>
            </div>
        </div>
    </div>

    <!-- Modal Recharge -->
    <div id="recharge-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-10">
                <h2 class="text-3xl font-black text-blue-900">recharger.</h2>
                <button onclick="closeModal('recharge-modal')" class="w-11 h-11 bg-slate-100 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>
            <div class="bg-blue-600 p-8 rounded-[40px] text-white mb-8">
                <p class="text-[10px] font-black uppercase tracking-widest opacity-70 mb-2">Instructions</p>
                <p class="text-sm font-medium leading-relaxed mb-6">Effectuez votre dépôt au 077 73 60 65 (Airtel) ou 066 45 71 72 (Moov). Entrez ensuite la référence ci-dessous.</p>
                <input type="number" id="rech-amount" class="w-full bg-white/10 border border-white/20 rounded-2xl p-4 text-white placeholder:text-white/40 mb-3 outline-none" placeholder="Montant déposé">
                <input type="text" id="rech-ref" class="w-full bg-white/10 border border-white/20 rounded-2xl p-4 text-white placeholder:text-white/40 mb-6 outline-none" placeholder="Référence Transaction">
                <button onclick="submitRecharge()" class="w-full bg-white text-blue-600 p-5 rounded-2xl font-black uppercase text-[10px]">Soumettre aux admins</button>
            </div>
        </div>
    </div>

    <!-- Modal Detail Produit -->
    <div id="detail-modal" class="modal-full">
        <div id="detail-content" class="max-w-md mx-auto"></div>
    </div>

    <!-- Firebase Logic -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc, updateDoc, collection, addDoc, query, where, orderBy, serverTimestamp, increment, runTransaction } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = 'echoppe241-pwa-final-v1';

        let user = null;
        let userData = null;
        let activeChatId = null;
        let currentRegRole = 'buyer';
        let currentPubBase64 = null;

        // AUTH LISTENER
        onAuthStateChanged(auth, (u) => {
            if (u) {
                user = u;
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        userData = snap.data();
                        document.getElementById('view-auth').classList.remove('active');
                        document.getElementById('app-content').style.display = 'block';
                        syncUserUI();
                        loadAppData();
                        if(userData.role === 'admin' || userData.isAdmin) setupAdminLogic();
                    } else {
                        // Init new user profile
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
                            fullName: u.displayName || "Client Echoppe",
                            walletBalance: 0,
                            role: currentRegRole,
                            isAdmin: false,
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`
                        });
                    }
                });
            } else {
                user = null;
                document.getElementById('app-content').style.display = 'none';
                document.getElementById('view-auth').classList.add('active');
            }
            hideSplash();
        });

        // AUTH ACTIONS
        window.handleLogin = async () => {
            const e = document.getElementById('login-email').value;
            const p = document.getElementById('login-pass').value;
            if(!e || !p) return showToast("Identifiants requis");
            try { await signInWithEmailAndPassword(auth, e, p); } catch(err) { showToast("Accès refusé"); }
        };

        window.handleRegister = async () => {
            const n = document.getElementById('reg-name').value;
            const e = document.getElementById('reg-email').value;
            const p = document.getElementById('reg-pass').value;
            if(!n || p.length < 6) return showToast("Vérifiez vos informations");
            try { await createUserWithEmailAndPassword(auth, e, p); } catch(err) { showToast("Erreur d'inscription"); }
        };

        window.handleLogout = () => signOut(auth);

        window.toggleAuth = (mode) => {
            document.getElementById('login-form').classList.toggle('hidden', mode === 'register');
            document.getElementById('register-form').classList.toggle('hidden', mode === 'login');
        };

        window.setRegRole = (role) => {
            currentRegRole = role;
            document.getElementById('role-btn-buyer').className = role === 'buyer' ? 'flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm text-blue-600 uppercase transition' : 'flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase transition';
            document.getElementById('role-btn-seller').className = role === 'seller' ? 'flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm text-green-600 uppercase transition' : 'flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase transition';
        };

        // UI SYNC
        function syncUserUI() {
            document.getElementById('header-balance').innerText = `${userData.walletBalance} F`;
            document.getElementById('header-avatar').src = userData.avatar;
            document.getElementById('p-avatar').src = userData.avatar;
            document.getElementById('p-name').innerText = userData.fullName;
            document.getElementById('p-balance').innerText = `${userData.walletBalance} FCFA`;
            
            const rb = document.getElementById('p-role-tag');
            rb.innerText = userData.role;
            rb.className = `role-badge role-${userData.role}`;

            if(userData.role === 'seller' || userData.isAdmin) {
                document.getElementById('btn-sell').classList.remove('hidden');
            }
            if(userData.isAdmin) {
                document.getElementById('admin-indicator').classList.remove('hidden');
                document.getElementById('admin-panel').classList.remove('hidden');
            }
        }

        // APP LOGIC
        function loadAppData() {
            // Products
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const list = document.getElementById('product-list');
                list.innerHTML = snap.docs.map(d => {
                    const p = d.data();
                    return `<div onclick="openProduct('${d.id}')" class="card-neo overflow-hidden active:scale-95 transition">
                        <img src="${p.image || 'https://images.unsplash.com/photo-1511556820780-d912e42b4980?w=400'}" class="h-36 w-full object-cover">
                        <div class="p-4"><p class="text-[10px] font-black uppercase text-blue-900 truncate mb-1">${p.name}</p><p class="text-blue-600 font-bold text-xs">${p.price} F</p></div>
                    </div>`;
                }).join('');
            });

            // Chats List
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'chats'), where('participants', 'array-contains', user.uid)), (snap) => {
                const cList = document.getElementById('conversations-list');
                cList.innerHTML = snap.docs.map(d => {
                    const c = d.data();
                    const otherName = c.names.find(n => n !== userData.fullName);
                    return `<div onclick="openChat('${d.id}', '${otherName}')" class="p-5 bg-white border border-slate-50 rounded-[24px] flex items-center justify-between active:bg-slate-50 transition cursor-pointer">
                        <div class="flex items-center gap-4">
                            <div class="w-12 h-12 bg-blue-50 rounded-2xl flex items-center justify-center text-blue-600 font-black text-sm">${otherName[0]}</div>
                            <div><p class="text-sm font-black text-blue-900">${otherName}</p><p class="text-[9px] text-slate-400 uppercase font-black tracking-widest">Voir la discussion</p></div>
                        </div>
                        <i class="fa-solid fa-chevron-right text-slate-200"></i>
                    </div>`;
                }).join('');
                if(snap.empty) cList.innerHTML = '<div class="text-center py-10 opacity-30 font-black uppercase text-[10px]">Aucune discussion.</div>';
            });
        }

        // MESSAGING
        window.openChat = (chatId, title) => {
            activeChatId = chatId;
            document.getElementById('chat-title').innerText = title;
            document.getElementById('chat-modal').style.display = 'block';
            document.getElementById('seller-tools').style.display = userData.role === 'seller' ? 'block' : 'none';

            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'chats', chatId, 'messages'), orderBy('timestamp', 'asc')), (snap) => {
                const box = document.getElementById('chat-messages');
                box.innerHTML = snap.docs.map(d => {
                    const m = d.data();
                    const isMe = m.senderId === user.uid;
                    
                    if(m.type === 'order_bill') {
                        let btnUi = '';
                        if(m.status === 'pending') {
                            btnUi = isMe ? '<p class="text-[9px] italic opacity-40 text-center font-bold">En attente du client...</p>' : `<button onclick="buyerSubmitPayment('${d.id}', ${m.amount}, '${m.senderId}', '${m.itemName}')" class="btn-blue !py-3 !text-[9px]">Payer avec solde</button>`;
                        } else if(m.status === 'waiting_admin') {
                            btnUi = `<div class="status-pill status-waiting w-full justify-center"><i class="fa-solid fa-clock"></i> Validation Admin</div>`;
                        } else {
                            btnUi = `<div class="status-pill status-paid w-full justify-center"><i class="fa-solid fa-check-double"></i> Transaction Validée</div>`;
                        }

                        return `<div class="self-center">
                            <div class="order-bill-card">
                                <p class="text-[9px] font-black text-blue-600 uppercase mb-3">Facture Vendeur</p>
                                <h4 class="text-base font-black text-slate-800 leading-tight">${m.itemName}</h4>
                                <p class="text-2xl font-black text-blue-900 mt-2">${m.amount} FCFA</p>
                                <div class="mt-5">${btnUi}</div>
                            </div>
                        </div>`;
                    }

                    return `<div class="chat-bubble ${isMe ? 'chat-me' : 'chat-them'}">
                        ${m.text}
                        <span class="chat-time">${m.timestamp ? new Date(m.timestamp.seconds*1000).toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}) : ''}</span>
                    </div>`;
                }).join('');
                box.scrollTop = box.scrollHeight;
            });
        };

        window.sendChatMessage = async () => {
            const i = document.getElementById('chat-input');
            if(!i.value.trim()) return;
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid, text: i.value, type: 'text', timestamp: serverTimestamp()
            });
            i.value = '';
        };

        window.sendOrderBill = async () => {
            const item = document.getElementById('bill-item').value;
            const amount = parseInt(document.getElementById('bill-amount').value);
            if(!item || !amount) return showToast("Informations manquantes");

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid, itemName: item, amount: amount, type: 'order_bill', status: 'pending', timestamp: serverTimestamp()
            });
            closeModal('bill-modal');
            showToast("Facture envoyée !");
        };

        // TRANSACTIONAL FLOW
        window.buyerSubmitPayment = async (msgId, amount, sellerId, itemName) => {
            if(userData.walletBalance < amount) return showToast("Solde insuffisant");
            if(!confirm("Procéder au paiement ? L'argent sera mis sous séquestre par les admins.")) return;

            try {
                // Update message UI
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages', msgId), { status: 'waiting_admin' });
                
                // Add to Global Admin Queue
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'global_orders'), {
                    chatId: activeChatId, msgId: msgId, buyerId: user.uid, buyerName: userData.fullName,
                    sellerId: sellerId, itemName: itemName, amount: amount, status: 'waiting_admin', timestamp: serverTimestamp()
                });
                showToast("Demande transmise aux admins.");
            } catch(e) { showToast("Erreur système."); }
        };

        window.adminProcessTransaction = async (orderId) => {
            const oRef = doc(db, 'artifacts', appId, 'public', 'data', 'global_orders', orderId);
            try {
                await runTransaction(db, async (tx) => {
                    const snap = await tx.get(oRef);
                    const o = snap.data();
                    
                    const buyerRef = doc(db, 'artifacts', appId, 'users', o.buyerId);
                    const sellerRef = doc(db, 'artifacts', appId, 'users', o.sellerId);
                    const msgRef = doc(db, 'artifacts', appId, 'public', 'data', 'chats', o.chatId, 'messages', o.msgId);

                    const bSnap = await tx.get(buyerRef);
                    if(bSnap.data().walletBalance < o.amount) throw "Fonds insuffisants chez l'acheteur.";

                    tx.update(buyerRef, { walletBalance: increment(-o.amount) });
                    tx.update(sellerRef, { walletBalance: increment(o.amount) });
                    tx.update(msgRef, { status: 'paid' });
                    tx.update(oRef, { status: 'paid' });
                });
                showToast("Transaction approuvée !");
            } catch(e) { showToast(e); }
        };

        function setupAdminLogic() {
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'global_orders'), where('status', '==', 'waiting_admin')), (snap) => {
                const q = document.getElementById('admin-orders-queue');
                q.innerHTML = snap.docs.map(d => {
                    const o = d.data();
                    return `<div class="bg-white p-5 border border-red-50 rounded-3xl flex items-center justify-between shadow-sm">
                        <div><p class="text-[10px] font-black text-blue-900 uppercase">${o.itemName}</p><p class="text-xs font-bold text-slate-500">${o.amount} F</p></div>
                        <button onclick="adminProcessTransaction('${d.id}')" class="px-4 py-2 bg-green-600 text-white rounded-xl text-[9px] font-black uppercase">Approuver</button>
                    </div>`;
                }).join('');
                if(snap.empty) q.innerHTML = '<p class="text-[9px] text-slate-300 italic text-center font-black">Aucune commande.</p>';
            });
        }

        // PRODUCT ACTIONS
        window.previewImage = (input) => {
            const f = input.files[0];
            if(!f) return;
            const reader = new FileReader();
            reader.onload = (e) => {
                document.getElementById('pub-preview-img').src = e.target.result;
                document.getElementById('pub-preview-img').classList.remove('hidden');
                currentPubBase64 = e.target.result;
            };
            reader.readAsDataURL(f);
        };

        window.publishProduct = async () => {
            const n = document.getElementById('pub-name').value;
            const p = document.getElementById('pub-price').value;
            const d = document.getElementById('pub-desc').value;
            if(!n || !p || !currentPubBase64) return showToast("Informations et photo requises");

            const btn = document.getElementById('btn-pub-submit');
            btn.disabled = true; btn.innerText = "ENVOI...";

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name: n, price: parseInt(p), description: d, image: currentPubBase64,
                    sellerId: user.uid, sellerName: userData.fullName, createdAt: serverTimestamp()
                });
                showToast("Publié avec succès !");
                closeModal('publish-modal');
                // reset
                document.getElementById('pub-name').value = ""; document.getElementById('pub-price').value = "";
                document.getElementById('pub-desc').value = ""; document.getElementById('pub-preview-img').classList.add('hidden');
            } catch(e) { showToast("Erreur publication."); }
            btn.disabled = false; btn.innerText = "PUBLIER MAINTENANT";
        };

        window.openProduct = (id) => {
            onSnapshot(doc(db, 'artifacts', appId, 'public', 'data', 'products', id), (snap) => {
                const p = snap.data();
                document.getElementById('detail-content').innerHTML = `
                    <button onclick="closeModal('detail-modal')" class="w-11 h-11 bg-slate-50 rounded-2xl mb-8 flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
                    <img src="${p.image}" class="w-full h-72 object-cover rounded-[40px] mb-8 shadow-2xl">
                    <h2 class="text-3xl font-black text-blue-900 tracking-tight mb-2">${p.name}</h2>
                    <p class="text-2xl font-black text-blue-600 mb-6">${p.price} FCFA</p>
                    <div class="bg-slate-50 p-6 rounded-[32px] mb-10">
                        <p class="text-xs font-black text-slate-400 uppercase tracking-widest mb-3">Détails de l'offre</p>
                        <p class="text-slate-600 leading-relaxed text-sm font-medium">${p.description || 'Pas de description.'}</p>
                    </div>
                    <button onclick="startDirectChat('${p.sellerId}', '${p.sellerName}')" class="btn-blue shadow-xl shadow-blue-100">Contacter le vendeur</button>
                `;
                openModal('detail-modal');
            });
        };

        window.startDirectChat = async (sid, sn) => {
            if(sid === user.uid) return showToast("C'est votre annonce");
            const cid = [user.uid, sid].sort().join('_');
            await setDoc(doc(db, 'artifacts', appId, 'public', 'data', 'chats', cid), {
                participants: [user.uid, sid], names: [userData.fullName, sn]
            }, {merge:true});
            closeModal('detail-modal');
            openChat(cid, sn);
        };

        window.submitRecharge = async () => {
            const a = document.getElementById('rech-amount').value;
            const r = document.getElementById('rech-ref').value;
            if(!a || !r) return showToast("Remplissez tout");
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharge_requests'), {
                userId: user.uid, amount: parseInt(a), ref: r, status: 'pending', createdAt: serverTimestamp()
            });
            showToast("Demande envoyée aux admins !");
            closeModal('recharge-modal');
        };

        // NAV & HELPERS
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(e => e.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(`nav-${v}`).classList.add('active');
        };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.showToast = (m) => {
            const t = document.getElementById('toast'); t.innerText = m; t.style.display = 'block';
            setTimeout(() => t.style.display = 'none', 3000);
        };
        function hideSplash() {
            const s = document.getElementById('splash-screen');
            if(s) { s.style.opacity = '0'; setTimeout(() => s.style.display = 'none', 500); }
        }
    </script>
</body>
</html>
