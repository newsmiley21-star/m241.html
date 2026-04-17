<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Le Marché 100% Gabonais</title> 
    
    <!-- PWA & SEO Gabon -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#00853f">
    <meta name="description" content="Echoppe241 : Votre marché digital de proximité au Gabon. Achetez et vendez vos produits locaux en toute sécurité.">
    
    <!-- Bibliothèques externes -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <style>
        :root { --emerald: #00853f; --emerald-dark: #006b32; --gabon-yellow: #fcd116; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; color: #1e293b; overflow-x: hidden; }
        
        /* Animations et Transitions */
        .view { display: none; opacity: 0; transform: translateY(15px); transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1); }
        .view.active { display: block; opacity: 1; transform: translateY(0); }
        
        /* Composants Premium */
        .glass-header { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px); border-bottom: 1px solid rgba(241, 245, 249, 1); }
        .btn-primary { background: linear-gradient(135deg, var(--emerald), var(--emerald-dark)); box-shadow: 0 10px 25px -5px rgba(0, 133, 63, 0.4); transition: all 0.3s ease; color: white; }
        .btn-primary:active { transform: scale(0.96); }
        .card-premium { background: white; border-radius: 28px; border: 1px solid #f1f5f9; box-shadow: 0 4px 20px rgba(0,0,0,0.03); overflow: hidden; }
        .premium-input { background: #f1f5f9; border: 2px solid transparent; border-radius: 18px; padding: 16px 20px; font-weight: 600; width: 100%; transition: all 0.3s; }
        .premium-input:focus { border-color: var(--emerald); background: white; outline: none; box-shadow: 0 0 0 4px rgba(0, 133, 63, 0.1); }
        
        /* Navigation basse */
        .tab-btn { position: relative; transition: all 0.3s; }
        .tab-btn.active i { color: var(--emerald); transform: translateY(-3px); }
        .tab-btn.active::after { content: ''; position: absolute; bottom: -8px; width: 4px; height: 4px; background: var(--emerald); border-radius: 50%; }

        /* Splash Screen */
        #splash { position: fixed; inset: 0; z-index: 9999; background: white; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        .loader-bar { width: 120px; height: 4px; background: #f1f5f9; border-radius: 10px; margin-top: 20px; overflow: hidden; }
        .loader-progress { width: 40%; height: 100%; background: var(--emerald); animation: load 1.5s infinite ease-in-out; }
        @keyframes load { 0% { transform: translateX(-100%); } 100% { transform: translateX(250%); } }
    </style>
</head>
<body class="pb-24">

    <!-- Splash Screen -->
    <div id="splash">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 mb-4">
        <h2 class="text-xl font-black italic uppercase tracking-tighter">Echoppe<span class="text-emerald-700">241</span></h2>
        <div class="loader-bar"><div class="loader-progress"></div></div>
    </div>

    <!-- HEADER FLOTTANT -->
    <header class="sticky top-0 z-50 glass-header px-6 h-20 flex items-center justify-between">
        <div class="flex items-center gap-3" onclick="navigateTo('home')">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10">
            <div class="leading-none">
                <h1 class="text-lg font-black uppercase tracking-tighter">Echoppe<span class="text-emerald-700">241</span></h1>
                <p class="text-[8px] font-bold text-slate-400 tracking-widest uppercase">Marché National</p>
            </div>
        </div>
        
        <div id="user-header" class="hidden flex items-center gap-3">
            <div id="admin-badge" class="hidden bg-red-100 text-red-600 text-[8px] font-black px-2 py-1 rounded-full uppercase">Admin</div>
            <div class="text-right">
                <p id="h-balance" class="text-xs font-black text-emerald-700">0 F</p>
                <p id="h-name" class="text-[9px] font-bold text-slate-400 uppercase">Chargement...</p>
            </div>
            <div class="relative" onclick="navigateTo('profile')">
                <img id="h-avatar" class="w-11 h-11 rounded-full border-2 border-white shadow-md object-cover">
                <div class="absolute -bottom-1 -right-1 w-4 h-4 bg-emerald-500 border-2 border-white rounded-full"></div>
            </div>
        </div>
        <button id="h-login" onclick="navigateTo('auth')" class="btn-primary px-5 py-2.5 rounded-2xl text-[10px] font-black uppercase tracking-widest">Connexion</button>
    </header>

    <main class="max-w-4xl mx-auto px-4 pt-6">
        
        <!-- VUE : ACCUEIL / MARCHÉ -->
        <div id="view-home" class="view active">
            <!-- Filtre Rapide -->
            <div class="flex gap-2 overflow-x-auto pb-6 no-scrollbar">
                <button onclick="filterProducts('all')" class="bg-white px-6 py-3 rounded-2xl shadow-sm text-[10px] font-black uppercase whitespace-nowrap border border-slate-100">Tout</button>
                <button onclick="filterProducts('Estuaire')" class="bg-white px-6 py-3 rounded-2xl shadow-sm text-[10px] font-black uppercase whitespace-nowrap border border-slate-100">Estuaire</button>
                <button onclick="filterProducts('Ogooué')" class="bg-white px-6 py-3 rounded-2xl shadow-sm text-[10px] font-black uppercase whitespace-nowrap border border-slate-100">Ogooué</button>
                <button onclick="filterProducts('Woleu-Ntem')" class="bg-white px-6 py-3 rounded-2xl shadow-sm text-[10px] font-black uppercase whitespace-nowrap border border-slate-100">Woleu-Ntem</button>
            </div>

            <div class="flex justify-between items-end mb-6">
                <h2 class="text-3xl font-black italic uppercase tracking-tighter leading-none">Arrivages <br><span class="text-emerald-700">du Jour</span></h2>
                <button onclick="navigateTo('publish')" class="text-[10px] font-black text-emerald-700 uppercase underline decoration-2 underline-offset-4">Vendre un produit</button>
            </div>

            <div id="grid-products" class="grid grid-cols-2 md:grid-cols-3 gap-4 lg:gap-6">
                <!-- Squelette de chargement -->
                <div class="animate-pulse bg-white h-64 rounded-3xl"></div>
                <div class="animate-pulse bg-white h-64 rounded-3xl"></div>
            </div>
        </div>

        <!-- VUE : AUTHENTIFICATION -->
        <div id="view-auth" class="view max-w-sm mx-auto">
            <div class="card-premium p-10 text-center">
                <h3 id="auth-title" class="text-2xl font-black italic uppercase mb-8">Espace Membre</h3>
                <div class="space-y-4">
                    <div id="signup-fields" class="hidden space-y-4">
                        <input type="text" id="auth-name" placeholder="Nom complet" class="premium-input">
                    </div>
                    <input type="email" id="auth-email" placeholder="Email" class="premium-input">
                    <input type="password" id="auth-pass" placeholder="Mot de passe" class="premium-input">
                    <button onclick="handleAuth()" id="btn-auth-submit" class="btn-primary w-full py-5 rounded-[22px] font-black uppercase tracking-widest mt-4">Continuer</button>
                    <button onclick="toggleAuthMode()" id="auth-switch" class="text-[10px] font-black uppercase text-slate-400 mt-4 block mx-auto underline">Nouveau ? Créer un compte</button>
                </div>
            </div>
        </div>

        <!-- VUE : RECHARGE (ECHOPEPAY) -->
        <div id="view-recharge" class="view max-w-xl mx-auto">
            <div class="card-premium p-8">
                <div class="flex items-center justify-between mb-8">
                    <h3 class="text-2xl font-black italic uppercase">Echoppe<span class="text-emerald-700">Pay</span></h3>
                    <span class="bg-emerald-100 text-emerald-700 px-3 py-1 rounded-full text-[9px] font-black uppercase">Paiement Sécurisé</span>
                </div>
                
                <div class="bg-slate-50 border border-slate-100 rounded-3xl p-6 mb-8">
                    <p class="text-[10px] font-black text-slate-400 uppercase mb-4 tracking-widest">Procédure de recharge</p>
                    <ul class="space-y-3 text-xs font-bold leading-relaxed">
                        <li class="flex gap-3"><span class="w-5 h-5 bg-emerald-600 text-white rounded-full flex items-center justify-center text-[10px]">1</span> Envoyez par Airtel Money au <span class="text-emerald-700">077 73 60 65</span></li>
                        <li class="flex gap-3"><span class="w-5 h-5 bg-emerald-600 text-white rounded-full flex items-center justify-center text-[10px]">2</span> Capturez le message de confirmation reçu</li>
                        <li class="flex gap-3"><span class="w-5 h-5 bg-emerald-600 text-white rounded-full flex items-center justify-center text-[10px]">3</span> Envoyez la capture et le montant ci-dessous</li>
                    </ul>
                </div>

                <div class="space-y-6">
                    <div onclick="document.getElementById('file-proof').click()" class="w-full h-48 bg-slate-50 border-2 border-dashed border-slate-200 rounded-[30px] flex flex-col items-center justify-center cursor-pointer overflow-hidden transition-all hover:border-emerald-300">
                        <div id="proof-preview" class="text-center px-4">
                            <i class="fa-solid fa-cloud-arrow-up text-4xl text-slate-300 mb-2"></i>
                            <p class="text-[9px] font-black text-slate-400 uppercase">Cliquez pour joindre la preuve d'envoi</p>
                        </div>
                        <input type="file" id="file-proof" class="hidden" accept="image/*">
                    </div>
                    <input type="number" id="recharge-amount" placeholder="Montant déposé (FCFA)" class="premium-input text-center text-xl">
                    <button onclick="submitRechargeRequest()" id="btn-recharge" class="btn-primary w-full py-5 rounded-[22px] font-black uppercase shadow-emerald-200">Envoyer pour validation</button>
                </div>
            </div>
        </div>

        <!-- VUE : PANEL ADMIN (Dépôts) -->
        <div id="view-admin" class="view max-w-2xl mx-auto">
            <h2 class="text-3xl font-black italic uppercase tracking-tighter mb-8">Demandes de <span class="text-red-600">Recharge</span></h2>
            <div id="admin-requests-list" class="space-y-4">
                <p class="text-center py-20 text-slate-300 font-bold uppercase text-[10px]">Aucune demande en attente</p>
            </div>
        </div>

        <!-- VUE : PROFIL & RÉGLAGES -->
        <div id="view-profile" class="view max-w-xl mx-auto">
            <div class="card-premium p-10 text-center mb-8">
                <div class="relative w-32 h-32 mx-auto mb-6">
                    <img id="p-avatar" class="w-full h-full rounded-full border-4 border-white shadow-xl object-cover bg-slate-100">
                    <button onclick="document.getElementById('change-avatar').click()" class="absolute bottom-0 right-0 w-10 h-10 bg-white shadow-lg rounded-full flex items-center justify-center border border-slate-100">
                        <i class="fa-solid fa-camera text-slate-400 text-sm"></i>
                    </button>
                    <input type="file" id="change-avatar" class="hidden" accept="image/*">
                </div>
                
                <h3 id="p-name" class="text-2xl font-black italic uppercase mb-8">Utilisateur</h3>

                <!-- Portefeuille -->
                <div class="bg-slate-900 rounded-[35px] p-8 text-white text-left shadow-2xl relative overflow-hidden">
                    <div class="absolute top-0 right-0 p-8 opacity-10"><i class="fa-solid fa-wallet text-6xl"></i></div>
                    <p class="text-[10px] font-black uppercase tracking-widest opacity-40 mb-1">Solde Disponible</p>
                    <h4 id="p-balance" class="text-4xl font-black italic mb-8 tracking-tighter">0 FCFA</h4>
                    <div class="grid grid-cols-2 gap-4">
                        <button onclick="navigateTo('recharge')" class="bg-emerald-600 py-4 rounded-2xl text-[9px] font-black uppercase tracking-widest">Recharger</button>
                        <button onclick="msg('Service bientôt disponible')" class="bg-white/10 py-4 rounded-2xl text-[9px] font-black uppercase tracking-widest">Retrait</button>
                    </div>
                </div>

                <div id="admin-access" class="hidden mt-6">
                    <button onclick="navigateTo('admin')" class="w-full py-5 border-2 border-red-500 text-red-500 rounded-2xl font-black uppercase text-[10px] hover:bg-red-50 transition-colors">Panel Admin Validation</button>
                </div>
            </div>

            <button onclick="handleLogout()" class="w-full py-5 bg-red-50 text-red-500 font-black rounded-2xl text-[10px] uppercase shadow-sm">Se Déconnecter</button>
        </div>

    </main>

    <!-- NAVIGATION BASSE -->
    <nav class="fixed bottom-0 inset-x-0 h-24 glass-header flex items-center justify-around px-8 z-40 pb-6">
        <button onclick="navigateTo('home')" class="tab-btn flex flex-col items-center gap-1 active">
            <i class="fa-solid fa-store text-xl text-slate-300"></i>
            <span class="text-[9px] font-black uppercase opacity-40">Marché</span>
        </button>
        <button onclick="navigateTo('publish')" class="tab-btn flex flex-col items-center gap-1">
            <div class="w-12 h-12 bg-emerald-700 text-white rounded-2xl flex items-center justify-center -mt-10 shadow-lg border-4 border-white shadow-emerald-200">
                <i class="fa-solid fa-plus text-lg"></i>
            </div>
            <span class="text-[9px] font-black uppercase text-emerald-700 mt-1">Vendre</span>
        </button>
        <button onclick="navigateTo('profile')" class="tab-btn flex flex-col items-center gap-1">
            <i class="fa-solid fa-circle-user text-xl text-slate-300"></i>
            <span class="text-[9px] font-black uppercase opacity-40">Compte</span>
        </button>
    </nav>

    <!-- Toast de notification -->
    <div id="toast" class="hidden fixed bottom-28 left-1/2 -translate-x-1/2 bg-slate-900 text-white px-8 py-4 rounded-full text-[11px] font-black uppercase z-[100] shadow-2xl flex items-center gap-3">
        <i id="toast-icon" class="fa-solid fa-check text-emerald-400"></i>
        <span id="toast-msg">Opération réussie</span>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, onSnapshot, addDoc, updateDoc, deleteDoc, serverTimestamp, increment, query, orderBy } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Initialisation Firebase
        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'echoppe241-prod-v1';

        // Constantes & Etat
        const ADMIN_EMAIL = "contact@echoppe241.com";
        let userState = null;
        let isSignup = false;
        let tempProof = null;
        let activeProvince = 'all';

        // AUTHENTIFICATION
        window.toggleAuthMode = () => {
            isSignup = !isSignup;
            document.getElementById('auth-title').innerText = isSignup ? 'Nouveau Compte' : 'Espace Membre';
            document.getElementById('signup-fields').classList.toggle('hidden', !isSignup);
            document.getElementById('auth-switch').innerText = isSignup ? 'Déjà inscrit ? Connexion' : 'Nouveau ? Créer un compte';
        };

        window.handleAuth = async () => {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;
            const btn = document.getElementById('btn-auth-submit');
            
            if(!email || !pass) return msg("Identifiants requis", "error");
            btn.disabled = true; btn.innerText = "Traitement...";

            try {
                if(isSignup) {
                    const name = document.getElementById('auth-name').value || "Utilisateur";
                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    const userData = {
                        uid: res.user.uid,
                        fullName: name,
                        email: email,
                        walletBalance: 0,
                        avatar: `https://ui-avatars.com/api/?name=${name}&background=00853f&color=fff`,
                        isAdmin: email === ADMIN_EMAIL,
                        createdAt: serverTimestamp()
                    };
                    await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid), userData);
                } else {
                    await signInWithEmailAndPassword(auth, email, pass);
                }
                navigateTo('home');
                msg("Bienvenue !");
            } catch (e) {
                msg("Erreur : Vérifiez vos infos", "error");
            } finally {
                btn.disabled = false; btn.innerText = "Continuer";
            }
        };

        // GESTION ETAT UTILISATEUR
        onAuthStateChanged(auth, (u) => {
            if (u && !u.isAnonymous) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        userState = snap.data();
                        updateInterface();
                        if(userState.isAdmin) listenToRecharges();
                    }
                });
            } else {
                if(!u) signInAnonymously(auth);
                userState = null;
                updateInterface();
            }
            // Masquer Splash
            setTimeout(() => document.getElementById('splash').style.display = 'none', 1200);
        });

        function updateInterface() {
            const isLogged = userState && !auth.currentUser.isAnonymous;
            const isAdmin = userState?.isAdmin;

            document.getElementById('user-header').classList.toggle('hidden', !isLogged);
            document.getElementById('h-login').classList.toggle('hidden', isLogged);
            document.getElementById('admin-badge').classList.toggle('hidden', !isAdmin);
            document.getElementById('admin-access').classList.toggle('hidden', !isAdmin);

            if (isLogged) {
                document.getElementById('h-name').innerText = userState.fullName;
                document.getElementById('h-balance').innerText = `${userState.walletBalance.toLocaleString()} F`;
                document.getElementById('h-avatar').src = userState.avatar;
                document.getElementById('p-name').innerText = userState.fullName;
                document.getElementById('p-avatar').src = userState.avatar;
                document.getElementById('p-balance').innerText = `${userState.walletBalance.toLocaleString()} FCFA`;
            }
        }

        // RECHARGE : ENVOI DE PREUVE
        document.getElementById('file-proof').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if(!file) return;
            const r = new FileReader();
            r.onload = (ev) => {
                tempProof = ev.target.result;
                document.getElementById('proof-preview').innerHTML = `<img src="${tempProof}" class="w-full h-full object-cover">`;
            };
            r.readAsDataURL(file);
        });

        window.submitRechargeRequest = async () => {
            const amount = parseInt(document.getElementById('recharge-amount').value);
            if(!amount || !tempProof) return msg("Preuve et montant requis", "error");
            
            const btn = document.getElementById('btn-recharge');
            btn.disabled = true; btn.innerText = "Envoi en cours...";

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                    userId: userState.uid,
                    userName: userState.fullName,
                    amount: amount,
                    proofImg: tempProof,
                    status: 'pending',
                    at: serverTimestamp()
                });
                msg("Demande envoyée ! Validation sous 1h.");
                navigateTo('profile');
                // Nettoyage
                document.getElementById('recharge-amount').value = '';
                tempProof = null;
                document.getElementById('proof-preview').innerHTML = `<i class="fa-solid fa-cloud-arrow-up text-4xl text-slate-300 mb-2"></i><p class="text-[9px] font-black text-slate-400 uppercase">Preuve de paiement</p>`;
            } catch (e) {
                msg("Erreur d'envoi", "error");
            } finally {
                btn.disabled = false; btn.innerText = "Envoyer pour validation";
            }
        };

        // ADMIN : VALIDATION MANUELLE
        function listenToRecharges() {
            // Pas de filtres complexes pour éviter le besoin d'index au lancement
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), (snap) => {
                const list = document.getElementById('admin-requests-list');
                const pending = snap.docs
                    .map(d => ({id: d.id, ...d.data()}))
                    .filter(r => r.status === 'pending');

                if(pending.length === 0) {
                    list.innerHTML = `<p class="text-center py-20 text-slate-300 font-bold uppercase text-[10px]">Aucune demande en attente</p>`;
                    return;
                }

                list.innerHTML = pending.map(r => `
                    <div class="card-premium p-6">
                        <div class="flex justify-between items-start mb-4">
                            <div>
                                <h4 class="font-black text-sm uppercase">${r.userName}</h4>
                                <p class="text-xl font-black text-emerald-700 tracking-tighter">${r.amount.toLocaleString()} FCFA</p>
                            </div>
                            <span class="bg-yellow-100 text-yellow-700 px-3 py-1 rounded-full text-[8px] font-black uppercase">Vérification</span>
                        </div>
                        <img src="${r.proofImg}" class="w-full h-56 object-cover rounded-2xl border mb-6 cursor-pointer" onclick="window.open(this.src)">
                        <div class="grid grid-cols-2 gap-4">
                            <button onclick="approveRecharge('${r.id}', '${r.userId}', ${r.amount})" class="bg-emerald-600 text-white font-black py-4 rounded-xl text-[10px] uppercase">Valider le dépôt</button>
                            <button onclick="rejectRecharge('${r.id}')" class="bg-red-50 text-red-500 font-black py-4 rounded-xl text-[10px] uppercase">Rejeter</button>
                        </div>
                    </div>
                `).join('');
            });
        }

        window.approveRecharge = async (docId, uid, amt) => {
            if(!confirm(`Confirmez-vous avoir reçu ${amt} F ?`)) return;
            try {
                // On met à jour le solde de l'utilisateur ET on marque la demande complétée
                await updateDoc(doc(db, 'artifacts', appId, 'users', uid), { walletBalance: increment(amt) });
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', docId), { status: 'completed' });
                msg("Portefeuille crédité !");
            } catch (e) { msg("Erreur de validation", "error"); }
        };

        window.rejectRecharge = async (id) => {
            if(!confirm("Supprimer cette demande invalide ?")) return;
            await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', id));
            msg("Demande supprimée");
        };

        // NAVIGATION & UI
        window.navigateTo = (v) => {
            // Garde d'accès
            if(['recharge', 'profile', 'admin'].includes(v) && (!userState || auth.currentUser.isAnonymous)) return navigateTo('auth');
            if(v === 'admin' && !userState.isAdmin) return msg("Accès refusé", "error");

            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            const map = {'home':0, 'publish':1, 'profile':2}[v];
            if(map !== undefined) document.querySelectorAll('.tab-btn')[map].classList.add('active');
            window.scrollTo({top:0, behavior:'smooth'});
        };

        function msg(m, type = 'success') {
            const t = document.getElementById('toast');
            const icon = document.getElementById('toast-icon');
            document.getElementById('toast-msg').innerText = m;
            
            icon.className = type === 'error' ? 'fa-solid fa-circle-exclamation text-red-400' : 'fa-solid fa-check text-emerald-400';
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // Marketplace (Statique pour démo dans ce bloc, à relier à vos produits réels)
        const products = [
            { id: 1, name: "Manioc d'Oyem", price: 2500, province: "Woleu-Ntem", img: "https://images.unsplash.com/photo-1590779033100-9f60a05a013d?auto=format&fit=crop&w=400" },
            { id: 2, name: "Poisson Fumé POG", price: 5000, province: "Ogooué", img: "https://images.unsplash.com/photo-1519708227418-c8fd9a32b7a2?auto=format&fit=crop&w=400" },
            { id: 3, name: "Banane Plantain", price: 1500, province: "Estuaire", img: "https://images.unsplash.com/photo-1528825871115-3581a5387919?auto=format&fit=crop&w=400" }
        ];

        function renderProducts() {
            const grid = document.getElementById('grid-products');
            grid.innerHTML = products.map(p => `
                <div class="card-premium group">
                    <div class="relative h-48">
                        <img src="${p.img}" class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110">
                        <div class="absolute top-3 left-3 bg-white/90 backdrop-blur px-3 py-1 rounded-full text-[8px] font-black uppercase tracking-widest">${p.province}</div>
                    </div>
                    <div class="p-4">
                        <h4 class="font-bold text-xs mb-1 uppercase">${p.name}</h4>
                        <div class="flex justify-between items-center">
                            <p class="text-emerald-700 font-black italic">${p.price.toLocaleString()} F</p>
                            <button onclick="msg('Vérifiez votre solde')" class="w-8 h-8 rounded-full bg-slate-900 text-white flex items-center justify-center text-[10px]"><i class="fa-solid fa-cart-shopping"></i></button>
                        </div>
                    </div>
                </div>
            `).join('');
        }
        
        window.onload = renderProducts;

    </script>
</body>
</html>
