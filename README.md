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
        .view.active { display: block; animation: fadeIn 0.3s ease-out forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

        .card-neo { background: var(--white); border-radius: 24px; border: 1px solid #f1f5f9; box-shadow: 0 4px 12px rgba(58, 117, 196, 0.05); }
        .btn-blue { background: var(--primary-blue); color: var(--white); padding: 18px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 11px; width: 100%; transition: 0.2s; border: none; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-blue:active { transform: scale(0.98); }
        
        .input-custom { background: #f8fafc; border-radius: 16px; padding: 16px; width: 100%; font-size: 14px; border: 2px solid transparent; outline: none; transition: 0.3s; }
        .input-custom:focus { border-color: var(--primary-blue); background: var(--white); }

        #toast { position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%); z-index: 10000; background: var(--dark); color: var(--white); padding: 12px 24px; border-radius: 50px; font-size: 11px; font-weight: 700; display: none; text-align: center; min-width: 250px; box-shadow: 0 10px 25px rgba(0,0,0,0.2); }

        .nav-item { color: #cbd5e1; transition: 0.3s; text-align: center; flex: 1; cursor: pointer; border: none; background: none; }
        .nav-item.active { color: var(--primary-blue); }

        .modal-full { position: fixed; inset: 0; background: var(--white); z-index: 9000; overflow-y: auto; display: none; }
        
        #splash-screen { position: fixed; inset: 0; background: var(--white); z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; transition: 0.5s; }
        
        .chat-bubble { max-width: 80%; padding: 12px 16px; border-radius: 20px; font-size: 14px; margin-bottom: 8px; line-height: 1.5; }
        .chat-me { background: var(--primary-blue); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .chat-them { background: #f1f5f9; color: var(--dark); align-self: flex-start; border-bottom-left-radius: 4px; }
        
        .order-card { background: #fff; border: 2px solid var(--primary-blue); border-radius: 20px; padding: 16px; margin: 8px 0; color: var(--dark); box-shadow: 0 4px 15px rgba(58, 117, 196, 0.1); }
        
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
                <input type="password" id="reg-admin" class="input-custom" placeholder="Code Invitation (Admin)">
                <button onclick="handleRegister()" class="btn-blue bg-[#00853f]">Créer mon compte</button>
                <p class="text-center text-xs text-slate-500">Déjà inscrit ? <span onclick="toggleAuth('login')" class="text-blue-600 font-bold underline cursor-pointer">Se connecter</span></p>
            </div>
        </div>
    </div>

    <!-- Interface Principale -->
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
            <!-- Shop -->
            <div id="view-home" class="view active">
                <h2 class="text-2xl font-black text-blue-900 mb-6 tracking-tight">Le Marché.</h2>
                <div id="product-list" class="grid grid-cols-2 gap-4"></div>
            </div>

            <!-- Messages -->
            <div id="view-messages" class="view">
                <h2 class="text-2xl font-black text-blue-900 mb-6 tracking-tight">Discussions.</h2>
                <div id="conversations-list" class="space-y-3"></div>
            </div>

            <!-- Profil -->
            <div id="view-profile" class="view">
                <div class="card-neo p-6 text-center mb-6">
                    <div class="relative w-20 h-20 mx-auto mb-4">
                        <img id="p-avatar" class="w-20 h-20 rounded-3xl object-cover border-4 border-white shadow-md">
                        <div id="admin-badge" class="hidden absolute -top-1 -right-1 bg-red-500 text-white text-[8px] font-black px-2 py-1 rounded-full">ADMIN</div>
                    </div>
                    <h3 id="p-name" class="font-black text-xl text-blue-900">...</h3>
                    
                    <div class="bg-blue-600 p-5 rounded-3xl text-white text-left shadow-lg mt-4">
                        <p class="text-[8px] font-black opacity-60 uppercase tracking-widest mb-1">Mon Solde Echoppe</p>
                        <p id="p-balance" class="text-xl font-black">0 FCFA</p>
                    </div>
                </div>

                <div id="admin-panel" class="hidden mb-8">
                    <h4 class="text-[10px] font-black text-red-500 uppercase tracking-widest mb-4">Administration</h4>
                    <div id="admin-tasks" class="space-y-3"></div>
                </div>

                <div class="space-y-2">
                    <button onclick="openModal('publish-modal')" class="w-full p-4 bg-slate-50 rounded-2xl flex items-center justify-between text-xs font-bold text-slate-700">Vendre un article <i class="fa-solid fa-tag opacity-30"></i></button>
                    <button onclick="openModal('recharge-modal')" class="w-full p-4 bg-slate-50 rounded-2xl flex items-center justify-between text-xs font-bold text-slate-700">Recharger Solde <i class="fa-solid fa-wallet opacity-30"></i></button>
                    <button onclick="handleLogout()" class="w-full p-4 text-red-500 font-black text-[10px] uppercase mt-4">Déconnexion</button>
                </div>
            </div>
        </main>

        <nav class="fixed bottom-0 inset-x-0 h-20 bg-white/95 backdrop-blur-md border-t border-slate-100 flex items-center justify-around z-[500] pb-4">
            <button onclick="navigateTo('home')" id="nav-home" class="nav-item active"><i class="fa-solid fa-shop text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Shop</p></button>
            <button onclick="navigateTo('messages')" id="nav-messages" class="nav-item"><i class="fa-solid fa-comment-dots text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Chat</p></button>
            <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item"><i class="fa-solid fa-user-circle text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Moi</p></button>
        </nav>
    </div>

    <!-- Modale Chat -->
    <div id="chat-modal" class="modal-full !p-0">
        <div class="flex flex-col h-full bg-white">
            <div class="p-4 border-b border-slate-50 flex items-center justify-between sticky top-0 bg-white z-10">
                <div class="flex items-center gap-3">
                    <button onclick="closeModal('chat-modal')" class="w-10 h-10 bg-slate-50 rounded-xl flex items-center justify-center"><i class="fa-solid fa-arrow-left"></i></button>
                    <div>
                        <h3 id="chat-title" class="font-black text-blue-900 text-sm">...</h3>
                        <p class="text-[8px] text-blue-500 font-black uppercase">En ligne</p>
                    </div>
                </div>
                <button onclick="openModal('create-order-modal')" class="bg-blue-600 text-white text-[9px] font-black px-4 py-2 rounded-xl shadow-lg">GÉRER UN BON</button>
            </div>
            
            <div id="chat-messages" class="flex-1 overflow-y-auto p-4 flex flex-col bg-slate-50/20"></div>

            <div class="p-4 flex gap-2 border-t bg-white">
                <input type="text" id="chat-input" class="input-custom !py-3 flex-1" placeholder="Ecrivez ici...">
                <button onclick="sendMessage()" class="w-12 h-12 bg-blue-600 text-white rounded-xl shadow-lg flex items-center justify-center"><i class="fa-solid fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <!-- Autres Modales -->
    <div id="create-order-modal" class="modal-full p-6">
        <div class="max-w-md mx-auto">
            <h2 class="text-2xl font-black text-blue-900 mb-6 italic">Générer un Bon.</h2>
            <div class="space-y-4">
                <input type="text" id="order-item" class="input-custom" placeholder="Article(s)">
                <input type="number" id="order-price" class="input-custom" placeholder="Prix Total (FCFA)">
                <button onclick="submitOrder()" class="btn-blue mt-4">Envoyer le Bon</button>
                <button onclick="closeModal('create-order-modal')" class="w-full py-4 text-xs font-bold text-slate-400">Retour</button>
            </div>
        </div>
    </div>

    <div id="recharge-modal" class="modal-full p-6">
        <div class="max-w-md mx-auto text-center">
            <h2 class="text-2xl font-black text-blue-900 mb-8 italic">Rechargement.</h2>
            <div class="bg-blue-600 p-8 rounded-[40px] text-white text-left mb-6 shadow-2xl">
                <p class="text-xs mb-4 opacity-80">Transférez sur le <span class="font-black">077 73 60 65</span> (Airtel Money)</p>
                <input type="number" id="rech-amount" class="w-full bg-white/10 p-4 rounded-2xl text-white outline-none mb-3 placeholder:text-white/40 font-bold" placeholder="Montant">
                <input type="text" id="rech-ref" class="w-full bg-white/10 p-4 rounded-2xl text-white outline-none placeholder:text-white/40" placeholder="Référence Transaction">
            </div>
            <button onclick="requestRecharge()" class="btn-blue">Envoyer Preuve</button>
            <button onclick="closeModal('recharge-modal')" class="w-full py-4 text-xs font-bold text-slate-400">Annuler</button>
        </div>
    </div>

    <div id="publish-modal" class="modal-full p-6">
        <div class="max-w-md mx-auto">
            <h2 class="text-2xl font-black text-blue-900 mb-6 italic">Vendre.</h2>
            <div class="space-y-4">
                <input type="text" id="pub-name" class="input-custom" placeholder="Titre de l'annonce">
                <input type="number" id="pub-price" class="input-custom" placeholder="Prix (FCFA)">
                <input type="text" id="pub-img-url" class="input-custom" placeholder="URL de l'image (Lien direct)">
                <button onclick="publishArticle()" class="btn-blue">Publier l'annonce</button>
                <button onclick="closeModal('publish-modal')" class="w-full py-4 text-xs font-bold text-slate-400">Retour</button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, updateDoc, collection, addDoc, query, where, orderBy, serverTimestamp, increment, runTransaction, getDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // VOS CONFIGURATIONS FIREBASE
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            databaseURL: "https://communautedugabon-default-rtdb.firebaseio.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe241-prod-v1';
        const SECRET_ADMIN = "GABON241";

        let user = null;
        let userData = null;
        let activeChatId = null;

        // AUTH
        onAuthStateChanged(auth, async (u) => {
            if (u) {
                user = u;
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        userData = snap.data();
                        document.getElementById('view-auth').classList.remove('active');
                        document.getElementById('app-content').style.display = 'block';
                        syncUI();
                        loadHome();
                        loadConversations();
                        if(userData.isAdmin) loadAdminInterface();
                    } else {
                        const isAdm = localStorage.getItem('tmp_is_adm') === 'true';
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
                            fullName: localStorage.getItem('tmp_name') || 'Membre Echoppe',
                            walletBalance: 0,
                            isAdmin: isAdm,
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`,
                            createdAt: serverTimestamp()
                        });
                    }
                });
            } else {
                user = null;
                document.getElementById('app-content').style.display = 'none';
                document.getElementById('view-auth').classList.add('active');
            }
            document.getElementById('splash-screen').style.opacity = '0';
            setTimeout(() => document.getElementById('splash-screen').style.display = 'none', 500);
        });

        window.handleLogin = () => {
            const e = document.getElementById('login-email').value;
            const p = document.getElementById('login-pass').value;
            signInWithEmailAndPassword(auth, e, p).catch(err => showToast("Erreur: " + err.message));
        };

        window.handleRegister = () => {
            const n = document.getElementById('reg-name').value;
            const e = document.getElementById('reg-email').value;
            const p = document.getElementById('reg-pass').value;
            const a = document.getElementById('reg-admin').value;
            if(!n || !e || p.length < 6) return showToast("Vérifiez vos informations.");
            
            localStorage.setItem('tmp_name', n);
            localStorage.setItem('tmp_is_adm', a === SECRET_ADMIN);
            createUserWithEmailAndPassword(auth, e, p).catch(err => showToast(err.message));
        };

        window.handleLogout = () => signOut(auth);

        function syncUI() {
            document.getElementById('header-balance').innerText = `${userData.walletBalance} F`;
            document.getElementById('p-balance').innerText = `${userData.walletBalance} FCFA`;
            document.getElementById('header-avatar').src = userData.avatar;
            document.getElementById('p-avatar').src = userData.avatar;
            document.getElementById('p-name').innerText = userData.fullName;
            if(userData.isAdmin) document.getElementById('admin-badge').classList.remove('hidden');
        }

        // MARCHÉ
        function loadHome() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const list = document.getElementById('product-list');
                list.innerHTML = snap.docs.map(d => {
                    const p = d.data();
                    return `<div onclick="startChat('${p.sellerId}', '${p.sellerName}')" class="card-neo overflow-hidden active:scale-95 transition cursor-pointer">
                        <img src="${p.image || 'https://via.placeholder.com/150'}" class="h-32 w-full object-cover">
                        <div class="p-3">
                            <p class="text-[10px] font-black text-blue-900 truncate">${p.name}</p>
                            <p class="text-blue-600 font-bold text-xs mt-1">${p.price} F</p>
                        </div>
                    </div>`;
                }).join('');
            });
        }

        window.publishArticle = async () => {
            const n = document.getElementById('pub-name').value;
            const p = document.getElementById('pub-price').value;
            const i = document.getElementById('pub-img-url').value;
            if(!n || !p) return showToast("Informations manquantes.");
            
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                name: n, price: parseInt(p), image: i, sellerId: user.uid, sellerName: userData.fullName, createdAt: serverTimestamp()
            });
            showToast("Annonce publiée !");
            closeModal('publish-modal');
        };

        // CHAT
        window.startChat = async (sid, sname) => {
            if(sid === user.uid) return navigateTo('profile');
            const cid = [user.uid, sid].sort().join('_');
            const chatRef = doc(db, 'artifacts', appId, 'public', 'data', 'chats', cid);
            const chatSnap = await getDoc(chatRef);
            
            if(!chatSnap.exists()) {
                await setDoc(chatRef, {
                    participants: [user.uid, sid],
                    names: { [user.uid]: userData.fullName, [sid]: sname },
                    lastUpdate: serverTimestamp()
                });
            }
            openChatUI(cid, sname);
        };

        function loadConversations() {
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'chats'), where('participants', 'array-contains', user.uid));
            onSnapshot(q, (snap) => {
                const list = document.getElementById('conversations-list');
                list.innerHTML = snap.docs.map(d => {
                    const c = d.data();
                    const otherId = c.participants.find(p => p !== user.uid);
                    const name = c.names[otherId] || "Utilisateur";
                    return `<div onclick="openChatUI('${d.id}', '${name}')" class="p-4 bg-white border border-slate-50 rounded-2xl flex items-center gap-3 active:bg-slate-50">
                        <div class="w-10 h-10 bg-blue-50 rounded-xl flex items-center justify-center text-blue-600 font-black">${name[0]}</div>
                        <div class="flex-1">
                            <p class="text-xs font-black text-blue-900">${name}</p>
                            <p class="text-[9px] text-slate-400">Ouvrir la discussion</p>
                        </div>
                    </div>`;
                }).join('');
            });
        }

        window.openChatUI = (cid, name) => {
            activeChatId = cid;
            document.getElementById('chat-title').innerText = name;
            document.getElementById('chat-modal').style.display = 'block';
            
            const msgQuery = query(collection(db, 'artifacts', appId, 'public', 'data', 'chats', cid, 'messages'), orderBy('timestamp', 'asc'));
            onSnapshot(msgQuery, (snap) => {
                const box = document.getElementById('chat-messages');
                box.innerHTML = snap.docs.map(d => {
                    const m = d.data();
                    const isMe = m.senderId === user.uid;
                    let body = m.text;

                    if(m.type === 'order') {
                        const status = m.status === 'validated' ? '✅ LIVRÉ' : (m.status === 'paid' ? '💰 PAYÉ (EN ATTENTE ADMIN)' : '🕒 EN ATTENTE');
                        body = `<div class="order-card">
                            <p class="text-[7px] font-black uppercase text-blue-500 mb-2">BON DE COMMANDE</p>
                            <p class="text-sm font-bold text-blue-900">${m.item}</p>
                            <p class="text-xs font-black text-blue-600 mt-1">${m.price} FCFA</p>
                            <div class="mt-4 flex items-center justify-between border-t border-slate-50 pt-3">
                                <span class="text-[8px] font-black text-slate-300">${status}</span>
                                ${(!isMe && m.status === 'pending') ? `<button onclick="processPayment('${d.id}', ${m.price}, '${m.senderId}')" class="bg-green-600 text-white text-[8px] font-black px-4 py-2 rounded-full">PAYER</button>` : ''}
                            </div>
                        </div>`;
                    }
                    return `<div class="chat-bubble ${isMe ? 'chat-me' : 'chat-them'}">${body}</div>`;
                }).join('');
                box.scrollTop = box.scrollHeight;
            });
        };

        window.sendMessage = async () => {
            const i = document.getElementById('chat-input');
            if(!i.value.trim()) return;
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid, text: i.value, type: 'text', timestamp: serverTimestamp()
            });
            i.value = '';
        };

        window.submitOrder = async () => {
            const item = document.getElementById('order-item').value;
            const price = document.getElementById('order-price').value;
            if(!item || !price) return showToast("Vérifiez les champs.");

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid, item, price: parseInt(price), type: 'order', status: 'pending', timestamp: serverTimestamp()
            });
            closeModal('create-order-modal');
        };

        window.processPayment = async (msgId, amount, sellerId) => {
            if(userData.walletBalance < amount) return showToast("Solde insuffisant.");
            try {
                await runTransaction(db, async (tx) => {
                    const buyerRef = doc(db, 'artifacts', appId, 'users', user.uid);
                    tx.update(buyerRef, { walletBalance: increment(-amount) });
                    const msgRef = doc(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages', msgId);
                    tx.update(msgRef, { status: 'paid' });
                    const adminTask = doc(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'));
                    tx.set(adminTask, {
                        type: 'payout', amount, sellerId, buyerId: user.uid, buyerName: userData.fullName, 
                        msgId, chatId: activeChatId, status: 'pending', createdAt: serverTimestamp()
                    });
                });
                showToast("Paiement effectué !");
            } catch(e) { showToast("Erreur transaction."); }
        };

        // ADMIN
        function loadAdminInterface() {
            document.getElementById('admin-panel').classList.remove('hidden');
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'), where('status', '==', 'pending'));
            onSnapshot(q, (snap) => {
                const box = document.getElementById('admin-tasks');
                box.innerHTML = snap.docs.map(d => {
                    const t = d.data();
                    const isRech = t.type === 'recharge';
                    return `<div class="bg-slate-50 p-4 rounded-2xl flex items-center justify-between border border-slate-100">
                        <div>
                            <p class="text-[8px] font-black uppercase text-blue-600">${isRech ? 'DÉPÔT' : 'VENTE'}</p>
                            <p class="text-xs font-bold text-blue-900">${t.userName || t.buyerName}</p>
                            <p class="text-[9px] font-bold">${t.amount} FCFA</p>
                        </div>
                        <button onclick="approveTask('${d.id}')" class="bg-blue-600 text-white text-[8px] font-black px-4 py-2 rounded-xl">OK</button>
                    </div>`;
                }).join('');
            });
        }

        window.approveTask = async (tid) => {
            const tRef = doc(db, 'artifacts', appId, 'public', 'data', 'admin_tasks', tid);
            const tSnap = await getDoc(tRef);
            const t = tSnap.data();

            if(t.type === 'recharge') {
                await updateDoc(doc(db, 'artifacts', appId, 'users', t.uid), { walletBalance: increment(t.amount) });
            } else {
                await updateDoc(doc(db, 'artifacts', appId, 'users', t.sellerId), { walletBalance: increment(t.amount) });
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'chats', t.chatId, 'messages', t.msgId), { status: 'validated' });
            }
            await updateDoc(tRef, { status: 'completed' });
            showToast("Approuvé !");
        };

        window.requestRecharge = async () => {
            const a = document.getElementById('rech-amount').value;
            const r = document.getElementById('rech-ref').value;
            if(!a || !r) return showToast("Informations requises.");
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'), {
                type: 'recharge', uid: user.uid, userName: userData.fullName, amount: parseInt(a), ref: r, status: 'pending', createdAt: serverTimestamp()
            });
            showToast("Demande envoyée.");
            closeModal('recharge-modal');
        };

        // NAV
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
    </script>
</body>
</html>
