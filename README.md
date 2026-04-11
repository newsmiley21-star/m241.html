<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>tchat+241 - Communauté Gabon</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Segoe+UI:wght@400;600;700&display=swap');
        
        :root {
            --fb-blue: #1877F2;
            --fb-bg: #F0F2F5;
            --fb-white: #FFFFFF;
            --fb-text: #1C1E21;
            --fb-gray: #65676B;
        }

        .dark {
            --fb-bg: #18191A;
            --fb-white: #242526;
            --fb-text: #E4E6EB;
            --fb-gray: #B0B3B8;
        }

        body {
            font-family: 'Segoe UI', Helvetica, Arial, sans-serif;
            background-color: var(--fb-bg);
            color: var(--fb-text);
            transition: background-color 0.3s ease;
            overflow-x: hidden;
        }

        .fb-card {
            background-color: var(--fb-white);
            border-radius: 8px;
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
        }

        .login-input {
            border: 1px solid #dddfe2;
            border-radius: 6px;
            padding: 14px 16px;
            font-size: 17px;
            width: 100%;
            outline-color: var(--fb-blue);
        }

        .btn-green {
            background-color: #42b72a;
            color: white;
            font-weight: bold;
            transition: filter 0.2s;
        }

        .btn-green:hover { filter: brightness(0.9); }

        #auth-container, #app-container { display: none; }

        .loading-dots:after {
            content: '.';
            animation: dots 1.5s steps(5, end) infinite;
        }

        @keyframes dots {
            0%, 20% { content: '.'; }
            40% { content: '..'; }
            60% { content: '...'; }
            80%, 100% { content: ''; }
        }

        .post-enter {
            animation: slideUp 0.4s ease-out;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="transition-colors duration-200">

    <!-- Splash Screen -->
    <div id="splash-screen" class="fixed inset-0 z-[1000] bg-white dark:bg-[#18191A] flex flex-col items-center justify-center">
        <div class="w-20 h-20 bg-[#1877F2] rounded-3xl flex items-center justify-center mb-6 shadow-xl animate-bounce">
            <span class="text-white font-bold text-3xl italic">241</span>
        </div>
        <div class="text-gray-400 font-medium">Connexion sécurisée<span class="loading-dots"></span></div>
    </div>

    <!-- Écran d'Authentification -->
    <div id="auth-container" class="min-h-screen flex flex-col items-center justify-center p-4 bg-[#F0F2F5] dark:bg-[#18191A]">
        <div class="max-w-[1000px] w-full flex flex-col lg:flex-row items-center gap-10 lg:gap-20">
            <div class="text-center lg:text-left flex-1">
                <h1 class="text-[#1877F2] text-5xl md:text-6xl font-bold mb-4 tracking-tighter">facebook 241</h1>
                <p class="text-2xl font-normal leading-tight text-gray-700 dark:text-gray-300">Retrouvez vos amis et la communauté du Gabon sur Facebook 241.</p>
            </div>

            <div class="w-full max-w-[400px]">
                <div class="fb-card p-4 dark:bg-[#242526]">
                    <form id="login-form" class="space-y-4">
                        <input type="email" id="login-email" placeholder="Adresse e-mail" class="login-input dark:bg-[#3A3B3C] dark:border-none dark:text-white" required>
                        <input type="password" id="login-pass" placeholder="Mot de passe" class="login-input dark:bg-[#3A3B3C] dark:border-none dark:text-white" required>
                        <button type="submit" id="login-btn" class="w-full bg-[#1877F2] text-white text-xl font-bold py-3 rounded-md hover:bg-blue-600 transition-colors">Se connecter</button>
                        <div class="text-center"><a href="#" class="text-[#1877F2] text-sm hover:underline">Mot de passe oublié ?</a></div>
                        <hr class="border-gray-300 dark:border-gray-700">
                        <div class="flex justify-center pt-2">
                            <button type="button" onclick="showSignup()" class="btn-green px-4 py-3 rounded-md text-lg">Créer un compte</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Inscription -->
    <div id="signup-modal" class="fixed inset-0 bg-white/80 dark:bg-black/80 backdrop-blur-sm z-[500] hidden items-center justify-center p-4">
        <div class="w-full max-w-[432px] fb-card dark:bg-[#242526] overflow-hidden">
            <div class="p-4 border-b border-gray-300 dark:border-gray-700 flex justify-between items-start">
                <div>
                    <h2 class="text-3xl font-bold">S'inscrire</h2>
                    <p class="text-gray-500 text-sm">C'est rapide et facile.</p>
                </div>
                <button onclick="hideSignup()" class="text-gray-500 text-2xl hover:bg-gray-100 dark:hover:bg-gray-700 w-8 h-8 rounded-full">&times;</button>
            </div>
            <form id="signup-form" class="p-4 space-y-3">
                <div class="flex gap-3">
                    <input type="text" id="reg-prenom" placeholder="Prénom" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                    <input type="text" id="reg-nom" placeholder="Nom" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                </div>
                <input type="email" id="reg-email" placeholder="E-mail" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                <input type="password" id="reg-pass" placeholder="Mot de passe" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                <p class="text-[11px] text-gray-500">En cliquant sur S'inscrire, vous acceptez nos Conditions générales.</p>
                <div class="flex justify-center pt-4">
                    <button type="submit" id="signup-btn" class="btn-green px-12 py-2 rounded-md text-lg min-w-[194px]">S'inscrire</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Interface Application Principale -->
    <div id="app-container">
        <header class="h-14 bg-white dark:bg-[#242526] border-b border-gray-300 dark:border-gray-700 flex items-center justify-between px-4 sticky top-0 z-[100] shadow-sm">
            <div class="flex items-center gap-2">
                <div class="w-10 h-10 bg-[#1877F2] rounded-full flex items-center justify-center shadow-lg">
                    <span class="text-white font-black text-2xl italic">f</span>
                </div>
                <span class="font-bold text-xl text-[#1877F2] hidden sm:block">241</span>
            </div>
            
            <!-- Navigation Desktop -->
            <div class="hidden md:flex items-center gap-10">
                <button class="text-[#1877F2] text-2xl border-b-4 border-[#1877F2] pb-1 px-8"><i class="fa-solid fa-house"></i></button>
                <button class="text-gray-500 text-2xl px-8 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg py-2"><i class="fa-solid fa-users"></i></button>
                <button class="text-gray-500 text-2xl px-8 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg py-2"><i class="fa-solid fa-play"></i></button>
            </div>

            <div class="flex items-center gap-2">
                <button onclick="toggleDarkMode()" class="w-10 h-10 rounded-full bg-gray-100 dark:bg-[#3A3B3C] flex items-center justify-center hover:bg-gray-200"><i id="theme-icon" class="fa-solid fa-moon"></i></button>
                <div class="flex items-center gap-2 bg-gray-100 dark:bg-[#3A3B3C] p-1 rounded-full pr-3 cursor-pointer">
                    <img id="user-avatar" src="" class="w-8 h-8 rounded-full bg-white object-cover" alt="me">
                    <span id="user-name-display" class="text-sm font-bold truncate max-w-[80px]">...</span>
                </div>
                <button onclick="logout()" class="w-10 h-10 rounded-full bg-gray-100 dark:bg-[#3A3B3C] text-gray-500 hover:text-red-500 flex items-center justify-center"><i class="fa-solid fa-right-from-bracket"></i></button>
            </div>
        </header>

        <main class="max-w-[600px] mx-auto p-4 space-y-4 pb-20">
            <!-- Widget Publication -->
            <div class="fb-card p-4 dark:bg-[#242526]">
                <div class="flex gap-3 items-center">
                    <img id="user-avatar-post" src="" class="w-10 h-10 rounded-full bg-white border" alt="">
                    <div onclick="openModal()" class="flex-1 bg-gray-100 dark:bg-[#3A3B3C] rounded-full px-4 py-2.5 text-gray-500 cursor-pointer hover:bg-gray-200 transition-colors">
                        Quoi de neuf, <span id="user-first-name"></span> ?
                    </div>
                </div>
                <hr class="my-3 border-gray-200 dark:border-gray-700">
                <div class="flex justify-around">
                    <button class="flex items-center gap-2 text-gray-500 font-semibold py-2 px-4 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><i class="fa-solid fa-video text-red-500"></i> Vidéo en direct</button>
                    <button class="flex items-center gap-2 text-gray-500 font-semibold py-2 px-4 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><i class="fa-solid fa-images text-green-500"></i> Photo/vidéo</button>
                </div>
            </div>

            <!-- Feed -->
            <div id="posts-container" class="space-y-4"></div>
        </main>
    </div>

    <!-- Modal Publication -->
    <div id="post-modal" class="fixed inset-0 bg-white/90 dark:bg-black/90 backdrop-blur-md z-[600] hidden items-center justify-center p-4">
        <div class="w-full max-w-[500px] fb-card dark:bg-[#242526] p-4 animate-in zoom-in">
             <div class="flex justify-between items-center mb-4 border-b pb-3 dark:border-gray-700">
                <h3 class="font-bold text-xl text-center w-full">Créer une publication</h3>
                <button onclick="closeModal()" class="absolute right-6 top-6 w-9 h-9 bg-gray-100 dark:bg-gray-700 rounded-full flex items-center justify-center text-gray-500"><i class="fa-solid fa-xmark text-xl"></i></button>
             </div>
             <div class="flex items-center gap-3 mb-4">
                <img id="modal-avatar" src="" class="w-11 h-11 rounded-full border">
                <div>
                    <p id="modal-user-name" class="font-bold">...</p>
                    <span class="bg-gray-200 dark:bg-gray-700 px-2 py-0.5 rounded text-xs font-semibold flex items-center gap-1"><i class="fa-solid fa-earth-africa"></i> Public</span>
                </div>
             </div>
             <textarea id="post-input" class="w-full h-40 bg-transparent outline-none text-xl resize-none placeholder-gray-400" placeholder="Que voulez-vous dire à la communauté ?"></textarea>
             <button onclick="createPost()" id="publish-btn" class="w-full bg-[#1877F2] text-white py-2.5 rounded-lg font-bold mt-4 disabled:opacity-50 disabled:cursor-not-allowed transition-all">Publier</button>
        </div>
    </div>

    <!-- Firebase Logic -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, onAuthStateChanged, signOut, updateProfile } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, onSnapshot, serverTimestamp, doc, setDoc, getDoc, updateDoc, arrayUnion, arrayRemove } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Configuration Firebase (Celle fournie)
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f",
            measurementId: "G-T7415FPS91"
        };

        // Init Firebase
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'facebook241-vfinal';

        let userProfile = null;

        // --- AUTH OBSERVER ---
        onAuthStateChanged(auth, async (user) => {
            const splash = document.getElementById('splash-screen');
            const authContainer = document.getElementById('auth-container');
            const appContainer = document.getElementById('app-container');

            if (user) {
                try {
                    // Récupération forcée du profil depuis Firestore
                    const profileRef = doc(db, 'artifacts', appId, 'public', 'users', user.uid);
                    const snap = await getDoc(profileRef);
                    
                    if (snap.exists()) {
                        userProfile = snap.data();
                    } else {
                        // Secours si le document n'est pas encore prêt (latence Firestore)
                        userProfile = {
                            prenom: user.displayName || 'Utilisateur',
                            nom: '',
                            uid: user.uid
                        };
                    }
                    
                    updateUI(userProfile);
                    authContainer.style.display = 'none';
                    appContainer.style.display = 'block';
                    listenForPosts();
                } catch (err) {
                    console.error("Auth error:", err);
                }
            } else {
                authContainer.style.display = 'flex';
                appContainer.style.display = 'none';
            }
            
            splash.style.opacity = '0';
            setTimeout(() => splash.classList.add('hidden'), 500);
        });

        // --- CONNEXION ---
        document.getElementById('login-form').onsubmit = async (e) => {
            e.preventDefault();
            const email = document.getElementById('login-email').value.trim();
            const pass = document.getElementById('login-pass').value;
            const btn = document.getElementById('login-btn');

            try {
                btn.disabled = true;
                btn.innerText = "Connexion en cours...";
                await signInWithEmailAndPassword(auth, email, pass);
            } catch (err) {
                alert("Erreur: Identifiants invalides.");
                btn.disabled = false;
                btn.innerText = "Se connecter";
            }
        };

        // --- INSCRIPTION ---
        document.getElementById('signup-form').onsubmit = async (e) => {
            e.preventDefault();
            const prenom = document.getElementById('reg-prenom').value.trim();
            const nom = document.getElementById('reg-nom').value.trim();
            const email = document.getElementById('reg-email').value.trim();
            const pass = document.getElementById('reg-pass').value;
            const btn = document.getElementById('signup-btn');

            try {
                btn.disabled = true;
                btn.innerText = "Création du compte...";
                
                const cred = await createUserWithEmailAndPassword(auth, email, pass);
                await updateProfile(cred.user, { displayName: prenom });

                const userData = {
                    prenom, nom, email, uid: cred.user.uid,
                    createdAt: serverTimestamp(),
                    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${prenom}${Date.now()}`
                };

                await setDoc(doc(db, 'artifacts', appId, 'public', 'users', cred.user.uid), userData);
                userProfile = userData;
                hideSignup();
            } catch (err) {
                alert("Erreur d'inscription: " + err.message);
                btn.disabled = false;
                btn.innerText = "S'inscrire";
            }
        };

        window.logout = () => signOut(auth);

        // --- POSTS ---
        async function createPost() {
            const content = document.getElementById('post-input').value.trim();
            if (!content || !auth.currentUser) return;

            const btn = document.getElementById('publish-btn');
            try {
                btn.disabled = true;
                btn.innerText = "Publication...";
                
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'posts'), {
                    content,
                    authorName: `${userProfile.prenom} ${userProfile.nom}`.trim(),
                    authorId: auth.currentUser.uid,
                    authorAvatar: userProfile.avatar || `https://api.dicebear.com/7.x/avataaars/svg?seed=${userProfile.prenom}`,
                    createdAt: serverTimestamp(),
                    likes: []
                });

                document.getElementById('post-input').value = "";
                closeModal();
            } catch (err) {
                alert("Erreur lors de la publication.");
            } finally {
                btn.disabled = false;
                btn.innerText = "Publier";
            }
        }
        window.createPost = createPost;

        function listenForPosts() {
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'posts'));
            onSnapshot(q, (snapshot) => {
                const posts = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }))
                    .sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0));
                renderPosts(posts);
            }, (err) => console.error("Snapshot error:", err));
        }

        function renderPosts(posts) {
            const container = document.getElementById('posts-container');
            if (posts.length === 0) {
                container.innerHTML = `<div class="fb-card p-10 text-center text-gray-500 font-semibold dark:bg-[#242526]">Rien à afficher pour le moment. Soyez le premier à poster !</div>`;
                return;
            }

            container.innerHTML = posts.map(post => {
                const isLiked = post.likes?.includes(auth.currentUser?.uid);
                return `
                <div class="fb-card p-4 dark:bg-[#242526] post-enter">
                    <div class="flex justify-between items-start mb-3">
                        <div class="flex gap-3">
                            <img src="${post.authorAvatar}" class="w-10 h-10 rounded-full border bg-white">
                            <div>
                                <p class="font-bold text-sm hover:underline cursor-pointer">${post.authorName}</p>
                                <p class="text-xs text-gray-500">${post.createdAt ? new Date(post.createdAt.seconds * 1000).toLocaleString() : 'En cours...'}</p>
                            </div>
                        </div>
                        <button class="text-gray-500 p-1 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-full"><i class="fa-solid fa-ellipsis"></i></button>
                    </div>
                    <p class="text-[15px] mb-3 whitespace-pre-wrap">${post.content}</p>
                    
                    <div class="flex items-center justify-between text-gray-500 text-sm py-2 border-t dark:border-gray-700 mt-4">
                        <div class="flex items-center gap-1">
                            <span class="w-5 h-5 bg-blue-500 rounded-full flex items-center justify-center text-white text-[10px]"><i class="fa-solid fa-thumbs-up"></i></span>
                            <span>${post.likes?.length || 0}</span>
                        </div>
                        <div>0 commentaires</div>
                    </div>

                    <div class="flex justify-around pt-1 border-t dark:border-gray-700 mt-1">
                        <button onclick="handleLike('${post.id}', ${isLiked})" class="flex-1 py-2 flex items-center justify-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg transition-colors ${isLiked ? 'text-[#1877F2] font-bold' : 'text-gray-500'}">
                            <i class="fa-${isLiked ? 'solid' : 'regular'} fa-thumbs-up"></i> J'aime
                        </button>
                        <button class="flex-1 py-2 flex items-center justify-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg text-gray-500 transition-colors">
                            <i class="fa-regular fa-comment"></i> Commenter
                        </button>
                    </div>
                </div>
            `}).join('');
        }

        async function handleLike(postId, isLiked) {
            const postRef = doc(db, 'artifacts', appId, 'public', 'data', 'posts', postId);
            const uid = auth.currentUser.uid;
            try {
                await updateDoc(postRef, {
                    likes: isLiked ? arrayRemove(uid) : arrayUnion(uid)
                });
            } catch (err) {
                console.error("Like error:", err);
            }
        }
        window.handleLike = handleLike;

        function updateUI(profile) {
            const avatar = profile.avatar || `https://api.dicebear.com/7.x/avataaars/svg?seed=${profile.prenom}`;
            document.getElementById('user-avatar').src = avatar;
            document.getElementById('user-avatar-post').src = avatar;
            document.getElementById('modal-avatar').src = avatar;
            document.getElementById('user-name-display').textContent = profile.prenom;
            document.getElementById('user-first-name').textContent = profile.prenom;
            document.getElementById('modal-user-name').textContent = `${profile.prenom} ${profile.nom || ''}`;
        }

        // --- UI HELPERS ---
        window.showSignup = () => document.getElementById('signup-modal').style.display = 'flex';
        window.hideSignup = () => document.getElementById('signup-modal').style.display = 'none';
        window.openModal = () => {
            document.getElementById('post-modal').style.display = 'flex';
            document.getElementById('post-input').focus();
        };
        window.closeModal = () => document.getElementById('post-modal').style.display = 'none';
        
        window.toggleDarkMode = () => {
            const isDark = document.documentElement.classList.toggle('dark');
            document.getElementById('theme-icon').className = isDark ? 'fa-solid fa-sun' : 'fa-solid fa-moon';
        };

        document.getElementById('post-input').oninput = (e) => {
            document.getElementById('publish-btn').disabled = !e.target.value.trim();
        };
    </script>
</body>
</html>
