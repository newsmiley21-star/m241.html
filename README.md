import React, { useState, useEffect, useRef } from 'react';
import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  onAuthStateChanged, 
  signInWithEmailAndPassword, 
  createUserWithEmailAndPassword,
  signOut,
  updateProfile,
  signInAnonymously,
  signInWithCustomToken
} from 'firebase/auth';
import { 
  getFirestore, 
  collection, 
  onSnapshot,
  addDoc,
  serverTimestamp,
  doc,
  setDoc,
  query,
  orderBy,
  limit,
  updateDoc,
  increment,
  deleteDoc,
  getDoc,
  arrayUnion,
  arrayRemove,
  where
} from 'firebase/firestore';
import { 
  Home, Tv, Store, Users, Bell, Menu, MessageCircle, Plus, Search, 
  Video, Image as ImageIcon, Smile, ThumbsUp, MessageSquare, Share2, 
  MoreHorizontal, Phone, Info, Mic, Send, LogOut, Camera, X, Heart,
  ChevronDown, RefreshCw, User, Settings, Check, Edit2, ImagePlus,
  Trash2, ShoppingBag, MapPin, Tag, MessageCircle as ChatIcon,
  Smartphone, Shirt, Utensils, Home as HomeIcon, Briefcase, Filter,
  Star, ExternalLink, Play, Eye, UsersRound, Globe, Lock, BellRing,
  Moon, Sun, CheckCheck, Paperclip, ShoppingCart, Package, AlertCircle
} from 'lucide-react';

// --- Configuration Firebase ---
const firebaseConfig = JSON.parse(__firebase_config);
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'messenger-241-prod';

