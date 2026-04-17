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

        /* Splash Screen */
        #splash-screen {
            position: fixed; inset: 0; background: white; z-index: 9999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 0.5s ease;
        }

        /* Forms Custom */
        .input-custom { background: #f1f5f9; border-radius: 16px; padding: 16px; width: 100%; font-size: 14px; font-weight: 500; border: 2px solid transparent; outline: none; transition: 0.3s; }
        .input-custom:focus { border-color: var(--primary); background: white; }
        
        /* Modal Inscription / KYC */
        #registration-modal {
            position: fixed; inset: 0; background: white; z-index: 9000;
            overflow-y: auto; padding: 32px 24px; display: none;
        }

        .card-neo { background: white; border-radius: 24px; box-shadow: 0 4px 15px rgba(0,0,0,0.03); }
        .btn-primary { background: var(--primary); color: white; padding: 18px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 12px; width: 100%; transition: transform 0.2s; }
        .btn-primary:active { transform: scale(0.98); }
        
        .id-upload-box {
            border: 2px dashed #cbd5e1; border-radius: 20px; height: 120px;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            cursor: pointer; position: relative; overflow: hidden; transition: 0.3s;
        }
        .id-upload-box.has-file { border-color: var(--primary); background: #f0fdf4; }

        /* Badges */
        .cert-badge { font-size: 10px; font-weight: 800; text-transform: uppercase; padding: 4px 10px; border-radius: 20px; }
        .cert-pending { background: #fef3c7; color: #92400e; }
        .cert-verified { background: #dcfce7; color: #166534; }

        /* Navigation */
        .nav-item { transition: 0.3s; color: #cbd5e1; }
        .nav-item.active { color: var(--primary); }
    </style>
</head>
<body class="pb-24">

    <!-- SPLASH SCREEN -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-32 h-32 animate-pulse mb-6" onerror="this.src='https://via.placeholder.com/150?text=Echoppe241'">
        <h2 class="text-2xl font-black italic">echoppe<span class="text-[#00853f]">241</span></h2>
        <div class="mt-8 flex gap-2">
            <div class="w-2 h-2 rounded-full bg-[#00853f] animate-bounce"></div>
            <div class="w-2 h-2 rounded-full bg-[#fcd116] animate-bounce [animation-delay:0.2s]"></div>
            <div class="w-2 h-2 rounded-full bg-[#3a75c4] animate-bounce [animation-delay:0.4s]"></div>
        </div>
    </div>

    <!-- MODAL D'INSCRIPTION KYC OBLIGATOIRE -->
    <div id="registration-modal">
        <div class="max-w-md mx-auto">
            <div class="flex items-center gap-3 mb-8">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 h-10">
                <h2 class="text-2xl font-black">Finaliser votre profil</h2>
            </div>
            <p class="text-slate-500 text-sm mb-8">Pour garantir la sécurité de la communauté au Gabon, veuillez fournir vos informations officielles.</p>
            
            <div class="space-y-4">
                <div>
                    <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Identité réelle</label>
                    <input type="text" id="reg-name" placeholder="Nom et Prénoms complets" class="input-custom mt-1">
                </div>
                
                <div class="flex gap-2">
                    <div class="flex-1">
                        <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Localisation</label>
                        <select id="reg-province" class="input-custom mt-1">
                            <option value="">Province</option>
                            <option>Estuaire</option><option>Haut-Ogooué</option><option>Moyen-Ogooué</option>
                            <option>Ngounié</option><option>Nyanga</option><option>Ogooué-Ivindo</option>
                            <option>Ogooué-Lolo</option><option>Ogooué-Maritime</option><option>Woleu-Ntem</option>
                        </select>
                    </div>
                    <div class="flex-[1.5]">
                        <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Ville/Quartier</label>
                        <input type="text" id="reg-city" placeholder="Ex: Akanda" class="input-custom mt-1">
                    </div>
                </div>

                <div>
                    <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Contact Mobile Money</label>
                    <input type="tel" id="reg-phone" placeholder="Numéro Airtel ou Moov" class="input-custom mt-1">
                </div>
                
                <div class="flex gap-2">
                    <div class="flex-1">
                        <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Sexe</label>
                        <select id="reg-gender" class="input-custom mt-1">
                            <option value="">Sexe</option>
                            <option>Homme</option><option>Femme</option>
                        </select>
                    </div>
                    <div class="flex-[1.5]">
                        <label class="text-[10px] font-bold uppercase text-slate-400 ml-2">Date de naissance</label>
                        <input type="date" id="reg-dob" class="input-custom mt-1">
                    </div>
                </div>

                <div class="pt-4">
                    <p class="text-[10px] font-black uppercase tracking-widest text-slate-400 mb-2">Pièce d'identité (Photo CNI/Passeport)</p>
                    <label class="id-upload-box" id="id-label">
                        <i class="fa-solid fa-camera text-2xl text-slate-300 mb-2"></i>
                        <span class="text-[10px] font-bold text-slate-400" id="id-status">Cliquez pour prendre en photo</span>
                        <input type="file" id="reg-id-file" class="hidden" accept="image/*">
                    </label>
                </div>

                <button onclick="submitRegistration()" id="btn-submit-reg" class="btn-primary mt-8">Rejoindre Echoppe241</button>
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
            <div class="text-right hidden sm:block">
                <p id="top-balance" class="currency text-xs font-bold text-emerald-600">0 F</p>
                <span id="top-status" class="cert-badge cert-pending">Vérification</span>
            </div>
            <img id="top-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-2xl bg-slate-100 object-cover border-2 border-white shadow-sm cursor-pointer">
        </div>
    </header>

    <main class="max-w-xl mx-auto px-6 pt-24 pb-10">
        <!-- ACCUEIL -->
        <div id="view-home" class="view active">
            <div class="flex items-center justify-between mb-6">
                <h2 class="text-3xl font-black leading-tight">Le Marché<br><span class="text-[#00853f]">National.</span></h2>
                <button class="w-12 h-12 bg-white rounded-2xl shadow-sm flex items-center justify-center text-slate-400">
                    <i class="fa-solid fa-sliders"></i>
                </button>
            </div>
            <div id="product-list" class="grid grid-cols-2 gap-4">
                <!-- Produits mockés -->
            </div>
        </div>

        <!-- MESSAGES -->
        <div id="view-messages" class="view">
            <h2 class="text-3xl font-black mb-6">Messages.</h2>
            <div class="card-neo p-12 text-center">
                <div class="w-20 h-20 bg-slate-50 rounded-full flex items-center justify-center mx-auto mb-4">
                    <i class="fa-solid fa-comments text-3xl text-slate-200"></i>
                </div>
                <p class="text-sm font-bold text-slate-400">Aucune discussion active.</p>
                <button onclick="navigateTo('home')" class="mt-4 text-[#00853f] text-xs font-bold uppercase">Parcourir les offres</button>
            </div>
        </div>

        <!-- PROFIL -->
        <div id="view-profile" class="view">
            <div class="card-neo overflow-hidden mb-6">
                <div class="h-28 bg-gradient-to-br from-[#00853f] via-[#fcd116] to-[#3a75c4]"></div>
                <div class="px-6 pb-6">
                    <div class="flex justify-between items-end -mt-12">
                        <img id="p-avatar-img" class="w-24 h-24 rounded-[32px] border-4 border-white bg-white object-cover shadow-lg">
                        <span id="p-status" class="cert-badge cert-pending mb-2 shadow-sm">En cours d'examen</span>
                    </div>
                    <h3 id="p-full-name" class="text-2xl font-black mt-4">Utilisateur</h3>
                    <p id="p-location" class="text-sm text-slate-500 font-medium italic"><i class="fa-solid fa-location-dot mr-1"></i> Gabon</p>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <div class="card-neo p-5">
                    <p class="text-[9px] font-black uppercase text-slate-400 mb-1">Solde EchoppePay</p>
                    <p id="p-wallet" class="currency text-xl font-bold text-emerald-600">0 F</p>
                </div>
                <div class="card-neo p-5">
                    <p class="text-[9px] font-black uppercase text-slate-400 mb-1">Status ID</p>
                    <p id="p-id-label" class="text-[10px] font-bold text-slate-900 truncate uppercase tracking-tighter">Attente Validation</p>
                </div>
            </div>
            
            <div class="space-y-3">
                <button class="w-full p-5 bg-white border border-slate-100 rounded-2xl flex items-center justify-between group">
                    <span class="text-xs font-bold uppercase text-slate-600">Mes Annonces</span>
                    <i class="fa-solid fa-chevron-right text-slate-300 group-hover:text-primary transition-colors"></i>
                </button>
                <button onclick="handleLogout()" class="w-full p-5 text-[10px] font-black uppercase text-red-500 bg-red-50 rounded-2xl tracking-widest">Déconnexion</button>
            </div>
        </div>
    </main>

    <!-- NAVIGATION BASSE -->
    <nav class="fixed bottom-0 inset-x-0 h-20 bg-white/95 backdrop-blur-xl border-t border-slate-100 flex items-center justify-around px-6 z-[1000]">
        <button onclick="navigateTo('home')" class="nav-item flex flex-col items-center gap-1 active" id="nav-home">
            <i class="fa-solid fa-shop text-xl"></i>
            <span class="text-[9px] font-bold uppercase">Marché</span>
        </button>
        <button onclick="navigateTo('messages')" class="nav-item flex flex-col items-center gap-1" id="nav-messages">
            <i class="fa-solid fa-comment-dots text-xl"></i>
            <span class="text-[9px] font-bold uppercase">Messages</span>
        </button>
        <button onclick="navigateTo('profile')" class="nav-item flex flex-col items-center gap-1" id="nav-profile">
            <i class="fa-solid fa-user-circle text-xl"></i>
            <span class="text-[9px] font-bold uppercase">Compte</span>
        </button>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc, updateDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // CONFIGURATION FIREBASE
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

        // AUTH & DATABASE SYNC
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
                        // Nouvel utilisateur anonyme
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
                            isRegistered: false,
                            walletBalance: 0,
                            isVerified: false,
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`
                        });
                    }
                }, (error) => console.error("Firestore Error:", error));
            } else {
                signInAnonymously(auth);
            }
        });

        // UI SYNC
        function updateUI() {
            if(!user) return;
            
            const statusEl = document.getElementById('top-status');
            const pStatusEl = document.getElementById('p-status');
            
            if(user.isVerified) {
                statusEl.innerText = "Certifié";
                statusEl.className = "cert-badge cert-verified";
                pStatusEl.innerText = "Profil Certifié";
                pStatusEl.className = "cert-badge cert-verified mb-2";
            } else {
                statusEl.innerText = "Vérification";
                statusEl.className = "cert-badge cert-pending";
                pStatusEl.innerText = "Examen en cours";
                pStatusEl.className = "cert-badge cert-pending mb-2";
            }

            document.getElementById('top-balance').innerText = `${user.walletBalance.toLocaleString()} F`;
            document.getElementById('p-wallet').innerText = `${user.walletBalance.toLocaleString()} F`;
            document.getElementById('top-avatar').src = user.avatar;
            document.getElementById('p-avatar-img').src = user.avatar;
            document.getElementById('p-full-name').innerText = user.fullName || "Utilisateur";
            document.getElementById('p-location').innerHTML = `<i class="fa-solid fa-location-dot mr-1"></i> ${user.city || 'Gabon'}, ${user.province || ''}`;
            document.getElementById('p-id-label').innerText = user.isVerified ? "ID VERIFIÉ" : "ID EN ATTENTE";
        }

        // REGISTRATION / KYC
        function showRegistration() {
            document.getElementById('registration-modal').style.display = 'block';
        }

        function hideRegistration() {
            document.getElementById('registration-modal').style.display = 'none';
        }

        document.getElementById('reg-id-file').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if(!file) return;
            if(file.size > 2 * 1024 * 1024) { alert("Image trop lourde (max 2Mo)"); return; }
            
            const reader = new FileReader();
            reader.onload = (re) => {
                idBase64 = re.target.result;
                document.getElementById('id-status').innerText = "Document prêt ✓";
                document.getElementById('id-label').classList.add('has-file');
            };
            reader.readAsDataURL(file);
        });

        window.submitRegistration = async () => {
            const name = document.getElementById('reg-name').value.trim();
            const province = document.getElementById('reg-province').value;
            const city = document.getElementById('reg-city').value.trim();
            const phone = document.getElementById('reg-phone').value.trim();
            const gender = document.getElementById('reg-gender').value;
            const dob = document.getElementById('reg-dob').value;

            if(!name || !province || !city || !phone || !gender || !dob || !idBase64) {
                alert("Veuillez remplir tous les champs et fournir votre pièce d'identité.");
                return;
            }

            const btn = document.getElementById('btn-submit-reg');
            btn.innerText = "Sécurisation du profil...";
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
                console.error(err);
                alert("Erreur de connexion. Réessayez.");
                btn.disabled = false;
                btn.innerText = "Recommencer";
            }
        };

        // NAVIGATION LOGIC
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            
            document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
            document.getElementById(`nav-${v}`).classList.add('active');
            
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };

        // INITIALIZATION
        window.addEventListener('load', () => {
            setTimeout(() => {
                const s = document.getElementById('splash-screen');
                s.style.opacity = '0';
                setTimeout(() => s.style.display = 'none', 500);
            }, 3000);
        });

        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // MOCK PRODUCTS
        const products = [
            { name: "Manioc Frais d'Oyem", price: 5000, img: "https://images.unsplash.com/photo-1590779033100-9f60705a2f3b?w=400" },
            { name: "Bananes douces", price: 2500, img: "https://images.unsplash.com/photo-1571771894821-ad99026.png?w=400" },
            { name: "Poisson Salé", price: 8000, img: "https://images.unsplash.com/photo-1519708227418-c8fd9a32b7a2?w=400" },
            { name: "Huile de Palme", price: 1500, img: "https://images.unsplash.com/photo-1474979266404-7eaacbcd87c5?w=400" }
        ];

        document.getElementById('product-list').innerHTML = products.map(p => `
            <div class="card-neo overflow-hidden">
                <img src="${p.img}" class="w-full h-32 object-cover">
                <div class="p-4">
                    <h4 class="brand-font text-[10px] font-bold uppercase truncate mb-1">${p.name}</h4>
                    <p class="currency text-xs font-black text-[#00853f] mb-3">${p.price.toLocaleString()} F</p>
                    <button class="w-full py-2.5 bg-slate-900 text-white rounded-xl text-[9px] font-bold uppercase tracking-wider">Acheter</button>
                </div>
            </div>
        `).join('');

    </script>
</body>
</html>
