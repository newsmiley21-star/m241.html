<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Production Ready</title>
    
    <!-- Meta PWA & Branding -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#00853f">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { --gabon-green: #00853f; --gabon-yellow: #fcd116; --gabon-blue: #3a75c4; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; color: #0f172a; -webkit-tap-highlight-color: transparent; }
        .brand-gradient { background: linear-gradient(135deg, var(--gabon-green), #10b981); }
        .glass { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-bottom: 1px solid rgba(0,0,0,0.05); }
        .product-card { transition: all 0.2s ease; border-radius: 1.5rem; background: white; overflow: hidden; border: 1px solid #f1f5f9; }
        .product-card:active { transform: scale(0.97); }
        .profile-img-large { width: 110px; height: 110px; border-radius: 35px; object-fit: cover; border: 4px solid white; box-shadow: 0 10px 20px rgba(0,0,0,0.1); }
        .hide-scroll::-webkit-scrollbar { display: none; }
        .safe-bottom { padding-bottom: env(safe-area-inset-bottom); }
        
        /* Shimmer effect for loading */
        .skeleton { background: linear-gradient(90deg, #f0f0f0 25%, #f8f8f8 50%, #f0f0f0 75%); background-size: 200% 100%; animation: skeleton-loading 1.5s infinite; }
        @keyframes skeleton-loading { 0% { background-position: -200% 0; } 100% { background-position: 200% 0; } }
    </style>
</head>
<body class="safe-bottom">

    <!-- Toast UI -->
    <div id="toast" class="fixed top-8 left-1/2 -translate-x-1/2 z-[3000] hidden">
        <div class="bg-slate-900 text-white px-6 py-3 rounded-2xl shadow-2xl flex items-center gap-3 border-b-2 border-amber-400">
            <span id="toast-msg" class="text-[10px] font-bold uppercase tracking-widest">Notification</span>
        </div>
    </div>

    <!-- Header -->
    <header class="sticky top-0 z-[1000] glass">
        <div class="h-[3px] flex">
            <div class="flex-1 bg-[var(--gabon-green)]"></div>
            <div class="flex-1 bg-[var(--gabon-yellow)]"></div>
            <div class="flex-1 bg-[var(--gabon-blue)]"></div>
        </div>
        <div class="max-w-6xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center gap-2 cursor-pointer" onclick="showView('market')">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Logo" class="h-8">
                <span class="font-extrabold text-xl tracking-tighter">ECHOPPE<span class="text-amber-500">241</span></span>
            </div>
            <div id="nav-actions">
                <button id="btn-login-open" onclick="toggleAuthModal()" class="bg-slate-900 text-white px-5 py-2.5 rounded-xl font-bold text-[10px] uppercase">Connexion</button>
                <div id="user-pill" class="hidden flex items-center gap-2">
                    <button onclick="showView('profile')" class="flex items-center gap-2 bg-slate-50 border p-1 pr-3 rounded-2xl">
                        <img id="head-avatar" src="" class="w-8 h-8 rounded-xl object-cover">
                        <span id="head-name" class="text-[10px] font-bold uppercase truncate max-w-[70px]">...</span>
                    </button>
                    <button onclick="handleLogout()" class="text-slate-300 hover:text-red-500 p-2"><i class="fa-solid fa-power-off"></i></button>
                </div>
            </div>
        </div>
    </header>

    <main class="max-w-6xl mx-auto p-4">
        
        <!-- VUE : MARCHÉ -->
        <section id="view-market" class="space-y-6">
            <div class="flex justify-between items-end">
                <div>
                    <h2 class="text-3xl font-black italic uppercase tracking-tighter">Le Marché</h2>
                    <p class="text-slate-400 text-[10px] font-bold uppercase tracking-widest">Produits frais du Gabon</p>
                </div>
                <button id="btn-sell-trigger" onclick="togglePublishModal()" class="hidden bg-emerald-600 text-white px-6 py-3 rounded-2xl font-black text-[10px] uppercase tracking-widest shadow-lg shadow-emerald-100">+ Vendre</button>
            </div>

            <!-- Filtres -->
            <div class="flex gap-2 overflow-x-auto pb-2 hide-scroll" id="filters-container">
                <button onclick="setProvinceFilter('Toutes')" class="bg-slate-900 text-white px-5 py-3 rounded-2xl text-[10px] font-bold uppercase whitespace-nowrap">Toutes</button>
            </div>

            <!-- Grille -->
            <div id="market-grid" class="grid grid-cols-2 md:grid-cols-4 gap-4">
                <div class="skeleton h-60 rounded-[1.5rem]"></div>
                <div class="skeleton h-60 rounded-[1.5rem]"></div>
            </div>
        </section>

        <!-- VUE : PROFIL -->
        <section id="view-profile" class="hidden max-w-2xl mx-auto">
            <div class="bg-white rounded-[2.5rem] p-8 shadow-sm border border-slate-100">
                <div class="flex flex-col items-center text-center mb-8">
                    <div class="relative mb-4">
                        <img id="prof-avatar-img" src="https://ui-avatars.com/api/?name=U&background=00853f&color=fff" class="profile-img-large">
                        <label id="avatar-label" for="avatar-input" class="absolute bottom-2 right-0 bg-white w-10 h-10 rounded-full shadow-lg flex items-center justify-center cursor-pointer border hover:scale-105 transition-transform">
                            <i class="fa-solid fa-camera text-emerald-600"></i>
                            <input type="file" id="avatar-input" accept="image/*" class="hidden" onchange="handleAvatarChange(event)">
                        </label>
                    </div>
                    <h2 id="prof-title-name" class="text-2xl font-black uppercase italic">Nom Utilisateur</h2>
                    <p id="prof-title-role" class="text-emerald-600 text-[10px] font-black uppercase mb-2">Chargement...</p>
                    <p id="avatar-lock-msg" class="hidden text-[9px] text-red-400 font-bold uppercase italic">Changement possible dans <span id="lock-days">0</span> j</p>
                </div>

                <form id="profile-form" class="space-y-4">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="space-y-1">
                            <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Nom / Coopérative</label>
                            <input type="text" id="form-name" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 focus:ring-2 focus:ring-emerald-500 font-bold" required>
                        </div>
                        <div class="space-y-1">
                            <label class="text-[9px] font-black uppercase text-slate-400 ml-1">WhatsApp (+241)</label>
                            <input type="tel" id="form-phone" placeholder="077..." class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 focus:ring-2 focus:ring-emerald-500 font-bold" required>
                        </div>
                        <div class="space-y-1">
                            <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Province</label>
                            <select id="form-province" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold">
                                <option>Estuaire</option><option>Haut-Ogooué</option><option>Moyen-Ogooué</option><option>Ngounié</option><option>Nyanga</option><option>Ogooué-Ivindo</option><option>Ogooué-Lolo</option><option>Ogooué-Maritime</option><option>Woleu-Ntem</option>
                            </select>
                        </div>
                        <div class="space-y-1">
                            <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Ville / Quartier</label>
                            <input type="text" id="form-city" placeholder="Ex: Akanda" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold">
                        </div>
                    </div>
                    <div class="space-y-1">
                        <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Ma Bio (Ma spécialité)</label>
                        <textarea id="form-bio" rows="3" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold"></textarea>
                    </div>
                    <div class="flex gap-4 pt-4">
                        <button type="submit" class="flex-grow bg-slate-900 text-white py-4 rounded-xl font-black uppercase text-[10px] tracking-widest shadow-xl">Enregistrer</button>
                        <button type="button" onclick="showView('market')" class="px-6 text-slate-400 font-bold uppercase text-[10px]">Retour</button>
                    </div>
                </form>
            </div>
        </section>
    </main>

    <!-- Modal : AUTH -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-sm rounded-[2.5rem] p-8 shadow-2xl relative">
            <button onclick="toggleAuthModal()" class="absolute top-6 right-6 text-slate-300 text-xl">&times;</button>
            <h2 id="auth-header" class="text-2xl font-black uppercase italic text-center mb-8">Bienvenue</h2>
            
            <div id="login-box" class="space-y-4">
                <input type="email" id="l-email" placeholder="Email" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold">
                <input type="password" id="l-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold">
                <button onclick="doLogin()" class="w-full brand-gradient text-white py-4 rounded-xl font-black uppercase text-[10px]">Connexion</button>
                <p class="text-center text-[10px] font-bold text-slate-400 uppercase tracking-widest pt-4">Nouveau ? <button onclick="switchAuth('reg')" class="text-emerald-600 underline">Créer un compte</button></p>
            </div>

            <div id="reg-box" class="space-y-4 hidden">
                <input type="text" id="r-name" placeholder="Nom Complet" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold">
                <input type="email" id="r-email" placeholder="Email" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold">
                <input type="password" id="r-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold">
                <div class="grid grid-cols-2 gap-2">
                    <button id="role-b" onclick="setRegRole('buyer')" class="p-3 rounded-xl border-2 border-emerald-500 bg-emerald-50 font-bold text-[9px] uppercase">Acheteur</button>
                    <button id="role-p" onclick="setRegRole('producer')" class="p-3 rounded-xl border-2 font-bold text-[9px] uppercase">Producteur</button>
                </div>
                <button onclick="doRegister()" class="w-full bg-slate-900 text-white py-4 rounded-xl font-black uppercase text-[10px]">S'inscrire</button>
                <p class="text-center text-[10px] font-bold text-slate-400 uppercase tracking-widest pt-4">Déjà inscrit ? <button onclick="switchAuth('login')" class="text-emerald-600 underline">Se connecter</button></p>
            </div>
        </div>
    </div>

    <!-- Modal : PUBLICATION PRODUIT -->
    <div id="pub-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-lg rounded-[2.5rem] p-8 shadow-2xl max-h-[90vh] overflow-y-auto hide-scroll">
            <h3 class="text-2xl font-black uppercase italic mb-6">Nouvelle Récolte</h3>
            <form id="pub-form" class="space-y-5">
                <!-- Zone Caméra -->
                <div class="space-y-2">
                    <label class="text-[9px] font-black uppercase text-slate-400">Photo du produit</label>
                    <input type="file" id="prod-file" accept="image/*" capture="environment" class="hidden" onchange="previewProdPhoto(event)">
                    <div onclick="document.getElementById('prod-file').click()" id="prod-photo-preview" class="w-full h-48 bg-slate-50 rounded-2xl border-2 border-dashed border-slate-200 flex flex-col items-center justify-center cursor-pointer overflow-hidden relative">
                        <i class="fa-solid fa-camera text-slate-300 text-3xl mb-2"></i>
                        <span class="text-[10px] font-bold text-slate-400 uppercase">Prendre la photo</span>
                    </div>
                    <button type="button" onclick="aiImageGen()" id="btn-ai" class="w-full text-[9px] font-black uppercase text-amber-600 bg-amber-50 py-2 rounded-lg border border-amber-100">
                        <i class="fa-solid fa-wand-magic-sparkles"></i> Pas de photo ? Utiliser l'IA
                    </button>
                </div>

                <div class="space-y-4">
                    <input type="text" id="p-name" placeholder="Nom du produit" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold" required>
                    <div class="grid grid-cols-2 gap-3">
                        <input type="number" id="p-price" placeholder="Prix (FCFA)" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold" required>
                        <input type="text" id="p-unit" placeholder="Unité (Tas, Sac, kg)" class="w-full bg-slate-50 p-4 rounded-xl border-none ring-1 ring-slate-100 font-bold" required>
                    </div>
                </div>

                <div class="flex gap-3 pt-4">
                    <button type="submit" class="flex-grow brand-gradient text-white py-4 rounded-xl font-black uppercase text-[10px] tracking-widest shadow-xl">Publier</button>
                    <button type="button" onclick="togglePublishModal()" class="px-6 text-slate-400 font-bold uppercase text-[10px]">Fermer</button>
                </div>
            </form>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, serverTimestamp, updateDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = 'echoppe241-v-production';
        const apiKey = ""; // La clé sera injectée par l'environnement

        let userData = null;
        let regRole = 'buyer';
        let currentProdImg = "";
        let currentFilter = 'Toutes';

        // --- AUTH & INITIALISATION ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                const d = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid));
                if (d.exists()) {
                    userData = { uid: user.uid, ...d.data() };
                    updateAppUI();
                } else {
                    userData = { uid: user.uid, role: 'guest' };
                }
                syncMarket();
            } else {
                signInAnonymously(auth);
            }
        });

        function updateAppUI() {
            const isLogged = userData && userData.role !== 'guest';
            document.getElementById('btn-login-open').classList.toggle('hidden', isLogged);
            document.getElementById('user-pill').classList.toggle('hidden', !isLogged);
            
            if (isLogged) {
                document.getElementById('head-name').innerText = userData.fullName;
                document.getElementById('head-avatar').src = userData.avatar || `https://ui-avatars.com/api/?name=${userData.fullName}&background=00853f&color=fff`;
                document.getElementById('btn-sell-trigger').classList.toggle('hidden', userData.role !== 'producer');
            }
        }

        // --- NAVIGATION ---
        window.showView = (v) => {
            document.getElementById('view-market').classList.toggle('hidden', v !== 'market');
            document.getElementById('view-profile').classList.toggle('hidden', v !== 'profile');
            if (v === 'profile') loadProfileForm();
        };

        // --- GESTION PROFIL (AVEC COOLDOWN PHOTO) ---
        function loadProfileForm() {
            document.getElementById('form-name').value = userData.fullName || "";
            document.getElementById('form-phone').value = userData.phone || "";
            document.getElementById('form-province').value = userData.province || "Estuaire";
            document.getElementById('form-city').value = userData.city || "";
            document.getElementById('form-bio').value = userData.bio || "";
            document.getElementById('prof-avatar-img').src = userData.avatar || `https://ui-avatars.com/api/?name=${userData.fullName}&background=00853f&color=fff`;
            document.getElementById('prof-title-name').innerText = userData.fullName;
            document.getElementById('prof-title-role').innerText = userData.role === 'producer' ? 'Producteur Certifié' : 'Acheteur';
            
            checkAvatarLock();
        }

        function checkAvatarLock() {
            const label = document.getElementById('avatar-label');
            const msg = document.getElementById('avatar-lock-msg');
            if (!userData.lastAvatarChange) return;

            const last = userData.lastAvatarChange.toDate();
            const now = new Date();
            const diffDays = Math.floor((now - last) / (1000 * 60 * 60 * 24));
            const COOLDOWN = 30;

            if (diffDays < COOLDOWN) {
                label.classList.add('opacity-50', 'pointer-events-none');
                msg.classList.remove('hidden');
                document.getElementById('lock-days').innerText = COOLDOWN - diffDays;
            } else {
                label.classList.remove('opacity-50', 'pointer-events-none');
                msg.classList.add('hidden');
            }
        }

        window.handleAvatarChange = (e) => {
            const file = e.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = async (event) => {
                const base64 = event.target.result;
                try {
                    await updateDoc(doc(db, 'artifacts', appId, 'users', userData.uid), {
                        avatar: base64,
                        lastAvatarChange: serverTimestamp()
                    });
                    userData.avatar = base64;
                    document.getElementById('prof-avatar-img').src = base64;
                    document.getElementById('head-avatar').src = base64;
                    showToast("Photo de profil mise à jour !");
                    checkAvatarLock();
                } catch (err) { showToast("Erreur de sauvegarde"); }
            };
            reader.readAsDataURL(file);
        };

        document.getElementById('profile-form').onsubmit = async (e) => {
            e.preventDefault();
            const upd = {
                fullName: document.getElementById('form-name').value,
                phone: document.getElementById('form-phone').value,
                province: document.getElementById('form-province').value,
                city: document.getElementById('form-city').value,
                bio: document.getElementById('form-bio').value,
                updatedAt: serverTimestamp()
            };
            try {
                await updateDoc(doc(db, 'artifacts', appId, 'users', userData.uid), upd);
                userData = { ...userData, ...upd };
                updateAppUI();
                showToast("Profil enregistré !");
                showView('market');
            } catch (err) { showToast("Erreur de mise à jour"); }
        };

        // --- MARCHÉ & PUBLICATION ---
        function syncMarket() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const grid = document.getElementById('market-grid');
                grid.innerHTML = "";
                let products = [];
                snap.forEach(d => products.push({id: d.id, ...d.data()}));
                
                const filtered = products.filter(p => currentFilter === 'Toutes' || p.province === currentFilter);
                
                if(filtered.length === 0) {
                    grid.innerHTML = `<div class="col-span-full py-20 text-center text-slate-300 font-bold uppercase text-[9px]">Aucun produit en ${currentFilter}</div>`;
                    return;
                }

                filtered.forEach(p => {
                    const card = document.createElement('div');
                    card.className = "product-card shadow-sm";
                    card.innerHTML = `
                        <div class="h-36 bg-slate-100 relative">
                            <img src="${p.image}" class="w-full h-full object-cover">
                            <div class="absolute top-2 right-2 bg-black/40 text-white px-2 py-1 rounded-lg text-[7px] font-bold backdrop-blur-md uppercase">${p.province}</div>
                        </div>
                        <div class="p-3">
                            <h4 class="font-bold text-[10px] uppercase truncate text-slate-800">${p.name}</h4>
                            <p class="text-[12px] font-black text-emerald-600 mb-2">${p.price.toLocaleString()} FCFA <span class="text-[7px] text-slate-400">/ ${p.unit}</span></p>
                            <div class="flex items-center gap-2 pt-2 border-t border-slate-50">
                                <div class="w-4 h-4 rounded-full brand-gradient text-[6px] text-white flex items-center justify-center font-bold">${p.ownerName.charAt(0)}</div>
                                <span class="text-[7px] font-bold uppercase text-slate-400 truncate">${p.ownerName}</span>
                            </div>
                        </div>
                    `;
                    grid.appendChild(card);
                });
            });
        }

        window.previewProdPhoto = (e) => {
            const file = e.target.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = (event) => {
                currentProdImg = event.target.result;
                document.getElementById('prod-photo-preview').innerHTML = `<img src="${currentProdImg}" class="w-full h-full object-cover">`;
            };
            reader.readAsDataURL(file);
        };

        window.aiImageGen = async () => {
            const name = document.getElementById('p-name').value;
            if(!name) return showToast("Saisissez le nom du produit d'abord");
            const btn = document.getElementById('btn-ai');
            btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin mr-2"></i> Génération...`;
            btn.disabled = true;

            try {
                const prompt = `Un produit frais gabonais: ${name}, style photographie professionnelle, lumière naturelle.`;
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/imagen-4.0-generate-001:predict?key=${apiKey}`, {
                    method: 'POST',
                    body: JSON.stringify({ instances: { prompt: prompt }, parameters: { sampleCount: 1 } })
                });
                const result = await response.json();
                if(result.predictions?.[0]?.bytesBase64Encoded) {
                    currentProdImg = `data:image/png;base64,${result.predictions[0].bytesBase64Encoded}`;
                    document.getElementById('prod-photo-preview').innerHTML = `<img src="${currentProdImg}" class="w-full h-full object-cover">`;
                    showToast("IA a trouvé une image !");
                }
            } catch (err) { showToast("Erreur IA, utilisez la caméra"); }
            finally { btn.innerHTML = `<i class="fa-solid fa-wand-magic-sparkles"></i> Pas de photo ? Utiliser l'IA`; btn.disabled = false; }
        };

        document.getElementById('pub-form').onsubmit = async (e) => {
            e.preventDefault();
            if(!currentProdImg) return showToast("Prenez une photo du produit");
            const data = {
                name: document.getElementById('p-name').value,
                price: parseInt(document.getElementById('p-price').value),
                unit: document.getElementById('p-unit').value,
                image: currentProdImg,
                province: userData.province || "Estuaire",
                ownerId: userData.uid,
                ownerName: userData.fullName,
                createdAt: serverTimestamp()
            };
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), data);
                togglePublishModal();
                showToast("Produit publié !");
                document.getElementById('pub-form').reset();
                currentProdImg = "";
                document.getElementById('prod-photo-preview').innerHTML = `<i class="fa-solid fa-camera text-slate-300 text-3xl mb-2"></i><span class="text-[10px] font-bold text-slate-400 uppercase">Prendre la photo</span>`;
            } catch (err) { showToast("Erreur de publication"); }
        };

        // --- AUTH UI LOGIC ---
        window.toggleAuthModal = () => document.getElementById('auth-modal').classList.toggle('hidden');
        window.switchAuth = (m) => {
            document.getElementById('login-box').classList.toggle('hidden', m === 'reg');
            document.getElementById('reg-box').classList.toggle('hidden', m === 'login');
            document.getElementById('auth-header').innerText = m === 'reg' ? "Inscription" : "Connexion";
        };
        window.setRegRole = (r) => {
            regRole = r;
            document.getElementById('role-b').className = r === 'buyer' ? 'p-3 rounded-xl border-2 border-emerald-500 bg-emerald-50 font-bold text-[9px] uppercase' : 'p-3 rounded-xl border-2 font-bold text-[9px] uppercase';
            document.getElementById('role-p').className = r === 'producer' ? 'p-3 rounded-xl border-2 border-emerald-500 bg-emerald-50 font-bold text-[9px] uppercase' : 'p-3 rounded-xl border-2 font-bold text-[9px] uppercase';
        };

        window.doLogin = async () => {
            const email = document.getElementById('l-email').value;
            const pass = document.getElementById('l-pass').value;
            try { await signInWithEmailAndPassword(auth, email, pass); toggleAuthModal(); } catch (e) { showToast("Accès refusé"); }
        };

        window.doRegister = async () => {
            const name = document.getElementById('r-name').value;
            const email = document.getElementById('r-email').value;
            const pass = document.getElementById('r-pass').value;
            try {
                const cred = await createUserWithEmailAndPassword(auth, email, pass);
                const data = { fullName: name, role: regRole, email, avatar: "", phone: "", province: "Estuaire", createdAt: serverTimestamp() };
                await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), data);
                toggleAuthModal();
            } catch (e) { showToast("Échec de création"); }
        };

        window.handleLogout = () => signOut(auth).then(() => location.reload());
        window.togglePublishModal = () => document.getElementById('pub-modal').classList.toggle('hidden');
        window.setProvinceFilter = (p) => { currentFilter = p; syncMarket(); };

        // --- UTILS ---
        function showToast(msg) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3000);
        }

        const provinces = ["Estuaire", "Haut-Ogooué", "Moyen-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Ogooué-Maritime", "Woleu-Ntem"];
        provinces.forEach(p => {
            const b = document.createElement('button');
            b.className = "bg-white border border-slate-100 px-5 py-3 rounded-2xl text-[10px] font-bold uppercase whitespace-nowrap shadow-sm";
            b.innerText = p;
            b.onclick = () => setProvinceFilter(p);
            document.getElementById('filters-container').appendChild(b);
        });
    </script>
</body>
</html>