const App = () => {
  // --- ÉTATS GLOBAUX ---
  const [user, setUser] = useState(null);
  const [isDarkMode, setIsDarkMode] = useState(false);
  const [authMode, setAuthMode] = useState('login'); 
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [username, setUsername] = useState('');
  const [error, setError] = useState('');
  const [loading, setLoading] = useState(true);

  // --- NAVIGATION & UI ---
  const [activeTab, setActiveTab] = useState('home');
  const [isChatOpen, setIsChatOpen] = useState(false);
  const [selectedFriend, setSelectedFriend] = useState(null);
  const [showMarketModal, setShowMarketModal] = useState(false);
  const [toast, setToast] = useState(null);

  // --- DONNÉES ---
  const [posts, setPosts] = useState([]);
  const [marketItems, setMarketItems] = useState([]);
  const [allUsers, setAllUsers] = useState([]);
  const [chatMessages, setChatMessages] = useState([]);
  
  // --- FORMULAIRES ---
  const [postText, setPostText] = useState('');
  const [postImageUrl, setPostImageUrl] = useState('');
  const [showImageInput, setShowImageInput] = useState(false);
  const [newMessage, setNewMessage] = useState('');
  const [marketForm, setMarketForm] = useState({ title: '', price: '', category: 'Électronique' });

  const chatEndRef = useRef(null);

  // --- SYSTÈME DE NOTIFICATION (TOAST) ---
  const showToast = (msg) => {
    setToast(msg);
    setTimeout(() => setToast(null), 3000);
  };

  // --- AUTHENTIFICATION ---
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        console.error("Auth init error", err);
      }
    };
    initAuth();
    const unsubscribe = onAuthStateChanged(auth, (u) => {
      setUser(u);
      setLoading(false);
    });
    return () => unsubscribe();
  }, []);

  const handleAuth = async (e) => {
    e.preventDefault();
    setError('');
    try {
      if (authMode === 'register') {
        const res = await createUserWithEmailAndPassword(auth, email, password);
        await updateProfile(res.user, { displayName: username });
        await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid), {
          uid: res.user.uid,
          displayName: username,
          createdAt: serverTimestamp(),
          isOnline: true
        });
      } else {
        await signInWithEmailAndPassword(auth, email, password);
      }
    } catch (err) { 
      setError(err.message.includes('auth/user-not-found') ? "Utilisateur inconnu" : "Vérifiez vos identifiants"); 
    }
  };

  // --- SYNC DONNÉES ---
  useEffect(() => {
    if (!user) return;

    // Synchronisation des Posts (Public)
    const unsubPosts = onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'posts'), (snap) => {
      const p = snap.docs.map(d => ({ id: d.id, ...d.data() }));
      setPosts(p.sort((a,b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0)));
    }, (err) => console.error("Posts Sync Error", err));

    // Synchronisation Marketplace (Public)
    const unsubMarket = onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'market'), (snap) => {
      setMarketItems(snap.docs.map(d => ({ id: d.id, ...d.data() })));
    }, (err) => console.error("Market Sync Error", err));

    // Synchronisation Utilisateurs (Public pour la liste de contacts)
    const unsubUsers = onSnapshot(collection(db, 'artifacts', appId, 'users'), (snap) => {
      setAllUsers(snap.docs.map(d => ({ id: d.id, ...d.data() })));
    }, (err) => console.error("Users Sync Error", err));

    return () => { unsubPosts(); unsubMarket(); unsubUsers(); };
  }, [user]);

  // --- LOGIQUE CHAT ---
  useEffect(() => {
    if (!user || !selectedFriend) return;
    
    const chatId = [user.uid, selectedFriend.uid].sort().join('_');
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'chats', chatId, 'messages');
    
    const unsubChat = onSnapshot(q, (snap) => {
      const msgs = snap.docs.map(d => ({ id: d.id, ...d.data() }));
      setChatMessages(msgs.sort((a,b) => (a.createdAt?.seconds || 0) - (b.createdAt?.seconds || 0)));
      setTimeout(() => chatEndRef.current?.scrollIntoView({ behavior: 'smooth' }), 100);
    }, (err) => console.error("Chat Sync Error", err));

    return () => unsubChat();
  }, [user, selectedFriend]);

  const sendMessage = async (e) => {
    e.preventDefault();
    if (!newMessage.trim() || !selectedFriend) return;

    const chatId = [user.uid, selectedFriend.uid].sort().join('_');
    const msgContent = newMessage;
    setNewMessage('');
    try {
      await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', chatId, 'messages'), {
        text: msgContent,
        senderId: user.uid,
        createdAt: serverTimestamp(),
      });
    } catch (err) { showToast("Erreur d'envoi"); }
  };

  const openChatWithSeller = (sellerName, sellerId) => {
    if (sellerId === user.uid) return;
    const seller = allUsers.find(u => u.uid === sellerId) || { displayName: sellerName, uid: sellerId };
    setSelectedFriend(seller);
    setIsChatOpen(true);
  };

  // --- LOGIQUE ACTIONS ---
  const handlePost = async () => {
    if (!postText.trim() && !postImageUrl) return;
    try {
      await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'posts'), {
        author: user.displayName || 'Anonyme',
        authorId: user.uid,
        content: postText,
        imageUrl: postImageUrl,
        createdAt: serverTimestamp()
      });
      setPostText('');
      setPostImageUrl('');
      setShowImageInput(false);
      showToast("Publication partagée !");
    } catch (e) { showToast("Erreur lors du partage"); }
  };

  const handleMarketSubmit = async (e) => {
    e.preventDefault();
    try {
      await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'market'), {
        ...marketForm,
        seller: user.displayName,
        sellerId: user.uid,
        createdAt: serverTimestamp()
      });
      setMarketForm({ title: '', price: '', category: 'Électronique' });
      setShowMarketModal(false);
      showToast("Article mis en vente !");
    } catch (e) { showToast("Erreur Marketplace"); }
  };

  if (loading) return (
    <div className="h-screen flex flex-col items-center justify-center bg-[#f0f2f5]">
      <div className="w-16 h-16 border-4 border-[#007fff] border-t-transparent rounded-full animate-spin mb-4"></div>
      <p className="font-black text-[#007fff] animate-pulse">LANCEMENT DE M241...</p>
    </div>
  );

  if (!user) {
    return (
      <div className="min-h-screen bg-[#f0f2f5] flex flex-col items-center justify-center p-6">
        <div className="text-center mb-8">
            <h1 className="text-[#007fff] text-7xl font-black tracking-tighter mb-2">m241</h1>
            <p className="text-gray-500 font-bold uppercase tracking-[0.2em] text-xs italic">Gabon Digital Network</p>
        </div>
        <div className="bg-white p-8 rounded-[40px] shadow-2xl w-full max-w-md border-b-[12px] border-[#36a100]">
          <h2 className="text-2xl font-black mb-6 text-center">{authMode === 'login' ? 'Heureux de vous revoir !' : 'Créer votre passeport 241'}</h2>
          <form onSubmit={handleAuth} className="space-y-4">
            {authMode === 'register' && (
              <div className="relative">
                <User className="absolute left-4 top-4 text-gray-400" size={20} />
                <input placeholder="Votre nom/pseudo" value={username} onChange={e => setUsername(e.target.value)} className="w-full p-4 pl-12 bg-gray-50 border-none rounded-2xl focus:ring-2 ring-blue-400 outline-none font-medium" required />
              </div>
            )}
            <div className="relative">
              <Globe className="absolute left-4 top-4 text-gray-400" size={20} />
              <input type="email" placeholder="Adresse Email" value={email} onChange={e => setEmail(e.target.value)} className="w-full p-4 pl-12 bg-gray-50 border-none rounded-2xl focus:ring-2 ring-blue-400 outline-none font-medium" required />
            </div>
            <div className="relative">
              <Lock className="absolute left-4 top-4 text-gray-400" size={20} />
              <input type="password" placeholder="Mot de passe" value={password} onChange={e => setPassword(e.target.value)} className="w-full p-4 pl-12 bg-gray-50 border-none rounded-2xl focus:ring-2 ring-blue-400 outline-none font-medium" required />
            </div>
            <button className="w-full bg-[#007fff] text-white py-4 rounded-2xl font-black uppercase shadow-lg hover:shadow-blue-200 active:scale-95 transition-all">
                {authMode === 'login' ? 'Entrer dans le Kwen' : 'Finaliser l\'inscription'}
            </button>
          </form>
          <button onClick={() => setAuthMode(authMode === 'login' ? 'register' : 'login')} className="w-full mt-6 text-sm text-gray-400 font-bold hover:text-[#007fff]">
            {authMode === 'login' ? "Pas encore de compte ? S'inscrire" : "Déjà membre ? Se connecter"}
          </button>
          {error && (
            <div className="mt-4 p-3 bg-red-50 text-red-500 rounded-xl flex items-center gap-2 text-xs font-bold border border-red-100 animate-bounce">
              <AlertCircle size={16} /> {error}
            </div>
          )}
        </div>
      </div>
    );
  }

  return (
    <div className={`min-h-screen transition-colors duration-500 font-sans ${isDarkMode ? 'bg-[#18191a] text-[#e4e6eb]' : 'bg-[#f0f2f5] text-[#1c1e21]'}`}>
      
      {/* TOAST NOTIFICATION */}
      {toast && (
        <div className="fixed top-20 left-1/2 -translate-x-1/2 z-[1000] bg-black/80 backdrop-blur text-white px-6 py-3 rounded-full text-sm font-black animate-in fade-in slide-in-from-top-4 duration-300 shadow-2xl border border-white/10">
          {toast}
        </div>
      )}

      {/* NAVBAR */}
      <nav className={`fixed top-0 w-full h-16 z-[100] flex items-center justify-between px-6 border-b transition-all ${isDarkMode ? 'bg-[#242526]/80 backdrop-blur border-gray-700' : 'bg-white/80 backdrop-blur border-yellow-400/50 shadow-sm'}`}>
        <div className="flex items-center gap-3">
            <div className="w-10 h-10 rounded-xl overflow-hidden flex flex-col shadow-lg rotate-3 group-hover:rotate-0 transition-transform">
                <div className="flex-1 bg-[#36a100]"></div>
                <div className="flex-1 bg-[#ffce00]"></div>
                <div className="flex-1 bg-[#007fff]"></div>
            </div>
            <span className="font-black text-2xl tracking-tighter bg-gradient-to-r from-[#36a100] via-yellow-500 to-[#007fff] bg-clip-text text-transparent">M241</span>
        </div>

        <div className="flex items-center gap-1 md:gap-4">
          <button onClick={() => setActiveTab('home')} className={`p-3 rounded-2xl transition-all ${activeTab === 'home' ? 'bg-[#007fff]/10 text-[#007fff]' : 'text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800'}`}><Home /></button>
          <button onClick={() => setActiveTab('market')} className={`p-3 rounded-2xl transition-all ${activeTab === 'market' ? 'bg-[#36a100]/10 text-[#36a100]' : 'text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800'}`}><Store /></button>
          <div className="w-px h-6 bg-gray-200 dark:bg-gray-700 mx-2 hidden sm:block"></div>
          <button onClick={() => setIsDarkMode(!isDarkMode)} className="p-3 rounded-2xl text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800 transition-all">
            {isDarkMode ? <Sun className="text-yellow-400" /> : <Moon />}
          </button>
          <button onClick={() => signOut(auth)} className="p-3 text-red-500 hover:bg-red-50 dark:hover:bg-red-900/20 rounded-2xl transition-colors"><LogOut /></button>
        </div>
      </nav>

      <div className="pt-16 flex h-screen max-w-screen-2xl mx-auto">
        
        {/* SIDEBAR GAUCHE */}
        <aside className="hidden lg:flex flex-col w-80 p-6 space-y-6 overflow-y-auto border-r dark:border-gray-800">
            <div className={`p-6 rounded-[32px] border ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200 shadow-sm'}`}>
                <div className="flex items-center gap-4 mb-6">
                    <div className="w-14 h-14 rounded-full bg-gradient-to-br from-[#36a100] via-[#ffce00] to-[#007fff] p-1 shadow-xl">
                        <div className={`w-full h-full rounded-full flex items-center justify-center font-black text-2xl border-2 border-white/20 ${isDarkMode ? 'bg-[#242526]' : 'bg-white'}`}>
                            {user.displayName?.charAt(0)}
                        </div>
                    </div>
                    <div>
                        <h4 className="font-black text-lg truncate w-32">{user.displayName}</h4>
                        <div className="flex items-center gap-1 text-[10px] text-[#36a100] font-black uppercase tracking-widest italic">
                           <Check size={12} /> Patriote Certifié
                        </div>
                    </div>
                </div>
                <div className="space-y-1">
                    <button className="w-full flex items-center gap-3 p-3 rounded-2xl hover:bg-blue-50 dark:hover:bg-blue-900/20 font-bold transition-all text-sm"><Users size={18} className="text-blue-500" /> Le Kwen (Amis)</button>
                    <button className="w-full flex items-center gap-3 p-3 rounded-2xl hover:bg-green-50 dark:hover:bg-green-900/20 font-bold transition-all text-sm"><Tv size={18} className="text-[#36a100]" /> Vidéos Gaboma</button>
                    <button className="w-full flex items-center gap-3 p-3 rounded-2xl hover:bg-yellow-50 dark:hover:bg-yellow-900/20 font-bold transition-all text-sm"><Settings size={18} className="text-yellow-600" /> Paramètres</button>
                </div>
            </div>

            <div>
                <h3 className="text-[10px] font-black uppercase text-gray-400 mb-4 px-2 tracking-[0.2em]">Histoires du 241</h3>
                <div className="grid grid-cols-2 gap-3">
                    <div className="h-44 rounded-[28px] bg-gray-200 dark:bg-gray-800 border-2 border-dashed border-blue-400 flex flex-col items-center justify-center text-blue-400 cursor-pointer hover:bg-blue-50 transition-all group">
                        <Plus size={32} className="group-hover:scale-125 transition-transform" />
                        <span className="text-[9px] font-black uppercase mt-2">Ajouter</span>
                    </div>
                    <div className="h-44 rounded-[28px] bg-gradient-to-t from-black/80 to-blue-500 relative overflow-hidden shadow-lg border-2 border-white cursor-pointer hover:scale-[1.02] transition-transform">
                         <div className="absolute top-3 left-3 w-8 h-8 rounded-full border-2 border-white bg-blue-600 flex items-center justify-center font-black text-white text-[10px] shadow-lg">M</div>
                         <div className="absolute bottom-3 left-3 right-3 text-[10px] font-black text-white leading-tight uppercase italic drop-shadow-md">Infos LBV</div>
                    </div>
                </div>
            </div>
        </aside>

        {/* CONTENU CENTRAL */}
        <main className="flex-1 overflow-y-auto p-4 flex justify-center scrollbar-hide">
            <div className="w-full max-w-[620px] space-y-6 pb-24">
                
                {activeTab === 'home' ? (
                    <>
                        {/* PUBLICATION */}
                        <div className={`p-6 rounded-[32px] border transition-all ${isDarkMode ? 'bg-[#242526] border-gray-700 shadow-xl' : 'bg-white border-yellow-200 shadow-lg shadow-yellow-100/50'}`}>
                            <div className="flex gap-4 mb-4">
                                <div className="w-12 h-12 rounded-full bg-blue-600 text-white flex items-center justify-center font-black text-xl shadow-lg shrink-0">{user.displayName?.charAt(0)}</div>
                                <textarea 
                                    value={postText}
                                    onChange={e => setPostText(e.target.value)}
                                    placeholder={`On dit quoi ce matin, ${user.displayName} ?`} 
                                    className={`flex-1 rounded-2xl p-4 text-sm outline-none resize-none h-28 transition-all focus:ring-4 ring-blue-400/10 ${isDarkMode ? 'bg-[#3a3b3c] placeholder-gray-500' : 'bg-[#f0f2f5] placeholder-gray-400'}`} 
                                />
                            </div>
                            
                            {showImageInput && (
                                <div className="mb-4 animate-in zoom-in duration-300">
                                    <div className="relative">
                                      <ImageIcon className="absolute left-4 top-4 text-gray-400" size={18} />
                                      <input 
                                          value={postImageUrl}
                                          onChange={e => setPostImageUrl(e.target.value)}
                                          placeholder="Coller l'URL d'une image ici..." 
                                          className={`w-full p-4 pl-12 rounded-2xl text-xs font-bold border-2 border-dashed ${isDarkMode ? 'bg-black/20 border-gray-600' : 'bg-gray-50 border-gray-200'}`}
                                      />
                                    </div>
                                </div>
                            )}

                            <div className="flex items-center justify-between border-t dark:border-gray-700 pt-4">
                                <button onClick={() => setShowImageInput(!showImageInput)} className={`flex items-center gap-2 px-5 py-2.5 rounded-2xl text-[10px] font-black uppercase tracking-widest transition-all ${showImageInput ? 'bg-green-500 text-white shadow-lg' : 'text-green-500 hover:bg-green-50 dark:hover:bg-green-900/20'}`}>
                                    <ImageIcon size={18} /> {showImageInput ? 'Annuler Image' : 'Ajouter Média'}
                                </button>
                                <button 
                                    onClick={handlePost}
                                    disabled={!postText.trim() && !postImageUrl}
                                    className={`px-10 py-3 rounded-2xl font-black text-xs uppercase tracking-[0.15em] shadow-xl transition-all active:scale-95 ${(!postText.trim() && !postImageUrl) ? 'bg-gray-300 cursor-not-allowed text-gray-500' : 'bg-[#007fff] text-white hover:bg-blue-600 hover:shadow-blue-200'}`}
                                >
                                    Publier
                                </button>
                            </div>
                        </div>

                        {/* FLUX DE POSTS */}
                        {posts.length === 0 ? (
                           <div className="py-20 text-center space-y-4 opacity-50">
                              <div className="w-20 h-20 bg-gray-200 dark:bg-gray-800 rounded-full mx-auto flex items-center justify-center text-gray-400">
                                <Search size={32} />
                              </div>
                              <p className="font-black text-sm uppercase italic">C'est encore calme au pays... Lance le premier débat !</p>
                           </div>
                        ) : posts.map(post => (
                            <div key={post.id} className={`rounded-[32px] border overflow-hidden transition-all hover:translate-y-[-2px] ${isDarkMode ? 'bg-[#242526] border-gray-700 shadow-2xl' : 'bg-white border-gray-100 shadow-sm'}`}>
                                <div className="p-6 flex items-center justify-between">
                                    <div className="flex items-center gap-4">
                                        <div className="w-12 h-12 rounded-full bg-gradient-to-tr from-[#36a100] to-yellow-400 flex items-center justify-center text-white font-black text-lg border-2 border-white shadow-lg">{post.author?.charAt(0)}</div>
                                        <div>
                                            <h4 className="font-black text-base">{post.author}</h4>
                                            <p className="text-[9px] text-gray-400 font-black uppercase flex items-center gap-1 italic">
                                              <Globe size={10} /> {post.createdAt ? new Date(post.createdAt.seconds * 1000).toLocaleDateString('fr-FR', {day: 'numeric', month: 'short', hour: '2-digit', minute: '2-digit'}) : 'À l\'instant'}
                                            </p>
                                        </div>
                                    </div>
                                    <button className="p-2 text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800 rounded-full transition-colors"><MoreHorizontal /></button>
                                </div>
                                
                                <div className="px-6 pb-6">
                                    <p className="text-sm leading-relaxed font-medium whitespace-pre-wrap">{post.content}</p>
                                </div>

                                {post.imageUrl && (
                                    <div className="w-full bg-black/5 relative group">
                                        <img src={post.imageUrl} alt="Post" className="w-full object-cover max-h-[500px] transition-transform duration-700 group-hover:scale-[1.02]" onError={(e) => e.target.style.display='none'} />
                                        <div className="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent pointer-events-none"></div>
                                    </div>
                                )}

                                <div className="px-6 py-4 flex items-center justify-around border-t dark:border-gray-700">
                                    <button className="flex-1 flex items-center justify-center gap-2 text-gray-500 hover:text-red-500 transition-colors font-black uppercase text-[10px] tracking-widest py-3 rounded-xl hover:bg-red-50 dark:hover:bg-red-900/10">
                                        <Heart size={18} /> Big Force
                                    </button>
                                    <button 
                                      onClick={() => openChatWithSeller(post.author, post.authorId)}
                                      className="flex-1 flex items-center justify-center gap-2 text-gray-500 hover:text-[#007fff] transition-colors font-black uppercase text-[10px] tracking-widest py-3 rounded-xl hover:bg-blue-50 dark:hover:bg-blue-900/10"
                                    >
                                        <MessageCircle size={18} /> Réagir
                                    </button>
                                    <button className="flex-1 flex items-center justify-center gap-2 text-gray-500 hover:text-[#36a100] transition-colors font-black uppercase text-[10px] tracking-widest py-3 rounded-xl hover:bg-green-50 dark:hover:bg-green-900/10">
                                        <Share2 size={18} /> Partager
                                    </button>
                                </div>
                            </div>
                        ))}
                    </>
                ) : (
                    /* MARKETPLACE EN PRODUCTION */
                    <div className="animate-in fade-in slide-in-from-bottom-4 duration-500">
                        <div className="flex items-center justify-between mb-8 px-2">
                            <div>
                                <h2 className="text-4xl font-black tracking-tighter italic">Le Grand Marché</h2>
                                <p className="text-[10px] text-[#36a100] font-black uppercase tracking-widest italic flex items-center gap-1">
                                  <Tag size={12}/> Achetez et vendez entre patriotes
                                </p>
                            </div>
                            <button 
                              onClick={() => setShowMarketModal(true)}
                              className="bg-[#36a100] text-white flex items-center gap-2 px-6 py-4 rounded-[24px] font-black uppercase text-xs shadow-xl shadow-green-200 hover:scale-105 active:scale-95 transition-all"
                            >
                                <Plus size={20} /> Vendre un truc
                            </button>
                        </div>

                        <div className="grid grid-cols-1 sm:grid-cols-2 gap-6">
                            {marketItems.length === 0 ? (
                               <div className="col-span-2 py-20 text-center text-gray-400 font-black uppercase text-xs italic opacity-30">
                                  L'étalage est vide... <br/> Profite de l'ouverture pour être en tête de liste !
                               </div>
                            ) : marketItems.map(item => (
                                <div key={item.id} className={`rounded-[32px] border overflow-hidden group hover:shadow-2xl transition-all relative ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-100 shadow-lg'}`}>
                                    <div className="h-48 bg-gray-100 dark:bg-gray-800 flex items-center justify-center relative overflow-hidden">
                                        <div className="absolute inset-0 bg-gradient-to-br from-blue-500/10 to-transparent"></div>
                                        <Package size={56} className="text-gray-300 group-hover:scale-125 transition-transform duration-500 relative z-10" />
                                        <div className="absolute top-4 right-4 bg-black text-white px-4 py-2 rounded-2xl font-black text-xs shadow-xl border border-white/20">
                                            {item.price} <span className="text-yellow-400">FCFA</span>
                                        </div>
                                        <div className="absolute bottom-4 left-4 bg-white/90 backdrop-blur px-3 py-1 rounded-full text-[8px] font-black uppercase tracking-widest shadow-sm">
                                            {item.category}
                                        </div>
                                    </div>
                                    <div className="p-6">
                                        <h4 className="font-black text-lg mb-1 truncate">{item.title}</h4>
                                        <p className="text-[10px] text-gray-400 font-bold uppercase mb-5 flex items-center gap-1"><MapPin size={10} className="text-red-500"/> LBV • Vendeur: <span className="text-blue-500">{item.seller}</span></p>
                                        <button 
                                          onClick={() => openChatWithSeller(item.seller, item.sellerId)}
                                          className="w-full bg-[#007fff] text-white py-4 rounded-2xl font-black text-xs uppercase tracking-[0.2em] flex items-center justify-center gap-2 hover:bg-blue-600 shadow-lg hover:shadow-blue-200 transition-all"
                                        >
                                            <ChatIcon size={16}/> Discuter le prix
                                        </button>
                                    </div>
                                </div>
                            ))}
                        </div>
                    </div>
                )}
            </div>
        </main>

        {/* BARRE DE CONTACTS DROITE */}
        <aside className="hidden xl:flex flex-col w-80 p-6 border-l dark:border-gray-800 bg-gray-50/30 dark:bg-transparent">
            <div className="flex items-center justify-between mb-8 px-2">
                <div className="flex items-center gap-2">
                  <div className="w-2 h-2 bg-[#36a100] rounded-full animate-ping"></div>
                  <h3 className="text-[10px] font-black uppercase text-gray-500 tracking-[0.2em]">Connectés au Gabon</h3>
                </div>
                <UsersRound size={16} className="text-gray-400" />
            </div>
            <div className="space-y-2">
                {allUsers.filter(u => u.uid !== user.uid).length === 0 ? (
                   <p className="text-[10px] font-black text-center text-gray-400 uppercase italic py-10">Personne n'est encore là...</p>
                ) : allUsers.filter(u => u.uid !== user.uid).map(contact => (
                    <button 
                        key={contact.uid}
                        onClick={() => { setSelectedFriend(contact); setIsChatOpen(true); }}
                        className={`w-full flex items-center gap-4 p-4 rounded-3xl transition-all group relative ${selectedFriend?.uid === contact.uid ? 'bg-blue-600 text-white shadow-xl scale-105 z-10' : 'hover:bg-white dark:hover:bg-gray-800 shadow-sm border border-transparent hover:border-blue-100 dark:hover:border-gray-700'}`}
                    >
                        <div className="relative">
                            <div className={`w-12 h-12 rounded-2xl flex items-center justify-center font-black text-sm shadow-inner transition-transform group-hover:rotate-6 ${selectedFriend?.uid === contact.uid ? 'bg-white text-blue-600' : 'bg-gray-100 dark:bg-gray-700 text-gray-500'}`}>
                                {contact.displayName?.charAt(0)}
                            </div>
                            <div className="absolute -bottom-1 -right-1 w-4 h-4 bg-[#36a100] border-[3px] border-white dark:border-[#18191a] rounded-full"></div>
                        </div>
                        <div className="text-left overflow-hidden">
                            <p className="text-sm font-black truncate">{contact.displayName}</p>
                            <p className={`text-[8px] font-black uppercase tracking-tighter ${selectedFriend?.uid === contact.uid ? 'text-blue-100' : 'text-[#36a100]'}`}>LBV / Online</p>
                        </div>
                    </button>
                ))}
            </div>
        </aside>

      </div>

      {/* CHAT FLOTTANT PRO */}
      {isChatOpen && selectedFriend && (
        <div className={`fixed bottom-0 right-4 w-full sm:w-[400px] h-[550px] shadow-[0_-20px_60px_rgba(0,0,0,0.15)] flex flex-col rounded-t-[40px] z-[500] border-t-8 border-[#ffce00] animate-in slide-in-from-bottom-10 duration-500 ${isDarkMode ? 'bg-[#242526]' : 'bg-white'}`}>
            <div className={`p-5 border-b flex items-center justify-between ${isDarkMode ? 'border-gray-700' : 'border-gray-100'}`}>
                <div className="flex items-center gap-3">
                    <div className="w-11 h-11 rounded-2xl bg-blue-600 flex items-center justify-center text-white font-black shadow-lg shadow-blue-200/50">{selectedFriend.displayName?.charAt(0)}</div>
                    <div>
                        <h4 className="text-sm font-black truncate w-32">{selectedFriend.displayName}</h4>
                        <div className="flex items-center gap-1">
                            <span className="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span>
                            <span className="text-[9px] text-green-500 font-black uppercase italic tracking-widest">Capte à 100%</span>
                        </div>
                    </div>
                </div>
                <div className="flex gap-1">
                    <button className="p-3 text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800 rounded-2xl transition-colors"><Phone size={18}/></button>
                    <button onClick={() => setIsChatOpen(false)} className="p-3 text-gray-400 hover:bg-red-50 hover:text-red-500 rounded-2xl transition-colors"><X size={18}/></button>
                </div>
            </div>
            
            <div className={`flex-1 overflow-y-auto p-5 space-y-4 scroll-smooth ${isDarkMode ? 'bg-[#18191a]' : 'bg-gray-50'}`}>
                {chatMessages.length === 0 && (
                   <div className="h-full flex flex-col items-center justify-center text-center px-10 space-y-4 opacity-20 grayscale">
                      <MessageCircle size={48} />
                      <p className="text-xs font-black uppercase italic">Lance la causerie avec {selectedFriend.displayName} !</p>
                   </div>
                )}
                {chatMessages.map(m => (
                    <div key={m.id} className={`flex ${m.senderId === user.uid ? 'justify-end' : 'justify-start'} animate-in fade-in zoom-in-95 duration-200`}>
                        <div className={`max-w-[85%] p-4 rounded-[24px] text-sm font-medium shadow-sm leading-relaxed ${
                            m.senderId === user.uid 
                                ? 'bg-blue-600 text-white rounded-br-none shadow-blue-100' 
                                : (isDarkMode ? 'bg-[#3a3b3c] text-white rounded-bl-none' : 'bg-white text-gray-800 rounded-bl-none border border-gray-100')
                        }`}>
                            {m.text}
                            <div className={`text-[8px] mt-2 text-right opacity-60 font-black flex items-center justify-end gap-1 uppercase tracking-tighter`}>
                                {m.createdAt ? new Date(m.createdAt.seconds * 1000).toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}) : '...'}
                                {m.senderId === user.uid && <CheckCheck size={10} />}
                            </div>
                        </div>
                    </div>
                ))}
                <div ref={chatEndRef} />
            </div>

            <form onSubmit={sendMessage} className={`p-5 border-t ${isDarkMode ? 'border-gray-700' : 'border-gray-100'}`}>
                <div className={`flex items-center gap-3 px-4 py-2.5 rounded-[20px] transition-all focus-within:ring-2 ring-blue-500/20 ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-[#f0f2f5]'}`}>
                    <button type="button" className="text-gray-400 hover:text-blue-500 transition-colors"><Paperclip size={20}/></button>
                    <input 
                        value={newMessage}
                        onChange={e => setNewMessage(e.target.value)}
                        placeholder="On dit quoi ?" 
                        className="flex-1 bg-transparent py-2 text-sm outline-none font-bold placeholder-gray-400" 
                    />
                    <button type="submit" disabled={!newMessage.trim()} className={`transition-all p-2 rounded-xl ${newMessage.trim() ? 'bg-blue-600 text-white shadow-lg scale-110' : 'text-gray-300'}`}>
                        <Send size={18}/>
                    </button>
                </div>
            </form>
        </div>
      )}

      {/* MODAL MARKETPLACE */}
      {showMarketModal && (
         <div className="fixed inset-0 z-[1000] bg-black/60 backdrop-blur-sm flex items-center justify-center p-4 animate-in fade-in duration-300">
            <div className={`w-full max-w-md rounded-[40px] overflow-hidden shadow-2xl animate-in zoom-in-95 duration-300 ${isDarkMode ? 'bg-[#242526]' : 'bg-white'}`}>
                <div className="p-8 border-b dark:border-gray-700 flex items-center justify-between bg-gradient-to-r from-[#36a100] to-green-500 text-white">
                    <h3 className="text-xl font-black uppercase tracking-widest">Mettre en vente</h3>
                    <button onClick={() => setShowMarketModal(false)} className="p-2 hover:bg-white/20 rounded-full transition-colors"><X/></button>
                </div>
                <form onSubmit={handleMarketSubmit} className="p-8 space-y-5">
                    <div className="space-y-2">
                        <label className="text-[10px] font-black uppercase text-gray-400 ml-2 tracking-widest">C'est quoi ?</label>
                        <input 
                          required
                          value={marketForm.title}
                          onChange={e => setMarketForm({...marketForm, title: e.target.value})}
                          placeholder="ex: iPhone 14 Pro Max..." 
                          className={`w-full p-4 rounded-2xl outline-none font-bold ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-gray-100'}`} 
                        />
                    </div>
                    <div className="grid grid-cols-2 gap-4">
                      <div className="space-y-2">
                          <label className="text-[10px] font-black uppercase text-gray-400 ml-2 tracking-widest">Le prix (FCFA)</label>
                          <input 
                            required
                            type="number"
                            value={marketForm.price}
                            onChange={e => setMarketForm({...marketForm, price: e.target.value})}
                            placeholder="ex: 500.000" 
                            className={`w-full p-4 rounded-2xl outline-none font-bold ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-gray-100'}`} 
                          />
                      </div>
                      <div className="space-y-2">
                          <label className="text-[10px] font-black uppercase text-gray-400 ml-2 tracking-widest">Catégorie</label>
                          <select 
                            value={marketForm.category}
                            onChange={e => setMarketForm({...marketForm, category: e.target.value})}
                            className={`w-full p-4 rounded-2xl outline-none font-bold appearance-none ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-gray-100'}`}
                          >
                            <option>Électronique</option>
                            <option>Mode</option>
                            <option>Immobilier</option>
                            <option>Services</option>
                          </select>
                      </div>
                    </div>
                    <button className="w-full bg-[#36a100] text-white py-5 rounded-[24px] font-black uppercase text-sm tracking-widest shadow-xl shadow-green-200 mt-4 active:scale-95 transition-all">
                        Balayer l'annonce
                    </button>
                </form>
            </div>
         </div>
      )}

      {/* NAVIGATION MOBILE OPTIMISÉE */}
      <div className={`lg:hidden fixed bottom-0 w-full h-22 flex items-center justify-around border-t z-[100] px-6 pb-4 ${isDarkMode ? 'bg-[#242526]/90 backdrop-blur border-gray-700' : 'bg-white/90 backdrop-blur border-gray-100 shadow-[0_-10px_30px_rgba(0,0,0,0.05)]'}`}>
          <button onClick={() => setActiveTab('home')} className={`flex flex-col items-center gap-1.5 transition-all ${activeTab === 'home' ? 'text-[#007fff] scale-110' : 'text-gray-400'}`}>
              <div className={`p-2 rounded-xl ${activeTab === 'home' ? 'bg-blue-50' : ''}`}><HomeIcon size={22} /></div>
              <span className="text-[8px] font-black uppercase tracking-tighter">Flux</span>
          </button>
          <button onClick={() => setActiveTab('market')} className={`flex flex-col items-center gap-1.5 transition-all ${activeTab === 'market' ? 'text-[#36a100] scale-110' : 'text-gray-400'}`}>
              <div className={`p-2 rounded-xl ${activeTab === 'market' ? 'bg-green-50' : ''}`}><Store size={22} /></div>
              <span className="text-[8px] font-black uppercase tracking-tighter">Market</span>
          </button>
          <div className="mb-10">
              <button 
                onClick={() => activeTab === 'home' ? setShowImageInput(true) : setShowMarketModal(true)}
                className="w-16 h-16 bg-gradient-to-tr from-[#36a100] via-yellow-400 to-[#007fff] rounded-[24px] flex items-center justify-center text-white shadow-2xl active:scale-90 transition-all border-4 border-white dark:border-[#242526] rotate-45"
              >
                  <Plus size={32} className="-rotate-45" />
              </button>
          </div>
          <button className="flex flex-col items-center gap-1.5 text-gray-400">
              <div className="p-2 rounded-xl"><Tv size={22} /></div>
              <span className="text-[8px] font-black uppercase tracking-tighter">Watch</span>
          </button>
          <button className="flex flex-col items-center gap-1.5 text-gray-400 relative">
              <div className="p-2 rounded-xl"><BellRing size={22} /></div>
              <span className="text-[8px] font-black uppercase tracking-tighter">Alertes</span>
              <div className="absolute top-2 right-2 w-2 h-2 bg-red-500 rounded-full border-2 border-white"></div>
          </button>
      </div>

    </div>
  );
};

export default App;
