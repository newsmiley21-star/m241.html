<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Premium</title> 
    
    <!-- PWA CONFIG -->
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#00853f">
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --emerald-primary: #00853f; 
            --emerald-dark: #006b32;
            --gold-accent: #fcd116; 
            --slate-bg: #fdfdfd; 
        }

        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background-color: var(--slate-bg); 
            -webkit-tap-highlight-color: transparent;
            color: #1a202c;
        }

        .glass-nav { 
            background: rgba(255, 255, 255, 0.85); 
            backdrop-filter: blur(20px); 
            -webkit-backdrop-filter: blur(20px); 
        }

        .view { display: none; opacity: 0; transform: translateY(12px); transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1); }
        .view.active { display: block; opacity: 1; transform: translateY(0); }

        .btn-primary {
            background: linear-gradient(135deg, var(--emerald-primary), var(--emerald-dark));
            box-shadow: 0 10px 20px -5px rgba(0, 133, 63, 0.3);
            transition: all 0.3s ease;
        }
        .btn-primary:active { transform: scale(0.95); opacity: 0.9; }

        .card-premium {
            background: white;
            border-radius: 32px;
            border: 1px solid rgba(226, 232, 240, 0.6);
            box-shadow: 0 4px 12px rgba(0,0,0,0.02);
            transition: all 0.3s ease;
        }
        .card-premium:hover { box-shadow: 0 20px 25px -5px rgba(0,0,0,0.05); transform: translateY(-4px); }

        .tab-icon { transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        .tab-btn.active .tab-icon { color: var(--emerald-primary); transform: translateY(-6px) scale(1.1); }
        .tab-btn.active .tab-label { color: var(--emerald-primary); font-weight: 700; opacity: 1; }

        #splash { 
            position: fixed; inset: 0; z-index: 9999; background: #fff; 
            display: flex; flex-direction: column; align-items: center; justify-content: center; 
        }

        .category-pill {
            white-space: nowrap;
            padding: 12px 24px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            transition: all 0.2s ease;
            border: 1.5px solid #f1f5f9;
        }
        .category-pill.active {
            background: var(--emerald-primary);
            color: white;
            border-color: var(--emerald-primary);
            box-shadow: 0 8px 16px -4px rgba(0, 133, 63, 0.3);
        }

        /* Custom inputs */
        .premium-input {
            background: #f8fafc;
            border: 2px solid transparent;
            border-radius: 20px;
            padding: 16px 20px;
            font-weight: 600;
            transition: all 0.3s ease;
        }
        .premium-input:focus {
            background: white;
            border-color: var(--emerald-primary);
            box-shadow: 0 0 0 4px rgba(0, 133, 63, 0.1);
            outline: none;
        }

        .no-scrollbar::-webkit-scrollbar { display: none; }
    </style>
