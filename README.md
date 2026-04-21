<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Plateforme Officielle</title> 
    
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
        body { font-family: 'Inter', sans-serif; background-color: var(--white); color: var(--dark); margin: 0; overflow-x: hidden; }
        h1, h2, h3, .brand-font { font-family: 'Syne', sans-serif; text-transform: lowercase; }
        
        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease; }
        
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        .card-neo { 
            background: var(--white); 
            border-radius: 24px; 
            border: 1px solid #f1f5f9;
            box-shadow: 0 4px 12px rgba(58, 117, 196, 0.05);
        }
        
        .btn-blue { 
            background: var(--primary-blue); 
            color: var(--white); 
            padding: 18px; 
            border-radius: 20px; 
            font-weight: 800; 
            text-transform: uppercase; 
            font-size: 11px; 
            width: 100%;
            transition: 0.2s;
            border: none;
            cursor: pointer;
            text-align: center;
        }
        .btn-blue:disabled { opacity: 0.5; cursor: not-allowed; }

        .input-custom { 
            background: #f8fafc; 
            border-radius: 16px; 
            padding: 16px; 
            width: 100%; 
            font-size: 14px;
            border: 2px solid transparent;
            outline: none;
            transition: 0.3s;
        }
        .input-custom:focus { border-color: var(--primary-blue); background: var(--white); }

        #toast {
            position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%);
            z-index: 10000; background: var(--dark); color: var(--white);
            padding: 12px 24px; border-radius: 50px; font-size: 11px; font-weight: 700;
            display: none; text-align: center; width: 80%;
        }

        .nav-item { color: #cbd5e1; transition: 0.3s; text-align: center; flex: 1; cursor: pointer; }
        .nav-item.active { color: var(--primary-blue); }

        .modal-full {
            position: fixed; inset: 0; background: var(--white); z-index: 9000;
            overflow-y: auto; padding: 32px 24px; display: none;
        }

        #splash-screen {
            position: fixed; inset: 0; background: var(--white); z-index: 9999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
        }

        .role-badge { font-size: 9px; padding: 3px 10px; border-radius: 50px; font-weight: 900; text-transform: uppercase; letter-spacing: 0.5px; }
        .role-admin { background: #fee2e2; color: #ef4444; }
        .role-seller { background: #dcfce7; color: #16a34a; }
        .role-buyer { background: #e0f2fe; color: #0284c7; }

        .photo-box {
            width: 100%; height: 180px; border-radius: 20px; background: #f8fafc;
            border: 2px dashed #e2e8f0; display: flex; flex-direction: column;
            align-items: center; justify-content: center; cursor: pointer;
            overflow: hidden; position: relative;
        }
        .photo-box img { width: 100%; height: 100%; object-fit: cover; }

        .chat-bubble { max-width: 80%; padding: 12px 16px; border-radius: 20px; font-size: 14px; margin-bottom: 8px; }
        .chat-me { background: var(--primary-blue); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .chat-them { background: #f1f5f9; color: var(--dark); align-self: flex-start; border-bottom-left-radius: 4px; }
    </style>
</head>
<body class="pb-24">

    <div id="toast">Message</div>

    <!-- SPLASH SCREEN -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-32 mb-6" alt="Logo Echoppe241">
        <div class="flex gap-2">
            <div class="w-3 h-3 rounded-full bg-[#3a75c4] animate-bounce"></div>
            <div class="w-3 h-3 rounded-full bg-[#fcd116] animate-bounce [animation-delay:0.2s]"></div>
            <div class="w-3 h-3 rounded-full bg-[#00853f] animate-bounce [animation-delay:0.4s]"></div>
        </div>
    </div>

    <!-- VUE AUTHENTIFICATION -->
    <div id="view-auth" class="view active min-h-screen flex items-center px-6">
        <div class="w-full max-w-md mx-auto">
            <div class="text-center mb-10">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 mx-auto mb-4" alt="Logo">
                <h1 class="text-3xl font-black text-blue-900 leading-tight">Accès Privé.</h1>
                <p class="text-slate-400 text-sm mt-2">Gérez vos négoces en toute sécurité.</p>
            </div>

            <div id="login-form" class="space-y-4">
                <input type="email" id="login-email" class="input-custom" placeholder="Email">
                <input type="password" id="login-pass" class="input-custom" placeholder="Mot de passe">
                <button onclick="handleLogin()" class="btn-blue shadow-lg shadow-blue-100 w-full">Se connecter</button>
                <p class="text-center text-xs text-slate-500 mt-6">Pas encore membre ? <span onclick="toggleAuth('register')" class="text-blue-600 font-bold cursor-pointer">Rejoindre Echoppe241</span></p>
            </div>

            <div id="register-form" class="space-y-4 hidden">
                <input type="text" id="reg-name" class="input-custom" placeholder="Nom ou Nom Boutique">
                <input type="email" id="reg-email" class="input-custom" placeholder="Email professionnel">
                <input type="password" id="reg-pass" class="input-custom" placeholder="Mot de passe (min. 6)">
                
                <p class="text-[10px] font-black text-slate-400 uppercase text-center">Vous êtes :</p>
                <div class="flex gap-2 p-1.5 bg-slate-50 rounded-2xl">
                    <button onclick="setRegRole('buyer')" id="role-btn-buyer" class="flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm text-blue-600 border border-blue-100 uppercase">Acheteur</button>
                    <button onclick="setRegRole('seller')" id="role-btn-seller" class="flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase">Vendeur</button>
                </div>

                <button onclick="handleRegister()" class="btn-blue bg-[#00853f] w-full">Créer mon compte</button>
                <p class="text-center text-xs text-slate-500 mt-6">Déjà inscrit ? <span onclick="toggleAuth('login')" class="text-blue-600 font-bold cursor-pointer">Connexion</span></p>
            </div>
        </div>
    </div>

    <!-- MODAL PUBLICATION -->
    <div id="publish-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-8">
                <h2 class="text-2xl font-black text-blue-900">mettre en vente.</h2>
                <button onclick="closeModal('publish-modal')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>

            <div class="space-y-4">
                <div onclick="triggerFile('item-photo-file')" class="photo-box" id="item-photo-preview">
                    <i class="fa-solid fa-camera text-3xl text-slate-300 mb-2"></i>
                    <p class="text-[10px] font-black text-slate-400 uppercase">Photo réelle de l'article</p>
                </div>
                <input type="file" id="item-photo-file" accept="image/*" capture="environment" class="hidden" onchange="handleImagePreview(this, 'item-photo-preview')">
                
                <input type="text" id="pub-name" class="input-custom" placeholder="Nom de l'article">
                <div class="flex gap-2">
                    <input type="number" id="pub-price" class="input-custom flex-1" placeholder="Prix (FCFA)">
                    <input type="number" id="pub-qty" class="input-custom w-1/3" placeholder="Quantité">
                </div>
                
                <select id="pub-province" class="input-custom">
                    <option value="">Province de vente...</option>
                    <option value="Estuaire">Estuaire (LBV)</option>
                    <option value="Haut-Ogooué">Haut-Ogooué</option>
                    <option value="Moyen-Ogooué">Moyen-Ogooué</option>
                    <option value="Ngounié">Ngounié</option>
                    <option value="Nyanga">Nyanga</option>
                    <option value="Ogooué-Ivindo">Ogooué-Ivindo</option>
                    <option value="Ogooué-Lolo">Ogooué-Lolo</option>
                    <option value="Ogooué-Maritime">Ogooué-Maritime</option>
                    <option value="Woleu-Ntem">Woleu-Ntem</option>
                </select>
                <textarea id="pub-desc" class="input-custom h-24" placeholder="Description / Bio du produit..."></textarea>
                
                <div class="p-4 bg-slate-50 rounded-2xl">
                    <label class="flex items-center gap-3 cursor-pointer">
                        <input type="checkbox" id="pub-delivery" class="w-5 h-5 accent-blue-600">
                        <span class="text-xs font-bold text-slate-700">Option Livraison CT241 intégrée</span>
                    </label>
                </div>
                <button onclick="processPublish()" id="btn-pub-final" class="btn-blue w-full">Publier maintenant</button>
            </div>
        </div>
    </div>

    <!-- MODAL RECHARGE -->
    <div id="recharge-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-8">
                <h2 class="text-2xl font-black text-blue-900">recharger.</h2>
                <button onclick="closeModal('recharge-modal')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>
            <div class="bg-blue-600 p-6 rounded-3xl text-white mb-6">
                <p class="text-[10px] font-black uppercase opacity-60 mb-1">Instructions de dépôt</p>
                <p class="text-sm font-bold mb-4">Envoyez vos fonds via Airtel ou Moov :</p>
                <div class="space-y-2">
                    <p class="text-lg font-black">+241 77 73 60 65</p>
                    <p class="text-lg font-black">+241 66 45 71 72</p>
                </div>
            </div>
            <div class="space-y-4">
                <input type="number" id="rech-amount" class="input-custom" placeholder="Montant à recharger" oninput="calculateFees(this.value)">
                <div id="fee-calc" class="px-4 text-[11px] font-bold text-blue-600 hidden">
                    Frais (7%) : <span id="fee-val">0</span> F | Total crédité : <span id="credit-val">0</span> F
                </div>
                <input type="text" id="rech-ref" class="input-custom" placeholder="Référence de la transaction">
                
                <div onclick="triggerFile('rech-proof-file')" class="photo-box h-32" id="rech-proof-preview">
                    <i class="fa-solid fa-file-invoice text-2xl text-slate-300 mb-1"></i>
                    <p class="text-[9px] font-black text-slate-400 uppercase text-center px-4">Capture de la preuve de dépôt</p>
                </div>
                <input type="file" id="rech-proof-file" accept="image/*" class="hidden" onchange="handleImagePreview(this, 'rech-proof-preview')">
                <button onclick="processRecharge()" class="btn-blue w-full bg-[#00853f]">Envoyer pour validation</button>
            </div>
        </div>
    </div>

    <!-- MODAL CHAT -->
    <div id="chat-modal" class="modal-full !p-0">
        <div class="flex flex-col h-full bg-white">
            <div class="p-6 border-b border-slate-50 flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <button onclick="closeModal('chat-modal')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-arrow-left"></i></button>
                    <div>
                        <h3 id="chat-user-name" class="font-black text-blue-900">Chat</h3>
                        <p class="text-[9px] font-bold text-green-500 uppercase">En ligne</p>
                    </div>
                </div>
            </div>
            <div id="chat-messages" class="flex-1 overflow-y-auto p-6 flex flex-col gap-2 bg-slate-50/20"></div>
            <div class="p-4 border-t border-slate-100 flex gap-2 items-center">
                <input type="text" id="chat-input" class="input-custom flex-1 !py-3" placeholder="Message...">
                <button onclick="sendChatMessage()" class="w-12 h-12 bg-blue-600 text-white rounded-2xl flex items-center justify-center"><i class="fa-solid fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <!-- CONTENU APPLICATION -->
    <div id="app-content" style="display:none">
        <header class="fixed top-0 inset-x-0 h-20 px-6 flex items-center justify-between bg-white/80 backdrop-blur-xl border-b border-slate-50 z-[500]">
            <div class="flex items-center gap-2">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-8" alt="Logo mini">
                <h1 class="text-xl font-black italic text-blue-900">echoppe<span class="text-[#fcd116]">241</span></h1>
            </div>
            <div class="flex items-center gap-3">
                <div class="text-right">
                    <p id="h-balance" class="text-[13px] font-black text-blue-600">0 F</p>
                    <span class="text-[8px] font-bold text-slate-400 uppercase">EchoppePay</span>
                </div>
                <img id="h-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-2xl bg-slate-100 border-2 border-white shadow-sm object-cover cursor-pointer">
            </div>
        </header>

        <main class="max-w-xl mx-auto px-6 pt-24 pb-24">
            <!-- ACCUEIL -->
            <div id="view-home" class="view active">
                <div class="flex items-center justify-between mb-6">
                    <h2 class="text-2xl font-black text-blue-900 leading-tight">Le Marché<br><span class="text-[#fcd116]">Libre.</span></h2>
                </div>
                <div id="product-list" class="grid grid-cols-2 gap-4"></div>
            </div>

            <!-- MESSAGES -->
            <div id="view-messages" class="view">
                <h2 class="text-2xl font-black text-blue-900 mb-6">Négoces.</h2>
                <div id="chats-list" class="space-y-3"></div>
            </div>

            <!-- PROFIL -->
            <div id="view-profile" class="view">
                <div class="card-neo p-6 mb-6 text-center">
                    <div class="relative w-28 h-28 mx-auto mb-4">
                        <img id="p-avatar" class="w-28 h-28 rounded-3xl border-4 border-white shadow-xl object-cover bg-slate-50" alt="Avatar">
                        <button onclick="triggerFile('p-avatar-file')" class="absolute bottom-0 right-0 w-9 h-9 bg-blue-600 text-white rounded-full border-2 border-white flex items-center justify-center shadow-lg"><i class="fa-solid fa-camera text-[10px]"></i></button>
                    </div>
                    <input type="file" id="p-avatar-file" accept="image/*" class="hidden" onchange="updateProfileAsset('avatar', this)">
                    
                    <h3 id="p-name" class="text-2xl font-black text-blue-900 mb-1">...</h3>
                    <div id="p-role" class="role-badge mb-4 inline-block">Rôle</div>

                    <div class="bg-blue-600 p-5 rounded-3xl text-white text-left relative overflow-hidden">
                        <div class="relative z-10">
                            <p class="text-[9px] font-bold opacity-60 uppercase tracking-widest mb-1">Solde EchoppePay</p>
                            <p id="p-wallet" class="text-2xl font-black">0 FCFA</p>
                        </div>
                        <i class="fa-solid fa-wallet absolute right-6 top-1/2 -translate-y-1/2 text-4xl opacity-10"></i>
                    </div>
                </div>

                <div id="admin-panel" class="hidden mb-6 p-5 bg-red-50 border border-red-100 rounded-3xl">
                    <h4 class="text-[10px] font-black text-red-500 uppercase tracking-widest mb-4">Administration</h4>
                    <div id="admin-requests" class="space-y-3"></div>
                </div>

                <div class="space-y-3">
                    <button onclick="openModal('recharge-modal')" class="w-full p-5 bg-white border border-slate-50 rounded-2xl flex items-center justify-between">
                        <span class="text-xs font-bold uppercase text-slate-700">Recharger Mon Compte</span>
                        <i class="fa-solid fa-plus text-blue-600"></i>
                    </button>
                    <button id="btn-show-publish" onclick="openModal('publish-modal')" class="hidden w-full p-5 bg-white border border-slate-50 rounded-2xl flex items-center justify-between">
                        <span class="text-xs font-bold uppercase text-slate-700">Vendre un article</span>
                        <i class="fa-solid fa-tag text-blue-600"></i>
                    </button>
                    <button onclick="handleLogout()" class="w-full p-5 text-red-500 text-[10px] font-black uppercase text-center">Déconnexion</button>
                </div>
            </div>
        </main>

        <nav class="fixed bottom-0 inset-x-0 h-20 bg-white border-t border-slate-50 flex items-center justify-around px-4 pb-2 z-[500]">
            <button onclick="navigateTo('home')" id="nav-home" class="nav-item active"><i class="fa-solid fa-house-chimney text-lg"></i><p class="text-[8px] font-bold mt-1 uppercase">Accueil</p></button>
            <button onclick="navigateTo('messages')" id="nav-messages" class="nav-item"><i class="fa-solid fa-comment-dots text-lg"></i><p class="text-[8px] font-bold mt-1 uppercase">Chat</p></button>
            <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item"><i class="fa-solid fa-user-ninja text-lg"></i><p class="text-[8px] font-bold mt-1 uppercase">Compte</p></button>
        </nav>
    </div>

    <!-- MODAL DETAIL -->
    <div id="detail-modal" class="modal-full">
        <div id="detail-content" class="max-w-md mx-auto"></div>
    </div>

    <script type="module">
        // Importation des SDK Firebase nécessaires
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAnalytics } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-analytics.js";
        import { getAuth, onAuthStateChanged, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc, updateDoc, collection, addDoc, query, where, serverTimestamp, orderBy, increment } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Configuration Firebase (Fusionnée avec vos clés)
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
        // Fonction pour supprimer un produit
window.deleteProduct = async (productId) => {
    if (confirm("Voulez-vous vraiment retirer cet article de la vente ?")) {
        try {
            // Importez 'deleteDoc' en haut de votre script si ce n'est pas fait
            // import { deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
            
            await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', productId));
            showToast("Article supprimé avec succès");
            closeModal('detail-modal'); // Ferme la vue détail après suppression
        } catch (error) {
            showToast("Erreur lors de la suppression");
            console.error(error);
        }
    }
};

        // Initialisation de Firebase
        const app = initializeApp(firebaseConfig);
        const analytics = getAnalytics(app);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe241-prod-v1';

        let user = null;
        let userData = null;
        let curRole = 'buyer';
        let activeChatId = null;

        // Écouteur d'état de l'authentification
        onAuthStateChanged(auth, (u) => {
            if (u) {
                user = u;
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        userData = snap.data();
                        document.getElementById('view-auth').classList.remove('active');
                        document.getElementById('app-content').style.display = 'block';
                        syncUI();
                        loadData();
                        if(userData.isAdmin) loadAdminTasks();
                    } else {
                        // Création du profil initial si nouveau
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
                            fullName: u.displayName || "Membre Echoppe",
                            email: u.email,
                            walletBalance: 0,
                            role: 'buyer',
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

        // Fonctions Globales
        window.setRegRole = (role) => {
            curRole = role;
            document.getElementById('role-btn-buyer').className = role === 'buyer' ? 'flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm text-blue-600 border border-blue-100 uppercase' : 'flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase';
            document.getElementById('role-btn-seller').className = role === 'seller' ? 'flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm text-green-600 border border-green-100 uppercase' : 'flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase';
        };

        window.handleRegister = async () => {
            const n = document.getElementById('reg-name').value;
            const e = document.getElementById('reg-email').value;
            const p = document.getElementById('reg-pass').value;
            if(!n || p.length < 6) return showToast("Informations invalides");
            try {
                const res = await createUserWithEmailAndPassword(auth, e, p);
                await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid), {
                    fullName: n, email: e, walletBalance: 0, role: curRole, isAdmin: false,
                    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${res.user.uid}`
                });
            } catch(err) { showToast("Échec de l'inscription"); }
        };

        window.handleLogin = async () => {
            const e = document.getElementById('login-email').value;
            const p = document.getElementById('login-pass').value;
            try { await signInWithEmailAndPassword(auth, e, p); } catch(err) { showToast("Identifiants incorrects"); }
        };

        window.handleLogout = () => signOut(auth);

        function syncUI() {
            if(!userData) return;
            document.getElementById('h-balance').innerText = `${userData.walletBalance} F`;
            document.getElementById('h-avatar').src = userData.avatar;
            document.getElementById('p-avatar').src = userData.avatar;
            document.getElementById('p-name').innerText = userData.fullName;
            document.getElementById('p-wallet').innerText = `${userData.walletBalance} FCFA`;
            
            const r = document.getElementById('p-role');
            r.innerText = userData.isAdmin ? 'Admin' : (userData.role === 'seller' ? 'Vendeur' : 'Acheteur');
            r.className = `role-badge mb-4 inline-block ${userData.isAdmin ? 'role-admin' : (userData.role === 'seller' ? 'role-seller' : 'role-buyer')}`;
            
            if(userData.role === 'seller' || userData.isAdmin) {
                document.getElementById('btn-show-publish').classList.remove('hidden');
            }
            if(userData.isAdmin) document.getElementById('admin-panel').classList.remove('hidden');
        }

        window.triggerFile = (id) => document.getElementById(id).click();
        window.handleImagePreview = (input, containerId) => {
            const file = input.files[0];
            if(file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    document.getElementById(containerId).innerHTML = `<img src="${e.target.result}" class="w-full h-full object-cover">`;
                };
                reader.readAsDataURL(file);
            }
        };

        window.updateProfileAsset = (type, input) => {
            const file = input.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = async (e) => {
                const base64 = e.target.result;
                const update = {}; update[type] = base64;
                await updateDoc(doc(db, 'artifacts', appId, 'users', user.uid), update);
                showToast("Profil mis à jour !");
            };
            reader.readAsDataURL(file);
        };

        window.processPublish = async () => {
            const name = document.getElementById('pub-name').value;
            const price = parseInt(document.getElementById('pub-price').value);
            const photoInput = document.getElementById('item-photo-file');
            if(!name || !price || !photoInput.files[0]) return showToast("Remplissez tous les champs !");
            
            const btn = document.getElementById('btn-pub-final');
            btn.disabled = true; btn.innerText = "PUBLICATION...";
            
            const reader = new FileReader();
            reader.onload = async (e) => {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name, price, image: e.target.result, 
                    sellerId: user.uid, sellerName: userData.fullName,
                    sellerAvatar: userData.avatar, createdAt: serverTimestamp(),
                    description: document.getElementById('pub-desc').value,
                    province: document.getElementById('pub-province').value,
                    deliveryOption: document.getElementById('pub-delivery').checked
                });
                showToast("Article publié !");
                closeModal('publish-modal');
                btn.disabled = false; btn.innerText = "Publier maintenant";
            };
            reader.readAsDataURL(photoInput.files[0]);
        };

        window.calculateFees = (val) => {
            const amount = parseInt(val);
            const box = document.getElementById('fee-calc');
            if(!amount || amount <= 0) { box.classList.add('hidden'); return; }
            box.classList.remove('hidden');
            const fee = Math.round(amount * 0.07);
            document.getElementById('fee-val').innerText = fee;
            document.getElementById('credit-val').innerText = amount - fee;
        };

        window.processRecharge = async () => {
            const amount = parseInt(document.getElementById('rech-amount').value);
            const ref = document.getElementById('rech-ref').value;
            const proof = document.getElementById('rech-proof-file').files[0];
            if(!amount || !ref || !proof) return showToast("Informations manquantes");
            
            const reader = new FileReader();
            reader.onload = async (e) => {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                    userId: user.uid, userName: userData.fullName, amount, reference: ref,
                    proof: e.target.result, status: 'pending', createdAt: serverTimestamp()
                });
                showToast("Demande envoyée !");
                closeModal('recharge-modal');
            };
            reader.readAsDataURL(proof);
        };

        function loadData() {
            // Chargement Produits
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'products'), orderBy('createdAt', 'desc')), (snap) => {
                const list = document.getElementById('product-list');
                list.innerHTML = snap.docs.map(d => {
                    const p = d.data();
                    return `
                        <div onclick="openProduct('${d.id}')" class="card-neo overflow-hidden flex flex-col cursor-pointer active:scale-95 transition">
                            <img src="${p.image}" class="w-full h-36 object-cover">
                            <div class="p-3">
                                <div class="flex items-center gap-1.5 mb-1.5">
                                    <img src="${p.sellerAvatar}" class="w-4 h-4 rounded-full border border-slate-100">
                                    <span class="text-[8px] font-black text-slate-400 uppercase truncate">${p.sellerName}</span>
                                </div>
                                <p class="text-[10px] font-black text-blue-900 truncate leading-none mb-1">${p.name}</p>
                                <p class="text-blue-600 font-bold text-xs">${p.price} F</p>
                            </div>
                        </div>
                    `;
                }).join('');
            });

            // Chargement Conversations
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'chats'), where('participants', 'array-contains', user.uid)), (snap) => {
                const list = document.getElementById('chats-list');
                list.innerHTML = snap.docs.map(d => {
                    const c = d.data();
                    const otherIdx = c.participants.indexOf(user.uid) === 0 ? 1 : 0;
                    return `
                        <div onclick="openChat('${d.id}', '${c.names[otherIdx]}')" class="p-4 bg-white border border-slate-50 rounded-2xl flex items-center justify-between cursor-pointer active:bg-slate-50">
                            <div class="flex items-center gap-3">
                                <div class="w-12 h-12 bg-blue-50 rounded-2xl flex items-center justify-center text-blue-600 font-black">${c.names[otherIdx][0]}</div>
                                <div><p class="text-xs font-black text-blue-900">${c.names[otherIdx]}</p><p class="text-[10px] text-slate-400">Négociation en cours</p></div>
                            </div>
                        </div>
                    `;
                }).join('');
            });
        }

        window.openProduct = (id) => {
            onSnapshot(doc(db, 'artifacts', appId, 'public', 'data', 'products', id), (snap) => {
                if(!snap.exists()) return;
                const p = snap.data();
       // RECHERCHEZ CE BLOC :
window.openProduct = (id) => {
    onSnapshot(doc(db, 'artifacts', appId, 'public', 'data', 'products', id), (snap) => {
        if(!snap.exists()) return;
        const p = snap.data();

        // --- C'EST ICI QU'ON AJOUTE LA LOGIQUE DE SUPPRESSION ---
        let deleteBtn = '';
        if (userData.isAdmin || p.sellerId === user.uid) {
            deleteBtn = `
                <button onclick="deleteProduct('${id}')" 
                        class="w-full p-4 mb-3 border-2 border-red-500 text-red-500 rounded-2xl font-black text-[10px] uppercase active:bg-red-50">
                    <i class="fa-solid fa-trash-can mr-2"></i> Supprimer cette annonce
                </button>`;
        }
        // -------------------------------------------------------

        document.getElementById('detail-content').innerHTML = `
            <button onclick="closeModal('detail-modal')" class="mb-4 w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-arrow-left"></i></button>
            <img src="${p.image}" class="w-full h-64 object-cover rounded-3xl shadow-lg mb-6">
            
            ${deleteBtn} <button onclick="initiateChat('${p.sellerId}', '${p.sellerName}')" class="btn-blue w-full">Lancer la négociation</button>
        `;
        openModal('detail-modal');
    });
};

document.getElementById('detail-content').innerHTML = `
    ${deleteBtn}
    <button onclick="initiateChat('${p.sellerId}', '${p.sellerName}')" class="btn-blue w-full">Lancer la négociation</button>
`;
                document.getElementById('detail-content').innerHTML = `
                    <button onclick="closeModal('detail-modal')" class="mb-4 w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-arrow-left"></i></button>
                    <img src="${p.image}" class="w-full h-64 object-cover rounded-3xl shadow-lg mb-6">
                    <div class="flex items-center gap-3 mb-6">
                        <img src="${p.sellerAvatar}" class="w-12 h-12 rounded-2xl object-cover">
                        <div><p class="text-xs font-black text-blue-900">${p.sellerName}</p><p class="text-[9px] font-bold text-slate-400 uppercase">${p.province}</p></div>
                    </div>
                    <h2 class="text-2xl font-black text-blue-900 mb-2">${p.name}</h2>
                    <p class="text-blue-600 font-bold text-xl mb-4">${p.price} FCFA</p>
                    <div class="bg-slate-50 p-4 rounded-2xl mb-8">
                        <p class="text-xs text-slate-600">${p.description || 'Pas de description.'}</p>
                    </div>
                    <button onclick="initiateChat('${p.sellerId}', '${p.sellerName}')" class="btn-blue w-full">Lancer la négociation</button>
                `;
                openModal('detail-modal');
            });
        };
        window.deleteProduct = async (productId) => {
    if (confirm("Voulez-vous vraiment retirer cet article de la vente ?")) {
        try {
            // Importation nécessaire pour que deleteDoc fonctionne
            const { deleteDoc } = await import("https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js");
            
            await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', productId));
            showToast("Article supprimé !");
            closeModal('detail-modal');
        } catch (e) {
            showToast("Erreur lors de la suppression");
        }
    }
};

        window.initiateChat = async (sid, sname) => {
            if(sid === user.uid) return showToast("C'est votre produit !");
            const cid = [user.uid, sid].sort().join('_');
            await setDoc(doc(db, 'artifacts', appId, 'public', 'data', 'chats', cid), {
                participants: [user.uid, sid],
                names: [userData.fullName, sname],
                lastUpdate: serverTimestamp()
            }, { merge: true });
            closeModal('detail-modal');
            openChat(cid, sname);
        };

        window.openChat = (cid, name) => {
            activeChatId = cid;
            document.getElementById('chat-user-name').innerText = name;
            openModal('chat-modal');
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'chats', cid, 'messages'), orderBy('timestamp', 'asc'));
            onSnapshot(q, (snap) => {
                const box = document.getElementById('chat-messages');
                box.innerHTML = snap.docs.map(d => {
                    const m = d.data();
                    const isMe = m.senderId === user.uid;
                    return `<div class="chat-bubble ${isMe ? 'chat-me' : 'chat-them'}">${m.text}</div>`;
                }).join('');
                box.scrollTop = box.scrollHeight;
            });
        };

        window.sendChatMessage = async () => {
            const inp = document.getElementById('chat-input');
            if(!inp.value.trim() || !activeChatId) return;
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid, text: inp.value.trim(), timestamp: serverTimestamp()
            });
            inp.value = "";
        };

        function loadAdminTasks() {
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), where('status', '==', 'pending')), (snap) => {
                const list = document.getElementById('admin-requests');
                if(snap.empty) { list.innerHTML = '<p class="text-[10px] text-slate-300 italic text-center py-4">Aucune attente.</p>'; return; }
                list.innerHTML = snap.docs.map(d => {
                    const r = d.data();
                    return `
                        <div class="bg-white p-4 rounded-2xl border border-red-100 flex flex-col gap-3">
                            <div class="flex justify-between">
                                <div><p class="text-[10px] font-black">${r.userName}</p><p class="text-xs font-black text-blue-600">${r.amount} F</p></div>
                                <img src="${r.proof}" class="w-12 h-12 rounded-lg object-cover" onclick="window.open('${r.proof}')">
                            </div>
                            <button onclick="approveRecharge('${d.id}', '${r.userId}', ${r.amount})" class="w-full py-2 bg-green-500 text-white rounded-xl text-[10px] font-black">VALIDER</button>
                        </div>
                    `;
                }).join('');
            });
        }

        window.approveRecharge = async (did, uid, fullAmount) => {
            const netAmount = Math.round(fullAmount * 0.93);
            await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', did), { status: 'approved' });
            await updateDoc(doc(db, 'artifacts', appId, 'users', uid), { walletBalance: increment(netAmount) });
            showToast("Compte crédité !");
        };

        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(e => e.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(e => e.classList.remove('active'));
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
            setTimeout(() => {
                const s = document.getElementById('splash-screen');
                if(s) { s.style.opacity = '0'; setTimeout(() => s.style.display = 'none', 500); }
            }, 1500);
        }
    </script>
</body>
</html>
