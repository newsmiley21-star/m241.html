<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Le Marché Digital du Gabon</title>
    
    <!-- Meta tags PWA -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">
    <meta name="theme-color" content="#00853f">
    
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --gabon-green: #00853f; 
            --gabon-yellow: #fcd116; 
            --gabon-blue: #3a75c4; 
            --deep-slate: #0f172a; 
            --accent: #f59e0b;
        }
        
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background-color: #fcfcfd; 
            color: var(--deep-slate); 
            overflow-x: hidden; 
            -webkit-tap-highlight-color: transparent; 
        }

        .brand-gradient { background: linear-gradient(135deg, var(--gabon-green), #059669); }
        .hero-gradient { background: linear-gradient(180deg, rgba(0, 133, 63, 0.05) 0%, rgba(252, 250, 252, 0) 100%); }
        
        .glass { background: rgba(255, 255, 255, 0.85); backdrop-filter: blur(12px); border-bottom: 1px solid rgba(0,0,0,0.05); }
        
        .btn-primary {
            background: var(--deep-slate);
            color: white;
            transition: all 0.2s ease;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }
        .btn-primary:active { transform: scale(0.96); opacity: 0.9; }

        .product-card { 
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); 
            border: 1px solid rgba(0,0,0,0.03);
            background: white;
        }
        .product-card:hover { transform: translateY(-8px); box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.05); }
        
        .hide-scroll::-webkit-scrollbar { display: none; }
        
        .chip-filter {
            transition: all 0.3s ease;
            white-space: nowrap;
        }
        .chip-filter.active {
            background: var(--gabon-green);
            color: white;
            border-color: var(--gabon-green);
            box-shadow: 0 4px 12px rgba(0, 133, 63, 0.2);
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .animate-fade { animation: fadeIn 0.5s ease-out forwards; }
    </style>
</head>
<body>

    <!-- Notification Toast -->
    <div id="toast" class="fixed top-6 left-1/2 -translate-x-1/2 z-[3000] hidden">
        <div class="bg-slate-900 text-white px-6 py-4 rounded-3xl shadow-2xl flex items-center gap-3 border-l-4 border-amber-400 min-w-[300px]">
            <i class="fa-solid fa-bell text-amber-400"></i>
            <span id="toast-msg" class="text-xs font-bold tracking-wide">Message</span>
        </div>
    </div>

    <!-- Header Navigation -->
    <header class="sticky top-0 z-[1000] glass">
        <div class="h-[4px] flex">
            <div class="flex-1 bg-[var(--gabon-green)]"></div>
            <div class="flex-1 bg-[var(--gabon-yellow)]"></div>
            <div class="flex-1 bg-[var(--gabon-blue)]"></div>
        </div>
        <div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Echoppe241" class="h-10 w-auto">
                <span class="font-black text-xl tracking-tighter uppercase hidden sm:block">Echoppe<span class="text-emerald-600">241</span></span>
            </div>
            
            <div class="flex items-center gap-3">
                <div id="user-profile" class="hidden flex items-center gap-2 bg-slate-100 p-1.5 rounded-full border">
                    <div id="user-initials" class="w-8 h-8 rounded-full brand-gradient text-white flex items-center justify-center font-black text-[10px] uppercase shadow-md tracking-tighter">?</div>
                    <button onclick="handleLogout()" class="w-8 h-8 flex items-center justify-center text-slate-400 hover:text-red-500 transition-colors"><i class="fa-solid fa-power-off text-xs"></i></button>
                </div>
                <button id="btn-login-trigger" onclick="toggleAuthModal()" class="btn-primary px-6 py-3 rounded-full font-extrabold text-[10px] uppercase tracking-widest">Compte</button>
            </div>
        </div>
    </header>

    <!-- Vitrine (Hero Section) -->
    <section id="hero-section" class="hero-gradient px-6 pt-12 pb-16 text-center max-w-5xl mx-auto">
        <span class="inline-block bg-emerald-100 text-emerald-700 px-4 py-1.5 rounded-full text-[9px] font-black uppercase tracking-widest mb-6">Plateforme 100% Gabonaise</span>
        <h1 class="text-5xl md:text-7xl font-black tracking-tighter leading-[0.9] mb-6 italic uppercase">
            Le Terroir à portée <br> de <span class="text-emerald-600 underline decoration-amber-400 underline-offset-8">clic</span>.
        </h1>
        <p class="text-slate-500 font-medium text-lg max-w-xl mx-auto mb-10 leading-relaxed">
            Soutenez l'économie locale. Commandez les meilleurs produits de nos 9 provinces et faites-vous livrer chez vous.
        </p>
        <div class="flex flex-wrap justify-center gap-4">
            <button onclick="document.getElementById('market-grid').scrollIntoView({behavior:'smooth'})" class="btn-primary px-10 py-5 rounded-full font-black uppercase text-[11px] tracking-widest">Explorer le marché</button>
            <button onclick="setRole('producer'); toggleAuthModal();" class="bg-white border-2 border-slate-200 px-10 py-5 rounded-full font-black uppercase text-[11px] tracking-widest hover:border-emerald-600 transition-all">Devenir Vendeur</button>
        </div>
    </section>

    <main class="max-w-7xl mx-auto px-6 pb-24">
        
        <!-- Navigation Provinces -->
        <div class="mb-12">
            <div class="flex items-center justify-between mb-4">
                <h3 class="font-black uppercase text-[11px] tracking-[0.2em] text-slate-400">Filtrer par province</h3>
                <span class="h-px flex-1 bg-slate-100 mx-6"></span>
            </div>
            <div class="flex gap-3 overflow-x-auto pb-4 hide-scroll" id="province-filters">
                <button onclick="setFilter('all')" class="chip-filter active px-6 py-3 rounded-full border-2 border-slate-200 text-[10px] font-black uppercase tracking-wider">Toutes</button>
                <!-- Provinces injectées par JS -->
            </div>
        </div>

        <div id="view-home">
            <!-- Espace Producteur (Masqué par défaut) -->
            <section id="view-producer" class="hidden mb-16 animate-fade">
                <div class="bg-slate-900 rounded-[3rem] p-8 md:p-12 text-white flex flex-col md:flex-row items-center justify-between gap-8">
                    <div>
                        <h2 class="text-3xl font-black uppercase italic mb-2 tracking-tighter">Votre Boutique</h2>
                        <div class="flex gap-8 mt-6">
                            <div>
                                <p class="text-[10px] uppercase font-black opacity-50 mb-1">Chiffre d'affaires</p>
                                <span id="stat-sales" class="text-3xl font-black">0 FCFA</span>
                            </div>
                        </div>
                    </div>
                    <button onclick="togglePublishModal()" class="bg-emerald-500 hover:bg-emerald-400 text-white w-full md:w-auto px-10 py-6 rounded-3xl font-black uppercase text-xs tracking-widest shadow-2xl transition-all">
                        <i class="fa-solid fa-plus mr-3"></i> Ajouter un produit
                    </button>
                </div>
            </section>

            <!-- Marché -->
            <div class="flex items-center gap-4 mb-8">
                <h2 class="text-3xl font-black uppercase italic tracking-tighter">Les pépites du moment</h2>
                <div class="flex-1 h-[1px] bg-slate-100"></div>
            </div>
            
            <div id="market-grid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-8">
                <!-- Squelettes de chargement -->
                <div class="bg-slate-50 aspect-[4/5] rounded-[2.5rem] animate-pulse"></div>
                <div class="bg-slate-50 aspect-[4/5] rounded-[2.5rem] animate-pulse"></div>
                <div class="bg-slate-50 aspect-[4/5] rounded-[2.5rem] animate-pulse"></div>
            </div>
        </div>

        <!-- Vue Paiement -->
        <div id="view-checkout" class="hidden max-w-2xl mx-auto py-12 animate-fade">
            <div class="bg-white rounded-[4rem] p-10 md:p-16 shadow-2xl border border-slate-50">
                <button onclick="navigateTo('home')" class="group flex items-center gap-2 text-slate-400 text-[10px] font-black uppercase mb-12 hover:text-emerald-600 transition-colors">
                    <i class="fa-solid fa-arrow-left group-hover:-translate-x-1 transition-transform"></i> Retour au marché
                </button>
                
                <h2 class="text-5xl font-black uppercase italic tracking-tighter mb-10 leading-[0.8]">Confirmation</h2>
                
                <div id="checkout-item-ui" class="bg-slate-50 p-8 rounded-[3rem] flex items-center gap-6 mb-12">
                    <!-- Rempli par JS -->
                </div>

                <div class="space-y-10">
                    <div>
                        <p class="text-[10px] font-black uppercase text-slate-400 mb-4 tracking-[0.2em] ml-2">Mode de réception</p>
                        <div class="grid grid-cols-2 gap-4">
                            <button onclick="setDeliveryMode('pickup')" id="opt-pickup" class="delivery-option active p-6 border-2 rounded-3xl text-center transition-all">
                                <i class="fa-solid fa-shop mb-2 block text-xl"></i>
                                <span class="font-black text-[10px] uppercase">Retrait magasin</span>
                            </button>
                            <button onclick="setDeliveryMode('delivery')" id="opt-delivery" class="delivery-option p-6 border-2 rounded-3xl text-center transition-all">
                                <i class="fa-solid fa-truck-fast mb-2 block text-xl"></i>
                                <span class="font-black text-[10px] uppercase">Livraison (+1500)</span>
                            </button>
                        </div>
                    </div>

                    <div id="delivery-address-box" class="hidden">
                        <label class="text-[10px] font-black uppercase text-slate-400 mb-2 block ml-2">Adresse de livraison</label>
                        <input type="text" id="pay-address" placeholder="Ex: Akanda, Carrefour Jiji" class="w-full bg-slate-50 p-6 rounded-3xl border-2 border-transparent focus:border-emerald-500 font-bold outline-none transition-all">
                    </div>

                    <div class="p-10 bg-emerald-600 text-white rounded-[3rem] flex flex-col items-center shadow-xl">
                        <p class="text-[10px] font-black uppercase opacity-60 mb-2">Montant Total</p>
                        <h4 id="checkout-total" class="text-4xl font-black tracking-tighter">0 FCFA</h4>
                    </div>

                    <div>
                        <label class="text-[10px] font-black uppercase text-slate-400 mb-2 block ml-2">Numéro Mobile Money</label>
                        <input type="tel" id="pay-phone" placeholder="07x xx xx xx" class="w-full bg-slate-100 p-8 rounded-[2.5rem] font-black text-3xl text-center outline-none focus:ring-4 ring-emerald-100 transition-all">
                    </div>

                    <button onclick="processOrder()" class="btn-primary w-full py-8 rounded-[2.5rem] font-black uppercase text-sm tracking-[0.3em] shadow-2xl hover:bg-emerald-700 transition-all">
                        Payer maintenant
                    </button>
                </div>
            </div>
        </div>
    </main>

    <!-- Modale Auth -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/80 backdrop-blur-xl z-[2000] hidden flex items-center justify-center p-6">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 animate-fade">
            <div class="flex justify-between items-center mb-10">
                <h3 class="text-3xl font-black italic uppercase tracking-tighter">Mon Espace</h3>
                <button onclick="toggleAuthModal()" class="w-10 h-10 rounded-full bg-slate-50 text-slate-400 flex items-center justify-center"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="space-y-6">
                <div class="grid grid-cols-2 gap-3 mb-4">
                    <button onclick="setRole('buyer')" id="role-buyer" class="role-btn active p-6 border-2 rounded-3xl flex flex-col items-center gap-2 transition-all">
                        <i class="fa-solid fa-basket-shopping text-xl"></i>
                        <span class="text-[9px] font-black uppercase">Acheteur</span>
                    </button>
                    <button onclick="setRole('producer')" id="role-producer" class="role-btn p-6 border-2 rounded-3xl flex flex-col items-center gap-2 transition-all">
                        <i class="fa-solid fa-wheat-awn text-xl"></i>
                        <span class="text-[9px] font-black uppercase">Producteur</span>
                    </button>
                </div>
                <div>
                    <label class="text-[9px] font-black uppercase text-slate-400 mb-2 block ml-2">Votre nom ou enseigne</label>
                    <input type="text" id="auth-name" placeholder="Ex: Les Jardins d'Oyem" class="w-full bg-slate-50 p-5 rounded-2xl border-2 border-transparent focus:border-emerald-500 font-bold outline-none transition-all">
                </div>
                <button onclick="handleAuthSubmit()" class="btn-primary w-full py-6 rounded-2xl font-black uppercase text-[11px] tracking-widest mt-4">Commencer l'aventure</button>
            </div>
        </div>
    </div>

    <!-- Modale Vente -->
    <div id="publish-modal" class="fixed inset-0 bg-slate-900/80 backdrop-blur-xl z-[2000] hidden flex items-center justify-center p-6">
        <div class="bg-white w-full max-w-lg rounded-[3.5rem] p-12 animate-fade overflow-y-auto max-h-[90vh]">
            <div class="flex justify-between items-center mb-8">
                <h3 class="text-3xl font-black italic uppercase tracking-tighter">Nouvel Article</h3>
                <button onclick="togglePublishModal()" class="text-slate-400 p-2"><i class="fa-solid fa-xmark text-xl"></i></button>
            </div>
            <div class="space-y-5">
                <input type="text" id="p-name" placeholder="Nom du produit" class="w-full bg-slate-50 p-5 rounded-2xl border-2 border-transparent focus:border-emerald-500 font-bold italic outline-none">
                <div class="relative">
                    <input type="number" id="p-price" placeholder="Prix de vente" class="w-full bg-slate-50 p-5 rounded-2xl border-2 border-transparent focus:border-emerald-500 font-bold outline-none">
                    <span class="absolute right-6 top-1/2 -translate-y-1/2 font-black text-[10px] text-slate-300">FCFA</span>
                </div>
                <select id="p-province" class="w-full bg-slate-50 p-5 rounded-2xl border-2 border-transparent focus:border-emerald-500 font-bold appearance-none outline-none">
                    <option value="">Sélectionnez la Province</option>
                    <option>Estuaire</option><option>Woleu-Ntem</option><option>Ogooué-Maritime</option>
                    <option>Haut-Ogooué</option><option>Ngounié</option><option>Nyanga</option>
                    <option>Ogooué-Ivindo</option><option>Ogooué-Lolo</option><option>Moyen-Ogooué</option>
                </select>
                <input type="text" id="p-image" placeholder="Lien de l'image (URL)" class="w-full bg-slate-50 p-5 rounded-2xl border-2 border-transparent focus:border-emerald-500 font-bold outline-none">
                <button onclick="submitProduct()" class="btn-primary w-full py-6 rounded-2xl font-black uppercase text-[11px] tracking-widest mt-4 shadow-2xl">Mettre en vitrine</button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = 'echoppe-241-prod';

        let userProfile = null;
        let selectedRole = 'buyer';
        let allProducts = [];
        let activeFilter = 'all';
        let deliveryMode = 'pickup';
        let currentCheckoutItem = null;

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                const uDoc = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid));
                if(uDoc.exists()) {
                    userProfile = { uid: user.uid, ...uDoc.data() };
                    updateUI();
                    syncData();
                }
            } else {
                signInAnonymously(auth);
            }
        });

        // UI HELPERS
        window.toggleAuthModal = () => document.getElementById('auth-modal').classList.toggle('hidden');
        window.togglePublishModal = () => document.getElementById('publish-modal').classList.toggle('hidden');
        
        window.setRole = (r) => {
            selectedRole = r;
            document.querySelectorAll('.role-btn').forEach(b => b.classList.remove('active', 'border-emerald-500'));
            document.getElementById(`role-${r}`).classList.add('active', 'border-emerald-500');
        };

        window.handleAuthSubmit = async () => {
            const name = document.getElementById('auth-name').value.trim();
            if(!name) return;
            await setDoc(doc(db, 'artifacts', appId, 'users', auth.currentUser.uid), {
                fullName: name,
                role: selectedRole,
                createdAt: serverTimestamp()
            });
            location.reload();
        };

        window.submitProduct = async () => {
            const name = document.getElementById('p-name').value;
            const price = parseInt(document.getElementById('p-price').value);
            const prov = document.getElementById('p-province').value;
            const img = document.getElementById('p-image').value || 'https://images.unsplash.com/photo-1542838132-92c53300491e?w=800';
            
            if(!name || !price || !prov) return showToast("Veuillez remplir les champs");

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                name, price, province: prov, image: img,
                ownerId: userProfile.uid, ownerName: userProfile.fullName,
                status: 'active', createdAt: serverTimestamp()
            });
            showToast("Produit ajouté à la vitrine !");
            togglePublishModal();
        };

        function syncData() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                allProducts = snap.docs.map(d => ({id: d.id, ...d.data()}));
                renderMarket();
                renderFilters();
            }, (err) => console.error(err));
            
            if(userProfile?.role === 'producer') {
                onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), (snap) => {
                    const orders = snap.docs.map(d => ({id: d.id, ...d.data()})).filter(o => o.sellerId === userProfile.uid);
                    let total = 0;
                    orders.forEach(o => total += o.total);
                    document.getElementById('stat-sales').innerText = `${total.toLocaleString()} FCFA`;
                });
            }
        }

        function renderFilters() {
            const provinces = ["Estuaire", "Woleu-Ntem", "Ogooué-Maritime", "Haut-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Moyen-Ogooué"];
            const container = document.getElementById('province-filters');
            const currentFilter = activeFilter;
            
            container.innerHTML = `<button onclick="setFilter('all')" class="chip-filter ${currentFilter==='all'?'active':''} px-6 py-3 rounded-full border-2 border-slate-200 text-[10px] font-black uppercase tracking-wider">Toutes</button>`;
            
            provinces.forEach(p => {
                const btn = document.createElement('button');
                btn.className = `chip-filter ${currentFilter===p?'active':''} px-6 py-3 rounded-full border-2 border-slate-200 text-[10px] font-black uppercase tracking-wider`;
                btn.innerText = p;
                btn.onclick = () => setFilter(p);
                container.appendChild(btn);
            });
        }

        function renderMarket() {
            const grid = document.getElementById('market-grid');
            grid.innerHTML = '';
            
            const filtered = allProducts.filter(p => activeFilter === 'all' || p.province === activeFilter);
            
            if(filtered.length === 0) {
                grid.innerHTML = `<div class="col-span-full py-20 text-center"><p class="text-slate-400 font-bold">Aucun produit dans cette province pour le moment.</p></div>`;
                return;
            }

            filtered.forEach(p => {
                const div = document.createElement('div');
                div.className = "product-card p-3 rounded-[2.5rem] flex flex-col cursor-pointer animate-fade";
                div.onclick = () => navigateTo('checkout', p);
                div.innerHTML = `
                    <div class="relative mb-4 overflow-hidden rounded-[2rem]">
                        <img src="${p.image}" class="h-56 w-full object-cover">
                        <div class="absolute top-4 left-4 bg-white/90 backdrop-blur px-3 py-1 rounded-full text-[8px] font-black uppercase tracking-widest text-emerald-700 shadow-sm">${p.province}</div>
                    </div>
                    <div class="px-2 pb-2">
                        <h4 class="font-black text-sm uppercase truncate mb-1">${p.name}</h4>
                        <div class="flex justify-between items-center">
                            <p class="font-black text-emerald-600 text-lg">${p.price.toLocaleString()} <span class="text-[10px]">F</span></p>
                            <span class="w-8 h-8 rounded-full bg-slate-100 flex items-center justify-center text-slate-400"><i class="fa-solid fa-cart-plus text-xs"></i></span>
                        </div>
                    </div>
                `;
                grid.appendChild(div);
            });
        }

        window.navigateTo = (v, data = null) => {
            document.getElementById('view-home').classList.toggle('hidden', v !== 'home');
            document.getElementById('hero-section').classList.toggle('hidden', v !== 'home');
            document.getElementById('view-checkout').classList.toggle('hidden', v !== 'checkout');
            
            if(v === 'checkout' && data) {
                currentCheckoutItem = data;
                document.getElementById('checkout-item-ui').innerHTML = `
                    <img src="${data.image}" class="w-20 h-20 rounded-2xl object-cover shadow-lg">
                    <div>
                        <p class="text-[9px] font-black uppercase text-emerald-600 mb-1">${data.province}</p>
                        <h5 class="font-black text-xl uppercase tracking-tighter">${data.name}</h5>
                        <p class="text-slate-400 font-bold">${data.price.toLocaleString()} FCFA</p>
                    </div>
                `;
                setDeliveryMode('pickup');
                window.scrollTo({top: 0, behavior: 'smooth'});
            }
        };

        window.setDeliveryMode = (m) => {
            deliveryMode = m;
            document.querySelectorAll('.delivery-option').forEach(el => el.classList.remove('active', 'border-emerald-500', 'bg-emerald-50'));
            document.getElementById(`opt-${m}`).classList.add('active', 'border-emerald-500', 'bg-emerald-50');
            document.getElementById('delivery-address-box').classList.toggle('hidden', m === 'pickup');
            
            const total = currentCheckoutItem.price + (m === 'delivery' ? 1500 : 0);
            document.getElementById('checkout-total').innerText = `${total.toLocaleString()} FCFA`;
        };

        window.processOrder = async () => {
            const phone = document.getElementById('pay-phone').value;
            if(!phone) return showToast("Numéro Mobile Money requis");
            
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), {
                productName: currentCheckoutItem.name,
                sellerId: currentCheckoutItem.ownerId,
                total: currentCheckoutItem.price + (deliveryMode === 'delivery' ? 1500 : 0),
                buyerPhone: phone,
                status: 'paid',
                createdAt: serverTimestamp()
            });
            
            showToast("Commande confirmée ! Merci.");
            setTimeout(() => navigateTo('home'), 1500);
        };

        function updateUI() {
            document.getElementById('btn-login-trigger').classList.toggle('hidden', !!userProfile);
            document.getElementById('user-profile').classList.toggle('hidden', !userProfile);
            if(userProfile) {
                document.getElementById('user-initials').innerText = userProfile.fullName[0];
                document.getElementById('view-producer').classList.toggle('hidden', userProfile.role !== 'producer');
            }
        }

        function showToast(m) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = m;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        window.handleLogout = () => signOut(auth).then(() => location.reload());
        window.setFilter = (f) => { 
            activeFilter = f; 
            renderMarket(); 
            renderFilters();
        };

    </script>
</body>
</html>
