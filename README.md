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
            background: var(--white); border-radius: 24px; 
            border: 1px solid #f1f5f9;
            box-shadow: 0 4px 12px rgba(58, 117, 196, 0.05);
        }
        
        .btn-blue { 
            background: var(--primary-blue); color: var(--white); 
            padding: 16px; border-radius: 18px; font-weight: 800; 
            text-transform: uppercase; font-size: 11px; width: 100%;
            transition: 0.2s; border: none; cursor: pointer; text-align: center;
        }
        .btn-blue:active { transform: scale(0.96); }

        .input-custom { 
            background: #f8fafc; border-radius: 16px; padding: 16px; 
            width: 100%; font-size: 14px; border: 2px solid transparent; outline: none;
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
            transition: opacity 0.5s ease;
        }

        .role-badge { font-size: 9px; padding: 3px 10px; border-radius: 50px; font-weight: 900; text-transform: uppercase; }
        .role-admin { background: #fee2e2; color: #ef4444; }
        .role-seller { background: #dcfce7; color: #16a34a; }
        .role-buyer { background: #e0f2fe; color: #0284c7; }
    </style>
</head>
<body class="pb-24">

    <div id="toast">Message</div>

    <!-- SPLASH SCREEN -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 mb-6">
        <div class="flex gap-2">
            <div class="w-2 h-2 rounded-full bg-blue-500 animate-bounce"></div>
            <div class="w-2 h-2 rounded-full bg-yellow-400 animate-bounce [animation-delay:0.2s]"></div>
            <div class="w-2 h-2 rounded-full bg-green-500 animate-bounce [animation-delay:0.4s]"></div>
        </div>
    </div>

    <!-- AUTH VIEW -->
    <div id="view-auth" class="view active min-h-screen flex items-center px-6">
        <div class="w-full max-w-md mx-auto text-center">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-20 mx-auto mb-10">
            <div id="login-form" class="space-y-4">
                <input type="email" id="login-email" class="input-custom" placeholder="Email">
                <input type="password" id="login-pass" class="input-custom" placeholder="Mot de passe">
                <button onclick="handleLogin()" class="btn-blue">Connexion</button>
                <p class="text-[11px] text-slate-400 mt-6 font-bold uppercase">Nouveau ? <span onclick="toggleAuth('register')" class="text-blue-600 cursor-pointer">Créer un compte</span></p>
            </div>
            <div id="register-form" class="space-y-4 hidden text-left">
                <input type="text" id="reg-name" class="input-custom" placeholder="Nom Complet">
                <input type="email" id="reg-email" class="input-custom" placeholder="Email">
                <input type="password" id="reg-pass" class="input-custom" placeholder="Mot de passe">
                <div class="flex gap-2 p-1.5 bg-slate-50 rounded-2xl">
                    <button onclick="setRegRole('buyer')" id="role-btn-buyer" class="flex-1 py-3 rounded-xl text-[9px] font-black bg-white shadow-sm text-blue-600 uppercase">Acheteur</button>
                    <button onclick="setRegRole('seller')" id="role-btn-seller" class="flex-1 py-3 rounded-xl text-[9px] font-black text-slate-400 uppercase">Vendeur</button>
                </div>
                <button onclick="handleRegister()" class="btn-blue bg-green-600">S'inscrire</button>
                <p class="text-center text-[11px] text-slate-400 mt-6 font-bold uppercase">Déjà membre ? <span onclick="toggleAuth('login')" class="text-blue-600 cursor-pointer">Se connecter</span></p>
            </div>
        </div>
    </div>

    <!-- MAIN APP CONTENT -->
    <div id="app-content" style="display:none">
        <header class="fixed top-0 inset-x-0 h-20 px-6 flex items-center justify-between bg-white border-b border-slate-50 z-[500]">
            <div class="flex items-center gap-2">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-7">
                <h1 class="text-lg font-black italic text-blue-900">echoppe<span class="text-yellow-400">241</span></h1>
            </div>
            <img id="h-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-2xl bg-slate-100 object-cover cursor-pointer shadow-sm">
        </header>

        <main class="max-w-xl mx-auto px-6 pt-24">
            <!-- HOME VIEW -->
            <div id="view-home" class="view active">
                <div class="flex items-center justify-between mb-6">
                    <h2 class="text-xl font-black text-blue-900">Articles Récents</h2>
                    <button onclick="openModal('publish-modal')" id="btn-home-publish" class="hidden text-[10px] font-black text-blue-600 uppercase"><i class="fa-solid fa-plus-circle mr-1"></i>Vendre</button>
                </div>
                <div id="product-list" class="grid grid-cols-2 gap-4"></div>
            </div>

            <!-- MESSAGES VIEW -->
            <div id="view-messages" class="view">
                <h2 class="text-xl font-black text-blue-900 mb-6">Messages</h2>
                <div class="text-center py-20 text-slate-300">
                    <i class="fa-solid fa-comments text-4xl mb-4 opacity-10"></i>
                    <p class="text-[10px] font-bold uppercase tracking-widest">Bientôt disponible</p>
                </div>
            </div>

            <!-- PROFILE VIEW -->
            <div id="view-profile" class="view">
                <div class="card-neo p-6 mb-6 text-center relative">
                    <button onclick="openModal('edit-profile-modal')" class="absolute top-4 right-4 w-8 h-8 bg-slate-50 rounded-full text-slate-400 flex items-center justify-center"><i class="fa-solid fa-pen text-[10px]"></i></button>
                    <img id="p-avatar" class="w-20 h-20 rounded-3xl mx-auto mb-4 object-cover border-4 border-white shadow-md">
                    <h3 id="p-name" class="text-lg font-black text-blue-900 mb-1">Chargement...</h3>
                    <div id="p-role" class="role-badge mb-4 inline-block">Membre</div>
                    
                    <div class="bg-blue-600 p-5 rounded-3xl text-white text-left shadow-lg overflow-hidden relative">
                        <div class="absolute -right-4 -top-4 w-20 h-20 bg-white/10 rounded-full blur-2xl"></div>
                        <p class="text-[9px] font-bold opacity-60 uppercase tracking-tighter">Mon Solde EchoppePay</p>
                        <p id="p-wallet" class="text-2xl font-black tracking-tight relative z-10">0 F</p>
                    </div>
                </div>

                <!-- ADMIN TASKS -->
                <div id="admin-panel" class="hidden mb-6">
                    <h4 class="text-[10px] font-black text-red-500 uppercase mb-3 px-2 flex items-center gap-2">
                        <i class="fa-solid fa-shield-halved"></i> Demandes en attente
                    </h4>
                    <div id="admin-requests" class="space-y-3"></div>
                </div>

                <!-- TRANSACTIONS -->
                <div class="mb-6">
                    <h4 class="text-[11px] font-black text-slate-400 uppercase tracking-widest mb-4">Activités Récentes</h4>
                    <div id="transaction-history" class="space-y-2"></div>
                </div>

                <div class="space-y-2 pb-10">
                    <button onclick="openModal('recharge-modal')" class="w-full p-4 bg-slate-50 rounded-2xl text-[11px] font-bold text-slate-700 text-left flex justify-between items-center transition active:bg-slate-100">Réapprovisionner mon compte <i class="fa-solid fa-wallet text-blue-600"></i></button>
                    <button onclick="handleLogout()" class="w-full p-4 text-red-400 text-[10px] font-black uppercase tracking-widest mt-4">Fermer la session</button>
                </div>
            </div>
        </main>

        <!-- NAVIGATION BAR -->
        <nav class="fixed bottom-0 inset-x-0 h-20 bg-white border-t border-slate-50 flex items-center justify-around z-[500] px-4">
            <button onclick="navigateTo('home')" id="nav-home" class="nav-item active flex flex-col items-center gap-1">
                <i class="fa-solid fa-house-chimney text-lg"></i>
                <span class="text-[8px] font-bold uppercase">Accueil</span>
            </button>
            <button onclick="navigateTo('messages')" id="nav-messages" class="nav-item flex flex-col items-center gap-1">
                <i class="fa-solid fa-message text-lg"></i>
                <span class="text-[8px] font-bold uppercase">Messages</span>
            </button>
            <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item flex flex-col items-center gap-1">
                <i class="fa-solid fa-user-ninja text-lg"></i>
                <span class="text-[8px] font-bold uppercase">Profil</span>
            </button>
        </nav>
    </div>

    <!-- MODAL: EDIT PROFILE -->
    <div id="edit-profile-modal" class="modal-full">
        <h2 class="text-xl font-black mb-8 text-blue-900">Mon Compte</h2>
        <div class="space-y-6">
            <div onclick="document.getElementById('edit-avatar-input').click()" class="mx-auto w-32 h-32 rounded-3xl bg-slate-100 overflow-hidden relative cursor-pointer border-4 border-slate-50 shadow-sm">
                <img id="edit-avatar-preview" class="w-full h-full object-cover">
                <div class="absolute inset-0 bg-black/30 flex items-center justify-center text-white"><i class="fa-solid fa-camera"></i></div>
            </div>
            <input type="file" id="edit-avatar-input" accept="image/*" class="hidden" onchange="previewFile(this, 'edit-avatar-preview')">
            
            <div class="space-y-2">
                <label class="text-[10px] font-black text-slate-400 uppercase ml-2">Nom Affiché</label>
                <input type="text" id="edit-name" class="input-custom" placeholder="Nom complet">
            </div>

            <button onclick="saveProfile()" class="btn-blue mt-4">Appliquer les changements</button>
            <button onclick="closeModal('edit-profile-modal')" class="w-full text-[10px] font-bold text-slate-400 py-4 uppercase">Annuler</button>
        </div>
    </div>

    <!-- MODAL: PUBLISH PRODUCT -->
    <div id="publish-modal" class="modal-full">
        <h2 class="text-xl font-black mb-8 text-blue-900">Publier un article</h2>
        <div class="space-y-4">
            <div onclick="document.getElementById('file-input').click()" class="w-full aspect-video bg-slate-50 rounded-3xl border-2 border-dashed border-slate-200 flex flex-col items-center justify-center cursor-pointer overflow-hidden relative">
                <img id="photo-preview" class="hidden absolute inset-0 w-full h-full object-cover">
                <div id="photo-placeholder" class="text-center">
                    <i class="fa-solid fa-cloud-arrow-up text-2xl text-blue-400 mb-2"></i>
                    <p class="text-[10px] font-bold text-slate-400 uppercase">Ajouter une photo</p>
                </div>
            </div>
            <input type="file" id="file-input" accept="image/*" class="hidden" onchange="previewFile(this, 'photo-preview', 'photo-placeholder')">
            <input type="text" id="pub-name" class="input-custom" placeholder="Titre de l'annonce">
            <input type="number" id="pub-price" class="input-custom" placeholder="Prix final (FCFA)">
            <textarea id="pub-desc" class="input-custom h-24" placeholder="Description de l'article..."></textarea>
            
            <button onclick="handlePublish()" class="btn-blue mt-4">Mettre en vente</button>
            <button onclick="closeModal('publish-modal')" class="w-full text-[10px] font-bold text-slate-400 py-4 uppercase">Retour</button>
        </div>
    </div>

    <!-- MODAL: RECHARGE SOLDE -->
    <div id="recharge-modal" class="modal-full">
        <h2 class="text-xl font-black mb-8 text-blue-900">Réapprovisionner</h2>
        <div class="space-y-4">
            <div class="bg-blue-50 p-5 rounded-3xl border border-blue-100">
                <p class="text-[10px] font-black text-blue-800 mb-2 uppercase">Procédure</p>
                <p class="text-[11px] text-blue-700 leading-relaxed">
                    Effectuez le transfert Airtel/Moov au <span class="font-black underline">077 73 60 65 / 066 45 71 72</span>. 
                    Ensuite, saisissez le montant et l'ID de transaction reçu par SMS.
                </p>
            </div>
            <input type="number" id="rech-amount" class="input-custom" placeholder="Montant à créditer">
            <input type="text" id="rech-ref" class="input-custom" placeholder="Référence de la transaction">
            
            <button onclick="handleRechargeRequest()" class="btn-blue mt-4">Soumettre la preuve</button>
            <button onclick="closeModal('recharge-modal')" class="w-full text-xs font-bold text-slate-400 py-4 uppercase">Fermer</button>
        </div>
    </div>

    <!-- MODAL: DETAIL PRODUCT -->
    <div id="detail-modal" class="modal-full"><div id="detail-content"></div></div>

    <!-- SCRIPTS LOGIQUES (Priorité aux versions précédentes) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut, updateProfile } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc, updateDoc, collection, addDoc, query, where, serverTimestamp, orderBy, increment, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // CONFIGURATION OFFICIELLE (FIGÉE)
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
        const appId = 'echoppe241-prod-v1';

        let user = null;
        let userData = null;
        let curRegRole = 'buyer';
        let tempImageBase64 = null;

        // AUTHENTIFICATION & SYNC EN TEMPS RÉEL
        onAuthStateChanged(auth, (u) => {
            if (u) {
                user = u;
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        userData = snap.data();
                        document.getElementById('view-auth').classList.remove('active');
                        document.getElementById('app-content').style.display = 'block';
                        syncUI();
                        loadProducts();
                        loadTransactions();
                        if(userData.isAdmin) loadAdminRequests();
                    } else {
                        // Création du profil initial Firestore
                        const d = { 
                            fullName: u.displayName || "Utilisateur", 
                            email: u.email, 
                            walletBalance: 0, 
                            role: curRegRole, 
                            isAdmin: false, 
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}` 
                        };
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), d);
                    }
                });
            } else {
                user = null;
                document.getElementById('app-content').style.display = 'none';
                document.getElementById('view-auth').classList.add('active');
            }
            hideSplash();
        });

        // MISE À JOUR DE L'INTERFACE
        function syncUI() {
            document.getElementById('h-avatar').src = userData.avatar;
            document.getElementById('p-avatar').src = userData.avatar;
            document.getElementById('edit-avatar-preview').src = userData.avatar;
            document.getElementById('p-name').innerText = userData.fullName;
            document.getElementById('edit-name').value = userData.fullName;
            document.getElementById('p-wallet').innerText = `${userData.walletBalance.toLocaleString()} F`;
            
            const r = document.getElementById('p-role');
            r.innerText = userData.isAdmin ? 'Administrateur' : (userData.role === 'seller' ? 'Vendeur' : 'Acheteur');
            r.className = `role-badge mb-4 inline-block ${userData.isAdmin ? 'role-admin' : (userData.role === 'seller' ? 'role-seller' : 'role-buyer')}`;
            
            // Permissions d'affichage
            if(userData.role === 'seller' || userData.isAdmin) document.getElementById('btn-home-publish').classList.remove('hidden');
            if(userData.isAdmin) document.getElementById('admin-panel').classList.remove('hidden');
        }

        // LOGIQUE DE PROFIL
        window.saveProfile = async () => {
            const newName = document.getElementById('edit-name').value;
            if(!newName) return showToast("Nom requis");
            
            showToast("Mise à jour...");
            const updates = { fullName: newName };
            if(tempImageBase64) updates.avatar = tempImageBase64;
            
            await updateDoc(doc(db, 'artifacts', appId, 'users', user.uid), updates);
            tempImageBase64 = null;
            showToast("Modifications enregistrées !");
            closeModal('edit-profile-modal');
        };

        // RECHARGE & TRANSACTIONS
        window.handleRechargeRequest = async () => {
            const amount = parseInt(document.getElementById('rech-amount').value);
            const ref = document.getElementById('rech-ref').value;
            if(!amount || !ref) return showToast("Veuillez remplir les champs");
            
            showToast("Envoi de la preuve...");
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                userId: user.uid, userName: userData.fullName, amount: amount, reference: ref,
                status: 'pending', createdAt: serverTimestamp()
            });

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), {
                fromId: 'SYSTEM', toId: user.uid, amount: amount, type: 'recharge', 
                status: 'en attente', label: 'Dépôt EchoppePay', createdAt: serverTimestamp()
            });

            closeModal('recharge-modal');
            showToast("Demande transmise à l'admin");
        };

        // LOGIQUE ADMIN (VALIDATION)
        function loadAdminRequests() {
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), where('status', '==', 'pending')), (snap) => {
                const list = document.getElementById('admin-requests');
                if(snap.empty) { list.innerHTML = `<p class="text-center text-[9px] font-bold text-slate-300 uppercase py-4">Aucune attente</p>`; return; }
                
                list.innerHTML = snap.docs.map(d => {
                    const r = d.data();
                    return `
                        <div class="bg-white p-4 rounded-2xl shadow-sm border border-red-50">
                            <div class="flex justify-between mb-3">
                                <div><p class="text-[10px] font-black text-slate-800">${r.userName}</p><p class="text-[9px] text-slate-400">${r.reference}</p></div>
                                <p class="text-xs font-black text-blue-600">${r.amount} F</p>
                            </div>
                            <div class="flex gap-2">
                                <button onclick="approveRequest('${d.id}', '${r.userId}', ${r.amount})" class="flex-1 py-2 bg-green-500 text-white text-[9px] font-black rounded-xl uppercase">Valider</button>
                                <button onclick="cancelRequest('${d.id}')" class="flex-1 py-2 bg-slate-100 text-red-500 text-[9px] font-black rounded-xl uppercase">Refuser</button>
                            </div>
                        </div>
                    `;
                }).join('');
            });
        }

        window.approveRequest = async (docId, uid, amount) => {
            await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', docId), { status: 'approved' });
            await updateDoc(doc(db, 'artifacts', appId, 'users', uid), { walletBalance: increment(amount) });
            
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), {
                fromId: 'ADMIN', toId: uid, amount: amount, type: 'recharge', 
                status: 'validé', label: 'Recharge Approuvée', createdAt: serverTimestamp()
            });
            showToast("Compte client crédité !");
        };

        window.cancelRequest = async (docId) => {
            if(confirm("Refuser cette demande ?")) {
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', docId), { status: 'cancelled' });
                showToast("Demande refusée");
            }
        };

        // GESTION DES PRODUITS
        window.handlePublish = async () => {
            const name = document.getElementById('pub-name').value;
            const price = parseInt(document.getElementById('pub-price').value);
            const desc = document.getElementById('pub-desc').value;
            
            if(!name || !price || !tempImageBase64) return showToast("Données incomplètes");
            
            showToast("Publication...");
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                name, price, description: desc, image: tempImageBase64,
                sellerId: user.uid, sellerName: userData.fullName, createdAt: serverTimestamp()
            });
            
            tempImageBase64 = null;
            closeModal('publish-modal');
            showToast("Votre article est en ligne !");
        };

        function loadProducts() {
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'products'), orderBy('createdAt', 'desc')), (snap) => {
                const list = document.getElementById('product-list');
                list.innerHTML = snap.docs.map(d => {
                    const p = d.data();
                    return `
                        <div onclick="openProduct('${d.id}')" class="card-neo overflow-hidden flex flex-col active:scale-95 transition cursor-pointer">
                            <img src="${p.image}" class="w-full h-32 object-cover">
                            <div class="p-3">
                                <p class="text-[10px] font-black text-blue-900 truncate uppercase">${p.name}</p>
                                <p class="text-blue-600 font-bold text-[11px]">${p.price.toLocaleString()} F</p>
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
                const isMine = p.sellerId === user.uid || userData.isAdmin;
                
                document.getElementById('detail-content').innerHTML = `
                    <button onclick="closeModal('detail-modal')" class="mb-4 w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-arrow-left"></i></button>
                    <img src="${p.image}" class="w-full h-64 object-cover rounded-3xl mb-6 shadow-md border border-slate-50">
                    <h2 class="text-2xl font-black text-blue-900 mb-1">${p.name}</h2>
                    <p class="text-blue-600 font-black text-xl mb-6">${p.price.toLocaleString()} FCFA</p>
                    <div class="bg-slate-50 p-5 rounded-3xl mb-8">
                        <p class="text-[11px] text-slate-500 leading-relaxed">${p.description || 'Pas de détails.'}</p>
                    </div>
                    ${isMine ? `<button onclick="deleteProduct('${id}')" class="w-full p-4 mb-3 text-red-400 font-black text-[10px] uppercase">Supprimer</button>` : ''}
                    <button onclick="buyProduct('${id}', '${p.sellerId}', ${p.price}, '${p.name}')" class="w-full p-5 bg-blue-600 text-white rounded-3xl font-black uppercase text-xs">Payer avec EchoppePay</button>
                `;
                openModal('detail-modal');
            });
        };

        window.buyProduct = async (pid, sid, price, name) => {
            if(sid === user.uid) return showToast("C'est votre produit");
            if(userData.walletBalance < price) return showToast("Solde insuffisant");
            
            if(confirm(`Confirmer l'achat de "${name}" ?`)) {
                showToast("Traitement...");
                await updateDoc(doc(db, 'artifacts', appId, 'users', user.uid), { walletBalance: increment(-price) });
                await updateDoc(doc(db, 'artifacts', appId, 'users', sid), { walletBalance: increment(price) });
                
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), {
                    fromId: user.uid, toId: sid, amount: price, type: 'achat', 
                    status: 'terminé', label: `Achat: ${name}`, createdAt: serverTimestamp()
                });
                
                showToast("Achat réussi !");
                closeModal('detail-modal');
            }
        };

        window.deleteProduct = async (pid) => {
            if(confirm("Confirmer la suppression ?")) {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', pid));
                closeModal('detail-modal');
                showToast("Article retiré");
            }
        };

        // HISTORIQUE DES TRANSACTIONS
        function loadTransactions() {
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), 
                      where('toId', '==', user.uid), orderBy('createdAt', 'desc'));
            const q2 = query(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), 
                      where('fromId', '==', user.uid), orderBy('createdAt', 'desc'));
            
            onSnapshot(q2, (snap) => {
                const list = document.getElementById('transaction-history');
                if(snap.empty) { list.innerHTML = `<p class="text-[9px] text-slate-300 font-bold uppercase text-center py-6">Aucune activité</p>`; return; }
                
                list.innerHTML = snap.docs.map(d => {
                    const t = d.data();
                    const isDebit = t.type !== 'recharge';
                    return `
                        <div class="flex justify-between items-center bg-slate-50 p-4 rounded-2xl border border-slate-100">
                            <div class="flex items-center gap-3">
                                <div class="w-8 h-8 rounded-full flex items-center justify-center ${isDebit ? 'bg-red-50 text-red-400' : 'bg-green-50 text-green-400'}">
                                    <i class="fa-solid ${isDebit ? 'fa-minus' : 'fa-plus'} text-[9px]"></i>
                                </div>
                                <div><p class="text-[10px] font-black text-slate-700">${t.label}</p><p class="text-[8px] font-bold text-slate-400 uppercase">${t.status}</p></div>
                            </div>
                            <p class="text-xs font-black ${isDebit ? 'text-red-500' : 'text-green-600'}">${isDebit ? '-' : '+'}${t.amount} F</p>
                        </div>
                    `;
                }).join('');
            });
        }

        // HELPERS (UI & AUTH)
        window.handleLogin = () => signInWithEmailAndPassword(auth, document.getElementById('login-email').value, document.getElementById('login-pass').value).catch(() => showToast("Erreur de connexion"));
        window.handleRegister = () => createUserWithEmailAndPassword(auth, document.getElementById('reg-email').value, document.getElementById('reg-pass').value).catch(() => showToast("Email déjà utilisé"));
        window.handleLogout = () => signOut(auth);

        window.previewFile = (input, imgId, placeholderId = null) => {
            if (input.files && input.files[0]) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    tempImageBase64 = e.target.result;
                    document.getElementById(imgId).src = tempImageBase64;
                    document.getElementById(imgId).classList.remove('hidden');
                    if(placeholderId) document.getElementById(placeholderId).classList.add('hidden');
                };
                reader.readAsDataURL(input.files[0]);
            }
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

        window.setRegRole = (r) => {
            curRegRole = r;
            document.getElementById('role-btn-buyer').className = r === 'buyer' ? 'flex-1 py-3 rounded-xl text-[9px] font-black bg-white shadow-sm text-blue-600 uppercase' : 'flex-1 py-3 rounded-xl text-[9px] font-black text-slate-400 uppercase';
            document.getElementById('role-btn-seller').className = r === 'seller' ? 'flex-1 py-3 rounded-xl text-[9px] font-black bg-white shadow-sm text-blue-600 uppercase' : 'flex-1 py-3 rounded-xl text-[9px] font-black text-slate-400 uppercase';
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
            }, 1200);
        }
    </script>
</body>
</html>