</head>
<body>

    <!-- SPLASH SCREEN ANIMÉ -->
    <div id="splash">
        <div class="relative">
            <div class="absolute inset-0 bg-emerald-100 rounded-full blur-3xl opacity-30 animate-pulse"></div>
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 relative z-10">
        </div>
        <p class="mt-8 text-[10px] font-black uppercase tracking-[0.5em] text-slate-300">Echoppe241 Gabon</p>
    </div>

    <!-- NAVBAR PREMIUM -->
    <header class="sticky top-0 z-50 glass-nav border-b border-slate-100/50 px-6 h-20 flex items-center justify-between">
        <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
            <div class="w-10 h-10 bg-emerald-50 rounded-2xl flex items-center justify-center">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-7">
            </div>
            <div>
                <h1 class="text-lg font-black italic uppercase tracking-tighter leading-none">Echoppe<span class="text-emerald-700">241</span></h1>
                <p class="text-[7px] font-bold text-slate-400 uppercase tracking-widest mt-0.5">Marché National</p>
            </div>
        </div>
        
        <div id="user-header" class="hidden flex items-center gap-3">
            <div class="text-right">
                <p id="h-balance" class="text-xs font-black text-emerald-700 leading-none">0 F</p>
                <p id="h-name" class="text-[8px] font-bold text-slate-400 uppercase truncate max-w-[80px]">Utilisateur</p>
            </div>
            <img id="h-avatar" class="w-10 h-10 rounded-full border-2 border-white shadow-md object-cover bg-slate-100" onclick="navigateTo('profile')">
        </div>
        <button id="h-login" onclick="navigateTo('auth')" class="btn-primary text-white px-5 py-2.5 rounded-2xl text-[10px] font-black uppercase tracking-widest">Connexion</button>
    </header>

    <main class="max-w-4xl mx-auto px-4 pt-8 pb-32">
        
        <!-- VUE : MARCHÉ -->
        <div id="view-home" class="view active">
            <section class="mb-10">
                <h2 class="text-4xl font-black italic uppercase leading-[0.85] tracking-tighter mb-8">
                    Découvrez le meilleur<br>
                    <span class="text-emerald-700">Du terroir gabonais.</span>
                </h2>
                
                <div class="flex gap-3 overflow-x-auto no-scrollbar py-2" id="categories">
                    <button onclick="setCat('all')" class="category-pill active">Tout</button>
                    <button onclick="setCat('Agriculture')" class="category-pill">Agriculture</button>
                    <button onclick="setCat('Artisanat')" class="category-pill">Artisanat</button>
                    <button onclick="setCat('Pêche')" class="category-pill">Pêche</button>
                    <button onclick="setCat('Électronique')" class="category-pill">Électronique</button>
                </div>
            </section>

            <div id="grid-products" class="grid grid-cols-2 md:grid-cols-3 gap-6">
                <!-- Produits injectés ici -->
            </div>
        </div>

        <!-- VUE : AUTHENTIFICATION -->
        <div id="view-auth" class="view max-w-sm mx-auto mt-10">
            <div class="bg-white p-10 rounded-[40px] shadow-2xl border border-slate-50">
                <div class="text-center mb-10">
                    <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-16 mx-auto mb-4 grayscale opacity-20">
                    <h3 id="auth-title" class="text-2xl font-black italic uppercase tracking-tighter">Bienvenue</h3>
                    <p class="text-[10px] font-bold text-slate-400 uppercase tracking-widest">Accédez à votre espace</p>
                </div>
                
                <div class="space-y-4">
                    <div id="signup-fields" class="hidden">
                        <input type="text" id="auth-name" placeholder="Nom complet" class="premium-input w-full">
                    </div>
                    <input type="email" id="auth-email" placeholder="Email" class="premium-input w-full">
                    <input type="password" id="auth-pass" placeholder="Mot de passe" class="premium-input w-full">
                    
                    <button onclick="submitAuth()" class="btn-primary w-full text-white font-black py-5 rounded-[25px] uppercase tracking-widest mt-4">
                        Continuer
                    </button>
                    
                    <button onclick="toggleAuth()" id="auth-switch" class="w-full text-[9px] font-black uppercase text-slate-400 underline pt-4 tracking-widest">
                        Créer un compte
                    </button>
                </div>
            </div>
        </div>

        <!-- VUE : PUBLIER -->
        <div id="view-publish" class="view max-w-xl mx-auto">
            <div class="bg-white p-8 rounded-[40px] shadow-xl border border-slate-50">
                <h3 class="text-2xl font-black italic uppercase mb-8 tracking-tighter">Vendre un <span class="text-emerald-700">Produit</span></h3>
                
                <div class="space-y-6">
                    <div onclick="document.getElementById('file-p').click()" class="w-full h-64 bg-slate-50 rounded-[35px] border-2 border-dashed border-slate-200 flex flex-col items-center justify-center cursor-pointer overflow-hidden group">
                        <div id="p-preview" class="text-center group-hover:scale-110 transition-transform">
                            <i class="fa-solid fa-camera-retro text-4xl text-slate-200 mb-3"></i>
                            <p class="text-[10px] font-black text-slate-300 uppercase tracking-widest">Ajouter une photo</p>
                        </div>
                        <input type="file" id="file-p" class="hidden" accept="image/*">
                    </div>

                    <input type="text" id="p-title" placeholder="Que vendez-vous ?" class="premium-input w-full">
                    
                    <div class="grid grid-cols-2 gap-4">
                        <input type="number" id="p-price" placeholder="Prix (FCFA)" class="premium-input w-full">
                        <select id="p-cat" class="premium-input w-full">
                            <option value="Agriculture">Agriculture</option>
                            <option value="Artisanat">Artisanat</option>
                            <option value="Pêche">Pêche</option>
                            <option value="Électronique">Électronique</option>
                        </select>
                    </div>

                    <select id="p-prov" class="premium-input w-full">
                        <option value="Estuaire">Estuaire (Libreville)</option>
                        <option value="Ogooué-Maritime">Ogooué-Maritime (POG)</option>
                        <option value="Haut-Ogooué">Haut-Ogooué (Franceville)</option>
                        <option value="Woleu-Ntem">Woleu-Ntem (Oyem)</option>
                    </select>

                    <button onclick="handlePublish()" id="btn-pub" class="btn-primary w-full text-white font-black py-5 rounded-[25px] uppercase tracking-widest">
                        Mettre en vente
                    </button>
                </div>
            </div>
        </div>

        <!-- VUE : PANIER -->
        <div id="view-cart" class="view max-w-xl mx-auto">
            <div class="flex justify-between items-end mb-8">
                <h2 class="text-4xl font-black italic uppercase tracking-tighter">Votre <span class="text-emerald-700">Panier</span></h2>
                <button onclick="clearCart()" class="text-[9px] font-black text-red-500 uppercase tracking-widest mb-2">Vider</button>
            </div>

            <div id="cart-items" class="space-y-4 mb-8">
                <!-- Items -->
            </div>

            <div id="cart-empty" class="hidden text-center py-20 opacity-20">
                <i class="fa-solid fa-cart-flatbed text-6xl mb-4"></i>
                <p class="font-black uppercase tracking-widest">Le panier est vide</p>
            </div>

            <div id="cart-footer" class="bg-slate-900 rounded-[40px] p-8 text-white shadow-2xl">
                <div class="flex justify-between items-center mb-6">
                    <p class="text-[10px] font-black uppercase tracking-widest opacity-60">Total de la commande</p>
                    <p id="c-total" class="text-3xl font-black italic">0 F</p>
                </div>
                <button onclick="handlePayment()" class="w-full bg-white text-slate-900 font-black py-5 rounded-[25px] uppercase tracking-widest btn-press flex items-center justify-center gap-3">
                    <i class="fa-solid fa-lock text-xs"></i> Confirmer l'achat
                </button>
            </div>
        </div>

        <!-- VUE : PROFIL -->
        <div id="view-profile" class="view">
            <div class="card-premium p-10 mb-8 text-center relative overflow-hidden">
                <div class="absolute top-0 left-0 w-full h-24 bg-emerald-700/5"></div>
                
                <div class="relative z-10">
                    <div class="relative w-32 h-32 mx-auto mb-6">
                        <img id="u-avatar" class="w-full h-full rounded-full object-cover border-4 border-white shadow-2xl bg-slate-100">
                        <button onclick="document.getElementById('file-av').click()" class="absolute bottom-0 right-0 bg-emerald-600 text-white w-10 h-10 rounded-full border-4 border-white">
                            <i class="fa-solid fa-pen text-[10px]"></i>
                        </button>
                        <input type="file" id="file-av" class="hidden" accept="image/*">
                    </div>
                    
                    <h3 id="u-name" class="text-2xl font-black italic uppercase tracking-tighter">...</h3>
                    <p id="u-email" class="text-[9px] font-bold text-slate-400 uppercase tracking-[0.3em] mb-8">...</p>

                    <div class="bg-gradient-to-br from-slate-900 to-emerald-900 rounded-[35px] p-8 text-left text-white shadow-2xl relative overflow-hidden">
                        <div class="absolute -right-10 -top-10 w-40 h-40 bg-white/5 rounded-full blur-3xl"></div>
                        <p class="text-[9px] font-black uppercase tracking-[0.2em] opacity-50 mb-1">Portefeuille EchoppePay</p>
                        <h4 id="u-balance" class="text-4xl font-black italic mb-8">0 FCFA</h4>
                        <div class="grid grid-cols-2 gap-3">
                            <button onclick="openTopup()" class="bg-white text-slate-900 text-[9px] font-black uppercase py-4 rounded-2xl tracking-widest active:scale-95">Recharger</button>
                            <button class="bg-white/10 text-white text-[9px] font-black uppercase py-4 rounded-2xl tracking-widest opacity-50 cursor-not-allowed">Retrait</button>
                        </div>
                    </div>
                </div>
            </div>

            <h5 class="text-[10px] font-black text-slate-300 uppercase tracking-[0.4em] mb-4 ml-6">Activité récente</h5>
            <div id="order-history" class="space-y-3 mb-12">
                <!-- Historique -->
            </div>

            <button onclick="handleLogout()" class="w-full py-5 bg-red-50 text-red-500 font-black rounded-[25px] text-[10px] uppercase tracking-widest border border-red-100">
                Déconnexion
            </button>
        </div>

    </main>

    <!-- TAB BAR PREMIUM -->
    <nav class="fixed bottom-0 left-0 right-0 h-24 glass-nav border-t border-slate-100/50 flex items-center justify-around px-8 z-[60] pb-6">
        <button onclick="navigateTo('home')" class="tab-btn flex flex-col items-center gap-1.5 active">
            <i class="fa-solid fa-grid-2 text-xl text-slate-300 tab-icon"></i>
            <span class="text-[8px] font-black uppercase tracking-widest opacity-40 tab-label">Marché</span>
        </button>
        <button onclick="navigateTo('publish')" class="tab-btn flex flex-col items-center gap-1.5">
            <div class="w-14 h-14 bg-emerald-700 text-white rounded-[22px] flex items-center justify-center shadow-lg shadow-emerald-200 -mt-12 border-4 border-white active:scale-90 transition-transform">
                <i class="fa-solid fa-plus text-xl"></i>
            </div>
            <span class="text-[8px] font-black uppercase tracking-widest text-emerald-700 mt-1">Vendre</span>
        </button>
        <button onclick="navigateTo('cart')" class="tab-btn relative flex flex-col items-center gap-1.5">
            <i class="fa-solid fa-bag-shopping text-xl text-slate-300 tab-icon"></i>
            <span id="cart-count" class="hidden absolute -top-1.5 -right-1.5 bg-red-500 text-white text-[8px] w-5 h-5 rounded-full flex items-center justify-center border-2 border-white font-black">0</span>
            <span class="text-[8px] font-black uppercase tracking-widest opacity-40 tab-label">Panier</span>
        </button>
        <button onclick="navigateTo('profile')" class="tab-btn flex flex-col items-center gap-1.5">
            <i class="fa-solid fa-user text-xl text-slate-300 tab-icon"></i>
            <span class="text-[8px] font-black uppercase tracking-widest opacity-40 tab-label">Compte</span>
        </button>
    </nav>

    <!-- MODAL RECHARGEMENT -->
    <div id="modal-topup" class="hidden fixed inset-0 z-[100] bg-slate-900/90 backdrop-blur-md flex items-center justify-center p-6">
        <div class="bg-white rounded-[45px] p-10 w-full max-w-sm shadow-2xl text-center">
            <h4 class="text-xl font-black italic uppercase mb-6">Echoppe<span class="text-emerald-700">Pay</span></h4>
            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-8 leading-relaxed">
                Envoyez via AirtelMoney ou MoovMoney au : <br>
                <span class="text-emerald-700 text-2xl font-black">077 73 60 65</span>
            </p>
            <input type="number" id="topup-amount" placeholder="Montant à créditer" class="premium-input w-full text-center text-xl mb-6">
            <button onclick="processTopup()" class="btn-primary w-full text-white font-black py-5 rounded-[22px] uppercase tracking-widest mb-4">Confirmer le dépôt</button>
            <button onclick="closeTopup()" class="text-[9px] font-black uppercase text-slate-300 tracking-widest">Annuler</button>
        </div>
    </div>

    <!-- TOAST NOTIFICATION -->
    <div id="toast" class="hidden fixed bottom-28 left-1/2 -translate-x-1/2 bg-slate-900 text-white px-8 py-4 rounded-full text-[10px] font-black uppercase tracking-widest z-[1000] shadow-2xl"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, onSnapshot, addDoc, updateDoc, serverTimestamp, increment, runTransaction } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // FIREBASE SETUP
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
        const appId = "echoppe241-premium-v1";

        let userState = null;
        let cart = [];
        let curCat = 'all';
        let tempImg = null;
        let isAuthSignup = false;

        // NAVIGATION
        window.navigateTo = (v) => {
            const priv = ['publish', 'cart', 'profile'];
            if(priv.includes(v) && (!userState || auth.currentUser.isAnonymous)) return navigateTo('auth');
            
            document.querySelectorAll('.view').forEach(e => e.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            const map = {'home':0, 'publish':1, 'cart':2, 'profile':3}[v];
            if(map !== undefined) document.querySelectorAll('.tab-btn')[map].classList.add('active');

            if(v === 'home') syncProducts();
            if(v === 'cart') syncCartUI();
            if(v === 'profile') syncHistory();
            window.scrollTo({top:0, behavior:'smooth'});
        };

        // AUTH LOGIC
        window.toggleAuth = () => {
            isAuthSignup = !isAuthSignup;
            document.getElementById('auth-title').innerText = isAuthSignup ? 'Créer un compte' : 'Bienvenue';
            document.getElementById('signup-fields').classList.toggle('hidden', !isAuthSignup);
            document.getElementById('auth-switch').innerText = isAuthSignup ? 'Déjà inscrit ? Connexion' : 'Créer un compte';
        };

        window.submitAuth = async () => {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;
            if(!email || pass.length < 6) return msg("Identifiants invalides");

            try {
                if(isAuthSignup) {
                    const name = document.getElementById('auth-name').value;
                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid), {
                        uid: res.user.uid, fullName: name || "Citoyen", email, walletBalance: 0,
                        avatar: `https://ui-avatars.com/api/?name=${name}&background=00853f&color=fff`,
                        at: serverTimestamp()
                    });
                } else {
                    await signInWithEmailAndPassword(auth, email, pass);
                }
                navigateTo('home');
            } catch(e) { msg("Erreur d'authentification"); }
        };

        onAuthStateChanged(auth, (u) => {
            if(u && !u.isAnonymous) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if(snap.exists()) {
                        userState = snap.data();
                        refreshHeader();
                        refreshProfile();
                    }
                });
            } else {
                if(!u) signInAnonymously(auth);
                userState = null;
                refreshHeader();
            }
            setTimeout(() => document.getElementById('splash').style.display = 'none', 1000);
        });

        function refreshHeader() {
            const logged = userState && !auth.currentUser.isAnonymous;
            document.getElementById('user-header').classList.toggle('hidden', !logged);
            document.getElementById('h-login').classList.toggle('hidden', logged);
            if(logged) {
                document.getElementById('h-name').innerText = userState.fullName;
                document.getElementById('h-balance').innerText = `${userState.walletBalance.toLocaleString()} F`;
                document.getElementById('h-avatar').src = userState.avatar;
            }
        }

        // MARCHÉ & PRODUITS
        window.setCat = (c) => {
            curCat = c;
            document.querySelectorAll('.category-pill').forEach(p => {
                p.classList.toggle('active', p.innerText.toLowerCase() === c.toLowerCase() || (c==='all' && p.innerText==='Tout'));
            });
            syncProducts();
        };

        function syncProducts() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                let items = snap.docs.map(d => ({id:d.id, ...d.data()}));
                if(curCat !== 'all') items = items.filter(i => i.category === curCat);

                const grid = document.getElementById('grid-products');
                grid.innerHTML = items.map(p => `
                    <div class="card-premium p-3 flex flex-col">
                        <div class="h-44 rounded-[24px] overflow-hidden bg-slate-50 relative">
                            <img src="${p.image}" class="w-full h-full object-cover">
                            <span class="absolute top-3 left-3 bg-white/90 backdrop-blur-md text-[7px] font-black uppercase px-2 py-1 rounded-full text-emerald-700">${p.category}</span>
                        </div>
                        <div class="pt-4 px-1 pb-1 flex-1 flex flex-col justify-between">
                            <div>
                                <h4 class="font-bold text-[11px] uppercase truncate tracking-tight">${p.title}</h4>
                                <p class="text-[8px] font-medium text-slate-400 mt-1"><i class="fa-solid fa-location-dot mr-1"></i>${p.province}</p>
                            </div>
                            <div class="flex justify-between items-center mt-4">
                                <p class="text-[13px] font-black italic">${p.price.toLocaleString()} F</p>
                                <button onclick="addToCart(${JSON.stringify(p).replace(/"/g, '&quot;')})" class="w-9 h-9 btn-primary text-white rounded-xl flex items-center justify-center">
                                    <i class="fa-solid fa-plus text-[10px]"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `).join('');
            });
        }

        // PANIER
        window.addToCart = (p) => {
            if(!userState) return navigateTo('auth');
            if(p.sellerId === userState.uid) return msg("C'est votre produit !");
            cart.push(p);
            updateCartBadge();
            msg("Ajouté au panier");
        };

        function updateCartBadge() {
            const el = document.getElementById('cart-count');
            el.innerText = cart.length;
            el.classList.toggle('hidden', cart.length === 0);
        }

        function syncCartUI() {
            const list = document.getElementById('cart-items');
            const footer = document.getElementById('cart-footer');
            const empty = document.getElementById('cart-empty');

            if(cart.length === 0) {
                list.innerHTML = '';
                footer.classList.add('hidden');
                empty.classList.remove('hidden');
                return;
            }

            footer.classList.remove('hidden');
            empty.classList.add('hidden');

            let total = 0;
            list.innerHTML = cart.map((p, i) => {
                total += p.price;
                return `
                    <div class="card-premium p-4 flex items-center gap-4">
                        <img src="${p.image}" class="w-16 h-16 rounded-2xl object-cover">
                        <div class="flex-1">
                            <h5 class="font-bold text-[10px] uppercase truncate">${p.title}</h5>
                            <p class="text-emerald-700 font-black text-sm">${p.price.toLocaleString()} F</p>
                        </div>
                        <button onclick="popCart(${i})" class="w-10 h-10 text-slate-200 hover:text-red-500 transition-colors">
                            <i class="fa-solid fa-circle-xmark text-xl"></i>
                        </button>
                    </div>
                `;
            }).join('');
            document.getElementById('c-total').innerText = `${total.toLocaleString()} F`;
        }

        window.popCart = (i) => { cart.splice(i, 1); syncCartUI(); updateCartBadge(); };
        window.clearCart = () => { cart = []; syncCartUI(); updateCartBadge(); };

        window.handlePayment = async () => {
            const total = cart.reduce((s, p) => s + p.price, 0);
            if(userState.walletBalance < total) return msg("Solde insuffisant");

            try {
                await runTransaction(db, async (t) => {
                    const bRef = doc(db, 'artifacts', appId, 'users', userState.uid);
                    t.update(bRef, { walletBalance: increment(-total) });

                    for(const p of cart) {
                        const sRef = doc(db, 'artifacts', appId, 'users', p.sellerId);
                        t.update(sRef, { walletBalance: increment(p.price) });

                        const oRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'));
                        t.set(oRef, {
                            buyer: userState.uid, seller: p.sellerId,
                            title: p.title, price: p.price, image: p.image,
                            at: serverTimestamp()
                        });
                    }
                });
                cart = []; updateCartBadge(); msg("Achat validé !"); navigateTo('profile');
            } catch(e) { msg("Échec du paiement"); }
        };

        // PUBLICATION
        document.getElementById('file-p').addEventListener('change', (e) => {
            const reader = new FileReader();
            reader.onload = (re) => {
                tempImg = re.target.result;
                document.getElementById('p-preview').innerHTML = `<img src="${tempImg}" class="w-full h-full object-cover">`;
            };
            reader.readAsDataURL(e.target.files[0]);
        });

        window.handlePublish = async () => {
            const t = document.getElementById('p-title').value;
            const p = parseInt(document.getElementById('p-price').value);
            const c = document.getElementById('p-cat').value;
            const v = document.getElementById('p-prov').value;

            if(!t || !p || !tempImg) return msg("Formulaire incomplet");

            const b = document.getElementById('btn-pub');
            b.disabled = true; b.innerText = "Mise en ligne...";

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    title: t, price: p, category: c, province: v, image: tempImg,
                    sellerId: userState.uid, at: serverTimestamp()
                });
                msg("Annonce publiée"); navigateTo('home');
                // Reset
                document.getElementById('p-title').value = '';
                document.getElementById('p-price').value = '';
                tempImg = null;
                document.getElementById('p-preview').innerHTML = `<i class="fa-solid fa-camera-retro text-4xl text-slate-200 mb-3"></i><p class="text-[10px] font-black text-slate-300 uppercase tracking-widest">Ajouter une photo</p>`;
            } catch(e) { msg("Erreur serveur"); }
            finally { b.disabled = false; b.innerText = "Mettre en vente"; }
        };

        // PROFIL & PORTEFEUILLE
        function refreshProfile() {
            document.getElementById('u-avatar').src = userState.avatar;
            document.getElementById('u-name').innerText = userState.fullName;
            document.getElementById('u-email').innerText = userState.email;
            document.getElementById('u-balance').innerText = `${userState.walletBalance.toLocaleString()} FCFA`;
        }

        window.openTopup = () => document.getElementById('modal-topup').classList.remove('hidden');
        window.closeTopup = () => document.getElementById('modal-topup').classList.add('hidden');
        window.processTopup = async () => {
            const val = parseInt(document.getElementById('topup-amount').value);
            if(!val || val < 100) return msg("Minimum 100 F");
            await updateDoc(doc(db, 'artifacts', appId, 'users', userState.uid), { walletBalance: increment(val) });
            msg("Solde mis à jour"); closeTopup();
        };

        function syncHistory() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), (snap) => {
                const list = document.getElementById('order-history');
                const mine = snap.docs.map(d => d.data()).filter(o => o.buyer === userState.uid || o.seller === userState.uid);
                
                if(mine.length === 0) {
                    list.innerHTML = `<p class="text-[9px] text-center text-slate-300 uppercase py-10 font-bold">Aucune activité</p>`;
                    return;
                }

                list.innerHTML = mine.sort((a,b) => b.at - a.at).map(o => {
                    const isBuy = o.buyer === userState.uid;
                    return `
                        <div class="bg-white p-4 rounded-3xl flex items-center gap-4 border border-slate-50 shadow-sm">
                            <img src="${o.image}" class="w-12 h-12 rounded-2xl object-cover">
                            <div class="flex-1">
                                <h6 class="font-bold text-[10px] uppercase truncate">${o.title}</h6>
                                <p class="text-[8px] font-bold text-slate-400">${isBuy ? 'ACHAT' : 'VENTE'} • ${new Date(o.at?.seconds*1000).toLocaleDateString()}</p>
                            </div>
                            <span class="font-black text-sm ${isBuy ? 'text-red-500' : 'text-emerald-700'}">${isBuy?'-':'+'}${o.price.toLocaleString()} F</span>
                        </div>
                    `;
                }).join('');
            });
        }

        document.getElementById('file-av').addEventListener('change', (e) => {
            const reader = new FileReader();
            reader.onload = async (re) => {
                await updateDoc(doc(db, 'artifacts', appId, 'users', userState.uid), { avatar: re.target.result });
                msg("Avatar mis à jour");
            };
            reader.readAsDataURL(e.target.files[0]);
        });

        function msg(m) {
            const t = document.getElementById('toast'); t.innerText = m;
            t.classList.remove('hidden'); setTimeout(() => t.classList.add('hidden'), 3000);
        }

        window.handleLogout = () => signOut(auth).then(() => location.reload());

    </script>
</body>
</html>
