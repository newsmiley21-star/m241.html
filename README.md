import React, { useState, useEffect, useRef, useCallback } from 'react';
import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  onAuthStateChanged, 
  signInAnonymously,
  signInWithCustomToken,
  signOut
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
  deleteDoc,
  updateDoc,
  arrayUnion,
  arrayRemove
} from 'firebase/firestore';
import { 
  Send, X, Phone, Video, Info, CheckCheck, 
  MessageCircle, Users, Store, Home, Moon, 
  Sun, LogOut, Plus, Search, UserPlus, Bell, 
  MoreHorizontal, ThumbsUp, MessageSquare, Share2,
  Image as ImageIcon, Video as VideoIcon, Smile,
  UserMinus, Settings, Menu, Flag, Heart, Loader2
} from 'lucide-react';

// --- CONFIGURATION FIREBASE ---
const firebaseConfig = JSON.parse(__firebase_config);
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'fb241-prod-v1';

// --- COMPOSANT PRINCIPAL ---
const App = () => {
  // États de l'application
  const [user, setUser] = useState(null);
  const [isDarkMode, setIsDarkMode] = useState(() => {
    const saved = localStorage.getItem('fb241_theme');
    return saved === 'dark';
  });
  const [activeTab, setActiveTab] = useState('feed'); 
  const [selectedFriend, setSelectedFriend] = useState(null);
  const [isChatOpen, setIsChatOpen] = useState(false);
  const [loading, setLoading] = useState(true);
  const [actionLoading, setActionLoading] = useState(false);

  // États des données
  const [posts, setPosts] = useState([]);
  const [allUsers, setAllUsers] = useState([]);
  const [friendRequests, setFriendRequests] = useState([]);
  const [myFriends, setMyFriends] = useState([]);
  const [chatMessages, setChatMessages] = useState([]);
  const [newMessage, setNewMessage] = useState('');
  const [postText, setPostText] = useState('');

  const chatEndRef = useRef(null);

  // Persistance du thème
  useEffect(() => {
    localStorage.setItem('fb241_theme', isDarkMode ? 'dark' : 'light');
    if (isDarkMode) document.documentElement.classList.add('dark');
    else document.documentElement.classList.remove('dark');
  }, [isDarkMode]);

  // --- LOGIQUE D'AUTHENTIFICATION ---
  useEffect(() => {
    const initAuth = async (retryCount = 0) => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        if (retryCount < 5) {
          const delay = Math.pow(2, retryCount) * 1000;
          setTimeout(() => initAuth(retryCount + 1), delay);
        }
      }
    };

    initAuth();

    const unsubscribeAuth = onAuthStateChanged(auth, async (u) => {
      setUser(u);
      setLoading(false);
      if (u) {
        // Enregistrement/Mise à jour du profil utilisateur
        try {
          await setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
            uid: u.uid,
            displayName: u.displayName || `Citoyen_${u.uid.slice(0, 5)}`,
            photoURL: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`,
            lastSeen: serverTimestamp(),
          }, { merge: true });
        } catch (e) { console.error("Erreur profil:", e); }
      }
    });

    return () => unsubscribeAuth();
  }, []);

  // --- SYNCHRONISATION DES DONNÉES TEMPS RÉEL ---
  useEffect(() => {
    if (!user) return;

    // 1. Flux de publications
    const unsubPosts = onSnapshot(
      collection(db, 'artifacts', appId, 'public', 'data', 'posts'), 
      (snap) => {
        const p = snap.docs.map(d => ({ id: d.id, ...d.data() }));
        setPosts(p.sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0)));
      },
      (err) => console.error("Erreur flux posts:", err)
    );

    // 2. Annuaire des utilisateurs
    const unsubUsers = onSnapshot(
      collection(db, 'artifacts', appId, 'users'), 
      (snap) => {
        setAllUsers(snap.docs.map(d => d.data()).filter(u => u.uid !== user.uid));
      },
      (err) => console.error("Erreur flux utilisateurs:", err)
    );

    // 3. Demandes d'amis entrantes
    const unsubReqs = onSnapshot(
      collection(db, 'artifacts', appId, 'users', user.uid, 'friendRequests'), 
      (snap) => {
        setFriendRequests(snap.docs.map(d => ({ id: d.id, ...d.data() })));
      },
      (err) => console.error("Erreur flux requêtes:", err)
    );

    // 4. Liste d'amis (Profil)
    const unsubMe = onSnapshot(
      doc(db, 'artifacts', appId, 'users', user.uid), 
      (doc) => {
        if (doc.exists()) setMyFriends(doc.data().friends || []);
      },
      (err) => console.error("Erreur flux profil:", err)
    );

    return () => { unsubPosts(); unsubUsers(); unsubReqs(); unsubMe(); };
  }, [user]);

  // --- SYNCHRONISATION DU CHAT ---
  useEffect(() => {
    if (!user || !selectedFriend || !isChatOpen) return;
    const chatId = [user.uid, selectedFriend.uid].sort().join('_');
    const unsub = onSnapshot(
      collection(db, 'artifacts', appId, 'public', 'data', 'chats', chatId, 'messages'), 
      (snap) => {
        const msgs = snap.docs.map(d => ({ id: d.id, ...d.data() }));
        setChatMessages(msgs.sort((a, b) => (a.createdAt?.seconds || 0) - (b.createdAt?.seconds || 0)));
        setTimeout(() => chatEndRef.current?.scrollIntoView({ behavior: 'smooth' }), 50);
      },
      (err) => console.error("Erreur flux chat:", err)
    );
    return () => unsub();
  }, [user, selectedFriend, isChatOpen]);

  // --- ACTIONS ACTIONS ---
  const handlePost = async () => {
    if (!postText.trim() || actionLoading) return;
    setActionLoading(true);
    try {
      await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'posts'), {
        authorId: user.uid,
        authorName: user.displayName || 'Citoyen',
        authorAvatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${user.uid}`,
        content: postText,
        likes: 0,
        createdAt: serverTimestamp()
      });
      setPostText('');
      setActiveTab('feed');
    } catch (e) {
      console.error("Erreur publication:", e);
    } finally {
      setActionLoading(false);
    }
  };

  const sendFriendRequest = async (target) => {
    try {
      await setDoc(doc(db, 'artifacts', appId, 'users', target.uid, 'friendRequests', user.uid), {
        fromId: user.uid,
        fromName: user.displayName || 'Citoyen',
        timestamp: serverTimestamp()
      });
    } catch (e) { console.error(e); }
  };

  const acceptFriend = async (req) => {
    try {
      const myDoc = doc(db, 'artifacts', appId, 'users', user.uid);
      const theirDoc = doc(db, 'artifacts', appId, 'users', req.fromId);
      await updateDoc(myDoc, { friends: arrayUnion(req.fromId) });
      await updateDoc(theirDoc, { friends: arrayUnion(user.uid) });
      await deleteDoc(doc(db, 'artifacts', appId, 'users', user.uid, 'friendRequests', req.fromId));
    } catch (e) { console.error(e); }
  };

  const removeFriend = async (friendId) => {
    try {
      await updateDoc(doc(db, 'artifacts', appId, 'users', user.uid), { friends: arrayRemove(friendId) });
      await updateDoc(doc(db, 'artifacts', appId, 'users', friendId), { friends: arrayRemove(user.uid) });
    } catch (e) { console.error(e); }
  };

  const sendMessage = async (e) => {
    e.preventDefault();
    if (!newMessage.trim() || !selectedFriend) return;
    const text = newMessage;
    setNewMessage('');
    try {
      const chatId = [user.uid, selectedFriend.uid].sort().join('_');
      await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', chatId, 'messages'), {
        text: text,
        senderId: user.uid,
        createdAt: serverTimestamp()
      });
    } catch (e) { console.error(e); }
  };

  if (loading) return (
    <div className="h-screen flex items-center justify-center bg-[#F0F2F5] dark:bg-[#18191a]">
      <div className="flex flex-col items-center">
        <div className="w-16 h-16 bg-[#1877F2] rounded-2xl flex items-center justify-center mb-6 shadow-xl animate-bounce">
          <span className="text-white font-bold text-2xl italic">241</span>
        </div>
        <Loader2 className="animate-spin text-[#1877F2]" size={32} />
      </div>
    </div>
  );

  return (
    <div className={`min-h-screen transition-colors duration-300 ${isDarkMode ? 'bg-[#18191a] text-[#e4e6eb]' : 'bg-[#f0f2f5] text-[#1c1e21]'}`}>
      
      {/* BARRE DE NAVIGATION SUPÉRIEURE */}
      <header className={`h-14 px-4 flex items-center justify-between sticky top-0 z-[100] border-b shadow-md transition-colors ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200'}`}>
        <div className="flex items-center gap-2">
          <div className="w-10 h-10 bg-[#1877F2] rounded-full flex items-center justify-center shadow-lg active:scale-90 transition-transform cursor-pointer">
            <span className="text-white font-black text-xl italic leading-none">f</span>
          </div>
          <div className={`flex items-center gap-2 px-3 py-2 rounded-full hidden sm:flex ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-[#f0f2f5]'}`}>
            <Search size={18} className="text-gray-500" />
            <input placeholder="Rechercher sur Facebook" className="bg-transparent border-none outline-none text-sm w-48" />
          </div>
        </div>

        {/* ONGLETS DESKTOP */}
        <nav className="hidden md:flex items-center gap-2 h-full">
          <TabButton icon={<Home size={28} />} active={activeTab === 'feed'} onClick={() => setActiveTab('feed')} />
          <TabButton icon={<Users size={28} />} active={activeTab === 'friends'} onClick={() => setActiveTab('friends')} count={friendRequests.length} />
          <TabButton icon={<VideoIcon size={28} />} active={false} />
          <TabButton icon={<Store size={28} />} active={false} />
        </nav>

        <div className="flex items-center gap-2">
          <div className="hidden lg:flex items-center gap-2 mr-2 p-1.5 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 cursor-pointer">
            <img src={`https://api.dicebear.com/7.x/avataaars/svg?seed=${user?.uid}`} className="w-7 h-7 rounded-full border border-gray-300" alt="me" />
            <span className="text-xs font-bold pr-1">{user?.displayName?.split('_')[0]}</span>
          </div>
          <HeaderIcon icon={<Menu size={20}/>} />
          <HeaderIcon icon={<MessageSquare size={20}/>} onClick={() => setActiveTab('friends')} />
          <HeaderIcon icon={<Bell size={20}/>} />
          <HeaderIcon icon={isDarkMode ? <Sun size={20}/> : <Moon size={20}/>} onClick={() => setIsDarkMode(!isDarkMode)} />
        </div>
      </header>

      <div className="max-w-7xl mx-auto flex gap-6 px-4 py-4">
        
        {/* COLONNE GAUCHE (SIDEBAR) */}
        <aside className="hidden lg:flex flex-col w-72 sticky top-[72px] h-[calc(100vh-72px)] overflow-y-auto pr-2 space-y-1">
          <div className="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-gray-800 cursor-pointer">
            <img src={`https://api.dicebear.com/7.x/avataaars/svg?seed=${user?.uid}`} className="w-9 h-9 rounded-full bg-white border" alt="me" />
            <span className="font-bold text-sm">{user?.displayName}</span>
          </div>
          <SidebarItem icon={<Users className="text-blue-500" />} label="Amis" onClick={() => setActiveTab('friends')} active={activeTab === 'friends'} />
          <SidebarItem icon={<MessageCircle className="text-green-500" />} label="Messenger" />
          <SidebarItem icon={<Flag className="text-orange-500" />} label="Pages" />
          <SidebarItem icon={<Store className="text-blue-400" />} label="Marketplace" />
          <SidebarItem icon={<Home className="text-blue-600" />} label="Fil d'actualité" onClick={() => setActiveTab('feed')} active={activeTab === 'feed'} />
          
          <hr className="my-4 border-gray-300 dark:border-gray-700" />
          <button onClick={() => signOut(auth)} className="flex items-center gap-3 p-3 rounded-lg hover:bg-red-50 dark:hover:bg-red-900/20 text-red-500 font-bold text-sm w-full transition-colors">
            <LogOut size={20} /> Se déconnecter
          </button>
        </aside>

        {/* CONTENU CENTRAL (FEED) */}
        <main className="flex-1 max-w-[600px] mx-auto space-y-5 pb-24">
          
          {activeTab === 'feed' && (
            <>
              {/* STORIES SECTION */}
              <div className="flex gap-2 overflow-x-auto pb-2 scrollbar-hide">
                <div className={`min-w-[110px] h-48 rounded-xl overflow-hidden relative shadow-sm cursor-pointer border ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200'}`}>
                  <img src={`https://api.dicebear.com/7.x/avataaars/svg?seed=${user?.uid}`} className="w-full h-[70%] object-cover" alt="story" />
                  <div className="absolute bottom-0 w-full h-[30%] bg-white dark:bg-[#242526] flex flex-col items-center">
                    <div className="w-8 h-8 bg-[#1877F2] rounded-full border-4 border-white dark:border-[#242526] -mt-4 flex items-center justify-center text-white"><Plus size={18}/></div>
                    <span className="text-[10px] font-bold mt-1">Créer une story</span>
                  </div>
                </div>
                {[101, 102, 103].map(id => (
                  <div key={id} className="min-w-[110px] h-48 rounded-xl bg-gray-300 relative overflow-hidden group cursor-pointer shadow-sm border dark:border-gray-700">
                    <img src={`https://picsum.photos/200/400?random=${id}`} className="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110" alt="story" />
                    <div className="absolute top-2 left-2 w-8 h-8 rounded-full border-2 border-[#1877F2] overflow-hidden bg-white shadow-md">
                      <img src={`https://api.dicebear.com/7.x/avataaars/svg?seed=user_${id}`} alt="user" />
                    </div>
                  </div>
                ))}
              </div>

              {/* BARRE DE PUBLICATION */}
              <div className={`p-4 rounded-xl shadow-sm border ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200'}`}>
                <div className="flex gap-3 mb-4">
                  <img src={`https://api.dicebear.com/7.x/avataaars/svg?seed=${user?.uid}`} className="w-10 h-10 rounded-full bg-white border" alt="me" />
                  <button 
                    onClick={() => setActiveTab('create_post')}
                    className={`flex-1 rounded-full px-4 text-left text-gray-500 text-sm hover:bg-gray-100 dark:hover:bg-[#3a3b3c] transition-colors ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-[#f0f2f5]'}`}
                  >
                    Que voulez-vous dire, {user?.displayName?.split('_')[0]} ?
                  </button>
                </div>
                <hr className={`mb-3 ${isDarkMode ? 'border-gray-700' : 'border-gray-200'}`} />
                <div className="flex items-center justify-around">
                  <ActionButton icon={<VideoIcon className="text-red-500" />} label="Direct" />
                  <ActionButton icon={<ImageIcon className="text-green-500" />} label="Photo" />
                  <ActionButton icon={<Smile className="text-yellow-500" />} label="Humeur" />
                </div>
              </div>

              {/* LISTE DES PUBLICATIONS */}
              {posts.length === 0 ? (
                <div className="text-center py-20 opacity-50">
                   <div className="w-16 h-16 bg-gray-200 dark:bg-gray-800 rounded-full mx-auto flex items-center justify-center mb-4">
                      <MessageSquare size={32} />
                   </div>
                   <p className="text-sm font-medium">Aucune publication pour le moment.</p>
                </div>
              ) : posts.map(post => (
                <div key={post.id} className={`rounded-xl shadow-sm border overflow-hidden animate-in fade-in slide-in-from-bottom-4 duration-300 ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200'}`}>
                  <div className="p-4 flex items-center justify-between">
                    <div className="flex gap-3">
                      <img src={post.authorAvatar} className="w-10 h-10 rounded-full border bg-white" alt="author" />
                      <div>
                        <h4 className="font-bold text-sm leading-none mb-1 hover:underline cursor-pointer">{post.authorName}</h4>
                        <p className="text-[11px] text-gray-500 flex items-center gap-1">
                          {post.createdAt ? new Date(post.createdAt.seconds * 1000).toLocaleDateString('fr-FR', { hour: '2-digit', minute: '2-digit' }) : 'À l\'instant'} • <Home size={10} />
                        </p>
                      </div>
                    </div>
                    <button className="p-2 hover:bg-gray-100 dark:hover:bg-gray-800 rounded-full transition-colors"><MoreHorizontal size={20}/></button>
                  </div>
                  <div className="px-4 py-2 text-sm leading-relaxed whitespace-pre-wrap">{post.content}</div>
                  <div className="px-4 py-3 flex items-center justify-between border-b dark:border-gray-700 mx-4">
                    <div className="flex items-center gap-1">
                      <div className="bg-blue-500 rounded-full p-1"><ThumbsUp size={10} className="text-white" fill="white" /></div>
                      <span className="text-xs text-gray-500">{post.likes || 0}</span>
                    </div>
                    <div className="flex gap-3 text-[11px] text-gray-500">
                      <span>{Math.floor(Math.random() * 5)} comm.</span>
                      <span>Partagé</span>
                    </div>
                  </div>
                  <div className="flex items-center justify-around p-1 mx-4">
                    <PostAction icon={<ThumbsUp size={20}/>} label="J'aime" />
                    <PostAction icon={<MessageSquare size={20}/>} label="Commenter" />
                    <PostAction icon={<Share2 size={20}/>} label="Partager" />
                  </div>
                </div>
              ))}
            </>
          )}

          {activeTab === 'friends' && (
            <div className="space-y-6">
              <h2 className="text-xl font-bold px-2">Social & Amis</h2>
              
              {/* Invitations en attente */}
              {friendRequests.length > 0 && (
                <div className="space-y-3">
                  <h3 className="text-sm font-bold text-gray-500 px-2 uppercase tracking-wide">Invitations</h3>
                  {friendRequests.map(req => (
                    <div key={req.id} className={`p-4 rounded-xl border flex items-center justify-between shadow-sm animate-pulse-slow ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200'}`}>
                      <div className="flex items-center gap-3">
                        <img src={`https://api.dicebear.com/7.x/avataaars/svg?seed=${req.fromId}`} className="w-14 h-14 rounded-full border bg-white" alt="avatar" />
                        <div>
                          <p className="font-bold text-sm">{req.fromName}</p>
                          <p className="text-[10px] text-gray-500 uppercase">Nouvelle demande</p>
                        </div>
                      </div>
                      <div className="flex gap-2">
                        <button onClick={() => acceptFriend(req)} className="bg-[#1877F2] text-white px-4 py-2 rounded-lg font-bold text-xs hover:bg-[#1565c0]">Confirmer</button>
                        <button className={`px-4 py-2 rounded-lg font-bold text-xs ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-gray-200'}`}>Refuser</button>
                      </div>
                    </div>
                  ))}
                </div>
              )}

              {/* Suggestions d'amis */}
              <div className="space-y-3">
                <h3 className="text-sm font-bold text-gray-500 px-2 uppercase tracking-wide">Personnes que vous pourriez connaître</h3>
                <div className="grid grid-cols-2 gap-3">
                  {allUsers.filter(u => !myFriends.includes(u.uid)).map(su => (
                    <div key={su.uid} className={`rounded-xl border overflow-hidden group shadow-sm transition-all hover:shadow-md ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200'}`}>
                      <img src={`https://picsum.photos/400/300?random=${su.uid}`} className="w-full h-32 object-cover group-hover:brightness-90 transition-all" alt="cover" />
                      <div className="p-3">
                        <p className="font-bold text-sm mb-3 truncate">{su.displayName}</p>
                        <button onClick={() => sendFriendRequest(su)} className="w-full bg-blue-100 dark:bg-blue-900/30 text-[#1877F2] py-2 rounded-lg font-bold text-xs flex items-center justify-center gap-2 hover:bg-blue-200 transition-colors">
                          <UserPlus size={16} /> Ajouter
                        </button>
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            </div>
          )}

        </main>

        {/* COLONNE DROITE (CONTACTS) */}
        <aside className="hidden xl:flex flex-col w-72 sticky top-[72px] h-[calc(100vh-72px)] space-y-4">
           <div>
             <div className="flex items-center justify-between px-2 mb-4">
                <h4 className="text-gray-500 font-bold text-sm uppercase tracking-wider">Contacts</h4>
                <div className="flex gap-3 text-gray-500">
                  <VideoIcon size={16} className="cursor-pointer hover:text-[#1877F2]" />
                  <Search size={16} className="cursor-pointer hover:text-[#1877F2]" />
                  <MoreHorizontal size={16} className="cursor-pointer hover:text-[#1877F2]" />
                </div>
             </div>
             <div className="space-y-1">
               {myFriends.length === 0 ? (
                 <p className="text-[11px] text-gray-500 italic px-2">Vos amis apparaîtront ici dès qu'ils accepteront votre demande.</p>
               ) : myFriends.map(fid => {
                 const f = allUsers.find(u => u.uid === fid);
                 if (!f) return null;
                 return (
                   <div 
                    key={fid} 
                    onClick={() => { setSelectedFriend(f); setIsChatOpen(true); }} 
                    className="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-gray-800 cursor-pointer relative group transition-colors"
                   >
                     <div className="relative">
                       <img src={f.photoURL} className="w-9 h-9 rounded-full bg-white border border-gray-300" alt="friend" />
                       <div className="absolute bottom-0 right-0 w-3 h-3 bg-green-500 border-2 border-white dark:border-[#242526] rounded-full"></div>
                     </div>
                     <span className="text-sm font-medium">{f.displayName}</span>
                     <button onClick={(e) => { e.stopPropagation(); removeFriend(f.uid); }} className="absolute right-2 opacity-0 group-hover:opacity-100 p-1 text-red-400 hover:scale-110 transition-all"><UserMinus size={14}/></button>
                   </div>
                 );
               })}
             </div>
           </div>
        </aside>
      </div>

      {/* CHAT FLOTTANT (DESKTOP) */}
      {isChatOpen && selectedFriend && (
        <div className={`fixed bottom-0 right-10 w-80 h-[450px] shadow-2xl rounded-t-xl border flex flex-col z-[200] animate-in slide-in-from-bottom-10 duration-300 ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200'}`}>
          <div className="p-3 border-b flex items-center justify-between shadow-sm">
            <div className="flex items-center gap-2">
               <div className="relative">
                 <img src={selectedFriend.photoURL} className="w-8 h-8 rounded-full bg-white border" alt="friend" />
                 <div className="absolute bottom-0 right-0 w-2.5 h-2.5 bg-green-500 border-2 border-white dark:border-[#242526] rounded-full"></div>
               </div>
               <div>
                 <p className="text-sm font-bold leading-none mb-1">{selectedFriend.displayName}</p>
                 <p className="text-[10px] text-gray-500">Actif il y a 2m</p>
               </div>
            </div>
            <div className="flex items-center gap-3 text-[#1877F2]">
              <Phone size={18} className="cursor-not-allowed opacity-50"/>
              <Video size={18} className="cursor-not-allowed opacity-50"/>
              <X size={20} className="cursor-pointer text-gray-400 hover:text-red-500 transition-colors" onClick={() => setIsChatOpen(false)}/>
            </div>
          </div>
          <div className="flex-1 overflow-y-auto p-4 space-y-3 bg-[#F0F2F5] dark:bg-[#1c1e21]">
             {chatMessages.length === 0 && <p className="text-center text-[10px] text-gray-400 mt-20">Dites bonjour à {selectedFriend.displayName} !</p>}
             {chatMessages.map(m => (
               <div key={m.id} className={`flex ${m.senderId === user.uid ? 'justify-end' : 'justify-start'}`}>
                  <div className={`max-w-[85%] p-2.5 px-3.5 rounded-2xl text-[13px] shadow-sm ${m.senderId === user.uid ? 'bg-[#1877F2] text-white rounded-tr-none' : 'bg-white dark:bg-[#3a3b3c] rounded-tl-none'}`}>
                    {m.text}
                  </div>
               </div>
             ))}
             <div ref={chatEndRef} />
          </div>
          <form onSubmit={sendMessage} className="p-3 flex items-center gap-2 border-t dark:border-gray-700">
            <Plus size={20} className="text-[#1877F2] cursor-pointer" />
            <input 
              value={newMessage} 
              onChange={(e) => setNewMessage(e.target.value)}
              placeholder="Aa" 
              className={`flex-1 p-2 px-4 rounded-full text-sm outline-none transition-all focus:ring-1 focus:ring-blue-400 ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-[#f0f2f5]'}`} 
            />
            <button type="submit" className={`transition-all ${newMessage.trim() ? 'text-[#1877F2] scale-110' : 'text-gray-300'}`} disabled={!newMessage.trim()}>
              <Send size={20}/>
            </button>
          </form>
        </div>
      )}

      {/* MODAL DE CRÉATION DE POST */}
      {activeTab === 'create_post' && (
        <div className="fixed inset-0 z-[300] bg-white/60 dark:bg-black/80 backdrop-blur-md flex items-center justify-center p-4">
          <div className={`w-full max-w-lg rounded-xl shadow-2xl border animate-in zoom-in-95 duration-200 ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-200'}`}>
            <div className="p-4 border-b text-center relative">
              <h3 className="font-bold text-lg">Créer une publication</h3>
              <button onClick={() => setActiveTab('feed')} className="absolute right-4 top-4 p-2 bg-gray-100 dark:bg-gray-800 rounded-full hover:bg-gray-200 transition-colors"><X size={20}/></button>
            </div>
            <div className="p-4">
               <div className="flex items-center gap-3 mb-4">
                 <img src={`https://api.dicebear.com/7.x/avataaars/svg?seed=${user?.uid}`} className="w-10 h-10 rounded-full border border-gray-300" alt="me" />
                 <div>
                   <p className="font-bold text-sm">{user?.displayName}</p>
                   <div className={`flex items-center gap-1 text-[10px] font-bold px-2 py-0.5 rounded-md ${isDarkMode ? 'bg-[#3a3b3c]' : 'bg-gray-200'}`}>🌍 Public</div>
                 </div>
               </div>
               <textarea 
                 autoFocus
                 value={postText}
                 onChange={(e) => setPostText(e.target.value)}
                 className="w-full h-44 bg-transparent border-none outline-none resize-none text-xl placeholder:text-gray-400 leading-snug"
                 placeholder={`Quoi de neuf, ${user?.displayName?.split('_')[0]} ?`}
               ></textarea>
               <div className="flex items-center justify-between p-3 border rounded-lg mb-4 dark:border-gray-700">
                 <span className="text-xs font-bold text-gray-500">Ajouter à votre publication</span>
                 <div className="flex gap-4">
                   <ImageIcon className="text-green-500 cursor-pointer active:scale-90" />
                   <UserPlus className="text-blue-500 cursor-pointer active:scale-90" />
                   <Smile className="text-yellow-500 cursor-pointer active:scale-90" />
                 </div>
               </div>
               <button 
                onClick={handlePost}
                disabled={!postText.trim() || actionLoading}
                className={`w-full py-3 rounded-lg font-bold text-sm transition-all flex items-center justify-center gap-2 ${postText.trim() && !actionLoading ? 'bg-[#1877F2] text-white shadow-lg active:scale-95' : 'bg-gray-200 text-gray-400 dark:bg-[#3a3b3c] cursor-not-allowed'}`}
               >
                 {actionLoading ? <Loader2 className="animate-spin" size={18} /> : 'Publier'}
               </button>
            </div>
          </div>
        </div>
      )}

      {/* NAVIGATION BASSE (MOBILE) */}
      <nav className={`md:hidden fixed bottom-0 left-0 right-0 h-16 flex items-center justify-around border-t z-[250] shadow-2xl transition-colors ${isDarkMode ? 'bg-[#242526] border-gray-700' : 'bg-white border-gray-100'}`}>
        <MobileTab icon={<Home size={24} />} label="Accueil" active={activeTab === 'feed'} onClick={() => setActiveTab('feed')} />
        <MobileTab icon={<Users size={24} />} label="Amis" active={activeTab === 'friends'} onClick={() => setActiveTab('friends')} count={friendRequests.length} />
        <button 
          onClick={() => setActiveTab('create_post')}
          className="w-12 h-12 bg-[#1877F2] rounded-full flex items-center justify-center text-white -mt-10 border-4 border-[#f0f2f5] dark:border-[#18191a] shadow-xl active:scale-90 transition-transform"
        >
          <Plus size={28} />
        </button>
        <MobileTab icon={<VideoIcon size={24} />} label="Watch" />
        <MobileTab icon={<Menu size={24} />} label="Menu" />
      </nav>

    </div>
  );
};

// --- SOUS-COMPOSANTS REUTILISABLES ---

const TabButton = ({ icon, active, onClick, count = 0 }) => (
  <button 
    onClick={onClick}
    className={`h-full px-10 border-b-4 transition-all relative group ${active ? 'border-[#1877F2] text-[#1877F2]' : 'border-transparent text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-800'}`}
  >
    {icon}
    {count > 0 && <span className="absolute top-1.5 right-6 bg-red-500 text-white text-[10px] font-bold px-1.5 rounded-full border-2 border-white dark:border-[#242526]">{count}</span>}
    {!active && <div className="absolute bottom-1 left-1/2 -translate-x-1/2 w-0 h-1 bg-gray-300 group-hover:w-1/2 transition-all"></div>}
  </button>
);

const HeaderIcon = ({ icon, onClick }) => (
  <button 
    onClick={onClick}
    className={`p-2.5 rounded-full transition-all active:scale-90 ${localStorage.getItem('fb241_theme') === 'dark' ? 'bg-[#3a3b3c] hover:bg-[#4e4f50]' : 'bg-[#e4e6eb] hover:bg-[#d8dadf]'}`}
  >
    {icon}
  </button>
);

const SidebarItem = ({ icon, label, onClick, active }) => (
  <div 
    onClick={onClick}
    className={`flex items-center gap-3 p-3 rounded-lg cursor-pointer transition-all ${active ? 'bg-blue-50 dark:bg-blue-900/20 text-[#1877F2]' : 'hover:bg-gray-200 dark:hover:bg-gray-800'}`}
  >
    <div className="w-8 h-8 flex items-center justify-center rounded-full overflow-hidden">
      {icon}
    </div>
    <span className="text-[13px] font-semibold">{label}</span>
  </div>
);

const MobileTab = ({ icon, label, active, onClick, count = 0 }) => (
  <button onClick={onClick} className={`flex flex-col items-center gap-1 flex-1 relative ${active ? 'text-[#1877F2]' : 'text-gray-400'}`}>
    {icon}
    <span className="text-[9px] font-bold uppercase">{label}</span>
    {count > 0 && <span className="absolute top-0 right-1/4 bg-red-500 text-white text-[9px] px-1.5 rounded-full border border-white">{count}</span>}
  </button>
);

const ActionButton = ({ icon, label }) => (
  <button className="flex-1 flex items-center justify-center gap-2 py-2 hover:bg-gray-100 dark:hover:bg-[#3a3b3c] rounded-lg transition-colors text-gray-500 font-bold text-xs">
    {icon} {label}
  </button>
);

const PostAction = ({ icon, label }) => (
  <button className="flex-1 flex items-center justify-center gap-2 p-2 rounded-md hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-500 font-bold text-xs transition-colors active:scale-95">
    {icon} <span className="hidden sm:inline">{label}</span>
  </button>
);

export default App;
