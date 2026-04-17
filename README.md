<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Officiel Gabon</title> 
    
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@600&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --primary: #00853f; --accent: #fcd116; --blue: #3a75c4;
            --dark: #0f172a; --bg-soft: #f8fafc;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--bg-soft); color: var(--dark); margin: 0; overflow-x: hidden; }
        h1, h2, h3, .brand-font { font-family: 'Syne', sans-serif; text-transform: lowercase; }
        .currency { font-family: 'JetBrains Mono', monospace; }

        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* Splash Screen corrigé */
        #splash-screen {
            position: fixed; inset: 0; background: white; z-index: 9999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
        }

        /* Forms */
        .input-custom { background: #f1f5f9; border-radius: 16px; padding: 16px; width: 100%; font-size: 14px; font-weight: 500; border: 2px solid transparent; outline: none; transition: 0.3s; }
        .input-custom:focus { border-color: var(--primary); background: white; }
        
        /* Modal Inscription */
        #registration-modal {
            position: fixed; inset: 0; background: white; z-index: 9000;
            overflow-y: auto; padding: 32px 24px; display: none;
        }

        .card-neo { background: white; border-radius: 24px; box-shadow: 0 4px 15px rgba(0,0,0,0.03); }
        .btn-primary { background: var(--primary); color: white; padding: 18px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 12px; width: 100%; }
        
        .id-upload-box {
            border: 2px dashed #cbd5e1; border-radius: 20px; height: 120px;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            cursor: pointer; position: relative; overflow: hidden;
        }
        .id-upload-box.has-file { border-color: var(--primary); background: #f0fdf4; }

        /* Badge certification */
        .cert-badge { font-size: 10px; font-weight: 800; text-transform: uppercase; padding: 4px 10px; border-radius: 20px; }
        .cert-pending { background: #fef3c7; color: #92400e; }
        .cert-verified { background: #dcfce7; color: #166534; }
    </style>
</head>
<body class="pb-24">

    <!-- SPLASH SCREEN -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-32 h-32 animate-pulse mb-6">
        <h2 class="text-2xl font-black italic">echoppe<span class="text-[#00853f]">241</span></h2>
        <div class="mt-8 flex gap-2">
            <div class="w-2 h-2 rounded-full bg-[#00853f] animate-bounce"></div>
            <div class="w-2 h-2 rounded-full bg-[#fcd116] animate-bounce [animation-delay:0.2s]"></div>
            <div class="w-2 h-2 rounded-full bg-[#3a75c4] animate-bounce [animation-delay:0.4s]"></div>
        </div>
    </div>

    <!-- MODAL D'INSCRIPTION KYC -->
    <div id="registration-modal">
        <div class="max-w-md mx-auto">
            <div class="flex items-center gap-3 mb-8">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 h-10">
                <h2 class="text-2xl font-black">Finaliser votre inscription</h2>
            </div>
            <p class="text-slate-500 text-sm mb-8">Pour garantir la sécurité de la communauté Gabonais, veuillez fournir vos informations officielles.</p>
            
            <div class="space-y-4">
                <input type="text" id="reg-name" placeholder="Nom et Prénoms complets" class="input-custom">
                
                <div class="flex gap-2">
                    <select id="reg-province" class="input-custom flex-1">
                        <option value="">Province</option>
                        <option>Estuaire</option><option>Haut-Ogooué</option><option>Moyen-Ogooué</option>
                        <option>Ngounié</option><option>Nyanga</option><option>Ogooué-Ivindo</option>
                        <option>Ogooué-Lolo</option><option>Ogooué-Maritime</option><option>Woleu-Ntem</option>
                    </select>
                    <input type="text" id="reg-city" placeholder="Ville/Quartier" class="input-custom flex-[1.5]">
                </div>

                <input type="tel" id="reg-phone" placeholder="Numéro Airtel ou Moov Money" class="input-custom">
                
                <div class="flex gap-2">
                    <select id="reg-gender" class="input-custom flex-1">
                        <option value="">Sexe</option>
                        <option>Homme</option><option>Femme</option>
                    </select>
                    <input type="date" id="reg-dob" class="input-custom flex-[1.5]">
                </div>

                <div class="pt-4">
                    <p class="text-[10px] font-black uppercase tracking-widest text-slate-400 mb-2">Pièce d'identité (CNI ou Passeport)</p>
                    <label class="id-upload-box" id="id-label">
                        <i class="fa-solid fa-id-card text-2xl text-slate-300 mb-2"></i>
                        <span class="text-[10px] font-bold text-slate-400" id="id-status">Cliquez pour scanner/choisir</span>
                        <input type="file" id="reg-id-file" class="hidden" accept="image/*">
                    </label>
                </div>

                <button onclick="submitRegistration()" id="btn-submit-reg" class="btn-primary mt-8">Commencer sur Echoppe241</button>
            </div>
        </div>
    </div>

    <!-- APP CONTENT -->
    <header class="fixed top-0 inset-x-0 z-[500] h-20 px-6 flex items-center justify-between bg-white/80 backdrop-blur-lg border-b border-slate-50">
        <div class="flex items-center gap-2" onclick="navigateTo('home')">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-9 h-9">
            <h1 class="text-xl font-extrabold italic">echoppe<span class="text-[#00853f]">241</span></h1>
        </div>
        <div id="user-zone" class="flex items-center gap-3">
            <div class="text-right">
                <p id="top-balance" class="currency text-xs font-bold text-emerald-600">0 F</p>
                <span id="top-status" class="cert-badge cert-pending">En attente</span>
            </div>
            <img id="top-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-2xl bg-slate-100 object-cover border-2 border-white shadow-sm cursor-pointer">
        </div>
    </header>

    <main class="max-w-xl mx-auto px-6 pt-24 pb-10">
        <!-- ACCUEIL -->
        <div id="view-home" class="view active">
            <h2 class="text-3xl font-black mb-6">Bienvenue au <br><span class="text-[#00853f]">Marché National.</span></h2>
            <div id="product-list" class="grid grid-cols-2 gap-4"></div>
        </div>

        <!-- MESSAGES -->
        <div id="view-messages" class="view">
            <h2 class="text-3xl font-black mb-6">Discussions.</h2>
            <div class="card-neo p-8 text-center text-slate-400">
                <i class="fa-solid fa-comments text-4xl mb-4"></i>
                <p class="text-sm font-medium">Aucun message pour le moment.</p>
            </div>
        </div>

        <!-- PROFIL -->
        <div id="view-profile" class="view">
            <div class="card-neo overflow-hidden mb-6">
                <div class="h-24 bg-gradient-to-r from-[#00853f] to-[#3a75c4]"></div>
                <div class="px-6 pb-6">
                    <div class="flex justify-between items-end -mt-10">
                        <img id="p-avatar-img" class="w-24 h-24 rounded-[32px] border-4 border-white bg-white object-cover">
                        <span id="p-status" class="cert-badge cert-pending mb-2">Vérification en cours</span>
                    </div>
                    <h3 id="p-full-name" class="text-2xl font-black mt-4">Utilisateur</h3>
                    <p id="p-location" class="text-sm text-slate-500 font-medium italic"><i class="fa-solid fa-location-dot mr-1"></i> Gabon</p>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <div class="card-neo p-4">
                    <p class="text-[9px] font-black uppercase text-slate-400 mb-1">Portefeuille</p>
                    <p id="p-wallet" class="currency text-xl font-bold text-emerald-600">0 F</p>
                </div>
                <div class="card-neo p-4">
                    <p class="text-[9px] font-black uppercase text-slate-400 mb-1">ID Certifié</p>
                    <p id="p-id-label" class="text-xs font-bold text-slate-900 truncate">En attente admin</p>
                </div>
            </div>
            
            <button onclick="handleLogout()" class="w-full p-4 text-[10px] font-black uppercase text-red-500 bg-red-50 rounded-2xl">Se déconnecter</button>
        </div>
    </main>

    <!-- NAVIGATION BASSE -->
    <nav class="fixed bottom-0 inset-x-0 h-20 bg-white/95 backdrop-blur-xl border-t border-slate-100 flex items-center justify-around px-6 z-[1000]">
        <button onclick="navigateTo('home')" class="nav-item group flex flex-col items-center gap-1 active">
            <i class="fa-solid fa-store text-xl text-slate-300 group-[.active]:text-[#00853f]"></i>
            <span class="text-[9px] font-bold uppercase">Marché</span>
        </button>
        <button onclick="navigateTo('messages')" class="nav-item group flex flex-col items-center gap-1">
            <i class="fa-solid fa-message text-xl text-slate-300 group-[.active]:text-[#00853f]"></i>
            <span class="text-[9px] font-bold uppercase">Messages</span>
        </button>
        <button onclick="navigateTo('profile')" class="nav-item group flex flex-col items-center gap-1">
            <i class="fa-solid fa-user-circle text-xl text-slate-300 group-[.active]:text-[#00853f]"></i>
            <span class="text-[9px] font-bold uppercase">Compte</span>
        </button>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc, updateDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // FIREBASE CONFIG
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
        
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'echoppe241-prod';

        let user = null;
        let idBase64 = null;

        // AUTH INITIALISATION
        onAuthStateChanged(auth, async (u) => {
            if (u) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if(snap.exists()) {
                        user = { uid: u.uid, ...snap.data() };
                        if(!user.isRegistered) {
                            showRegistration();
                        } else {
                            hideRegistration();
                            updateUI();
                        }
                    } else {
                        // Créer un squelette d'utilisateur
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
                            isRegistered: false,
                            walletBalance: 0,
                            isVerified: false,
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`
                        });
                    }
                });
            } else {
                signInAnonymously(auth);
            }
        });

        // UI UPDATES
        function updateUI() {
            if(!user) return;
            // Barres de statut
            const statusEl = document.getElementById('top-status');
            const pStatusEl = document.getElementById('p-status');
            
            if(user.isVerified) {
                statusEl.innerText = "Certifié";
                statusEl.className = "cert-badge cert-verified";
                pStatusEl.innerText = "Vendeur Certifié";
                pStatusEl.className = "cert-badge cert-verified mb-2";
            } else {
                statusEl.innerText = "En attente";
                statusEl.className = "cert-badge cert-pending";
            }

            document.getElementById('top-balance').innerText = `${user.walletBalance.toLocaleString()} F`;
            document.getElementById('p-wallet').innerText = `${user.walletBalance.toLocaleString()} F`;
            document.getElementById('top-avatar').src = user.avatar;
            document.getElementById('p-avatar-img').src = user.avatar;
            document.getElementById('p-full-name').innerText = user.fullName;
            document.getElementById('p-location').innerHTML = `<i class="fa-solid fa-location-dot mr-1"></i> ${user.city}, ${user.province}`;
            document.getElementById('p-id-label').innerText = user.isVerified ? "Document vérifié" : "Preuve envoyée";
        }

        // REGISTRATION MANAGEMENT
        function showRegistration() {
            document.getElementById('registration-modal').style.display = 'block';
        }

        function hideRegistration() {
            document.getElementById('registration-modal').style.display = 'none';
        }

        // GESTION UPLOAD PIECE D'IDENTITÉ
        document.getElementById('reg-id-file').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = (re) => {
                idBase64 = re.target.result;
                document.getElementById('id-status').innerText = "Document chargé ✓";
                document.getElementById('id-label').classList.add('has-file');
            };
            reader.readAsDataURL(file);
        });

        window.submitRegistration = async () => {
            const name = document.getElementById('reg-name').value;
            const province = document.getElementById('reg-province').value;
            const city = document.getElementById('reg-city').value;
            const phone = document.getElementById('reg-phone').value;
            const gender = document.getElementById('reg-gender').value;
            const dob = document.getElementById('reg-dob').value;

            if(!name || !province || !city || !phone || !gender || !dob || !idBase64) {
                alert("Tous les champs, y compris la pièce d'identité, sont obligatoires.");
                return;
            }

            const btn = document.getElementById('btn-submit-reg');
            btn.innerText = "Traitement en cours...";
            btn.disabled = true;

            try {
                await updateDoc(doc(db, 'artifacts', appId, 'users', auth.currentUser.uid), {
                    fullName: name,
                    province,
                    city,
                    phone,
                    gender,
                    dob,
                    idDocument: idBase64,
                    isRegistered: true,
                    registrationDate: new Date().toISOString()
                });
            } catch (err) {
                alert("Erreur lors de l'inscription. Vérifiez votre connexion.");
                btn.disabled = false;
                btn.innerText = "Recommencer";
            }
        };

        // NAVIGATION
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(btn => {
                btn.classList.toggle('active', btn.onclick.toString().includes(v));
            });
            window.scrollTo(0,0);
        };

        // SPLASH SCREEN
        window.addEventListener('load', () => {
            setTimeout(() => {
                const s = document.getElementById('splash-screen');
                s.style.opacity = '0';
                setTimeout(() => s.style.display = 'none', 500);
            }, 3000);
        });

        // LOGOUT
        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // MOCK DATA
        const products = [
            { id: 1, name: "Manioc Frais d'Oyem", price: 5000, img: "https://images.unsplash.com/photo-1590779033100-9f60705a2f3b?w=400" },
            { id: 2, name: "Atanga de Port-Gentil", price: 3500, img: "https://images.unsplash.com/photo-1614735241165-6756e1df61ab?w=400" },
            { id: 3, name: "Poisson Salé", price: 8000, img: "https://images.unsplash.com/photo-1519708227418-c8fd9a32b7a2?w=400" },
            { id: 4, name: "Huile de Palme", price: 1500, img: "https://images.unsplash.com/photo-1474979266404-7eaacbcd87c5?w=400" }
        ];

        document.getElementById('product-list').innerHTML = products.map(p => `
            <div class="card-neo overflow-hidden">
                <img src="${p.img}" class="w-full h-32 object-cover">
                <div class="p-4">
                    <h4 class="brand-font text-[10px] font-bold uppercase truncate">${p.name}</h4>
                    <p class="currency text-xs font-bold text-[#00853f] mb-3">${p.price.toLocaleString()} F</p>
                    <button class="w-full py-2.5 bg-slate-900 text-white rounded-xl text-[9px] font-bold uppercase">Commander</button>
                </div>
            </div>
        `).join('');

    </script>
</body>
</html>
