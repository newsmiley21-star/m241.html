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

        body { font-family: 'Inter', sans-serif; background-color: var(--white); color: var(--dark); margin: 0; overflow-x: hidden; }
        h1, h2, h3, .brand-font { font-family: 'Syne', sans-serif; text-transform: lowercase; }

        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .card-neo { background: var(--white); border-radius: 24px; border: 1px solid #f1f5f9; box-shadow: 0 4px 12px rgba(58, 117, 196, 0.05); }
        .btn-blue { background: var(--primary-blue); color: var(--white); padding: 18px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 11px; width: 100%; transition: 0.2s; border: none; cursor: pointer; }
        .btn-blue:disabled { opacity: 0.5; cursor: not-allowed; }
        
        .input-custom { background: #f8fafc; border-radius: 16px; padding: 16px; width: 100%; font-size: 14px; border: 2px solid transparent; outline: none; transition: 0.3s; }
        .input-custom:focus { border-color: var(--primary-blue); background: var(--white); }

        #toast { position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%); z-index: 10000; background: var(--dark); color: var(--white); padding: 12px 24px; border-radius: 50px; font-size: 11px; font-weight: 700; display: none; }

        .nav-item { color: #cbd5e1; transition: 0.3s; text-align: center; flex: 1; }
        .nav-item.active { color: var(--primary-blue); }

        .modal-full { position: fixed; inset: 0; background: var(--white); z-index: 9000; overflow-y: auto; padding: 32px 24px; display: none; }
        
        #splash-screen { position: fixed; inset: 0; background: var(--white); z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; transition: opacity 0.5s ease; }
        
        .chat-bubble { max-width: 80%; padding: 12px 16px; border-radius: 20px; font-size: 14px; margin-bottom: 8px; }
        .chat-me { background: var(--primary-blue); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .chat-them { background: #f1f5f9; color: var(--dark); align-self: flex-start; border-bottom-left-radius: 4px; }
    </style>
</head>
<body class="pb-24">

    <div id="toast">Action confirmée !</div>

    <!-- SPLASH SCREEN -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 h-24 mb-6">
        <div class="flex gap-1.5">
            <div class="w-2 h-2 rounded-full bg-[#3a75c4] animate-bounce"></div>
            <div class="w-2 h-2 rounded-full bg-[#fcd116] animate-bounce [animation-delay:0.2s]"></div>
            <div class="w-2 h-2 rounded-full bg-[#00853f] animate-bounce [animation-delay:0.4s]"></div>
        </div>
    </div>

    <!-- VUE AUTH (CONNEXION/INSCRIPTION) -->
    <div id="view-auth" class="view active min-h-screen flex items-center px-6">
        <div class="w-full max-w-md mx-auto">
            <div class="text-center mb-10">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-20 mx-auto mb-4">
                <h1 class="text-3xl font-black text-blue-900">Bienvenue.</h1>
                <p class="text-slate-400 text-sm mt-2">Le marché gabonais dans votre poche.</p>
            </div>

            <div id="auth-forms">
                <!-- Login -->
                <div id="login-form" class="space-y-4">
                    <input type="email" id="login-email" class="input-custom" placeholder="Email">
                    <input type="password" id="login-pass" class="input-custom" placeholder="Mot de passe">
                    <button onclick="handleLogin()" id="btn-login" class="btn-blue shadow-lg shadow-blue-100">Se connecter</button>
                    <p class="text-center text-xs text-slate-500 mt-4">Pas de compte ? <span onclick="toggleAuth('register')" class="text-blue-600 font-bold cursor-pointer">S'inscrire</span></p>
                </div>

                <!-- Register -->
                <div id="register-form" class="space-y-4 hidden">
                    <input type="text" id="reg-name" class="input-custom" placeholder="Nom complet">
                    <input type="email" id="reg-email" class="input-custom" placeholder="Email">
                    <input type="password" id="reg-pass" class="input-custom" placeholder="Mot de passe (min 6 car.)">
                    <button onclick="handleRegister()" id="btn-register" class="btn-blue bg-[#00853f]">Créer mon compte</button>
                    <p class="text-center text-xs text-slate-500 mt-4">Déjà inscrit ? <span onclick="toggleAuth('login')" class="text-blue-600 font-bold cursor-pointer">Connexion</span></p>
                </div>
            </div>
        </div>
    </div>

    <!-- MODALS -->
    <div id="publish-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-10">
                <h2 class="text-2xl font-black text-blue-900">publier.</h2>
                <button onclick="closeModal('publish-modal')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>
            <div class="space-y-5">
                <input type="text" id="pub-name" class="input-custom" placeholder="Nom du produit">
                <input type="number" id="pub-price" class="input-custom" placeholder="Prix en FCFA">
                <textarea id="pub-desc" class="input-custom h-32" placeholder="Description..."></textarea>
                <input type="text" id="pub-img" class="input-custom" placeholder="URL Image (ex: Unsplash)">
                <button onclick="publishProduct()" class="btn-blue">Mettre en vente</button>
            </div>
        </div>
    </div>

    <div id="recharge-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-8">
                <h2 class="text-2xl font-black text-blue-900">echoppepay.</h2>
                <button onclick="closeModal('recharge-modal')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>
            <div class="bg-blue-50 p-6 rounded-3xl mb-8">
                <p class="text-xs font-bold text-blue-800 mb-2">Paiement Manuel :</p>
                <p class="text-[10px] text-blue-600 mb-4">Envoyez au 077 73 60 65 (Airtel) ou 066 45 71 72 (Moov)</p>
                <input type="number" id="rech-amount" class="input-custom mb-3" placeholder="Montant">
                <input type="text" id="rech-ref" class="input-custom mb-4" placeholder="Référence Transaction">
                <button onclick="submitRecharge()" class="btn-blue bg-[#00853f]">Soumettre aux Admins</button>
            </div>
        </div>
    </div>

    <div id="chat-modal" class="modal-full !p-0">
        <div class="flex flex-col h-full">
            <div class="p-6 border-b border-slate-50 flex items-center gap-4">
                <button onclick="closeModal('chat-modal')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-arrow-left"></i></button>
                <div>
                    <h3 id="chat-title" class="font-black text-blue-900">Chat</h3>
                    <p class="text-[10px] text-slate-400 uppercase font-bold">Vendeur Echoppe</p>
                </div>
            </div>
            <div id="chat-messages" class="flex-1 overflow-y-auto p-6 flex flex-col gap-2 bg-slate-50/30"></div>
            <div class="p-4 border-t border-slate-50 bg-white flex gap-2">
                <input type="text" id="chat-input" class="input-custom !py-3" placeholder="Votre message...">
                <button onclick="sendChatMessage()" class="w-12 h-12 bg-blue-600 text-white rounded-2xl"><i class="fa-solid fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <div id="detail-modal" class="modal-full">
        <div id="detail-content" class="max-w-md mx-auto"></div>
    </div>

    <!-- MAIN APP WRAPPER (HIDDEN IF NOT AUTH) -->
    <div id="app-content" style="display:none">
        <header class="fixed top-0 inset-x-0 z-[500] h-20 px-6 flex items-center justify-between bg-white/90 backdrop-blur-xl border-b border-slate-50">
            <div class="flex items-center gap-2">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-8 h-8">
                <h1 class="text-xl font-black italic text-blue-900">echoppe<span class="text-[#fcd116]">241</span></h1>
            </div>
            <div class="flex items-center gap-4">
                <div class="text-right">
                    <p id="header-balance" class="text-[13px] font-black text-blue-600">0 F</p>
                    <span class="text-[8px] font-bold uppercase text-slate-400">EchoppePay</span>
                </div>
                <img id="header-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-2xl bg-slate-50 border-2 border-white shadow-sm object-cover">
            </div>
        </header>

        <main class="max-w-xl mx-auto px-6 pt-24 pb-12">
            <!-- ACCUEIL -->
            <div id="view-home" class="view active">
                <h2 class="text-3xl font-black text-blue-900 mb-8 leading-tight">Le Marché<br><span class="text-[#fcd116]">Gabonais.</span></h2>
                <div id="product-list" class="grid grid-cols-2 gap-4"></div>
            </div>

            <!-- CHATS LIST -->
            <div id="view-messages" class="view">
                <h2 class="text-3xl font-black text-blue-900 mb-8">Messages.</h2>
                <div id="conversations-list" class="space-y-3"></div>
            </div>

            <!-- PROFIL -->
            <div id="view-profile" class="view">
                <div class="card-neo p-6 mb-6 text-center">
                    <img id="p-img" class="w-24 h-24 rounded-3xl mx-auto mb-4 border-4 border-white shadow-lg object-cover">
                    <h3 id="p-name" class="text-2xl font-black text-blue-900">...</h3>
                    <p id="p-email" class="text-xs text-slate-400 mb-6">...</p>
                    
                    <div class="bg-blue-600 p-5 rounded-3xl text-white">
                        <p class="text-[9px] font-bold opacity-60 uppercase tracking-widest mb-1">Portefeuille EchoppePay</p>
                        <p id="p-wallet" class="text-xl font-black">0 FCFA</p>
                    </div>
                </div>

                <div class="space-y-3">
                    <button onclick="openModal('recharge-modal')" class="w-full p-5 bg-white border border-slate-100 rounded-2xl flex items-center justify-between">
                        <span class="text-xs font-bold uppercase text-slate-700">Recharger Mon Solde</span>
                        <i class="fa-solid fa-plus text-blue-600"></i>
                    </button>
                    <button onclick="openModal('publish-modal')" class="w-full p-5 bg-white border border-slate-100 rounded-2xl flex items-center justify-between">
                        <span class="text-xs font-bold uppercase text-slate-700">Vendre un article</span>
                        <i class="fa-solid fa-box text-blue-600"></i>
                    </button>
                    <button onclick="handleLogout()" class="w-full p-4 text-red-500 text-[10px] font-black uppercase">Déconnexion</button>
                </div>
            </div>
        </main>

        <nav class="fixed bottom-0 inset-x-0 h-20 bg-white border-t border-slate-100 flex items-center justify-around px-4 pb-2 z-[500]">
            <button onclick="navigateTo('home')" id="nav-home" class="nav-item active"><i class="fa-solid fa-house-chimney text-lg"></i><p class="text-[8px] font-bold mt-1">ACCUEIL</p></button>
            <button onclick="navigateTo('messages')" id="nav-messages" class="nav-item"><i class="fa-solid fa-comment-dots text-lg"></i><p class="text-[8px] font-bold mt-1">CHAT</p></button>
            <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item"><i class="fa-solid fa-user-ninja text-lg"></i><p class="text-[8px] font-bold mt-1">COMPTE</p></button>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc, updateDoc, collection, addDoc, query, where, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = 'echoppe241-stable-v1';

        let user = null;
        let userData = null;
        let activeChatId = null;

        // AUTH LOGIC
        onAuthStateChanged(auth, (u) => {
            if (u) {
                user = u;
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        userData = snap.data();
                        document.getElementById('view-auth').classList.remove('active');
                        document.getElementById('app-content').style.display = 'block';
                        updateProfileUI();
                        loadHomeData();
                    } else {
                        // Init profile if missing
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
                            fullName: u.displayName || "Utilisateur",
                            email: u.email,
                            walletBalance: 0,
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

        window.toggleAuth = (mode) => {
            document.getElementById('login-form').classList.toggle('hidden', mode === 'register');
            document.getElementById('register-form').classList.toggle('hidden', mode === 'login');
        };

        window.handleRegister = async () => {
            const name = document.getElementById('reg-name').value;
            const email = document.getElementById('reg-email').value;
            const pass = document.getElementById('reg-pass').value;
            if (!name || pass.length < 6) return showToast("Vérifiez vos infos (pass min 6)");
            
            try {
                const res = await createUserWithEmailAndPassword(auth, email, pass);
                await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid), {
                    fullName: name,
                    email: email,
                    walletBalance: 0,
                    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${res.user.uid}`
                });
            } catch (e) { showToast("Erreur d'inscription"); }
        };

        window.handleLogin = async () => {
            const email = document.getElementById('login-email').value;
            const pass = document.getElementById('login-pass').value;
            try { await signInWithEmailAndPassword(auth, email, pass); } 
            catch (e) { showToast("Identifiants incorrects"); }
        };

        window.handleLogout = () => signOut(auth);

        // UI SYNC
        function updateProfileUI() {
            if (!userData) return;
            document.getElementById('header-balance').innerText = `${userData.walletBalance} F`;
            document.getElementById('header-avatar').src = userData.avatar;
            document.getElementById('p-img').src = userData.avatar;
            document.getElementById('p-name').innerText = userData.fullName;
            document.getElementById('p-email').innerText = userData.email;
            document.getElementById('p-wallet').innerText = `${userData.walletBalance} FCFA`;
        }

        // PRODUCTS & HOME
        function loadHomeData() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const list = document.getElementById('product-list');
                list.innerHTML = snap.docs.map(d => {
                    const p = d.data();
                    return `
                        <div onclick="openDetail('${d.id}')" class="card-neo overflow-hidden flex flex-col">
                            <img src="${p.image || 'https://images.unsplash.com/photo-1542838132-92c53300491e?w=400'}" class="w-full h-32 object-cover">
                            <div class="p-3">
                                <p class="text-[10px] font-black uppercase truncate">${p.name}</p>
                                <p class="text-blue-600 font-bold text-xs">${p.price} F</p>
                            </div>
                        </div>
                    `;
                }).join('');
            });

            // Listen for user conversations
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'chats'), where('participants', 'array-contains', user.uid)), (snap) => {
                const list = document.getElementById('conversations-list');
                list.innerHTML = snap.docs.map(d => {
                    const c = d.data();
                    const otherName = c.names.find(n => n !== userData.fullName);
                    return `
                        <div onclick="openChat('${d.id}', '${otherName}')" class="p-4 bg-white border border-slate-50 rounded-2xl flex items-center justify-between">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 bg-blue-50 rounded-xl flex items-center justify-center text-blue-600"><i class="fa-solid fa-user"></i></div>
                                <div><p class="text-xs font-bold text-blue-900">${otherName}</p><p class="text-[10px] text-slate-400">Cliquez pour discuter</p></div>
                            </div>
                            <i class="fa-solid fa-chevron-right text-slate-200"></i>
                        </div>
                    `;
                }).join('');
            });
        }

        window.openDetail = (id) => {
            onSnapshot(doc(db, 'artifacts', appId, 'public', 'data', 'products', id), (snap) => {
                if (!snap.exists()) return;
                const p = snap.data();
                document.getElementById('detail-content').innerHTML = `
                    <button onclick="closeModal('detail-modal')" class="mb-4 w-10 h-10 bg-slate-100 rounded-full"><i class="fa-solid fa-times"></i></button>
                    <img src="${p.image || 'https://images.unsplash.com/photo-1542838132-92c53300491e?w=600'}" class="w-full h-64 object-cover rounded-3xl mb-6">
                    <h2 class="text-2xl font-black text-blue-900 mb-2">${p.name}</h2>
                    <p class="text-blue-600 font-bold text-xl mb-4">${p.price} FCFA</p>
                    <p class="text-slate-500 text-sm mb-8">${p.description}</p>
                    <button onclick="startChat('${p.sellerId}', '${p.sellerName}')" class="btn-blue">Contacter le vendeur</button>
                `;
                openModal('detail-modal');
            });
        };

        // MESSAGING SYSTEM
        window.startChat = async (sellerId, sellerName) => {
            if (sellerId === user.uid) return showToast("C'est votre annonce !");
            const chatId = [user.uid, sellerId].sort().join('_');
            await setDoc(doc(db, 'artifacts', appId, 'public', 'data', 'chats', chatId), {
                participants: [user.uid, sellerId],
                names: [userData.fullName, sellerName],
                lastUpdate: serverTimestamp()
            }, { merge: true });
            closeModal('detail-modal');
            openChat(chatId, sellerName);
        };

        window.openChat = (chatId, title) => {
            activeChatId = chatId;
            document.getElementById('chat-title').innerText = title;
            openModal('chat-modal');
            
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'chats', chatId, 'messages'), orderBy('timestamp', 'asc')), (snap) => {
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
            const txt = document.getElementById('chat-input').value;
            if (!txt || !activeChatId) return;
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid,
                text: txt,
                timestamp: serverTimestamp()
            });
            document.getElementById('chat-input').value = "";
        };

        // ECHOPPEPAY & PUBLISH
        window.publishProduct = async () => {
            const name = document.getElementById('pub-name').value;
            const price = document.getElementById('pub-price').value;
            const desc = document.getElementById('pub-desc').value;
            const img = document.getElementById('pub-img').value;
            if (!name || !price) return showToast("Nom et prix requis");

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                name, price: parseInt(price), description: desc, image: img,
                sellerId: user.uid, sellerName: userData.fullName, createdAt: serverTimestamp()
            });
            showToast("Produit publié !");
            closeModal('publish-modal');
        };

        window.submitRecharge = async () => {
            const amount = document.getElementById('rech-amount').value;
            const ref = document.getElementById('rech-ref').value;
            if (!amount || !ref) return showToast("Infos incomplètes");

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                userId: user.uid, amount: parseInt(amount), reference: ref, status: 'pending', createdAt: serverTimestamp()
            });
            showToast("Soumis ! En attente de validation admin.");
            closeModal('recharge-modal');
        };

        // HELPERS
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
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
            setTimeout(() => {
                const s = document.getElementById('splash-screen');
                if (s) { s.style.opacity = '0'; setTimeout(() => s.style.display = 'none', 500); }
            }, 1000);
        }
    </script>
</body>
</html>
