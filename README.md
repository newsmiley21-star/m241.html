<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 | Profils Vérifiés & Marketplace</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    
    <style>
        :root { --gab-green: #00853f; --gab-yellow: #fcd116; --gab-blue: #3a75c4; }
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .bg-gab-green { background-color: var(--gab-green); }
        .bg-gab-yellow { background-color: var(--gab-yellow); }
        .bg-gab-blue { background-color: var(--gab-blue); }
        .text-gab-blue { color: var(--gab-blue); }
        .animate-fade-in { animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        #receipt-template { position: absolute; left: -9999px; top: 0; width: 400px; background: white; padding: 20px; border: 2px solid #3a75c4; border-radius: 20px; }
    </style>
</head>
<body class="antialiased">

    <div class="h-1 flex sticky top-0 z-[60]">
        <div class="flex-1 bg-gab-green"></div>
        <div class="flex-1 bg-gab-yellow"></div>
        <div class="flex-1 bg-gab-blue"></div>
    </div>

    <!-- Navigation -->
    <header class="bg-white/90 backdrop-blur-md shadow-sm sticky top-1 z-50 border-b border-slate-100">
        <div class="max-w-7xl mx-auto px-4 py-2 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="showSection('marketplace')">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Echoppe241 Logo" class="h-10 w-auto object-contain">
                <div class="flex flex-col leading-none">
                    <span class="font-black text-lg tracking-tighter text-slate-800">ECHOPPE<span class="text-gab-blue">241</span></span>
                    <span class="text-[8px] font-bold text-gab-green uppercase tracking-widest">Producteurs du Gabon</span>
                </div>
            </div>

            <div class="flex items-center gap-3">
                <button onclick="checkAuthAndShow('publish')" class="hidden md:block bg-gab-green text-white px-4 py-2 rounded-full text-xs font-bold hover:opacity-90">Vendre</button>
                <button onclick="toggleCart()" class="relative p-2 text-slate-600">
                    <i class="fa-solid fa-cart-shopping text-xl"></i>
                    <span id="cartCount" class="absolute -top-1 -right-1 bg-red-500 text-white text-[9px] font-bold w-5 h-5 flex items-center justify-center rounded-full border-2 border-white">0</span>
                </button>
                <button id="authBtn" onclick="handleAuthClick()" class="bg-slate-100 text-slate-700 p-2 rounded-full text-xs font-bold flex items-center gap-2">
                    <i class="fa-solid fa-user"></i>
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-4">
        <!-- SECTION: MARKETPLACE -->
        <section id="section-marketplace" class="space-y-8 animate-fade-in">
            <div id="productGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 py-6"></div>
        </section>

        <!-- SECTION: PROFIL UTILISATEUR (MODIFICATION) -->
        <section id="section-profile" class="hidden max-w-2xl mx-auto py-10 animate-fade-in">
            <div class="bg-white rounded-[2.5rem] shadow-xl border border-slate-100 overflow-hidden">
                <div class="bg-gab-blue p-6 text-white text-center">
                    <h2 class="text-xl font-black uppercase italic">Paramètres du Compte</h2>
                </div>
                <form id="profileForm" class="p-8 space-y-6">
                    <div class="flex flex-col items-center gap-4">
                        <div class="relative group">
                            <img id="profilePreview" src="https://ui-avatars.com/api/?name=User&background=random" class="w-32 h-32 rounded-full object-cover border-4 border-gab-yellow shadow-lg">
                            <label class="absolute bottom-0 right-0 bg-white p-2 rounded-full shadow-md cursor-pointer">
                                <i class="fa-solid fa-camera text-gab-blue"></i>
                                <input type="file" id="profileImgInput" accept="image/*" class="hidden">
                            </label>
                        </div>
                        <p class="text-[10px] font-bold text-slate-400 uppercase">Photo de profil</p>
                    </div>

                    <div class="space-y-4">
                        <input type="text" id="profName" placeholder="Nom complet / Nom d'entreprise" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none" required>
                        <textarea id="profBio" placeholder="Bio: Parlez de votre savoir-faire..." class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none h-24"></textarea>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <select id="profProvince" class="bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none">
                                <option value="">Choisir la province</option>
                                <option value="Estuaire">Estuaire</option>
                                <option value="Haut-Ogooué">Haut-Ogooué</option>
                                <option value="Moyen-Ogooué">Moyen-Ogooué</option>
                                <option value="Ngounié">Ngounié</option>
                                <option value="Nyanga">Nyanga</option>
                                <option value="Ogooué-Ivindo">Ogooué-Ivindo</option>
                                <option value="Ogooué-Lolo">Ogooué-Lolo</option>
                                <option value="Ogooué-Maritime">Ogooué-Maritime</option>
                                <option value="Woleu-Ntem">Woleu-Ntem</option>
                            </select>
                            <input type="tel" id="profPhone" placeholder="Numéro WhatsApp (Privé)" class="bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none" required>
                        </div>

                        <div class="p-4 border-2 border-dashed border-slate-200 rounded-2xl bg-slate-50">
                            <p class="text-[10px] font-black text-slate-400 uppercase mb-2">Pièce d'identité (Visible Admin uniquement)</p>
                            <label class="flex items-center gap-4 cursor-pointer">
                                <div class="bg-white p-3 rounded-xl border"><i class="fa-solid fa-id-card text-slate-400"></i></div>
                                <span id="idFileName" class="text-xs text-slate-500 font-bold">Télécharger Recto/Verso</span>
                                <input type="file" id="idCardInput" accept="image/*" class="hidden">
                            </label>
                        </div>
                    </div>

                    <button type="submit" id="saveProfileBtn" class="w-full bg-gab-blue text-white py-5 rounded-2xl font-black uppercase tracking-widest shadow-lg">Mettre à jour le profil</button>
                    <button type="button" onclick="signOutUser()" class="w-full text-red-500 font-bold text-xs uppercase py-2">Déconnexion</button>
                </form>
            </div>
        </section>

        <!-- SECTION: PUBLIER -->
        <section id="section-publish" class="hidden max-w-xl mx-auto py-10 animate-fade-in">
            <div class="bg-white rounded-[2.5rem] shadow-xl border border-slate-100 overflow-hidden">
                <div class="bg-gab-green p-6 text-white text-center">
                    <h2 class="text-xl font-black uppercase">Vendre un article</h2>
                </div>
                <form id="productForm" class="p-8 space-y-4">
                    <input type="text" id="pName" placeholder="Nom du produit" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none" required>
                    <input type="number" id="pPrice" placeholder="Prix (FCFA)" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none" required>
                    
                    <div class="space-y-2">
                        <label class="text-[10px] font-black text-slate-400 uppercase">Photo de l'article</label>
                        <label class="flex flex-col items-center justify-center w-full h-32 border-2 border-slate-200 border-dashed rounded-2xl cursor-pointer bg-slate-50 hover:bg-slate-100">
                            <i class="fa-solid fa-camera text-2xl text-slate-400 mb-2"></i>
                            <input type="file" id="pImgFile" accept="image/*" capture="environment" class="hidden" />
                        </label>
                        <img id="previewImg" class="hidden w-full h-32 object-cover rounded-xl mt-2 border">
                    </div>

                    <button type="submit" id="publishBtn" class="w-full bg-gab-green text-white py-5 rounded-2xl font-black uppercase tracking-widest shadow-lg">Mettre en ligne</button>
                </form>
            </div>
        </section>
    </main>

    <!-- SIDE CART -->
    <div id="cartSidebar" class="fixed inset-y-0 right-0 w-full md:w-96 bg-white shadow-2xl z-[100] transform translate-x-full transition-transform duration-300 flex flex-col">
        <div class="p-6 border-b flex justify-between items-center bg-slate-50">
            <h3 class="font-black text-xl italic text-gab-blue">Panier</h3>
            <button onclick="toggleCart()" class="text-2xl">&times;</button>
        </div>
        <div id="cartItems" class="flex-grow overflow-y-auto p-6 space-y-4"></div>
        <div class="p-6 border-t bg-slate-50 space-y-3">
            <div class="flex justify-between font-black text-xl">
                <span class="text-slate-400 uppercase text-xs self-center">Total</span>
                <span id="cartTotal" class="text-gab-blue">0 FCFA</span>
            </div>
            <button onclick="generateReceiptAndSend()" id="checkoutBtn" class="w-full bg-gab-blue text-white py-5 rounded-2xl font-black uppercase tracking-widest flex items-center justify-center gap-3">
                <i class="fa-brands fa-whatsapp text-xl"></i> Confirmer
            </button>
        </div>
    </div>

    <!-- REÇU TEMPLATE (HIDDEN) -->
    <div id="receipt-template">
        <div class="text-center mb-4">
            <h1 class="font-black text-xl">ECHOPPE<span class="text-blue-600">241</span></h1>
            <p class="text-[10px] uppercase font-bold text-gab-green">Reçu de Commande</p>
        </div>
        <div class="border-y-2 border-dashed border-slate-200 py-4 my-4">
            <p id="r-order-id" class="font-black text-slate-800 uppercase text-center"></p>
            <div id="r-items" class="mt-4 space-y-2"></div>
        </div>
        <div class="flex justify-between items-center">
            <div id="r-total" class="font-black text-2xl text-gab-blue"></div>
            <div id="qrcode"></div>
        </div>
    </div>

    <!-- MODAL AUTH -->
    <div id="authModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-[200] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 animate-fade-in">
            <h2 id="authTitle" class="text-xl font-black text-slate-800 uppercase mb-6 italic">Accès Membre</h2>
            <form id="authForm" class="space-y-4">
                <div id="displayNameGroup" class="hidden">
                    <input type="text" id="authName" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none" placeholder="Nom complet">
                </div>
                <input type="email" id="authEmail" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none" placeholder="Email" required>
                <input type="password" id="authPassword" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none" placeholder="Mot de passe" required>
                <button type="submit" class="w-full bg-gab-blue text-white py-4 rounded-2xl font-black uppercase">Valider</button>
            </form>
            <button id="authSwitch" class="w-full mt-4 text-xs font-bold text-slate-500">Créer un compte</button>
        </div>
    </div>

    <div id="notif" class="fixed top-20 left-1/2 -translate-x-1/2 z-[300] bg-slate-800 text-white px-6 py-3 rounded-full text-[10px] font-black uppercase hidden"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut, updateProfile } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, query, onSnapshot, orderBy, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = "echoppe241-v3-profiles";
        const ADMIN_WHATSAPP = "24177736065";

        let currentUser = null;
        let cart = [];
        let tempBase64 = { profile: "", idCard: "", product: "" };

        // INITIALISATION AUTH
        onAuthStateChanged(auth, async (user) => {
            currentUser = user;
            if (user) {
                const docSnap = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid, 'profile', 'info'));
                if (docSnap.exists()) {
                    const data = docSnap.data();
                    document.getElementById('profilePreview').src = data.photo || `https://ui-avatars.com/api/?name=${user.displayName}`;
                    document.getElementById('profName').value = data.name || user.displayName || "";
                    document.getElementById('profBio').value = data.bio || "";
                    document.getElementById('profProvince').value = data.province || "";
                    document.getElementById('profPhone').value = data.phone || "";
                }
                closeAuthModal();
            }
            updateAuthBtn();
        });

        function updateAuthBtn() {
            const btn = document.getElementById('authBtn');
            btn.innerHTML = currentUser ? `<img src="${document.getElementById('profilePreview').src}" class="w-6 h-6 rounded-full object-cover"> Profil` : `Connexion`;
        }

        window.handleAuthClick = () => { if (!currentUser) toggleAuthModal(); else showSection('profile'); };

        // CONVERSION IMAGE
        function handleImageUpload(inputId, previewId, key) {
            document.getElementById(inputId).addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = (ev) => {
                        tempBase64[key] = ev.target.result;
                        if (previewId) {
                            const prev = document.getElementById(previewId);
                            prev.src = ev.target.result;
                            prev.classList.remove('hidden');
                        }
                        if (inputId === 'idCardInput') document.getElementById('idFileName').innerText = file.name;
                    };
                    reader.readAsDataURL(file);
                }
            });
        }
        handleImageUpload('profileImgInput', 'profilePreview', 'profile');
        handleImageUpload('idCardInput', null, 'idCard');
        handleImageUpload('pImgFile', 'previewImg', 'product');

        // MISE À JOUR PROFIL (PRIVÉ / PUBLIC)
        document.getElementById('profileForm').onsubmit = async (e) => {
            e.preventDefault();
            const btn = document.getElementById('saveProfileBtn');
            btn.disabled = true; btn.innerText = "Enregistrement...";

            try {
                const profileData = {
                    name: document.getElementById('profName').value,
                    bio: document.getElementById('profBio').value,
                    province: document.getElementById('profProvince').value,
                    phone: document.getElementById('profPhone').value,
                    photo: tempBase64.profile || document.getElementById('profilePreview').src,
                    idCard: tempBase64.idCard || "", // Visible seulement par Admin et User
                    updatedAt: serverTimestamp()
                };

                // Sauvegarde dans le document privé de l'utilisateur
                await setDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, 'profile', 'info'), profileData);
                
                // Sauvegarde d'une version publique (sans ID Card ni téléphone complet si besoin)
                await setDoc(doc(db, 'artifacts', appId, 'public', 'sellers', currentUser.uid), {
                    name: profileData.name,
                    bio: profileData.bio,
                    province: profileData.province,
                    photo: profileData.photo
                });

                notify("Profil mis à jour !");
                updateAuthBtn();
            } catch (err) { notify("Erreur de sauvegarde"); }
            finally { btn.disabled = false; btn.innerText = "Mettre à jour le profil"; }
        };

        // VENDRE PRODUIT
        document.getElementById('productForm').onsubmit = async (e) => {
            e.preventDefault();
            if (!currentUser) return toggleAuthModal();
            if (!tempBase64.product) return notify("Photo de l'article requise");

            const btn = document.getElementById('publishBtn');
            btn.disabled = true;
            
            const prodId = "241-" + Math.random().toString(36).substring(2, 6).toUpperCase();

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'products'), {
                    prodId: prodId,
                    name: document.getElementById('pName').value,
                    price: parseInt(document.getElementById('pPrice').value),
                    img: tempBase64.product,
                    sellerId: currentUser.uid,
                    createdAt: serverTimestamp()
                });
                notify("Article publié ! ID: " + prodId);
                document.getElementById('productForm').reset();
                document.getElementById('previewImg').classList.add('hidden');
                tempBase64.product = "";
                showSection('marketplace');
            } catch (err) { notify("Erreur Firebase"); }
            finally { btn.disabled = false; }
        };

        // MARKETPLACE
        onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'products'), orderBy('createdAt', 'desc')), (snap) => {
            const grid = document.getElementById('productGrid');
            grid.innerHTML = '';
            snap.forEach(async dDoc => {
                const p = dDoc.data();
                grid.innerHTML += `
                    <div class="bg-white rounded-[2rem] overflow-hidden shadow-sm border border-slate-100 p-4 animate-fade-in group">
                        <div class="relative mb-3 overflow-hidden rounded-2xl h-44">
                            <img src="${p.img}" class="w-full h-full object-cover group-hover:scale-105 transition duration-500">
                            <span class="absolute top-2 left-2 bg-gab-yellow text-gab-blue text-[8px] font-black px-2 py-1 rounded-full uppercase">${p.prodId}</span>
                        </div>
                        <h3 class="font-black text-slate-800 truncate text-sm uppercase">${p.name}</h3>
                        <p class="text-gab-blue font-black text-lg mb-4">${p.price.toLocaleString()} FCFA</p>
                        <button onclick="window.addToCart('${p.prodId}', '${p.name.replace(/'/g, "\\'")}', ${p.price})" class="w-full bg-slate-900 text-white py-3 rounded-xl text-[10px] font-black uppercase tracking-widest active:scale-95 transition">Acheter</button>
                    </div>`;
            });
        });

        window.addToCart = (id, name, price) => { cart.push({ id, name, price }); updateCartUI(); notify("Ajouté !"); };
        function updateCartUI() {
            document.getElementById('cartCount').innerText = cart.length;
            let total = 0;
            document.getElementById('cartItems').innerHTML = cart.map((item, idx) => {
                total += item.price;
                return `<div class="flex justify-between items-center bg-slate-50 p-4 rounded-2xl border border-slate-100 animate-fade-in">
                    <div><p class="text-[8px] font-black text-slate-400">#${item.id}</p><p class="text-xs font-bold">${item.name}</p></div>
                    <button onclick="window.removeFromCart(${idx})" class="text-red-400"><i class="fa-solid fa-trash"></i></button>
                </div>`;
            }).join('');
            document.getElementById('cartTotal').innerText = `${total.toLocaleString()} FCFA`;
        }
        window.removeFromCart = (idx) => { cart.splice(idx,1); updateCartUI(); };

        window.generateReceiptAndSend = async () => {
            if (cart.length === 0) return notify("Panier vide");
            const orderId = "CMD-" + Date.now().toString().slice(-6);
            const total = cart.reduce((a,b) => a + b.price, 0);

            document.getElementById('r-order-id').innerText = orderId;
            document.getElementById('r-total').innerText = total.toLocaleString() + " FCFA";
            document.getElementById('r-items').innerHTML = cart.map(i => `<div class="flex justify-between text-xs font-bold uppercase"><span>${i.name}</span><span>${i.price}</span></div>`).join('');

            const qrContainer = document.getElementById('qrcode');
            qrContainer.innerHTML = '';
            new QRCode(qrContainer, { text: orderId, width: 60, height: 60 });

            setTimeout(async () => {
                const msg = `🧾 *BON DE COMMANDE ECHOPPE241*%0A------------------------%0A*ID:* ${orderId}%0A*CLIENT:* ${currentUser?.displayName || 'Client'}%0A%0A*PANIER:*%0A${cart.map(i => `- ${i.name} (${i.id})`).join('%0A')}%0A%0A*TOTAL:* ${total.toLocaleString()} FCFA%0A------------------------%0A_Avez-vous bien reçu mon QR code ?_`;
                window.open(`https://wa.me/${ADMIN_WHATSAPP}?text=${msg}`, '_blank');
                cart = []; updateCartUI(); toggleCart();
            }, 500);
        };

        // UI HELPERS
        window.showSection = (id) => {
            document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`section-${id}`).classList.remove('hidden');
            window.scrollTo(0,0);
        };
        window.toggleAuthModal = () => document.getElementById('authModal').classList.toggle('hidden');
        window.closeAuthModal = () => document.getElementById('authModal').classList.add('hidden');
        window.toggleCart = () => document.getElementById('cartSidebar').classList.toggle('translate-x-full');
        window.notify = (msg) => {
            const el = document.getElementById('notif'); el.innerText = msg; el.classList.remove('hidden');
            setTimeout(() => el.classList.add('hidden'), 3000);
        };
        window.signOutUser = () => signOut(auth).then(() => location.reload());

        document.getElementById('authSwitch').onclick = () => {
            const group = document.getElementById('displayNameGroup');
            group.classList.toggle('hidden');
            document.getElementById('authTitle').innerText = group.classList.contains('hidden') ? "Accès Membre" : "Rejoindre l'Echoppe";
            document.getElementById('authSwitch').innerText = group.classList.contains('hidden') ? "Créer un compte" : "Déjà membre ?";
        };

        document.getElementById('authForm').onsubmit = async (e) => {
            e.preventDefault();
            const email = document.getElementById('authEmail').value;
            const pass = document.getElementById('authPassword').value;
            const name = document.getElementById('authName').value;
            try {
                if (!document.getElementById('displayNameGroup').classList.contains('hidden')) {
                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    await updateProfile(res.user, { displayName: name });
                } else { await signInWithEmailAndPassword(auth, email, pass); }
            } catch (err) { notify("Accès refusé"); }
        };

        window.checkAuthAndShow = (id) => { if (!currentUser) toggleAuthModal(); else showSection(id); };
    </script>
</body>
</html>
