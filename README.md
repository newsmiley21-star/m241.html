<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Officiel</title> 
    
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --primary-blue: #3a75c4; /* Bleu */
            --accent-yellow: #fcd116; /* Jaune */
            --subtle-green: #00853f; /* Vert subtil */
            --white: #ffffff;
            --dark: #0f172a;
            --bg-soft: #f8fafc;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--white); color: var(--dark); margin: 0; overflow-x: hidden; }
        h1, h2, h3, .brand-font { font-family: 'Syne', sans-serif; text-transform: lowercase; }

        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* Splash Screen */
        #splash-screen {
            position: fixed; inset: 0; background: var(--white); z-index: 9999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 0.5s ease;
        }

        /* UI Elements */
        .card-neo { background: var(--white); border-radius: 24px; border: 1px solid #f1f5f9; box-shadow: 0 4px 12px rgba(58, 117, 196, 0.05); }
        
        /* Buttons */
        .btn-blue { background: var(--primary-blue); color: var(--white); padding: 18px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 11px; width: 100%; transition: 0.2s; }
        .btn-blue:active { transform: scale(0.98); opacity: 0.9; }
        
        .btn-yellow { background: var(--accent-yellow); color: var(--dark); padding: 12px; border-radius: 16px; font-weight: 700; font-size: 10px; text-transform: uppercase; }

        .input-custom { background: #f8fafc; border-radius: 16px; padding: 16px; width: 100%; font-size: 14px; border: 2px solid transparent; outline: none; transition: 0.3s; }
        .input-custom:focus { border-color: var(--primary-blue); background: var(--white); }

        /* Toasts */
        #toast {
            position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%);
            z-index: 10000; background: var(--dark); color: var(--white);
            padding: 12px 24px; border-radius: 50px; font-size: 11px; font-weight: 700; display: none;
        }

        .nav-item { color: #cbd5e1; transition: 0.3s; }
        .nav-item.active { color: var(--primary-blue); }
        
        /* Subtle Green Touch */
        .green-dot { width: 8px; height: 8px; background: var(--subtle-green); border-radius: full; }
        .verified-badge { background: rgba(0, 133, 63, 0.1); color: var(--subtle-green); font-size: 9px; font-weight: 800; padding: 4px 12px; border-radius: 20px; }

        .modal-full { position: fixed; inset: 0; background: var(--white); z-index: 9000; overflow-y: auto; padding: 32px 24px; display: none; }
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

    <!-- MODAL EDITION PROFIL -->
    <div id="edit-profile-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-10">
                <h2 class="text-2xl font-black text-blue-900">votre profil.</h2>
                <button onclick="closeEditProfile()" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>

            <div class="flex flex-col items-center mb-10">
                <div class="relative">
                    <img id="edit-avatar-preview" class="w-28 h-28 rounded-[35px] object-cover bg-slate-50 border-4 border-white shadow-xl">
                    <button onclick="regenerateAvatar()" class="absolute -bottom-2 -right-2 w-10 h-10 bg-[#fcd116] shadow-lg rounded-2xl flex items-center justify-center text-blue-900">
                        <i class="fa-solid fa-arrows-rotate"></i>
                    </button>
                </div>
                <p class="text-[10px] font-black text-slate-400 mt-6 uppercase tracking-[0.2em]">personnalisation</p>
            </div>

            <div class="space-y-5">
                <div>
                    <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Nom public</label>
                    <input type="text" id="edit-name" class="input-custom mt-1" placeholder="Votre nom">
                </div>
                <div>
                    <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Bio vendeur</label>
                    <textarea id="edit-bio" class="input-custom mt-1 h-28" placeholder="Parlez de vous ou de vos produits..."></textarea>
                </div>
                <div>
                    <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Numéro WhatsApp</label>
                    <input type="tel" id="edit-whatsapp" class="input-custom mt-1" placeholder="077... / 066...">
                </div>
                <button onclick="saveProfile()" class="btn-blue mt-6 shadow-lg shadow-blue-200">Enregistrer les changements</button>
            </div>
        </div>
    </div>

    <!-- HEADER NAVIGATION -->
    <header class="fixed top-0 inset-x-0 z-[500] h-20 px-6 flex items-center justify-between bg-white/90 backdrop-blur-xl border-b border-slate-50">
        <div class="flex items-center gap-2">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-8 h-8">
            <h1 class="text-xl font-black italic text-blue-900">echoppe<span class="text-[#fcd116]">241</span></h1>
        </div>
        <div class="flex items-center gap-4">
            <div class="text-right">
                <p id="header-balance" class="text-[13px] font-black text-blue-600">0 F</p>
                <div class="flex items-center justify-end gap-1">
                    <div class="green-dot"></div>
                    <span class="text-[8px] font-bold uppercase text-slate-400">Pay</span>
                </div>
            </div>
            <img id="header-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-2xl bg-slate-50 cursor-pointer object-cover border-2 border-white shadow-sm">
        </div>
    </header>

    <main class="max-w-xl mx-auto px-6 pt-24 pb-12">
        <!-- VUE ACCUEIL -->
        <div id="view-home" class="view active">
            <div class="flex items-center justify-between mb-8">
                <h2 class="text-3xl font-black text-blue-900 leading-tight">Le Marché<br><span class="text-[#fcd116]">Gabonais.</span></h2>
                <button class="w-12 h-12 bg-slate-50 rounded-2xl flex items-center justify-center text-blue-900">
                    <i class="fa-solid fa-magnifying-glass"></i>
                </button>
            </div>
            
            <div id="product-list" class="grid grid-cols-2 gap-4">
                <!-- Chargement dynamique -->
            </div>
        </div>

        <!-- VUE PROFIL -->
        <div id="view-profile" class="view">
            <div class="card-neo p-6 mb-6">
                <div class="flex items-start justify-between mb-6">
                    <img id="p-img" class="w-24 h-24 rounded-[32px] object-cover bg-slate-50 shadow-md">
                    <div class="flex flex-col items-end gap-2">
                        <span id="p-status-badge" class="verified-badge">Standard</span>
                        <button onclick="openEditProfile()" class="px-5 py-2.5 bg-blue-50 text-blue-600 text-[10px] font-black uppercase rounded-xl">
                            Éditer
                        </button>
                    </div>
                </div>
                <h3 id="p-name" class="text-2xl font-black text-blue-900">...</h3>
                <p id="p-bio" class="text-sm text-slate-500 mt-2 leading-relaxed">Bienvenue sur votre profil Echoppe241.</p>
                
                <div class="grid grid-cols-2 gap-3 mt-8">
                    <div class="bg-blue-600 p-5 rounded-3xl text-white shadow-lg shadow-blue-100">
                        <p class="text-[9px] font-bold opacity-60 uppercase tracking-widest mb-1">Portefeuille</p>
                        <p id="p-wallet" class="text-xl font-black italic">0 FCFA</p>
                    </div>
                    <div class="bg-[#fcd116] p-5 rounded-3xl text-blue-900">
                        <p class="text-[9px] font-bold opacity-60 uppercase tracking-widest mb-1">Annonces</p>
                        <p class="text-xl font-black italic">0</p>
                    </div>
                </div>
            </div>

            <div class="space-y-3">
                <button class="w-full p-5 bg-white border border-slate-100 rounded-2xl flex items-center justify-between">
                    <div class="flex items-center gap-4">
                        <div class="w-10 h-10 bg-emerald-50 text-[#00853f] rounded-xl flex items-center justify-center text-lg"><i class="fa-solid fa-bolt"></i></div>
                        <span class="text-xs font-bold uppercase text-slate-700">Recharger via Mobile Money</span>
                    </div>
                    <i class="fa-solid fa-chevron-right text-slate-300"></i>
                </button>
                <button class="w-full p-5 bg-white border border-slate-100 rounded-2xl flex items-center justify-between">
                    <div class="flex items-center gap-4">
                        <div class="w-10 h-10 bg-blue-50 text-blue-600 rounded-xl flex items-center justify-center"><i class="fa-solid fa-box-open"></i></div>
                        <span class="text-xs font-bold uppercase text-slate-700">Publier un produit</span>
                    </div>
                    <i class="fa-solid fa-plus text-slate-300"></i>
                </button>
            </div>
        </div>
    </main>

    <!-- TAB BAR -->
    <nav class="fixed bottom-0 inset-x-0 h-22 bg-white/95 backdrop-blur-xl border-t border-slate-50 flex items-center justify-around px-8 pb-4">
        <button onclick="navigateTo('home')" id="nav-home" class="nav-item flex flex-col items-center gap-1.5 active">
            <i class="fa-solid fa-house-chimney text-xl"></i>
            <span class="text-[9px] font-black uppercase">Home</span>
        </button>
        <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item flex flex-col items-center gap-1.5">
            <i class="fa-solid fa-circle-user text-xl"></i>
            <span class="text-[9px] font-black uppercase">Compte</span>
        </button>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, updateDoc, setDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // CONFIGURATION OFFICIELLE
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f",
            measurementId: "G-T7415FPS91"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe241-prod-final';

        let currentUser = null;

        onAuthStateChanged(auth, async (u) => {
            if (u) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        currentUser = { uid: u.uid, ...snap.data() };
                        updateUI();
                    } else {
                        const initData = {
                            fullName: "Membre Echoppe",
                            walletBalance: 0,
                            bio: "Gabonais et fier de l'être.",
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`,
                            isVerified: false,
                            createdAt: new Date().toISOString()
                        };
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), initData);
                    }
                });
            } else { signInAnonymously(auth); }
        });

        function updateUI() {
            if (!currentUser) return;
            // Header
            document.getElementById('header-balance').innerText = `${currentUser.walletBalance.toLocaleString()} F`;
            document.getElementById('header-avatar').src = currentUser.avatar;
            // Profil
            document.getElementById('p-img').src = currentUser.avatar;
            document.getElementById('p-name').innerText = currentUser.fullName;
            document.getElementById('p-bio').innerText = currentUser.bio || "Aucune description.";
            document.getElementById('p-wallet').innerText = `${currentUser.walletBalance.toLocaleString()} FCFA`;
            
            const badge = document.getElementById('p-status-badge');
            badge.innerText = currentUser.isVerified ? "Vendeur Certifié" : "Membre Standard";
            badge.style.color = currentUser.isVerified ? "#00853f" : "#3a75c4";
        }

        // ACTIONS PROFIL
        window.openEditProfile = () => {
            document.getElementById('edit-name').value = currentUser.fullName;
            document.getElementById('edit-bio').value = currentUser.bio || "";
            document.getElementById('edit-whatsapp').value = currentUser.whatsapp || "";
            document.getElementById('edit-avatar-preview').src = currentUser.avatar;
            document.getElementById('edit-profile-modal').style.display = 'block';
        };

        window.closeEditProfile = () => document.getElementById('edit-profile-modal').style.display = 'none';

        window.regenerateAvatar = () => {
            const seed = Math.random().toString(36).substring(7);
            document.getElementById('edit-avatar-preview').src = `https://api.dicebear.com/7.x/avataaars/svg?seed=${seed}`;
        };

        window.saveProfile = async () => {
            const name = document.getElementById('edit-name').value;
            const bio = document.getElementById('edit-bio').value;
            const wa = document.getElementById('edit-whatsapp').value;
            const av = document.getElementById('edit-avatar-preview').src;

            try {
                await updateDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid), {
                    fullName: name, bio: bio, whatsapp: wa, avatar: av
                });
                showToast("Profil enregistré !");
                closeEditProfile();
            } catch (e) { showToast("Erreur de sauvegarde"); }
        };

        // NAVIGATION
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(`nav-${v}`).classList.add('active');
        };

        window.showToast = (msg) => {
            const t = document.getElementById('toast');
            t.innerText = msg;
            t.style.display = 'block';
            setTimeout(() => t.style.display = 'none', 3000);
        };

        // MOCKUP PRODUITS
        const products = [
            { name: "Manioc de Mouila", price: 3500, img: "https://images.unsplash.com/photo-1590779033100-9f60705a2f3b?w=400" },
            { name: "Bananes Douces (Régime)", price: 5000, img: "https://images.unsplash.com/photo-1571771894821-ad99026.png?w=400" },
            { name: "Poisson Fumée (Owendo)", price: 12000, img: "https://images.unsplash.com/photo-1519708227418-c8fd9a32b7a2?w=400" },
            { name: "Huile de Palme Pure", price: 2500, img: "https://images.unsplash.com/photo-1474979266404-7eaacbcd87c5?w=400" }
        ];

        document.getElementById('product-list').innerHTML = products.map(p => `
            <div class="card-neo overflow-hidden flex flex-col">
                <img src="${p.img}" class="w-full h-36 object-cover">
                <div class="p-4 flex flex-col flex-1">
                    <p class="text-[10px] font-black uppercase text-blue-900 truncate mb-1">${p.name}</p>
                    <p class="text-xs font-black text-blue-600 mb-4 italic">${p.price.toLocaleString()} F</p>
                    <button class="mt-auto w-full py-2.5 bg-[#fcd116] text-blue-900 rounded-xl text-[9px] font-black uppercase shadow-sm">
                        Voir l'offre
                    </button>
                </div>
            </div>
        `).join('');

        window.addEventListener('load', () => {
            setTimeout(() => {
                const s = document.getElementById('splash-screen');
                s.style.opacity = '0';
                setTimeout(() => s.style.display = 'none', 500);
            }, 2500);
        });
    </script>
</body>
</html>
