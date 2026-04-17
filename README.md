<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Plateforme Officielle</title> 
    
     <!-- Favicon et Icônes -->
    <link rel="icon" type="image/png" href="[url=https://imgbb.com/][img]https://i.ibb.co/RkhcRdCs/echoppe241-logo.png[/img][/url]">
    <link rel="apple-touch-icon" href="[url=https://imgbb.com/][img]https://i.ibb.co/RkhcRdCs/echoppe241-logo.png[/img][/url]">
 
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
        :root { --gabon-green: #00853f; --gabon-yellow: #fcd116; --gabon-blue: #3a75c4; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px); }
        .btn-primary { @apply bg-[#00853f] text-white font-black uppercase text-[10px] tracking-widest px-6 py-4 rounded-2xl shadow-lg active:scale-95 transition-all disabled:opacity-50 disabled:pointer-events-none; }
        .input-box { @apply w-full bg-white border border-slate-200 rounded-2xl p-4 outline-none focus:border-[#00853f] font-semibold text-slate-700 transition-all focus:ring-4 focus:ring-emerald-50; }
        .tab-btn { @apply px-6 py-3 font-bold text-[10px] uppercase tracking-wider text-slate-400 transition-all border-b-2 border-transparent; }
        .tab-btn.active { @apply text-[#00853f] border-[#00853f]; }
        .view { display: none; min-height: calc(100vh - 80px); }
        .view.active { display: block; animation: viewIn 0.4s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes viewIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        .hide-scroll::-webkit-scrollbar { display: none; }
        
        /* Spinner */
        .loader { width: 20px; height: 20px; border: 3px solid #FFF; border-bottom-color: transparent; border-radius: 50%; display: inline-block; animation: rotation 1s linear infinite; }
        @keyframes rotation { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body class="text-slate-900 overflow-x-hidden">

    <!-- Notifications Toast -->
    <div id="toast" class="fixed top-6 left-1/2 -translate-x-1/2 z-[9999] hidden pointer-events-none">
        <div class="bg-slate-900 text-white px-8 py-4 rounded-3xl shadow-2xl flex items-center gap-4 animate-bounce">
            <i class="fa-solid fa-bell text-emerald-400"></i>
            <span id="toast-msg" class="text-[11px] font-black uppercase tracking-wider">Message</span>
        </div>
    </div>

    <!-- Navigation -->
    <nav class="sticky top-0 z-[1000] glass border-b border-slate-100">
        <div class="max-w-7xl mx-auto px-4 h-20 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer group" onclick="showView('home')">
                <div class="w-12 h-12 bg-white rounded-xl flex items-center justify-center shadow-sm border border-slate-100 overflow-hidden group-active:scale-90 transition-transform">
                    <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Logo" class="w-10 h-10 object-contain">
                </div>
                <div class="flex flex-col leading-none">
                    <span class="font-black text-xl italic tracking-tighter">ECHOPPE<span class="text-emerald-600">241</span></span>
                    <span class="text-[8px] font-bold uppercase tracking-widest text-slate-400">Gabon Digital</span>
                </div>
            </div>

            <div class="flex items-center gap-2 md:gap-4">
                <button onclick="showView('cart')" class="relative p-3 text-slate-600 hover:bg-slate-50 rounded-2xl transition-colors">
                    <i class="fa-solid fa-bag-shopping text-xl"></i>
                    <span id="cart-badge" class="absolute top-1 right-1 bg-red-500 text-white text-[9px] font-bold w-5 h-5 rounded-full flex items-center justify-center border-2 border-white hidden">0</span>
                </button>
                <div id="auth-controls">
                    <button onclick="toggleModal('modal-auth')" class="btn-primary py-3 px-5">Connexion</button>
                </div>
                <div id="user-controls" class="hidden flex items-center">
                    <button onclick="showView('profile')" class="flex items-center gap-2 p-1 pl-3 bg-white border border-slate-100 rounded-2xl shadow-sm active:scale-95 transition-all">
                        <span id="nav-user-name" class="hidden md:block text-[10px] font-black uppercase tracking-tight mr-1">Profil</span>
                        <img id="nav-avatar" src="" class="w-10 h-10 rounded-xl object-cover border border-slate-50">
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <main class="max-w-7xl mx-auto p-4 md:p-8">

        <!-- VUE : ACCUEIL -->
        <section id="view-home" class="view active">
            <div class="mb-10 text-center md:text-left">
                <h1 class="text-4xl md:text-5xl font-black italic uppercase tracking-tighter mb-2">Le Marché <span class="text-emerald-600">Local</span></h1>
                <p class="text-slate-400 text-sm font-semibold">Soutenons nos producteurs et commerçants Gabonais.</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-4 gap-8">
                <aside class="space-y-6">
                    <div class="bg-white p-6 rounded-[2.5rem] shadow-sm border border-slate-100">
                        <h3 class="font-black uppercase text-[10px] tracking-widest text-slate-400 mb-6">Provinces du Gabon</h3>
                        <div class="flex flex-col gap-1" id="filter-provinces">
                            <button onclick="setFilter('all')" class="province-btn text-left text-[11px] font-bold p-3 rounded-2xl bg-slate-50 text-emerald-600 border border-emerald-100">Toutes les provinces</button>
                            <!-- Injected JS -->
                        </div>
                    </div>
                </aside>

                <div class="lg:col-span-3">
                    <div id="market-grid" class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6">
                        <!-- Skeletons loading... -->
                    </div>
                    <div id="market-empty" class="hidden text-center py-20">
                        <div class="w-20 h-20 bg-slate-100 rounded-full flex items-center justify-center mx-auto mb-4">
                            <i class="fa-solid fa-magnifying-glass text-slate-300 text-2xl"></i>
                        </div>
                        <p class="font-bold text-slate-400">Aucun produit dans cette province pour le moment.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- VUE : PROFIL -->
        <section id="view-profile" class="view">
            <div class="max-w-4xl mx-auto space-y-8">
                <div class="bg-white rounded-[3rem] p-8 shadow-sm border border-slate-100">
                    <div class="flex flex-col md:flex-row gap-10 items-center">
                        <div class="relative group">
                            <img id="p-avatar" class="w-44 h-44 rounded-[2.5rem] object-cover shadow-2xl border-4 border-white transition-transform group-hover:scale-105">
                            <label class="absolute -bottom-2 -right-2 w-12 h-12 bg-emerald-600 text-white rounded-2xl flex items-center justify-center cursor-pointer shadow-lg hover:bg-emerald-700 active:scale-90 transition-all">
                                <i class="fa-solid fa-camera"></i>
                                <input type="file" id="change-avatar" class="hidden" accept="image/*">
                            </label>
                        </div>
                        <div class="flex-1 text-center md:text-left space-y-6">
                            <div>
                                <h2 id="p-name" class="text-4xl font-black italic uppercase tracking-tighter">Utilisateur</h2>
                                <div class="flex flex-wrap justify-center md:justify-start gap-2 mt-2">
                                    <span id="p-role" class="bg-emerald-50 text-emerald-700 px-4 py-1.5 rounded-full text-[9px] font-black uppercase tracking-widest">Client</span>
                                    <span id="p-badge-admin" class="hidden bg-red-50 text-red-600 px-4 py-1.5 rounded-full text-[9px] font-black uppercase tracking-widest">Administrateur</span>
                                </div>
                            </div>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <input type="tel" id="p-phone-input" class="input-box" placeholder="Numéro WhatsApp (ex: 077...)">
                                <input type="text" id="p-city-input" class="input-box" placeholder="Ville actuelle">
                            </div>
                            <div class="flex flex-col md:flex-row gap-3">
                                <button onclick="saveProfile()" id="btn-save-profile" class="btn-primary flex-1">Enregistrer les changements</button>
                                <button onclick="handleLogout()" class="px-6 py-4 text-red-500 font-bold text-xs uppercase tracking-widest hover:bg-red-50 rounded-2xl transition-all">Se déconnecter</button>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="bg-white rounded-[3rem] p-2 shadow-sm border border-slate-100 overflow-hidden">
                    <div class="flex justify-center border-b border-slate-50 p-2 gap-2">
                        <button onclick="switchTab('orders')" id="tab-orders" class="tab-btn active flex-1">Mes Commandes</button>
                        <button onclick="switchTab('messages')" id="tab-messages" class="tab-btn flex-1">Messagerie Support</button>
                        <button onclick="switchTab('inventory')" id="tab-inventory" class="tab-btn hidden flex-1">Ma Boutique</button>
                        <button onclick="switchTab('admin')" id="tab-admin" class="tab-btn hidden flex-1 text-red-600">Admin</button>
                    </div>

                    <div class="p-6 min-h-[400px]">
                        <div id="tab-content-orders" class="tab-pane active space-y-4"></div>
                        
                        <div id="tab-content-messages" class="tab-pane hidden flex flex-col h-[500px]">
                            <div id="chat-box" class="flex-1 overflow-y-auto p-4 space-y-4 bg-slate-50 rounded-[2rem] mb-4 hide-scroll"></div>
                            <div class="flex gap-3 items-center">
                                <input type="text" id="chat-input" class="input-box" placeholder="Votre message...">
                                <button onclick="sendChatMessage()" class="btn-primary !p-4"><i class="fa-solid fa-paper-plane text-lg"></i></button>
                            </div>
                        </div>

                        <div id="tab-content-inventory" class="tab-pane hidden space-y-8">
                            <div class="flex justify-between items-center">
                                <h3 class="text-xl font-black italic uppercase">Catalogue Vendeur</h3>
                                <button onclick="toggleModal('modal-publish')" class="bg-slate-900 text-white px-6 py-3 rounded-2xl text-[10px] font-black uppercase tracking-widest active:scale-95 transition-all">Ajouter un produit</button>
                            </div>
                            <div id="my-products-list" class="grid grid-cols-1 sm:grid-cols-2 gap-4"></div>
                        </div>

                        <div id="tab-content-admin" class="tab-pane hidden space-y-6">
                            <h3 class="text-xl font-black text-red-600 uppercase italic">Commandes Globales</h3>
                            <div id="admin-orders-list" class="space-y-4"></div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- VUE : PANIER -->
        <section id="view-cart" class="view">
            <div class="max-w-3xl mx-auto space-y-8">
                <h2 class="text-4xl font-black italic uppercase tracking-tighter text-center">Votre <span class="text-emerald-600">Panier</span></h2>
                <div id="cart-container" class="space-y-4"></div>
                
                <div id="cart-footer" class="hidden bg-white p-10 rounded-[3rem] shadow-xl border border-slate-100">
                    <div class="space-y-3 mb-8">
                        <div class="flex justify-between text-slate-400 font-bold text-xs uppercase">
                            <span>Sous-total</span>
                            <span id="cart-subtotal">0 FCFA</span>
                        </div>
                        <div class="flex justify-between items-center pt-3 border-t border-slate-50">
                            <span class="font-black text-lg uppercase tracking-tight">Total Final</span>
                            <span id="cart-total" class="text-3xl font-black text-emerald-600 tracking-tighter">0 FCFA</span>
                        </div>
                    </div>
                    <button onclick="processCheckout()" id="btn-checkout" class="btn-primary w-full py-6 text-sm flex items-center justify-center gap-3">
                        <span>Confirmer ma commande</span>
                        <i class="fa-solid fa-arrow-right"></i>
                    </button>
                    <p class="text-center mt-4 text-[9px] font-bold text-slate-400 uppercase tracking-widest">Paiement à la livraison ou via Airtel Money après confirmation.</p>
                </div>

                <div id="cart-empty" class="text-center py-20 bg-white rounded-[3rem] border border-dashed border-slate-200">
                    <div class="w-24 h-24 bg-slate-50 rounded-full flex items-center justify-center mx-auto mb-6">
                        <i class="fa-solid fa-bag-shopping text-slate-200 text-4xl"></i>
                    </div>
                    <p class="font-bold text-slate-500 mb-6 uppercase text-sm">Votre panier est vide.</p>
                    <button onclick="showView('home')" class="btn-primary">Commencer mes achats</button>
                </div>
            </div>
        </section>

    </main>

    <!-- MODALES -->
    <div id="modal-auth" class="fixed inset-0 bg-slate-900/80 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 animate-viewIn shadow-2xl">
            <div class="flex justify-between items-center mb-10">
                <h3 id="auth-title" class="text-3xl font-black italic uppercase tracking-tighter">Connexion</h3>
                <button onclick="toggleModal('modal-auth')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center text-slate-300 hover:text-red-500"><i class="fa-solid fa-xmark"></i></button>
            </div>
            
            <div class="space-y-4">
                <div class="flex p-1.5 bg-slate-100 rounded-2xl mb-6">
                    <button onclick="setAuthMode('login')" id="btn-mode-login" class="flex-1 py-3 rounded-xl font-black text-[10px] uppercase tracking-wider bg-white shadow-sm transition-all">Login</button>
                    <button onclick="setAuthMode('register')" id="btn-mode-reg" class="flex-1 py-3 rounded-xl font-black text-[10px] uppercase tracking-wider text-slate-400 transition-all">S'inscrire</button>
                </div>

                <div id="reg-fields" class="hidden space-y-4">
                    <input type="text" id="auth-name" class="input-box" placeholder="Nom Complet / Nom Boutique">
                    <div class="flex flex-col gap-2">
                        <label class="text-[9px] font-black text-slate-400 uppercase ml-2 tracking-widest">Type de compte</label>
                        <select id="auth-role" class="input-box">
                            <option value="buyer">Acheteur (Particulier)</option>
                            <option value="producer">Vendeur (Producteur / Boutique)</option>
                        </select>
                    </div>
                </div>

                <input type="email" id="auth-email" class="input-box" placeholder="Adresse Email">
                <input type="password" id="auth-pass" class="input-box" placeholder="Mot de passe">
                
                <button onclick="handleAuth()" id="btn-auth-submit" class="btn-primary w-full mt-6 py-5">
                    Se connecter
                </button>
                <p id="auth-msg" class="text-center text-red-500 text-[10px] font-bold mt-4 h-4"></p>
            </div>
        </div>
    </div>

    <div id="modal-publish" class="fixed inset-0 bg-slate-900/80 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-xl rounded-[3rem] p-10 animate-viewIn max-h-[90vh] overflow-y-auto shadow-2xl">
            <div class="flex justify-between items-center mb-8">
                <h3 class="text-3xl font-black italic uppercase tracking-tighter">Vendre un article</h3>
                <button onclick="toggleModal('modal-publish')"><i class="fa-solid fa-xmark text-slate-300"></i></button>
            </div>
            
            <div class="space-y-6">
                <div class="relative group">
                    <div id="upload-preview" class="w-full h-64 bg-slate-50 border-2 border-dashed border-slate-200 rounded-[2.5rem] flex flex-col items-center justify-center text-slate-300 overflow-hidden transition-all group-hover:border-emerald-300">
                        <i class="fa-solid fa-camera text-5xl mb-3"></i>
                        <span class="text-[10px] font-black uppercase tracking-widest">Ajouter une belle photo</span>
                    </div>
                    <input type="file" id="prod-img-input" class="absolute inset-0 opacity-0 cursor-pointer" accept="image/*">
                </div>

                <input type="text" id="prod-name" class="input-box" placeholder="Nom du produit">
                <div class="grid grid-cols-2 gap-4">
                    <input type="number" id="prod-price" class="input-box" placeholder="Prix (FCFA)">
                    <select id="prod-province" class="input-box"></select>
                </div>
                <textarea id="prod-desc" class="input-box h-32 resize-none" placeholder="Description courte (Origine, Poids, etc.)"></textarea>
                
                <button onclick="publishProduct()" id="btn-publish" class="btn-primary w-full py-6 flex items-center justify-center gap-3">
                    <span>Publier maintenant</span>
                    <i class="fa-solid fa-check"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- Scripts Firebase -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, where, deleteDoc, updateDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = JSON.parse('{"apiKey": "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg", "authDomain": "communautedugabon.firebaseapp.com", "projectId": "communautedugabon", "storageBucket": "communautedugabon.firebasestorage.app", "messagingSenderId": "647862371022", "appId": "1:647862371022:web:b209bfc8eb81accb1fc69f"}');
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe241-prod-v1';

        let userProfile = null;
        let cart = [];
        let authMode = 'login';
        let currentProducts = [];
        let activeFilter = 'all';
        let tempProdImg = null;
        let tempAvatar = null;

        const provinces = ["Estuaire", "Woleu-Ntem", "Ogooué-Maritime", "Haut-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Moyen-Ogooué"];

        // UTILS
        const showToast = (m) => {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = m;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        };

        const compressImage = async (base64Str, maxWidth = 800) => {
            return new Promise((resolve) => {
                const img = new Image();
                img.src = base64Str;
                img.onload = () => {
                    const canvas = document.createElement('canvas');
                    const scale = maxWidth / img.width;
                    if (scale >= 1) return resolve(base64Str);
                    canvas.width = maxWidth;
                    canvas.height = img.height * scale;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    resolve(canvas.toDataURL('image/jpeg', 0.7));
                };
            });
        };

        // NAVIGATION
        window.showView = (v) => {
            document.querySelectorAll('.view').forEach(view => view.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };

        window.toggleModal = (id) => {
            document.getElementById(id).classList.toggle('hidden');
        };

        window.switchTab = (tab) => {
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.querySelectorAll('.tab-pane').forEach(p => p.classList.add('hidden'));
            document.getElementById(`tab-${tab}`).classList.add('active');
            document.getElementById(`tab-content-${tab}`).classList.remove('hidden');
        };

        // AUTH
        window.setAuthMode = (mode) => {
            authMode = mode;
            document.getElementById('auth-title').innerText = mode === 'login' ? 'Connexion' : 'Inscription';
            document.getElementById('reg-fields').classList.toggle('hidden', mode === 'login');
            document.getElementById('btn-auth-submit').innerText = mode === 'login' ? 'Se connecter' : "Créer mon compte";
            document.getElementById('btn-mode-login').className = mode === 'login' ? 'flex-1 py-3 rounded-xl font-black text-[10px] uppercase tracking-wider bg-white shadow-sm transition-all' : 'flex-1 py-3 rounded-xl font-black text-[10px] uppercase tracking-wider text-slate-400 transition-all';
            document.getElementById('btn-mode-reg').className = mode === 'register' ? 'flex-1 py-3 rounded-xl font-black text-[10px] uppercase tracking-wider bg-white shadow-sm transition-all' : 'flex-1 py-3 rounded-xl font-black text-[10px] uppercase tracking-wider text-slate-400 transition-all';
        };

        window.handleAuth = async () => {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;
            const btn = document.getElementById('btn-auth-submit');
            if(!email || !pass) return;

            btn.disabled = true;
            btn.innerHTML = '<span class="loader"></span>';

            try {
                if(authMode === 'register') {
                    const name = document.getElementById('auth-name').value;
                    const role = document.getElementById('auth-role').value;
                    if(!name) throw new Error("Nom requis");

                    const cred = await createUserWithEmailAndPassword(auth, email, pass);
                    const profile = {
                        fullName: name,
                        email: email,
                        role: role,
                        avatar: `https://ui-avatars.com/api/?name=${encodeURIComponent(name)}&background=00853f&color=fff&size=200`,
                        phone: "",
                        city: "",
                        isAdmin: email.toLowerCase().includes('admin.echoppe'),
                        createdAt: serverTimestamp()
                    };
                    await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), profile);
                } else {
                    await signInWithEmailAndPassword(auth, email, pass);
                }
                toggleModal('modal-auth');
            } catch (e) {
                document.getElementById('auth-msg').innerText = e.message;
            } finally {
                btn.disabled = false;
                btn.innerText = authMode === 'login' ? 'Se connecter' : "Créer mon compte";
            }
        };

        onAuthStateChanged(auth, async (user) => {
            if(user) {
                const snap = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid));
                if(snap.exists()) {
                    userProfile = { uid: user.uid, ...snap.data() };
                    updateAuthUI();
                    loadProfileData();
                    syncData();
                }
            } else {
                userProfile = null;
                updateAuthUI();
                renderMarket(); // Guest view
            }
        });

        function updateAuthUI() {
            const logged = !!userProfile;
            document.getElementById('auth-controls').classList.toggle('hidden', logged);
            document.getElementById('user-controls').classList.toggle('hidden', !logged);
            if(logged) {
                document.getElementById('nav-avatar').src = userProfile.avatar;
                document.getElementById('nav-user-name').innerText = userProfile.fullName.split(' ')[0];
                if(userProfile.role === 'producer') document.getElementById('tab-inventory').classList.remove('hidden');
                if(userProfile.isAdmin) {
                    document.getElementById('tab-admin').classList.remove('hidden');
                    document.getElementById('p-badge-admin').classList.remove('hidden');
                }
            }
        }

        // SYNC DATA
        function syncData() {
            if(!userProfile) return;

            // Global Products
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                currentProducts = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderMarket();
                renderMyInventory();
            });

            // My Orders
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), where('buyerId', '==', userProfile.uid)), (snap) => {
                renderOrders(snap.docs.map(d => ({id: d.id, ...d.data()})), 'tab-content-orders');
            });

            // Admin Global Orders
            if(userProfile.isAdmin) {
                onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), (snap) => {
                    renderOrders(snap.docs.map(d => ({id: d.id, ...d.data()})), 'admin-orders-list', true);
                });
            }

            // Messages
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'messages'), (snap) => {
                const msgs = snap.docs.map(d => d.data()).sort((a,b) => a.at - b.at);
                renderMessages(msgs);
            });
        }

        // MARKET RENDERING
        window.setFilter = (p) => {
            activeFilter = p;
            document.querySelectorAll('.province-btn').forEach(b => {
                b.classList.remove('bg-slate-50', 'text-emerald-600', 'border-emerald-100');
                if(b.innerText === p || (p === 'all' && b.innerText.includes('Toutes'))) {
                    b.classList.add('bg-slate-50', 'text-emerald-600', 'border-emerald-100');
                }
            });
            renderMarket();
        };

        function renderMarket() {
            const grid = document.getElementById('market-grid');
            const empty = document.getElementById('market-empty');
            grid.innerHTML = '';
            
            // In guest mode, currentProducts might be empty until we fetch them once
            if (currentProducts.length === 0 && !userProfile) {
                // Fetch manually if guest
                onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                    currentProducts = snap.docs.map(d => ({id: d.id, ...d.data()}));
                    renderMarket();
                });
                return;
            }

            const list = activeFilter === 'all' ? currentProducts : currentProducts.filter(p => p.province === activeFilter);
            
            empty.classList.toggle('hidden', list.length > 0);

            list.forEach(p => {
                const div = document.createElement('div');
                div.className = "bg-white rounded-[2.5rem] p-4 shadow-sm hover:shadow-2xl transition-all duration-500 border border-slate-50 group cursor-pointer";
                div.innerHTML = `
                    <div class="relative aspect-square rounded-[2rem] overflow-hidden mb-5">
                        <img src="${p.image}" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-1000">
                        <div class="absolute top-4 left-4 glass px-4 py-1.5 rounded-full text-[9px] font-black uppercase text-emerald-800 border border-white/50">${p.province}</div>
                    </div>
                    <div class="px-2">
                        <p class="text-[9px] font-black text-slate-400 uppercase tracking-widest mb-2">${p.ownerName}</p>
                        <h4 class="font-black text-sm uppercase tracking-tight truncate mb-3">${p.name}</h4>
                        <div class="flex justify-between items-center">
                            <span class="text-2xl font-black text-emerald-700 tracking-tighter">${p.price.toLocaleString()}<small class="text-xs ml-0.5">F</small></span>
                            <button onclick="addToCart('${p.id}')" class="bg-slate-900 text-white w-12 h-12 rounded-2xl flex items-center justify-center active:scale-90 transition-transform shadow-xl">
                                <i class="fa-solid fa-cart-plus text-sm"></i>
                            </button>
                        </div>
                    </div>
                `;
                grid.appendChild(div);
            });
        }

        // CART
        window.addToCart = (id) => {
            const prod = currentProducts.find(p => p.id === id);
            cart.push(prod);
            updateCartUI();
            showToast(`${prod.name} ajouté !`);
        };

        window.removeFromCart = (idx) => {
            cart.splice(idx, 1);
            updateCartUI();
        };

        function updateCartUI() {
            const badge = document.getElementById('cart-badge');
            const container = document.getElementById('cart-container');
            const totalEl = document.getElementById('cart-total');
            const subtotalEl = document.getElementById('cart-subtotal');
            const empty = document.getElementById('cart-empty');
            const footer = document.getElementById('cart-footer');

            badge.innerText = cart.length;
            badge.classList.toggle('hidden', cart.length === 0);
            
            container.innerHTML = '';
            if(cart.length === 0) {
                empty.classList.remove('hidden');
                footer.classList.add('hidden');
                return;
            }

            empty.classList.add('hidden');
            footer.classList.remove('hidden');

            let total = 0;
            cart.forEach((item, idx) => {
                total += item.price;
                const div = document.createElement('div');
                div.className = "bg-white p-5 rounded-[2rem] flex items-center justify-between border border-slate-50 shadow-sm";
                div.innerHTML = `
                    <div class="flex items-center gap-5">
                        <img src="${item.image}" class="w-20 h-20 rounded-3xl object-cover shadow-sm">
                        <div>
                            <h5 class="font-black text-[11px] uppercase tracking-tight">${item.name}</h5>
                            <p class="text-emerald-600 font-black text-lg tracking-tighter">${item.price.toLocaleString()} F</p>
                        </div>
                    </div>
                    <button onclick="removeFromCart(${idx})" class="w-10 h-10 bg-red-50 text-red-400 rounded-xl flex items-center justify-center"><i class="fa-solid fa-trash-can"></i></button>
                `;
                container.appendChild(div);
            });
            subtotalEl.innerText = `${total.toLocaleString()} FCFA`;
            totalEl.innerText = `${total.toLocaleString()} FCFA`;
        }

        window.processCheckout = async () => {
            if(!userProfile) return toggleModal('modal-auth');
            const btn = document.getElementById('btn-checkout');
            btn.disabled = true;
            btn.innerHTML = '<span class="loader"></span>';
            
            try {
                const orderData = {
                    buyerId: userProfile.uid,
                    buyerName: userProfile.fullName,
                    items: cart.map(i => ({name: i.name, price: i.price, sellerId: i.ownerId})),
                    total: cart.reduce((s,i) => s + i.price, 0),
                    status: 'En attente',
                    at: Date.now()
                };

                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), orderData);
                cart = [];
                updateCartUI();
                showToast("Commande envoyée !");
                showView('profile');
                switchTab('orders');
            } catch (e) {
                showToast("Erreur : " + e.message);
            } finally {
                btn.disabled = false;
                btn.innerHTML = '<span>Confirmer ma commande</span><i class="fa-solid fa-arrow-right"></i>';
            }
        };

        // PROFILE
        function loadProfileData() {
            document.getElementById('p-avatar').src = userProfile.avatar;
            document.getElementById('p-name').innerText = userProfile.fullName;
            document.getElementById('p-role').innerText = userProfile.role === 'producer' ? 'Producteur Gabonais' : 'Acheteur';
            document.getElementById('p-phone-input').value = userProfile.phone || '';
            document.getElementById('p-city-input').value = userProfile.city || '';
        }

        window.saveProfile = async () => {
            const btn = document.getElementById('btn-save-profile');
            btn.disabled = true;
            btn.innerHTML = '<span class="loader"></span>';

            const updates = {
                phone: document.getElementById('p-phone-input').value,
                city: document.getElementById('p-city-input').value
            };
            if(tempAvatar) updates.avatar = await compressImage(tempAvatar, 400);

            try {
                await updateDoc(doc(db, 'artifacts', appId, 'users', userProfile.uid), updates);
                showToast("Profil enregistré !");
                userProfile = {...userProfile, ...updates};
                updateAuthUI();
                tempAvatar = null;
            } catch (e) {
                showToast("Erreur de sauvegarde");
            } finally {
                btn.disabled = false;
                btn.innerText = "Enregistrer les changements";
            }
        };

        // INVENTORY
        function renderMyInventory() {
            if(!userProfile || userProfile.role !== 'producer') return;
            const container = document.getElementById('my-products-list');
            container.innerHTML = '';
            
            const mine = currentProducts.filter(p => p.ownerId === userProfile.uid);
            mine.forEach(p => {
                const div = document.createElement('div');
                div.className = "flex items-center gap-4 bg-white p-4 rounded-[2rem] border border-slate-100 shadow-sm";
                div.innerHTML = `
                    <img src="${p.image}" class="w-16 h-16 rounded-2xl object-cover shadow-sm">
                    <div class="flex-1 overflow-hidden">
                        <p class="font-black text-[10px] uppercase truncate">${p.name}</p>
                        <p class="text-sm font-black text-emerald-600">${p.price.toLocaleString()} F</p>
                    </div>
                    <button onclick="deleteProduct('${p.id}')" class="w-10 h-10 bg-red-50 text-red-500 rounded-xl flex items-center justify-center">
                        <i class="fa-solid fa-trash-can text-xs"></i>
                    </button>
                `;
                container.appendChild(div);
            });
        }

        window.publishProduct = async () => {
            const btn = document.getElementById('btn-publish');
            const name = document.getElementById('prod-name').value;
            const price = parseInt(document.getElementById('prod-price').value);
            const prov = document.getElementById('prod-province').value;
            const desc = document.getElementById('prod-desc').value;

            if(!name || !price || !prov || !tempProdImg) return showToast("Veuillez remplir tous les champs obligatoires");

            btn.disabled = true;
            btn.innerHTML = '<span class="loader"></span>';

            try {
                const finalImg = await compressImage(tempProdImg, 800);
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name, price, province: prov, description: desc,
                    image: finalImg,
                    ownerId: userProfile.uid,
                    ownerName: userProfile.fullName,
                    at: Date.now()
                });

                showToast("Publication réussie !");
                toggleModal('modal-publish');
                // Reset form
                document.getElementById('prod-name').value = '';
                document.getElementById('prod-price').value = '';
                document.getElementById('prod-desc').value = '';
                document.getElementById('upload-preview').innerHTML = '<i class="fa-solid fa-camera text-5xl mb-3"></i><span class="text-[10px] font-black uppercase tracking-widest">Ajouter une belle photo</span>';
                tempProdImg = null;
            } catch (e) {
                showToast("Erreur de publication");
            } finally {
                btn.disabled = false;
                btn.innerHTML = '<span>Publier maintenant</span><i class="fa-solid fa-check"></i>';
            }
        };

        window.deleteProduct = async (id) => {
            if(confirm("Supprimer cet article de la vente ?")) {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', id));
                showToast("Produit retiré");
            }
        };

        // CHAT
        window.sendChatMessage = async () => {
            const input = document.getElementById('chat-input');
            const txt = input.value.trim();
            if(!txt) return;

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'messages'), {
                uid: userProfile.uid,
                name: userProfile.fullName,
                text: txt,
                isAdmin: userProfile.isAdmin,
                at: Date.now()
            });
            input.value = '';
        };

        function renderMessages(msgs) {
            const box = document.getElementById('chat-box');
            box.innerHTML = '';
            msgs.forEach(m => {
                const isMe = m.uid === userProfile.uid;
                const div = document.createElement('div');
                div.className = `flex ${isMe ? 'justify-end' : 'justify-start'}`;
                div.innerHTML = `
                    <div class="max-w-[85%] ${isMe ? 'bg-emerald-600 text-white rounded-tr-none' : 'bg-white text-slate-800 rounded-tl-none border border-slate-100'} p-5 rounded-[2rem] shadow-sm">
                        <div class="flex items-center gap-2 mb-1 opacity-60">
                            <span class="text-[8px] font-black uppercase tracking-widest">${m.name}</span>
                            ${m.isAdmin ? '<span class="bg-red-500 text-white px-1.5 rounded-[4px] text-[6px] font-black">ADMIN</span>' : ''}
                        </div>
                        <p class="text-xs font-semibold leading-relaxed">${m.text}</p>
                    </div>
                `;
                box.appendChild(div);
            });
            box.scrollTop = box.scrollHeight;
        }

        // ORDERS UI
        function renderOrders(orders, containerId, isAdmin = false) {
            const container = document.getElementById(containerId);
            container.innerHTML = '';
            
            if(orders.length === 0) {
                container.innerHTML = `<div class="text-center py-20 opacity-20"><i class="fa-solid fa-receipt text-6xl mb-4"></i><p class="font-bold uppercase text-[10px]">Aucune commande</p></div>`;
                return;
            }

            orders.sort((a,b) => b.at - a.at).forEach(o => {
                const div = document.createElement('div');
                div.className = "bg-white p-6 rounded-[2.5rem] border border-slate-100 shadow-sm flex flex-col md:flex-row justify-between items-start md:items-center gap-4";
                div.innerHTML = `
                    <div>
                        <div class="flex items-center gap-3 mb-2">
                            <span class="text-[9px] font-black text-slate-300 uppercase">Ref: ${o.id.slice(-6).toUpperCase()}</span>
                            <span class="px-3 py-1 rounded-full text-[8px] font-black uppercase ${o.status === 'En attente' ? 'bg-amber-50 text-amber-600' : 'bg-emerald-50 text-emerald-600'}">${o.status}</span>
                        </div>
                        <h4 class="font-black text-xl tracking-tighter">${o.total.toLocaleString()} FCFA</h4>
                        <p class="text-[10px] text-slate-400 font-bold uppercase mt-1">${o.items.length} Article(s) • Client: ${o.buyerName}</p>
                    </div>
                    ${isAdmin ? `
                        <div class="flex gap-2 w-full md:w-auto">
                            <button onclick="updateStatus('${o.id}', 'Traitée')" class="flex-1 md:flex-none bg-slate-900 text-white px-5 py-3 rounded-2xl text-[9px] font-black uppercase tracking-widest">Confirmer</button>
                            <button onclick="updateStatus('${o.id}', 'Annulée')" class="flex-1 md:flex-none bg-red-50 text-red-500 px-5 py-3 rounded-2xl text-[9px] font-black uppercase tracking-widest">Annuler</button>
                        </div>
                    ` : ''}
                `;
                container.appendChild(div);
            });
        }

        window.updateStatus = async (id, status) => {
            await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'orders', id), { status });
            showToast("Statut mis à jour");
        };

        // IMAGE HANDLING
        const setupImageInput = (id, callback) => {
            document.getElementById(id).addEventListener('change', (e) => {
                const file = e.target.files[0];
                if(!file) return;
                const reader = new FileReader();
                reader.onload = (re) => callback(re.target.result);
                reader.readAsDataURL(file);
            });
        };

        setupImageInput('prod-img-input', (data) => {
            tempProdImg = data;
            document.getElementById('upload-preview').innerHTML = `<img src="${data}" class="w-full h-full object-cover">`;
        });

        setupImageInput('change-avatar', (data) => {
            tempAvatar = data;
            document.getElementById('p-avatar').src = data;
        });

        // INIT
        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // Fill Selects & Filters
        const pSelect = document.getElementById('prod-province');
        const pFilter = document.getElementById('filter-provinces');
        provinces.forEach(p => {
            const opt = document.createElement('option');
            opt.value = p; opt.innerText = p;
            pSelect.appendChild(opt);

            const btn = document.createElement('button');
            btn.className = "province-btn text-left text-[11px] font-bold p-3 rounded-2xl text-slate-500 hover:bg-slate-50 transition-all border border-transparent";
            btn.innerText = p;
            btn.onclick = () => setFilter(p);
            pFilter.appendChild(btn);
        });

    </script>
</body>
</html>
