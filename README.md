<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 | Supply Chain & Messagerie</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Urbanist:wght@300;400;700;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --brand-dark: #0f172a;
            --brand-gold: #fcd116;
            --brand-green: #00853f;
            --brand-blue: #3a75c4;
        }
        body { font-family: 'Urbanist', sans-serif; background-color: #f8fafc; color: var(--brand-dark); overflow-x: hidden; }
        
        .role-card { transition: all 0.3s ease; cursor: pointer; border: 2px solid transparent; }
        .role-card.active { border-color: var(--brand-gold); background: #fffbeb; transform: translateY(-5px); }

        .logistics-graph {
            background: linear-gradient(90deg, #f1f5f9 2px, transparent 2px) 0 0 / 40px 40px,
                        linear-gradient(#f1f5f9 2px, transparent 2px) 0 0 / 40px 40px;
            background-color: white;
            position: relative;
            height: 150px;
            border-radius: 20px;
            overflow: hidden;
        }

        .line-animate {
            position: absolute;
            height: 2px;
            background: repeating-linear-gradient(90deg, var(--brand-blue), var(--brand-blue) 10px, transparent 10px, transparent 20px);
            animation: flow 2s linear infinite;
        }

        @keyframes flow { from { background-position: 0 0; } to { background-position: 40px 0; } }
        .hidden { display: none; }
        
        .status-badge {
            font-size: 8px; font-weight: 900; padding: 2px 8px; border-radius: 10px; text-transform: uppercase;
        }
        .status-pending { background: #fef3c7; color: #92400e; }
        .status-transit { background: #dbeafe; color: #1e40af; }
        .status-delivered { background: #d1fae5; color: #065f46; }

        .chat-bubble { max-width: 80%; padding: 12px 16px; border-radius: 20px; margin-bottom: 8px; font-size: 13px; font-weight: 600; line-height: 1.4; }
        .chat-mine { background: var(--brand-blue); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .chat-theirs { background: #f1f5f9; color: var(--brand-dark); align-self: flex-start; border-bottom-left-radius: 4px; }

        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
    </style>
</head>
<body class="antialiased">

    <!-- Notification Toast -->
    <div id="toast" class="fixed bottom-10 left-1/2 -translate-x-1/2 z-[1000] hidden animate-bounce">
        <div class="bg-slate-900 text-white px-6 py-3 rounded-full shadow-2xl flex items-center gap-3 border border-amber-400">
            <i class="fa-solid fa-bell text-amber-400"></i>
            <span id="toast-msg" class="text-xs font-bold uppercase tracking-widest">Action réussie !</span>
        </div>
    </div>

    <!-- Navigation -->
    <header class="bg-white/80 backdrop-blur-md border-b sticky top-0 z-[100]">
        <div class="h-1 flex">
            <div class="flex-1 bg-[#00853f]"></div>
            <div class="flex-1 bg-[#fcd116]"></div>
            <div class="flex-1 bg-[#3a75c4]"></div>
        </div>
        <div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="location.reload()">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Logo" class="h-10">
                <span class="font-black text-xl tracking-tighter uppercase">ECHOPPE<span class="text-amber-500">241</span></span>
            </div>
            
            <div id="user-controls" class="flex items-center gap-4">
                <div id="user-profile-brief" class="hidden text-right">
                    <p id="nav-user-name" class="text-xs font-black uppercase text-slate-900 leading-none"></p>
                    <p id="nav-user-role" class="text-[9px] font-bold text-amber-600 uppercase tracking-widest"></p>
                </div>
                <button id="auth-btn" onclick="toggleAuthModal()" class="bg-slate-900 text-white px-6 py-3 rounded-2xl font-black text-[10px] uppercase tracking-widest hover:bg-slate-700 transition shadow-lg shadow-slate-200">
                    Connexion
                </button>
                <button id="logout-btn" onclick="handleLogout()" class="hidden bg-red-50 text-red-500 p-3 rounded-xl hover:bg-red-100 transition">
                    <i class="fa-solid fa-power-off"></i>
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-6 min-h-screen">
        <!-- Dashboard Admin -->
        <section id="view-admin" class="hidden space-y-8 animate-in fade-in duration-500">
            <div class="grid grid-cols-1 md:grid-cols-4 gap-6">
                <div class="bg-white p-6 rounded-3xl border shadow-sm">
                    <h5 class="text-[10px] font-black uppercase text-slate-400 mb-2">Total Produits</h5>
                    <p class="text-3xl font-black" id="stat-products">0</p>
                </div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm">
                    <h5 class="text-[10px] font-black uppercase text-slate-400 mb-2">Flux Transit</h5>
                    <p class="text-3xl font-black text-blue-600" id="stat-transit">0</p>
                </div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm border-amber-200 bg-amber-50">
                    <h5 class="text-[10px] font-black uppercase text-amber-600 mb-2">Comptes à valider</h5>
                    <p class="text-3xl font-black text-amber-700" id="stat-pending">0</p>
                </div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm">
                    <h5 class="text-[10px] font-black uppercase text-slate-400 mb-2">Communauté</h5>
                    <p class="text-3xl font-black" id="stat-users">0</p>
                </div>
            </div>
            
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white rounded-[2rem] p-8 border">
                    <h3 class="font-black text-xl mb-6 flex items-center gap-3 uppercase tracking-tighter">
                        <i class="fa-solid fa-user-shield text-blue-500"></i> Nouveaux Producteurs
                    </h3>
                    <div id="admin-pending-producers" class="space-y-4"></div>
                </div>
                <div class="bg-white rounded-[2rem] p-8 border">
                    <h3 class="font-black text-xl mb-6 flex items-center gap-3 uppercase tracking-tighter">
                        <i class="fa-solid fa-truck-moving text-emerald-500"></i> Contrôle Logistique
                    </h3>
                    <div id="admin-logistics-list" class="space-y-4 max-h-[400px] overflow-y-auto pr-4"></div>
                </div>
            </div>
        </section>

        <!-- Dashboard Producteur -->
        <section id="view-producer" class="hidden space-y-8">
            <div id="producer-pending-alert" class="hidden bg-amber-50 border border-amber-200 p-6 rounded-3xl flex items-center gap-4">
                <div class="w-12 h-12 bg-amber-500 text-white rounded-2xl flex items-center justify-center text-xl animate-pulse">
                    <i class="fa-solid fa-hourglass-half"></i>
                </div>
                <div>
                    <h4 class="font-black text-amber-900 uppercase text-xs">Validation en cours</h4>
                    <p class="text-sm text-amber-800">L'équipe Echoppe241 vérifie vos documents officiels.</p>
                </div>
            </div>

            <div class="flex flex-col md:flex-row justify-between items-start md:items-center bg-slate-900 text-white p-10 rounded-[2.5rem] gap-6">
                <div>
                    <h2 class="text-4xl font-black tracking-tighter uppercase">Console Business</h2>
                    <p class="text-amber-400 text-xs font-black uppercase tracking-widest mt-2">Gestion de vos expéditions nationales</p>
                </div>
                <button id="btn-new-dispatch" onclick="openPublishModal()" class="bg-white text-slate-900 px-8 py-5 rounded-2xl font-black text-xs uppercase tracking-widest hover:bg-amber-400 transition shadow-xl">
                    + Publier un produit
                </button>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                <div class="lg:col-span-1 space-y-8">
                    <div class="bg-white p-8 rounded-[2.5rem] border">
                        <h3 class="font-black text-xs mb-6 uppercase text-slate-400 tracking-widest">Flux Supply Chain</h3>
                        <div class="logistics-graph border p-4 flex items-center justify-between px-6">
                            <div class="flex flex-col items-center gap-2">
                                <div class="w-10 h-10 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center"><i class="fa-solid fa-warehouse"></i></div>
                                <span class="text-[8px] font-black uppercase">Origine</span>
                            </div>
                            <div class="flex-grow mx-2 relative h-1 bg-slate-100 rounded-full">
                                <div class="line-animate w-full h-full rounded-full"></div>
                            </div>
                            <div class="flex flex-col items-center gap-2">
                                <div class="w-10 h-10 bg-blue-100 text-blue-600 rounded-full flex items-center justify-center"><i class="fa-solid fa-truck-fast"></i></div>
                                <span class="text-[8px] font-black uppercase">Hub</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-white p-8 rounded-[2.5rem] border shadow-sm">
                        <h3 class="font-black text-xs mb-6 uppercase text-slate-400 tracking-widest">Messages Clients</h3>
                        <div id="producer-chats" class="space-y-3">
                             <p class="text-[10px] text-slate-400 italic">Connectez-vous pour voir vos messages.</p>
                        </div>
                    </div>
                </div>

                <div class="lg:col-span-2 bg-white p-8 rounded-[2.5rem] border shadow-sm">
                    <div class="flex justify-between items-center mb-8">
                        <h3 class="font-black text-xs uppercase text-slate-400 tracking-widest">Inventaire & Suivi</h3>
                        <span class="bg-slate-100 px-3 py-1 rounded-full text-[10px] font-bold" id="prod-inv-count">0 articles</span>
                    </div>
                    <div id="producer-inventory" class="grid grid-cols-1 md:grid-cols-2 gap-4 max-h-[600px] overflow-y-auto pr-2">
                        <!-- Produits producteur -->
                    </div>
                </div>
            </div>
        </section>

        <!-- Vue Acheteur -->
        <section id="view-buyer" class="space-y-12">
            <div class="bg-gradient-to-br from-slate-900 to-slate-800 rounded-[3rem] p-12 text-white relative overflow-hidden flex items-center min-h-[450px] shadow-2xl">
                <div class="absolute top-0 right-0 w-1/2 h-full bg-cover bg-center opacity-20" style="background-image: url('https://images.unsplash.com/photo-1542601906990-b4d3fb773b09?auto=format&fit=crop&w=800&q=80');"></div>
                <div class="relative z-10 max-w-2xl">
                    <div class="flex gap-2 mb-6">
                        <span class="bg-emerald-500 text-[10px] font-black px-4 py-1 rounded-full uppercase">100% Gabonais</span>
                        <span class="bg-amber-500 text-[10px] font-black px-4 py-1 rounded-full uppercase">Direct Producteur</span>
                    </div>
                    <h2 class="text-7xl font-black mb-6 leading-none tracking-tighter">Mangez <br><span class="text-amber-400">Local</span>, Soutenez <br>le <span class="text-emerald-400">Gabon</span>.</h2>
                    <p class="text-slate-300 text-lg mb-8 font-medium">L'Echoppe241 connecte les 9 provinces pour vous livrer le meilleur de nos terroirs.</p>
                    <div class="flex gap-4">
                         <button onclick="document.getElementById('buyer-grid').scrollIntoView({behavior:'smooth'})" class="bg-white text-slate-900 px-10 py-5 rounded-2xl font-black uppercase text-[10px] tracking-widest hover:scale-105 transition shadow-xl">Parcourir les produits</button>
                         <button class="border border-white/20 px-10 py-5 rounded-2xl font-black uppercase text-[10px] tracking-widest hover:bg-white/10 transition">Nos provinces</button>
                    </div>
                </div>
            </div>

            <div class="flex justify-between items-end">
                <div>
                    <h3 class="text-4xl font-black tracking-tighter uppercase">Le Marché <span class="text-amber-500">Echoppe</span></h3>
                    <p class="text-slate-500 text-xs font-bold uppercase tracking-widest mt-1">Sélection du jour</p>
                </div>
                <div class="flex gap-2">
                    <button class="w-10 h-10 border rounded-full flex items-center justify-center text-slate-400 hover:border-slate-900 hover:text-slate-900 transition"><i class="fa-solid fa-filter text-xs"></i></button>
                </div>
            </div>

            <div id="buyer-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
                <!-- Produits dynamiques -->
            </div>
        </section>
    </main>

    <!-- Modals -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[500] hidden flex items-center justify-center p-6">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 shadow-2xl relative">
            <button onclick="toggleAuthModal()" class="absolute top-8 right-8 text-slate-300 text-2xl hover:text-slate-900">&times;</button>
            <div class="text-center mb-10">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" class="h-10 mx-auto mb-4">
                <h2 id="auth-title" class="text-2xl font-black tracking-tighter uppercase">Connexion</h2>
            </div>
            <form id="register-form" class="space-y-4 hidden">
                <input type="text" id="reg-name" placeholder="Nom Complet" class="w-full bg-slate-100 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-amber-400 transition" required>
                <input type="email" id="reg-email" placeholder="Email" class="w-full bg-slate-100 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-amber-400 transition" required>
                <input type="tel" id="reg-phone" placeholder="Numéro WhatsApp (+241)" class="w-full bg-slate-100 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-amber-400 transition" required>
                <input type="password" id="reg-pass" placeholder="Mot de passe" class="w-full bg-slate-100 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-amber-400 transition" required>
                <div class="py-4">
                    <div class="grid grid-cols-2 gap-3">
                        <div onclick="selectRegRole('buyer')" id="role-buyer" class="role-card active p-4 rounded-2xl bg-slate-50 text-center">
                            <i class="fa-solid fa-bag-shopping block mb-1"></i>
                            <span class="text-[10px] font-black uppercase tracking-widest">Acheteur</span>
                        </div>
                        <div onclick="selectRegRole('producer')" id="role-producer" class="role-card p-4 rounded-2xl bg-slate-50 text-center">
                            <i class="fa-solid fa-wheat-awn block mb-1"></i>
                            <span class="text-[10px] font-black uppercase tracking-widest">Producteur</span>
                        </div>
                    </div>
                </div>
                <button type="submit" class="w-full bg-slate-900 text-white py-5 rounded-2xl font-black uppercase text-xs tracking-widest shadow-xl">Créer mon compte</button>
                <p class="text-center text-[10px] mt-4 font-bold uppercase text-slate-400">Déjà membre ? <button type="button" onclick="switchAuthMode('login')" class="text-amber-600 underline">Se connecter</button></p>
            </form>
            <form id="login-form" class="space-y-4">
                <input type="email" id="login-email" placeholder="Email" class="w-full bg-slate-100 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-amber-400 transition" required>
                <input type="password" id="login-pass" placeholder="Mot de passe" class="w-full bg-slate-100 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-amber-400 transition" required>
                <button type="submit" class="w-full bg-slate-900 text-white py-5 rounded-2xl font-black uppercase text-xs tracking-widest hover:bg-slate-700 transition">Entrer dans l'Echoppe</button>
                <p class="text-center text-[10px] mt-4 font-bold uppercase text-slate-400">Nouveau ? <button type="button" onclick="switchAuthMode('register')" class="text-amber-600 underline">S'inscrire</button></p>
            </form>
        </div>
    </div>

    <div id="publish-modal" class="fixed inset-0 bg-slate-900/80 backdrop-blur-sm z-[200] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-2xl rounded-[3rem] p-10 relative">
            <button onclick="document.getElementById('publish-modal').classList.add('hidden')" class="absolute top-8 right-8 text-slate-300 text-2xl hover:text-slate-900">&times;</button>
            <h3 class="text-3xl font-black mb-8 uppercase tracking-tighter">Nouveau Produit Gabonais</h3>
            <form id="productForm" class="space-y-6">
                <input type="text" id="pName" placeholder="Nom du Produit (ex: Huile de Palme Moanda)" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-emerald-500" required>
                <div class="grid grid-cols-2 gap-6">
                    <input type="number" id="pPrice" placeholder="Prix (FCFA)" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-emerald-500" required>
                    <input type="number" id="pStock" placeholder="Stock disponible" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-emerald-500" required>
                </div>
                <select id="pProvince" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-emerald-500">
                    <option value="" disabled selected>Province d'origine</option>
                    <option>Estuaire</option><option>Haut-Ogooué</option><option>Moyen-Ogooué</option><option>Ngounié</option>
                    <option>Nyanga</option><option>Ogooué-Ivindo</option><option>Ogooué-Lolo</option><option>Ogooué-Maritime</option><option>Woleu-Ntem</option>
                </select>
                <textarea id="pDesc" placeholder="Description du produit et conseils d'utilisation..." class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border-2 border-transparent focus:border-emerald-500 h-32"></textarea>
                <button type="submit" class="w-full bg-emerald-600 text-white py-5 rounded-2xl font-black uppercase hover:bg-emerald-700 transition shadow-xl shadow-emerald-100">Lancer l'expédition</button>
            </form>
        </div>
    </div>

    <!-- Modal Messagerie -->
    <div id="chat-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[600] hidden flex items-center justify-end p-6">
        <div class="bg-white w-full max-w-lg h-[80vh] rounded-[3rem] shadow-2xl flex flex-col overflow-hidden animate-in slide-in-from-right duration-300">
            <div class="p-8 bg-slate-900 text-white flex justify-between items-center">
                <div>
                    <h4 id="chat-title" class="font-black text-xl uppercase tracking-tighter">Négociation</h4>
                    <p id="chat-subtitle" class="text-[10px] font-bold text-amber-400 uppercase tracking-widest">En direct avec le producteur</p>
                </div>
                <button onclick="closeChat()" class="text-white text-3xl">&times;</button>
            </div>
            <div id="chat-messages" class="flex-grow p-6 overflow-y-auto flex flex-col">
                <!-- Bulles de message -->
            </div>
            <div class="p-6 border-t bg-slate-50">
                <form id="chat-form" class="flex gap-3">
                    <input type="text" id="chat-input" placeholder="Écrivez votre message..." class="flex-grow bg-white p-4 rounded-2xl outline-none font-bold text-sm border shadow-sm">
                    <button type="submit" class="bg-blue-600 text-white w-14 h-14 rounded-2xl flex items-center justify-center text-xl shadow-lg shadow-blue-100"><i class="fa-solid fa-paper-plane"></i></button>
                </form>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, orderBy, serverTimestamp, updateDoc, where } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = 'echoppe241-prod-v2';

        let userData = null;
        let selectedRole = 'buyer';
        let currentChatId = null;
        let activeProducerId = null;

        // --- AUTH ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                const docRef = doc(db, 'artifacts', appId, 'users', user.uid);
                const docSnap = await getDoc(docRef);
                if (docSnap.exists()) {
                    userData = { uid: user.uid, ...docSnap.data() };
                    updateUI();
                    syncData();
                    if(userData.role === 'producer') syncProducerChats();
                }
            } else {
                userData = null;
                resetUI();
                syncData();
            }
        });

        document.getElementById('register-form').onsubmit = async (e) => {
            e.preventDefault();
            const btn = e.target.querySelector('button');
            btn.disabled = true; btn.innerText = "CRÉATION...";
            try {
                const cred = await createUserWithEmailAndPassword(auth, document.getElementById('reg-email').value, document.getElementById('reg-pass').value);
                await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), {
                    fullName: document.getElementById('reg-name').value,
                    email: document.getElementById('reg-email').value,
                    phone: document.getElementById('reg-phone').value,
                    role: selectedRole,
                    status: selectedRole === 'producer' ? 'pending' : 'active',
                    createdAt: serverTimestamp()
                });
                showToast("Bienvenue sur Echoppe241 !");
                toggleAuthModal();
            } catch (err) { alert(err.message); }
            btn.disabled = false; btn.innerText = "CRÉER MON COMPTE";
        };

        document.getElementById('login-form').onsubmit = async (e) => {
            e.preventDefault();
            try {
                await signInWithEmailAndPassword(auth, document.getElementById('login-email').value, document.getElementById('login-pass').value);
                showToast("Heureux de vous revoir !");
                toggleAuthModal();
            } catch (err) { alert("Identifiants incorrects."); }
        };

        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // --- UI CORE ---
        function updateUI() {
            document.getElementById('auth-btn').classList.add('hidden');
            document.getElementById('logout-btn').classList.remove('hidden');
            document.getElementById('user-profile-brief').classList.remove('hidden');
            document.getElementById('nav-user-name').innerText = userData.fullName;
            document.getElementById('nav-user-role').innerText = userData.role;

            ['view-buyer', 'view-admin', 'view-producer'].forEach(v => document.getElementById(v).classList.add('hidden'));

            if(userData.role === 'admin') document.getElementById('view-admin').classList.remove('hidden');
            else if(userData.role === 'producer') {
                document.getElementById('view-producer').classList.remove('hidden');
                document.getElementById('producer-pending-alert').classList.toggle('hidden', userData.status === 'active');
            } else {
                document.getElementById('view-buyer').classList.remove('hidden');
            }
        }

        function resetUI() {
            document.getElementById('auth-btn').classList.remove('hidden');
            document.getElementById('logout-btn').classList.add('hidden');
            document.getElementById('user-profile-brief').classList.add('hidden');
            document.getElementById('view-buyer').classList.remove('hidden');
        }

        // --- DATA SYNC ---
        function syncData() {
            const productsQuery = query(collection(db, 'artifacts', appId, 'public', 'data', 'products'), orderBy('timestamp', 'desc'));
            
            onSnapshot(productsQuery, (snap) => {
                const grid = document.getElementById('buyer-grid');
                const inv = document.getElementById('producer-inventory');
                const adminLog = document.getElementById('admin-logistics-list');
                
                grid.innerHTML = ''; 
                if(inv) inv.innerHTML = '';
                if(adminLog) adminLog.innerHTML = '';

                let transitCount = 0;
                let prodCount = 0;

                snap.forEach(d => {
                    const p = d.data();
                    const pid = d.id;

                    if(p.status === 'transit') transitCount++;

                    // Grille Acheteur
                    grid.innerHTML += `
                        <div class="bg-white rounded-[2.5rem] p-6 shadow-sm border border-slate-100 group hover:shadow-2xl transition relative overflow-hidden flex flex-col">
                            <div class="flex justify-between items-start mb-4">
                                <span class="status-badge ${p.status === 'transit' ? 'status-transit' : (p.status === 'delivered' ? 'status-delivered' : 'status-pending')}">
                                    ${p.status || 'collecte'}
                                </span>
                                <span class="text-[10px] font-black text-slate-300">#${pid.substring(0,5)}</span>
                            </div>
                            <h3 class="font-black text-xl mb-1 group-hover:text-amber-600 transition">${p.name}</h3>
                            <p class="text-[11px] text-slate-400 font-bold uppercase tracking-widest mb-4"><i class="fa-solid fa-map-location-dot mr-1"></i>${p.province}</p>
                            <p class="text-emerald-600 font-black text-2xl mb-6">${p.price.toLocaleString()} FCFA</p>
                            
                            <div class="mt-auto grid grid-cols-5 gap-2">
                                <button onclick="openChat('${p.ownerId}', '${p.name}')" class="col-span-1 bg-blue-50 text-blue-600 p-4 rounded-2xl hover:bg-blue-600 hover:text-white transition"><i class="fa-solid fa-comments"></i></button>
                                <button class="col-span-4 bg-slate-900 text-white p-4 rounded-2xl font-black text-[10px] uppercase tracking-widest hover:bg-amber-500 transition">Commander</button>
                            </div>
                        </div>
                    `;

                    // Inventaire Producteur
                    if(userData && p.ownerId === userData.uid && inv) {
                        prodCount++;
                        inv.innerHTML += `
                            <div class="p-6 bg-slate-50 rounded-3xl border border-slate-200">
                                <div class="flex justify-between mb-4">
                                    <span class="status-badge ${p.status === 'transit' ? 'status-transit' : (p.status === 'delivered' ? 'status-delivered' : 'status-pending')}">${p.status}</span>
                                    <span class="text-[9px] font-bold text-slate-400">Stock: ${p.stock}</span>
                                </div>
                                <h4 class="font-black text-sm">${p.name}</h4>
                                <p class="text-emerald-600 font-black text-xs mt-1">${p.price} FCFA</p>
                            </div>
                        `;
                    }

                    // Admin Logistics
                    if(userData && userData.role === 'admin' && adminLog) {
                        adminLog.innerHTML += `
                            <div class="p-4 bg-slate-50 rounded-2xl flex justify-between items-center border hover:border-blue-400 transition">
                                <div>
                                    <p class="font-black text-xs uppercase">${p.name}</p>
                                    <p class="text-[9px] text-slate-400 font-bold tracking-widest">${p.province} • Origine ID: ${p.ownerId.substring(0,6)}</p>
                                </div>
                                <div class="flex gap-2">
                                    <button onclick="updateStatus('${pid}', 'transit')" class="bg-blue-600 text-white px-3 py-1.5 rounded-lg text-[9px] font-black uppercase">Transit</button>
                                    <button onclick="updateStatus('${pid}', 'delivered')" class="bg-emerald-600 text-white px-3 py-1.5 rounded-lg text-[9px] font-black uppercase">Livré</button>
                                </div>
                            </div>
                        `;
                    }
                });

                if(document.getElementById('stat-products')) document.getElementById('stat-products').innerText = snap.size;
                if(document.getElementById('stat-transit')) document.getElementById('stat-transit').innerText = transitCount;
                if(document.getElementById('prod-inv-count')) document.getElementById('prod-inv-count').innerText = `${prodCount} articles`;
            });

            // Admin: Validation utilisateurs
            if(userData && userData.role === 'admin') {
                onSnapshot(collection(db, 'artifacts', appId, 'users'), (snap) => {
                    const list = document.getElementById('admin-pending-producers');
                    list.innerHTML = ''; let pending = 0;
                    snap.forEach(d => {
                        const u = d.data();
                        if(u.role === 'producer' && u.status === 'pending') {
                            pending++;
                            list.innerHTML += `
                                <div class="p-5 border-2 border-amber-100 bg-amber-50 rounded-3xl flex justify-between items-center animate-in fade-in">
                                    <div>
                                        <p class="font-black text-sm uppercase">${u.fullName}</p>
                                        <p class="text-[10px] font-bold text-amber-700 tracking-widest">${u.phone}</p>
                                    </div>
                                    <button onclick="approvePro('${d.id}')" class="bg-amber-500 text-white px-6 py-3 rounded-2xl text-[10px] font-black uppercase shadow-lg">Valider</button>
                                </div>
                            `;
                        }
                    });
                    document.getElementById('stat-pending').innerText = pending;
                    document.getElementById('stat-users').innerText = snap.size;
                    if(pending === 0) list.innerHTML = '<p class="text-xs text-slate-400 italic text-center py-4">Aucune demande en attente.</p>';
                });
            }
        }

        // --- MESSAGERIE ---
        window.openChat = async (producerId, productName) => {
            if(!userData) { toggleAuthModal(); return; }
            if(userData.uid === producerId) { showToast("C'est votre produit !"); return; }
            
            activeProducerId = producerId;
            currentChatId = [userData.uid, producerId].sort().join('_');
            
            document.getElementById('chat-title').innerText = "Contact Producteur";
            document.getElementById('chat-subtitle').innerText = `À propos de : ${productName}`;
            document.getElementById('chat-modal').classList.remove('hidden');
            
            loadMessages();
        };

        function loadMessages() {
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'chats', currentChatId, 'messages'), orderBy('timestamp', 'asc'));
            onSnapshot(q, (snap) => {
                const box = document.getElementById('chat-messages');
                box.innerHTML = '';
                snap.forEach(d => {
                    const m = d.data();
                    const isMine = m.senderId === userData.uid;
                    box.innerHTML += `
                        <div class="chat-bubble ${isMine ? 'chat-mine' : 'chat-theirs'}">
                            ${m.text}
                            <div class="text-[8px] opacity-50 mt-1">${m.timestamp ? new Date(m.timestamp.seconds*1000).toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}) : '...'}</div>
                        </div>
                    `;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        document.getElementById('chat-form').onsubmit = async (e) => {
            e.preventDefault();
            const input = document.getElementById('chat-input');
            if(!input.value.trim()) return;

            const msg = input.value;
            input.value = '';

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', currentChatId, 'messages'), {
                text: msg,
                senderId: userData.uid,
                senderName: userData.fullName,
                timestamp: serverTimestamp()
            });

            // Enregistrer la conversation dans la liste du producteur
            await setDoc(doc(db, 'artifacts', appId, 'users', activeProducerId, 'active_chats', currentChatId), {
                lastMsg: msg,
                customerName: userData.fullName,
                customerId: userData.uid,
                timestamp: serverTimestamp()
            });
        };

        function syncProducerChats() {
            onSnapshot(collection(db, 'artifacts', appId, 'users', userData.uid, 'active_chats'), (snap) => {
                const list = document.getElementById('producer-chats');
                list.innerHTML = '';
                if(snap.empty) list.innerHTML = '<p class="text-[10px] text-slate-400 italic">Aucun message pour le moment.</p>';
                snap.forEach(d => {
                    const c = d.data();
                    list.innerHTML += `
                        <div onclick="openProducerChat('${c.customerId}', '${c.customerName}')" class="p-4 bg-slate-50 rounded-2xl border cursor-pointer hover:border-blue-400 transition">
                            <p class="font-black text-[11px] uppercase">${c.customerName}</p>
                            <p class="text-[10px] text-slate-500 truncate mt-1">${c.lastMsg}</p>
                        </div>
                    `;
                });
            });
        }

        window.openProducerChat = (custId, custName) => {
            currentChatId = [userData.uid, custId].sort().join('_');
            activeProducerId = userData.uid;
            document.getElementById('chat-title').innerText = custName;
            document.getElementById('chat-subtitle').innerText = "Demande client";
            document.getElementById('chat-modal').classList.remove('hidden');
            loadMessages();
        };

        window.closeChat = () => document.getElementById('chat-modal').classList.add('hidden');

        // --- ACTIONS ---
        window.approvePro = async (uid) => {
            await updateDoc(doc(db, 'artifacts', appId, 'users', uid), { status: 'active' });
            showToast("Partenaire validé avec succès !");
        };

        window.updateStatus = async (pid, status) => {
            await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', pid), { status: status });
            showToast(`Mise à jour : Colis en ${status}`);
        };

        document.getElementById('productForm').onsubmit = async (e) => {
            e.preventDefault();
            const btn = e.target.querySelector('button');
            btn.disabled = true; btn.innerText = "EXPÉDITION EN COURS...";
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name: document.getElementById('pName').value,
                    price: parseInt(document.getElementById('pPrice').value),
                    stock: parseInt(document.getElementById('pStock').value),
                    province: document.getElementById('pProvince').value,
                    desc: document.getElementById('pDesc').value,
                    ownerId: userData.uid,
                    status: 'collecte',
                    timestamp: serverTimestamp()
                });
                showToast("Produit publié dans toute la nation !");
                document.getElementById('publish-modal').classList.add('hidden');
                e.target.reset();
            } catch (err) { alert(err.message); }
            btn.disabled = false; btn.innerText = "LANCER L'EXPÉDITION";
        };

        function showToast(msg) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        window.toggleAuthModal = () => document.getElementById('auth-modal').classList.toggle('hidden');
        window.switchAuthMode = (m) => {
            document.getElementById('login-form').classList.toggle('hidden', m === 'register');
            document.getElementById('register-form').classList.toggle('hidden', m === 'login');
            document.getElementById('auth-title').innerText = m === 'register' ? "Inscription" : "Connexion";
        };
        window.selectRegRole = (r) => {
            selectedRole = r;
            document.getElementById('role-buyer').classList.toggle('active', r === 'buyer');
            document.getElementById('role-producer').classList.toggle('active', r === 'producer');
        };
        window.openPublishModal = () => document.getElementById('publish-modal').classList.remove('hidden');

    </script>
</body>
</html>
