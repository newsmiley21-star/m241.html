<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 | Plateforme Nationale du Made in Gabon</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --gab-green: #00853f;
            --gab-yellow: #fcd116;
            --gab-blue: #3a75c4;
        }
        body { font-family: 'Inter', sans-serif; background-color: #f3f4f6; }
        
        .bg-gab-green { background-color: var(--gab-green); }
        .text-gab-green { color: var(--gab-green); }
        .bg-gab-yellow { background-color: var(--gab-yellow); }
        .bg-gab-blue { background-color: var(--gab-blue); }
        .border-gab-blue { border-color: var(--gab-blue); }

        .chat-window { height: 400px; display: flex; flex-direction: column; }
        .message { max-width: 80%; padding: 10px 15px; border-radius: 15px; margin-bottom: 8px; font-size: 13px; }
        .msg-admin { background: #e2e8f0; align-self: flex-start; }
        .msg-user { background: var(--gab-blue); color: white; align-self: flex-end; }
        .msg-prod { background: var(--gab-green); color: white; align-self: flex-start; }

        .nav-tab.active { border-bottom: 3px solid var(--gab-yellow); color: var(--gab-yellow); }
        
        /* Modal Transitions */
        .modal-overlay { transition: opacity 0.3s ease; }
        .modal-content { transition: transform 0.3s ease; }
    </style>
</head>
<body class="antialiased">

    <!-- Header National -->
    <header class="bg-gab-blue text-white shadow-lg sticky top-0 z-50">
        <div class="h-1.5 flex">
            <div class="flex-1 bg-gab-green"></div>
            <div class="flex-1 bg-gab-yellow"></div>
            <div class="flex-1 bg-gab-blue"></div>
        </div>
        <div class="max-w-7xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="location.reload()">
                <div class="bg-white p-1 rounded-lg">
                    <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Logo" class="h-10">
                </div>
                <span class="font-black text-xl tracking-tighter">ECHOPPE<span class="text-gab-yellow">241</span></span>
            </div>

            <div class="hidden md:flex gap-6 text-xs font-bold uppercase tracking-widest">
                <button onclick="showSection('marketplace')" class="hover:text-gab-yellow transition">Marché</button>
                <button onclick="showSection('publish')" class="hover:text-gab-yellow transition">Publier Produit</button>
                <button onclick="toggleChat()" class="hover:text-gab-yellow transition"><i class="fa-solid fa-comments mr-1"></i> Messagerie</button>
            </div>

            <div class="flex items-center gap-4">
                <button onclick="toggleAuth()" class="bg-white text-gab-blue px-4 py-2 rounded-full text-xs font-bold hover:bg-gab-yellow transition">
                    <i class="fa-solid fa-user-circle mr-2"></i>Connexion
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="max-w-7xl mx-auto p-4 md:p-8">
        
        <!-- SECTION: MARKETPLACE -->
        <section id="section-marketplace" class="block">
            <div class="flex justify-between items-end mb-8">
                <div>
                    <h1 class="text-3xl font-black text-slate-800">Le Terroir Gabonais</h1>
                    <p class="text-slate-500">Directement des 9 provinces vers votre table.</p>
                </div>
                <div class="flex gap-2">
                    <span class="bg-gab-green/10 text-gab-green px-3 py-1 rounded-full text-[10px] font-bold">Estuaire</span>
                    <span class="bg-gab-blue/10 text-gab-blue px-3 py-1 rounded-full text-[10px] font-bold">Woleu-Ntem</span>
                </div>
            </div>

            <div id="productGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                <!-- Produits injectés ici -->
            </div>
        </section>

        <!-- SECTION: PUBLIER UN PRODUIT (Producteurs uniquement) -->
        <section id="section-publish" class="hidden max-w-2xl mx-auto bg-white rounded-3xl shadow-xl overflow-hidden border border-slate-100">
            <div class="bg-gab-green p-6 text-white">
                <h2 class="text-xl font-black uppercase tracking-tight">Espace Producteur : Publier une offre</h2>
                <p class="text-xs opacity-80">Remplissez les détails pour soumettre votre produit à l'administration.</p>
            </div>
            <form id="productForm" class="p-8 space-y-6">
                <div class="grid grid-cols-2 gap-4">
                    <div class="col-span-2">
                        <label class="block text-[10px] font-black text-slate-400 uppercase mb-2">Nom du produit</label>
                        <input type="text" placeholder="ex: Manioc d'Oyem 50kg" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-green transition" required>
                    </div>
                    <div>
                        <label class="block text-[10px] font-black text-slate-400 uppercase mb-2">Prix (FCFA)</label>
                        <input type="number" placeholder="15000" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-green transition" required>
                    </div>
                    <div>
                        <label class="block text-[10px] font-black text-slate-400 uppercase mb-2">Province d'origine</label>
                        <select class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-green transition">
                            <option>Estuaire</option>
                            <option>Haut-Ogooué</option>
                            <option>Moyen-Ogooué</option>
                            <option>Ngounié</option>
                            <option>Nyanga</option>
                            <option>Ogooué-Ivindo</option>
                            <option>Ogooué-Lolo</option>
                            <option>Ogooué-Maritime</option>
                            <option>Woleu-Ntem</option>
                        </select>
                    </div>
                </div>

                <div>
                    <label class="block text-[10px] font-black text-slate-400 uppercase mb-2">Photos du produit (Minimum 1)</label>
                    <div class="border-2 border-dashed border-slate-200 rounded-3xl p-8 text-center hover:border-gab-green transition cursor-pointer group" onclick="document.getElementById('photoInput').click()">
                        <i class="fa-solid fa-cloud-arrow-up text-4xl text-slate-300 group-hover:text-gab-green mb-3"></i>
                        <p class="text-xs text-slate-500 font-medium">Cliquez pour ajouter des photos ou glissez-déposez</p>
                        <input type="file" id="photoInput" class="hidden" multiple accept="image/*">
                    </div>
                    <div id="photoPreview" class="flex gap-2 mt-4 overflow-x-auto py-2"></div>
                </div>

                <button type="submit" class="w-full bg-gab-green text-white py-5 rounded-2xl font-black uppercase tracking-widest hover:shadow-lg transition transform active:scale-95">Soumettre pour Validation</button>
            </form>
        </section>

    </main>

    <!-- CHAT FLOATING WINDOW -->
    <div id="chatBox" class="fixed bottom-6 right-6 w-80 md:w-96 bg-white rounded-3xl shadow-2xl border border-slate-100 z-[100] hidden overflow-hidden flex flex-col">
        <div class="bg-gab-blue p-4 text-white flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="w-2 h-2 bg-green-400 rounded-full animate-pulse"></div>
                <span class="font-bold text-sm">Assistance & Producteurs</span>
            </div>
            <button onclick="toggleChat()" class="text-2xl hover:text-gab-yellow transition">&times;</button>
        </div>
        <div id="chatMessages" class="chat-window p-4 overflow-y-auto bg-slate-50">
            <div class="message msg-admin">Bienvenue sur Echoppe241 ! Comment pouvons-nous vous aider aujourd'hui ?</div>
            <div class="message msg-prod">Bonjour, je suis Producteur à Lastourville, j'ai une cargaison d'huile prête.</div>
        </div>
        <div class="p-4 border-t flex gap-2">
            <input type="text" id="chatInput" placeholder="Votre message..." class="flex-grow bg-slate-100 border-none rounded-full px-4 py-2 text-sm outline-none focus:ring-2 focus:ring-gab-blue">
            <button onclick="sendMessage()" class="bg-gab-blue text-white w-10 h-10 rounded-full flex items-center justify-center hover:scale-105 transition">
                <i class="fa-solid fa-paper-plane"></i>
            </button>
        </div>
    </div>

    <!-- AUTH MODAL -->
    <div id="authModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-[200] hidden flex items-center justify-center p-4 modal-overlay">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] shadow-2xl overflow-hidden modal-content">
            <div class="p-8">
                <div class="flex justify-between items-center mb-8">
                    <h2 class="text-2xl font-black text-slate-800 tracking-tighter uppercase">Portail Echoppe241</h2>
                    <button onclick="toggleAuth()" class="text-3xl text-slate-300 hover:text-red-500">&times;</button>
                </div>
                
                <div class="flex gap-4 mb-8 bg-slate-100 p-1 rounded-2xl">
                    <button id="tab-login" onclick="switchAuth('login')" class="flex-1 py-3 rounded-xl text-xs font-black uppercase transition bg-white shadow-sm text-gab-blue">Connexion</button>
                    <button id="tab-signup" onclick="switchAuth('signup')" class="flex-1 py-3 rounded-xl text-xs font-black uppercase transition text-slate-500">S'inscrire</button>
                </div>

                <form id="authForm" class="space-y-4">
                    <div id="signup-fields" class="hidden space-y-4">
                        <select class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue font-bold text-sm">
                            <option value="client">Je suis un Client</option>
                            <option value="prod">Je suis un Producteur</option>
                            <option value="admin">Administration (Restreint)</option>
                        </select>
                        <input type="text" placeholder="Nom complet / Entreprise" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue">
                    </div>
                    <input type="email" placeholder="Adresse Email" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue" required>
                    <input type="password" placeholder="Mot de passe" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue" required>
                    
                    <button type="submit" class="w-full bg-gab-blue text-white py-5 rounded-2xl font-black uppercase tracking-widest hover:bg-blue-700 transition shadow-lg mt-4">Accéder au Dashboard</button>
                </form>
            </div>
            <div class="bg-gab-yellow p-4 text-center">
                <p class="text-[10px] font-black uppercase text-gab-blue">Partenaire officiel de la transformation locale</p>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, collection, addDoc, query, onSnapshot, serverTimestamp, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

        // Firebase Config (Simulation pour l'environnement de démo)
        const firebaseConfig = {
            apiKey: "",
            authDomain: "communautedugabon.firebaseapp.com",
            projectId: "communautedugabon",
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        const appId = "echoppe241-full-app";

        // Global State
        let currentUser = null;

        // Init Auth
        const init = async () => {
            try {
                await signInAnonymously(auth);
                onAuthStateChanged(auth, (user) => {
                    currentUser = user;
                    loadMessages();
                });
            } catch (e) { console.error("Firebase init failed"); }
        };
        init();

        // 💬 CHAT SYSTEM
        window.sendMessage = async () => {
            const input = document.getElementById('chatInput');
            if (!input.value.trim() || !currentUser) return;

            const msg = {
                text: input.value,
                uid: currentUser.uid,
                role: 'user', // Simulé ici, en production tiré du profil utilisateur
                timestamp: serverTimestamp()
            };

            input.value = '';
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'messages'), msg);
        };

        function loadMessages() {
            if (!currentUser) return;
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'messages'), orderBy('timestamp', 'asc'));
            
            onSnapshot(q, (snapshot) => {
                const container = document.getElementById('chatMessages');
                container.innerHTML = '';
                snapshot.forEach(doc => {
                    const m = doc.data();
                    const div = document.createElement('div');
                    const isMe = m.uid === currentUser.uid;
                    div.className = `message ${isMe ? 'msg-user' : (m.role === 'admin' ? 'msg-admin' : 'msg-prod')}`;
                    div.innerText = m.text;
                    container.appendChild(div);
                });
                container.scrollTop = container.scrollHeight;
            }, (error) => console.log("Chat error:", error));
        }

        // 📦 PRODUCT SUBMISSION (Formulaire enrichi)
        document.getElementById('productForm').onsubmit = async (e) => {
            e.preventDefault();
            const btn = e.target.querySelector('button');
            btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin mr-2"></i> Enregistrement...`;
            btn.disabled = true;

            // Simulation d'envoi vers Firestore
            setTimeout(() => {
                alert("Produit envoyé avec succès ! Il apparaîtra après validation administrative.");
                btn.innerHTML = `Soumettre pour Validation`;
                btn.disabled = false;
                e.target.reset();
                document.getElementById('photoPreview').innerHTML = '';
                showSection('marketplace');
            }, 1500);
        };

        // 📸 Photo Preview
        document.getElementById('photoInput').addEventListener('change', function(e) {
            const preview = document.getElementById('photoPreview');
            preview.innerHTML = '';
            Array.from(this.files).forEach(file => {
                const reader = new FileReader();
                reader.onload = function(ex) {
                    const img = document.createElement('img');
                    img.src = ex.target.result;
                    img.className = "h-20 w-20 object-cover rounded-xl border-2 border-gab-green";
                    preview.appendChild(img);
                };
                reader.readAsDataURL(file);
            });
        });

    </script>

    <script>
        // UI Helpers
        function showSection(id) {
            document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`section-${id}`).classList.remove('hidden');
            window.scrollTo(0, 0);
        }

        function toggleChat() {
            const chat = document.getElementById('chatBox');
            chat.classList.toggle('hidden');
        }

        function toggleAuth() {
            const modal = document.getElementById('authModal');
            modal.classList.toggle('hidden');
        }

        function switchAuth(type) {
            const signupFields = document.getElementById('signup-fields');
            const tabLogin = document.getElementById('tab-login');
            const tabSignup = document.getElementById('tab-signup');

            if(type === 'signup') {
                signupFields.classList.remove('hidden');
                tabSignup.classList.add('bg-white', 'shadow-sm', 'text-gab-blue');
                tabSignup.classList.remove('text-slate-500');
                tabLogin.classList.remove('bg-white', 'shadow-sm', 'text-gab-blue');
                tabLogin.classList.add('text-slate-500');
            } else {
                signupFields.classList.add('hidden');
                tabLogin.classList.add('bg-white', 'shadow-sm', 'text-gab-blue');
                tabLogin.classList.remove('text-slate-500');
                tabSignup.classList.remove('bg-white', 'shadow-sm', 'text-gab-blue');
                tabSignup.classList.add('text-slate-500');
            }
        }

        // Mock Products
        const products = [
            { id: 1, name: "Huile de Palme Rouge", price: 18000, province: "Ogooué-Lolo", img: "https://images.unsplash.com/photo-1620916566398-39f1143f2c0a?auto=format&fit=crop&w=400&q=80" },
            { id: 2, name: "Sac de Manioc Sec", price: 12500, province: "Woleu-Ntem", img: "https://images.unsplash.com/photo-1590779033100-9f60705a013d?auto=format&fit=crop&w=400&q=80" },
            { id: 3, name: "Piment de Mouila", price: 5000, province: "Ngounié", img: "https://images.unsplash.com/photo-1588252303782-cb80119abd6d?auto=format&fit=crop&w=400&q=80" },
            { id: 4, name: "Miel Naturel Forêt", price: 8500, province: "Ogooué-Ivindo", img: "https://images.unsplash.com/photo-1587049352846-4a222e784d38?auto=format&fit=crop&w=400&q=80" }
        ];

        function loadProducts() {
            const grid = document.getElementById('productGrid');
            grid.innerHTML = products.map(p => `
                <div class="bg-white rounded-[2rem] overflow-hidden shadow-sm border border-slate-100 hover:shadow-xl transition group">
                    <div class="h-48 overflow-hidden relative">
                        <img src="${p.img}" class="w-full h-full object-cover group-hover:scale-110 transition duration-500">
                        <div class="absolute top-4 left-4 bg-gab-yellow text-gab-blue text-[10px] font-black px-3 py-1 rounded-full uppercase shadow-sm">
                            ${p.province}
                        </div>
                    </div>
                    <div class="p-6">
                        <h3 class="font-bold text-slate-800 mb-2">${p.name}</h3>
                        <p class="text-gab-blue font-black text-xl mb-4">${p.price.toLocaleString()} FCFA</p>
                        <button class="w-full bg-slate-100 text-slate-600 py-3 rounded-xl text-xs font-black uppercase hover:bg-gab-blue hover:text-white transition">Commander</button>
                    </div>
                </div>
            `).join('');
        }

        window.onload = loadProducts;
    </script>
</body>
</html>
