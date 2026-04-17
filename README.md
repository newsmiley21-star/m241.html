<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Officiel</title> 
    
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

        body { font-family: 'Inter', sans-serif; background-color: var(--white); color: var(--dark); margin: 0; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        h1, h2, h3, .brand-font { font-family: 'Syne', sans-serif; text-transform: lowercase; }

        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .card-neo { background: var(--white); border-radius: 24px; border: 1px solid #f1f5f9; box-shadow: 0 4px 12px rgba(58, 117, 196, 0.05); }
        .btn-blue { background: var(--primary-blue); color: var(--white); padding: 18px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 11px; width: 100%; transition: 0.2s; border: none; cursor: pointer; }
        .btn-blue:active { transform: scale(0.98); opacity: 0.9; }
        
        .input-custom { background: #f8fafc; border-radius: 16px; padding: 16px; width: 100%; font-size: 14px; border: 2px solid transparent; outline: none; transition: 0.3s; }
        .input-custom:focus { border-color: var(--primary-blue); background: var(--white); }

        #toast { position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%); z-index: 10000; background: var(--dark); color: var(--white); padding: 12px 24px; border-radius: 50px; font-size: 11px; font-weight: 700; display: none; text-align: center; min-width: 200px; box-shadow: 0 10px 25px rgba(0,0,0,0.2); }

        .nav-item { color: #cbd5e1; transition: 0.3s; text-align: center; flex: 1; cursor: pointer; border: none; background: none; }
        .nav-item.active { color: var(--primary-blue); }

        .modal-full { position: fixed; inset: 0; background: var(--white); z-index: 9000; overflow-y: auto; padding: 32px 24px; display: none; }
        
        #splash-screen { position: fixed; inset: 0; background: var(--white); z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; transition: opacity 0.5s ease; }
        
        /* Chat Styles */
        .chat-bubble { max-width: 85%; padding: 12px 16px; border-radius: 20px; font-size: 14px; margin-bottom: 8px; position: relative; word-wrap: break-word; }
        .chat-me { background: var(--primary-blue); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .chat-them { background: #f1f5f9; color: var(--dark); align-self: flex-start; border-bottom-left-radius: 4px; }
        
        .order-card { background: #fff; border: 2px solid var(--primary-blue); border-radius: 20px; padding: 16px; margin: 10px 0; color: var(--dark); }
        .voice-wave { display: flex; align-items: center; gap: 2px; height: 16px; }
        .voice-bar { width: 3px; background: currentColor; border-radius: 10px; animation: wave 1s infinite ease-in-out; }
        @keyframes wave { 0%, 100% { height: 4px; } 50% { height: 14px; } }

        ::-webkit-scrollbar { display: none; }
    </style>
</head>
<body class="pb-24">

    <div id="toast">Action effectuée</div>

    <!-- Splash Screen -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 h-24 mb-6">
        <div class="flex gap-2">
            <div class="w-2 h-2 rounded-full bg-[#3a75c4] animate-bounce"></div>
            <div class="w-2 h-2 rounded-full bg-[#fcd116] animate-bounce [animation-delay:0.2s]"></div>
            <div class="w-2 h-2 rounded-full bg-[#00853f] animate-bounce [animation-delay:0.4s]"></div>
        </div>
    </div>

    <!-- Authentification -->
    <div id="view-auth" class="view active min-h-screen flex items-center px-6">
        <div class="w-full max-w-sm mx-auto text-center">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-20 mx-auto mb-6">
            <h1 class="text-4xl font-black text-blue-900 tracking-tighter mb-12 italic">echoppe<span class="text-[#fcd116]">241</span></h1>
            
            <div id="login-form" class="space-y-4">
                <input type="email" id="login-email" class="input-custom" placeholder="Email">
                <input type="password" id="login-pass" class="input-custom" placeholder="Mot de passe">
                <button onclick="handleLogin()" class="btn-blue shadow-xl shadow-blue-100">Connexion</button>
                <p class="text-xs text-slate-500">Pas de compte ? <span onclick="toggleAuth('register')" class="text-blue-600 font-bold underline cursor-pointer">S'inscrire</span></p>
            </div>

            <div id="register-form" class="space-y-4 hidden text-left">
                <input type="text" id="reg-name" class="input-custom" placeholder="Nom complet">
                <input type="email" id="reg-email" class="input-custom" placeholder="Email">
                <input type="password" id="reg-pass" class="input-custom" placeholder="Mot de passe (6 car. min)">
                <input type="password" id="reg-admin" class="input-custom" placeholder="Code Admin (Optionnel)">
                <button onclick="handleRegister()" class="btn-blue bg-[#00853f]">Créer mon compte</button>
                <p class="text-center text-xs text-slate-500">Déjà inscrit ? <span onclick="toggleAuth('login')" class="text-blue-600 font-bold underline cursor-pointer">Se connecter</span></p>
            </div>
        </div>
    </div>

    <!-- App Content -->
    <div id="app-content" style="display:none">
        <header class="fixed top-0 inset-x-0 h-20 px-6 flex items-center justify-between bg-white/90 backdrop-blur-xl border-b border-slate-50 z-[500]">
            <div class="flex items-center gap-2">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-8 h-8">
                <h1 class="text-xl font-black text-blue-900 italic">echoppe<span class="text-[#fcd116]">241</span></h1>
            </div>
            <div class="flex items-center gap-3">
                <div class="text-right">
                    <p id="header-balance" class="text-sm font-black text-blue-600">0 F</p>
                </div>
                <img id="header-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-xl bg-slate-100 object-cover cursor-pointer">
            </div>
        </header>

        <main class="max-w-xl mx-auto px-6 pt-24 pb-28">
            <!-- Accueil / Marché -->
            <div id="view-home" class="view active">
                <h2 class="text-2xl font-black text-blue-900 mb-6 tracking-tight">Le Marché.</h2>
                <div id="product-list" class="grid grid-cols-2 gap-4"></div>
            </div>

            <!-- Messages / Discussions -->
            <div id="view-messages" class="view">
                <h2 class="text-2xl font-black text-blue-900 mb-6 tracking-tight">Discussions.</h2>
                <div id="conversations-list" class="space-y-3"></div>
            </div>

            <!-- Profil Utilisateur -->
            <div id="view-profile" class="view">
                <div class="card-neo p-6 text-center mb-6">
                    <div class="relative w-20 h-20 mx-auto mb-4">
                        <img id="p-avatar" class="w-20 h-20 rounded-3xl object-cover border-4 border-white shadow-md">
                        <div id="admin-badge" class="hidden absolute -top-1 -right-1 bg-red-500 text-white text-[8px] font-black px-2 py-1 rounded-full">ADMIN</div>
                    </div>
                    <h3 id="p-name" class="font-black text-xl text-blue-900">...</h3>
                    <p id="p-bio" class="text-xs text-slate-400 mt-1 mb-4 italic">Chargement...</p>
                    
                    <div class="bg-blue-600 p-5 rounded-3xl text-white text-left shadow-lg">
                        <p class="text-[8px] font-black opacity-60 uppercase tracking-widest mb-1">Mon Solde Echoppe</p>
                        <p id="p-balance" class="text-xl font-black">0 FCFA</p>
                    </div>
                </div>

                <!-- Panel Admin -->
                <div id="admin-panel" class="hidden mb-8">
                    <h4 class="text-[10px] font-black text-red-500 uppercase tracking-widest mb-4">Actions Administrateur</h4>
                    <div id="admin-tasks" class="space-y-3"></div>
                </div>

                <div class="space-y-2">
                    <button onclick="openModal('publish-modal')" class="w-full p-4 bg-slate-50 rounded-2xl flex items-center justify-between text-xs font-bold text-slate-700">Publier un article <i class="fa-solid fa-plus opacity-30"></i></button>
                    <button onclick="openModal('edit-profile-modal')" class="w-full p-4 bg-slate-50 rounded-2xl flex items-center justify-between text-xs font-bold text-slate-700">Paramètres Profil <i class="fa-solid fa-gear opacity-30"></i></button>
                    <button onclick="openModal('recharge-modal')" class="w-full p-4 bg-slate-50 rounded-2xl flex items-center justify-between text-xs font-bold text-slate-700">Recharger mon solde <i class="fa-solid fa-wallet opacity-30"></i></button>
                    <button onclick="handleLogout()" class="w-full p-4 text-red-500 font-black text-[10px] uppercase">Se déconnecter</button>
                </div>
            </div>
        </main>

        <!-- Navigation -->
        <nav class="fixed bottom-0 inset-x-0 h-20 bg-white/95 backdrop-blur-md border-t border-slate-100 flex items-center justify-around z-[500] pb-4">
            <button onclick="navigateTo('home')" id="nav-home" class="nav-item active"><i class="fa-solid fa-house-chimney text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Accueil</p></button>
            <button onclick="navigateTo('messages')" id="nav-messages" class="nav-item"><i class="fa-solid fa-comments text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Messages</p></button>
            <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item"><i class="fa-solid fa-circle-user text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Compte</p></button>
        </nav>
    </div>

    <!-- Modale Chat Dynamique -->
    <div id="chat-modal" class="modal-full !p-0">
        <div class="flex flex-col h-full bg-white">
            <div class="p-4 border-b border-slate-50 flex items-center justify-between sticky top-0 bg-white z-10">
                <div class="flex items-center gap-3">
                    <button onclick="closeModal('chat-modal')" class="w-10 h-10 bg-slate-50 rounded-xl flex items-center justify-center"><i class="fa-solid fa-chevron-left"></i></button>
                    <div onclick="viewPartnerProfile()">
                        <h3 id="chat-title" class="font-black text-blue-900 text-sm">...</h3>
                        <p class="text-[8px] text-blue-500 font-black uppercase tracking-tighter">Voir profil partenaire</p>
                    </div>
                </div>
                <button onclick="openModal('create-order-modal')" class="bg-blue-600 text-white text-[9px] font-black px-4 py-2 rounded-xl shadow-lg">BON DE COMMANDE</button>
            </div>
            
            <div id="chat-messages" class="flex-1 overflow-y-auto p-4 flex flex-col bg-slate-50/20"></div>

            <!-- Outils Multimédia -->
            <div class="px-4 py-2 flex items-center gap-4 bg-white border-t border-slate-50">
                <button onclick="document.getElementById('chat-img').click()" class="text-slate-300"><i class="fa-solid fa-image"></i></button>
                <input type="file" id="chat-img" class="hidden" accept="image/*" onchange="sendImgMessage(this)">
                <button onclick="sendUserLocation()" class="text-slate-300"><i class="fa-solid fa-location-dot"></i></button>
                <button id="voice-btn" onmousedown="startVoice()" onmouseup="stopVoice()" ontouchstart="startVoice()" ontouchend="stopVoice()" class="text-slate-300"><i class="fa-solid fa-microphone"></i></button>
            </div>
            <!-- Input Texte -->
            <div class="p-4 flex gap-2">
                <input type="text" id="chat-input" class="input-custom !py-3 flex-1" placeholder="Votre message...">
                <button onclick="sendMessage()" class="w-12 h-12 bg-blue-600 text-white rounded-xl shadow-lg"><i class="fa-solid fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <!-- Modale Création Bon de Commande -->
    <div id="create-order-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <h2 class="text-2xl font-black text-blue-900 mb-6 italic">Générer un Bon.</h2>
            <div class="space-y-4">
                <input type="text" id="order-item" class="input-custom" placeholder="Article(s) concerné(s)">
                <input type="number" id="order-price" class="input-custom" placeholder="Montant total (FCFA)">
                <textarea id="order-note" class="input-custom h-24" placeholder="Instructions ou lieu de RDV..."></textarea>
                <button onclick="submitOrder()" class="btn-blue mt-4">Envoyer le bon à l'acheteur</button>
                <button onclick="closeModal('create-order-modal')" class="w-full py-4 text-xs font-bold text-slate-400">Annuler</button>
            </div>
        </div>
    </div>

    <!-- Modales Profil & Recharge -->
    <div id="edit-profile-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <h2 class="text-2xl font-black text-blue-900 mb-6">Editer Profil.</h2>
            <div class="space-y-4">
                <input type="text" id="edit-city" class="input-custom" placeholder="Ville actuelle">
                <input type="text" id="edit-phone" class="input-custom" placeholder="Numéro WhatsApp">
                <textarea id="edit-bio" class="input-custom h-24" placeholder="Décrivez votre activité..."></textarea>
                <button onclick="updateUserProfile()" class="btn-blue">Mettre à jour</button>
                <button onclick="closeModal('edit-profile-modal')" class="w-full py-4 text-xs font-bold text-slate-400">Retour</button>
            </div>
        </div>
    </div>

    <div id="recharge-modal" class="modal-full">
        <div class="max-w-md mx-auto text-center">
            <h2 class="text-2xl font-black text-blue-900 mb-8 italic">Rechargement.</h2>
            <div class="bg-blue-600 p-8 rounded-[40px] text-white text-left mb-6">
                <p class="text-xs mb-4">Transférez le montant sur le numéro admin <span class="font-black">077 73 60 65</span> via Airtel Money.</p>
                <input type="number" id="rech-amount" class="w-full bg-white/10 p-4 rounded-2xl text-white outline-none mb-3 placeholder:text-white/40" placeholder="Montant">
                <input type="text" id="rech-ref" class="w-full bg-white/10 p-4 rounded-2xl text-white outline-none placeholder:text-white/40" placeholder="Référence Transaction">
            </div>
            <button onclick="requestRecharge()" class="btn-blue">Demander la validation</button>
            <button onclick="closeModal('recharge-modal')" class="w-full py-4 text-xs font-bold text-slate-400">Annuler</button>
        </div>
    </div>

    <!-- Modale Publication Article -->
    <div id="publish-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <h2 class="text-2xl font-black text-blue-900 mb-6 italic">Vendre.</h2>
            <div class="space-y-4">
                <div onclick="document.getElementById('pub-file').click()" class="h-48 w-full bg-slate-50 rounded-[32px] border-2 border-dashed border-slate-200 flex items-center justify-center overflow-hidden">
                    <img id="pub-preview" class="hidden w-full h-full object-cover">
                    <i id="pub-icon" class="fa-solid fa-camera text-slate-200 text-3xl"></i>
                </div>
                <input type="file" id="pub-file" class="hidden" accept="image/*" onchange="previewPub(this)">
                <input type="text" id="pub-name" class="input-custom" placeholder="Nom de l'article">
                <input type="number" id="pub-price" class="input-custom" placeholder="Prix">
                <textarea id="pub-desc" class="input-custom h-24" placeholder="Description courte..."></textarea>
                <button onclick="publishArticle()" class="btn-blue">Mettre en vente</button>
                <button onclick="closeModal('publish-modal')" class="w-full py-4 text-xs font-bold text-slate-400">Annuler</button>
            </div>
        </div>
    </div>

    <!-- Scripts Firebase & Logique -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, updateDoc, collection, addDoc, query, where, orderBy, serverTimestamp, increment, runTransaction } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "", 
            authDomain: "communautedugabon.firebaseapp.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe241-official-v1';
        const ADMIN_CODE = "GABON241";

        let user = null;
        let userData = null;
        let activeChatId = null;
        let partnerId = null;
        let recorder = null;
        let voiceChunks = [];

        // --- AUTH ---
        onAuthStateChanged(auth, (u) => {
            if (u) {
                user = u;
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        userData = snap.data();
                        document.getElementById('view-auth').classList.remove('active');
                        document.getElementById('app-content').style.display = 'block';
                        updateUI();
                        loadHome();
                        loadChats();
                        if(userData.isAdmin) loadAdminTasks();
                    } else {
                        const isAdm = localStorage.getItem('tmp_adm') === ADMIN_CODE;
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
                            fullName: localStorage.getItem('tmp_name') || 'Membre Echoppe',
                            walletBalance: 0,
                            isAdmin: isAdm,
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`,
                            bio: "Bienvenue sur mon profil Echoppe241 !",
                            city: "Libreville",
                            phone: ""
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

        window.handleLogin = () => {
            const e = document.getElementById('login-email').value;
            const p = document.getElementById('login-pass').value;
            signInWithEmailAndPassword(auth, e, p).catch(() => showToast("Échec de connexion"));
        };

        window.handleRegister = () => {
            const n = document.getElementById('reg-name').value;
            const e = document.getElementById('reg-email').value;
            const p = document.getElementById('reg-pass').value;
            const a = document.getElementById('reg-admin').value;
            if(!n || p.length < 6) return showToast("Infos invalides");
            localStorage.setItem('tmp_name', n);
            localStorage.setItem('tmp_adm', a);
            createUserWithEmailAndPassword(auth, e, p).catch(() => showToast("Échec d'inscription"));
        };

        window.handleLogout = () => signOut(auth);

        // --- UI ---
        function updateUI() {
            document.getElementById('header-balance').innerText = `${userData.walletBalance} F`;
            document.getElementById('p-balance').innerText = `${userData.walletBalance} FCFA`;
            document.getElementById('header-avatar').src = userData.avatar;
            document.getElementById('p-avatar').src = userData.avatar;
            document.getElementById('p-name').innerText = userData.fullName;
            document.getElementById('p-bio').innerText = userData.bio;
            if(userData.isAdmin) document.getElementById('admin-badge').classList.remove('hidden');
        }

        // --- HOME & CHATS ---
        function loadHome() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const list = document.getElementById('product-list');
                list.innerHTML = snap.docs.map(d => {
                    const p = d.data();
                    return `<div onclick="initChat('${p.sellerId}', '${p.sellerName}')" class="card-neo overflow-hidden active:scale-95 transition cursor-pointer">
                        <img src="${p.image}" class="h-32 w-full object-cover">
                        <div class="p-3">
                            <p class="text-[9px] font-black uppercase truncate text-blue-900">${p.name}</p>
                            <p class="text-blue-600 font-bold text-xs mt-1">${p.price} F</p>
                        </div>
                    </div>`;
                }).join('');
            });
        }

        function loadChats() {
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'chats'), where('participants', 'array-contains', user.uid)), (snap) => {
                const list = document.getElementById('conversations-list');
                list.innerHTML = snap.docs.map(d => {
                    const c = d.data();
                    const otherId = c.participants.find(p => p !== user.uid);
                    const name = c.names[otherId] || "Partenaire";
                    return `<div onclick="openChat('${d.id}', '${name}', '${otherId}')" class="p-4 bg-white border border-slate-50 rounded-2xl flex items-center gap-3 shadow-sm">
                        <div class="w-10 h-10 bg-blue-100 rounded-xl flex items-center justify-center text-blue-600 font-black">${name[0]}</div>
                        <div><p class="text-xs font-black text-blue-900">${name}</p><p class="text-[8px] text-slate-300 font-bold uppercase mt-1 italic">Discussion active</p></div>
                    </div>`;
                }).join('');
            });
        }

        window.openChat = (chatId, name, pId) => {
            activeChatId = chatId;
            partnerId = pId;
            document.getElementById('chat-title').innerText = name;
            document.getElementById('chat-modal').style.display = 'block';
            
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'chats', chatId, 'messages'), orderBy('timestamp', 'asc')), (snap) => {
                const box = document.getElementById('chat-messages');
                box.innerHTML = snap.docs.map(d => {
                    const m = d.data();
                    const isMe = m.senderId === user.uid;
                    let content = m.text;

                    if(m.type === 'order') {
                        const paid = m.status === 'paid' || m.status === 'validated';
                        const statusLabel = m.status === 'validated' ? '✅ Validé par Admin' : (m.status === 'paid' ? '💰 Payé - Attente Admin' : 'En attente de paiement');
                        content = `<div class="order-card">
                            <p class="text-[8px] font-black uppercase text-blue-600 mb-1 italic">Bon de Commande</p>
                            <p class="text-sm font-bold text-blue-900">${m.item}</p>
                            <p class="text-xs font-black mt-1">${m.price} FCFA</p>
                            <div class="mt-4 pt-3 border-t border-slate-50 flex items-center justify-between">
                                <span class="text-[8px] font-black uppercase text-slate-400">${statusLabel}</span>
                                ${(!isMe && m.status === 'pending') ? `<button onclick="payOrder('${d.id}', ${m.price}, '${m.senderId}')" class="bg-[#00853f] text-white text-[8px] font-black px-4 py-2 rounded-full">PAYER</button>` : ''}
                            </div>
                        </div>`;
                    } else if(m.type === 'image') {
                        content = `<img src="${m.url}" class="rounded-xl w-full max-w-[200px] border-2 border-white shadow-md">`;
                    } else if(m.type === 'voice') {
                        content = `<div class="voice-wave ${isMe ? 'text-white' : 'text-blue-600'}"><div class="voice-bar"></div><div class="voice-bar" style="animation-delay:0.2s"></div><div class="voice-bar" style="animation-delay:0.4s"></div><span class="ml-2 text-[9px] font-black">Note vocale</span></div>`;
                    } else if(m.type === 'location') {
                        content = `<a href="${m.url}" target="_blank" class="flex items-center gap-2 text-[10px] font-black underline"><i class="fa-solid fa-map-location-dot"></i> Position partagée</a>`;
                    }

                    return `<div class="chat-bubble ${isMe ? 'chat-me' : 'chat-them'}">${content}</div>`;
                }).join('');
                box.scrollTop = box.scrollHeight;
            });
        };

        // --- COMMANDES & PAIEMENTS ---
        window.submitOrder = async () => {
            const item = document.getElementById('order-item').value;
            const price = document.getElementById('order-price').value;
            const note = document.getElementById('order-note').value;
            if(!item || !price) return showToast("Complétez le bon");

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid, item, price: parseInt(price), note, type: 'order', status: 'pending', timestamp: serverTimestamp()
            });
            showToast("Bon envoyé");
            closeModal('create-order-modal');
        };

        window.payOrder = async (msgId, amount, sellerId) => {
            if(userData.walletBalance < amount) return showToast("Solde insuffisant");
            try {
                await runTransaction(db, async (tx) => {
                    const buyerRef = doc(db, 'artifacts', appId, 'users', user.uid);
                    tx.update(buyerRef, { walletBalance: increment(-amount) });
                    const msgRef = doc(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages', msgId);
                    tx.update(msgRef, { status: 'paid' });
                    
                    const taskRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'));
                    tx.set(taskRef, {
                        type: 'sale', buyerId: user.uid, buyerName: userData.fullName, sellerId, amount, msgId, chatId: activeChatId, status: 'pending', createdAt: serverTimestamp()
                    });
                });
                showToast("Paiement effectué ! En attente admin.");
            } catch(e) { showToast("Erreur transaction"); }
        };

        // --- ADMIN PANEL ---
        function loadAdminTasks() {
            document.getElementById('admin-panel').classList.remove('hidden');
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'), where('status', '==', 'pending')), (snap) => {
                const box = document.getElementById('admin-tasks');
                box.innerHTML = snap.docs.map(d => {
                    const t = d.data();
                    const isRech = t.type === 'recharge';
                    return `<div class="bg-red-50 p-4 rounded-2xl flex items-center justify-between border border-red-100">
                        <div>
                            <p class="text-[8px] font-black uppercase text-red-500">${isRech ? 'Recharge' : 'Validation Vente'}</p>
                            <p class="text-xs font-bold text-blue-900">${t.userName || t.buyerName}</p>
                            <p class="text-[10px] font-black">${t.amount} FCFA</p>
                        </div>
                        <button onclick="approveAdminTask('${d.id}')" class="bg-red-500 text-white text-[9px] font-black px-4 py-2 rounded-xl">VALIDER</button>
                    </div>`;
                }).join('');
            });
        }

        window.approveAdminTask = async (taskId) => {
            const taskDoc = doc(db, 'artifacts', appId, 'public', 'data', 'admin_tasks', taskId);
            onSnapshot(taskDoc, async (snap) => {
                if(!snap.exists()) return;
                const t = snap.data();
                if(t.type === 'recharge') {
                    await updateDoc(doc(db, 'artifacts', appId, 'users', t.uid), { walletBalance: increment(t.amount) });
                } else {
                    await updateDoc(doc(db, 'artifacts', appId, 'users', t.sellerId), { walletBalance: increment(t.amount) });
                    await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'chats', t.chatId, 'messages', t.msgId), { status: 'validated' });
                }
                await updateDoc(taskDoc, { status: 'done' });
                showToast("Action validée par l'administrateur");
            }, { once: true });
        };

        // --- MULTIMEDIA ---
        window.sendMessage = async () => {
            const i = document.getElementById('chat-input');
            if(!i.value.trim()) return;
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid, text: i.value, type: 'text', timestamp: serverTimestamp()
            });
            i.value = '';
        };

        window.sendImgMessage = (input) => {
            const reader = new FileReader();
            reader.onload = async (e) => {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                    senderId: user.uid, url: e.target.result, type: 'image', timestamp: serverTimestamp()
                });
            };
            reader.readAsDataURL(input.files[0]);
        };

        window.startVoice = async () => {
            const s = await navigator.mediaDevices.getUserMedia({ audio: true });
            recorder = new MediaRecorder(s);
            recorder.ondataavailable = e => voiceChunks.push(e.data);
            recorder.start();
            document.getElementById('voice-btn').classList.add('text-red-500');
        };

        window.stopVoice = () => {
            recorder.stop();
            document.getElementById('voice-btn').classList.remove('text-red-500');
            recorder.onstop = async () => {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                    senderId: user.uid, type: 'voice', timestamp: serverTimestamp()
                });
                voiceChunks = [];
            };
        };

        window.sendUserLocation = () => {
            navigator.geolocation.getCurrentPosition(async (pos) => {
                const url = `https://www.google.com/maps?q=${pos.coords.latitude},${pos.coords.longitude}`;
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                    senderId: user.uid, url, type: 'location', timestamp: serverTimestamp()
                });
            });
        };

        // --- PUBLISH & PROFILE ---
        window.publishArticle = async () => {
            const n = document.getElementById('pub-name').value;
            const p = document.getElementById('pub-price').value;
            const d = document.getElementById('pub-desc').value;
            if(!n || !p || !window.tmp_img) return showToast("Manque infos/photo");
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                name: n, price: parseInt(p), description: d, image: window.tmp_img, sellerId: user.uid, sellerName: userData.fullName, createdAt: serverTimestamp()
            });
            showToast("Article publié !");
            closeModal('publish-modal');
        };

        window.previewPub = (input) => {
            const reader = new FileReader();
            reader.onload = e => {
                document.getElementById('pub-preview').src = e.target.result;
                document.getElementById('pub-preview').classList.remove('hidden');
                document.getElementById('pub-icon').classList.add('hidden');
                window.tmp_img = e.target.result;
            };
            reader.readAsDataURL(input.files[0]);
        };

        window.requestRecharge = async () => {
            const a = document.getElementById('rech-amount').value;
            const r = document.getElementById('rech-ref').value;
            if(!a || !r) return showToast("Infos incomplètes");
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'), {
                type: 'recharge', uid: user.uid, userName: userData.fullName, amount: parseInt(a), ref: r, status: 'pending', createdAt: serverTimestamp()
            });
            showToast("Demande soumise aux admins");
            closeModal('recharge-modal');
        };

        window.initChat = async (sid, sname) => {
            if(sid === user.uid) return navigateTo('profile');
            const cid = [user.uid, sid].sort().join('_');
            const names = {}; names[user.uid] = userData.fullName; names[sid] = sname;
            await setDoc(doc(db, 'artifacts', appId, 'public', 'data', 'chats', cid), { participants: [user.uid, sid], names }, {merge: true});
            openChat(cid, sname, sid);
        };

        // --- HELPERS ---
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(`nav-${v}`).classList.add('active');
        };
        window.toggleAuth = (m) => {
            document.getElementById('login-form').classList.toggle('hidden', m === 'register');
            document.getElementById('register-form').classList.toggle('hidden', m === 'login');
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
