<!doctype html>
<html lang="id" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SIZKA - Sistem Izin Kesehatan</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script src="https://cdn.jsdelivr.net/npm/lucide@0.263.0/dist/umd/lucide.min.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="/_sdk/data_sdk.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&amp;display=swap" rel="stylesheet">
  <script>
tailwind.config = {
  theme: {
    extend: {
      fontFamily: { jakarta: ['Plus Jakarta Sans', 'sans-serif'] },
      colors: {
        brand: { 50:'#eef7ff',100:'#d9edff',200:'#bce0ff',300:'#8ecdff',400:'#53b1ff',500:'#2a91ff',600:'#1570f5',700:'#1059e1',800:'#1248b6',900:'#13408f' },
        mint: { 50:'#effef4',100:'#d9ffe7',200:'#b5fdd0',300:'#7cf8ab',400:'#3ce97e',500:'#14d05c',600:'#0aad49',700:'#0d873d',800:'#0e6a34',900:'#10572d' }
      }
    }
  }
}
</script>
  <style>
* { box-sizing: border-box; }
html, body { height: 100%; margin: 0; }
body { font-family: 'Plus Jakarta Sans', sans-serif; }
.page { display: none; }
.page.active { display: flex; flex-direction: column; }
.fade-in { animation: fadeIn 0.3s ease; }
@keyframes fadeIn { from{opacity:0;transform:translateY(8px)} to{opacity:1;transform:translateY(0)} }
@keyframes pulse-ring { 0%{transform:scale(0.8);opacity:1} 100%{transform:scale(1.4);opacity:0} }
.pulse-badge::before { content:''; position:absolute; inset:-4px; border-radius:50%; background:inherit; animation:pulse-ring 1.5s ease infinite; z-index:-1; }
input:focus, select:focus, textarea:focus { outline: none; box-shadow: 0 0 0 3px rgba(42,145,255,0.2); }
.toast { animation: toastIn 0.3s ease, toastOut 0.3s ease 2.7s forwards; }
@keyframes toastIn { from{opacity:0;transform:translateY(-20px)} to{opacity:1;transform:translateY(0)} }
@keyframes toastOut { from{opacity:1} to{opacity:0} }
.card-hover { transition: all 0.2s ease; }
.card-hover:active { transform: scale(0.97); }
::-webkit-scrollbar { width: 4px; }
::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
.permission-letter { background: white; padding: 32px; font-family: 'Times New Roman', serif; }
</style>
  <style>body { box-sizing: border-box; }</style>
 </head>
 <body class="h-full bg-slate-50">
  <div id="app" class="h-full w-full max-w-md mx-auto relative overflow-auto bg-white shadow-xl"><!-- Toast Container -->
   <div id="toast-container" class="fixed top-4 left-1/2 -translate-x-1/2 z-[100] flex flex-col gap-2"></div><!-- HOME PAGE -->
   <div id="page-home" class="page active h-full items-center justify-center px-6" style="background: linear-gradient(135deg, #1059e1 0%, #14d05c 100%);">
    <div class="text-center fade-in"><!-- Logo -->
     <div class="w-28 h-28 mx-auto mb-6 bg-white rounded-3xl shadow-2xl flex items-center justify-center relative" style="box-shadow: 0 20px 60px rgba(0,0,0,0.3);">
      <svg width="64" height="64" viewbox="0 0 64 64" fill="none"><path d="M32 8C22 8 16 14 16 22c0 6 4 10 8 12v4h16v-4c4-2 8-6 8-12 0-8-6-14-16-14z" fill="#1570f5" /> <rect x="22" y="38" width="20" height="4" rx="2" fill="#14d05c" /> <rect x="24" y="44" width="16" height="4" rx="2" fill="#14d05c" /> <path d="M28 22h8v2h-8z" fill="white" /> <path d="M31 19v8" stroke="white" stroke-width="2" /> <circle cx="32" cy="16" r="2" fill="#fff" opacity="0.6" /> <path d="M20 50h24l-2 6H22z" fill="#0d873d" />
      </svg>
     </div>
     <h1 id="home-title" class="text-4xl font-800 text-white tracking-tight mb-2" style="text-shadow:0 2px 10px rgba(0,0,0,0.2);">SIZKA</h1>
     <p id="home-subtitle" class="text-white/80 text-sm font-500 mb-12 tracking-wide">Sistem Izin Kesehatan</p>
     <div class="space-y-4"><button onclick="showLogin('admin')" class="w-full bg-white text-brand-700 font-700 py-4 rounded-2xl shadow-lg card-hover flex items-center justify-center gap-3 text-lg"> <i data-lucide="shield-check" class="w-6 h-6"></i> Administration </button> <button onclick="showLogin('doctor')" class="w-full bg-white/15 backdrop-blur text-white font-700 py-4 rounded-2xl border border-white/30 card-hover flex items-center justify-center gap-3 text-lg"> <i data-lucide="stethoscope" class="w-6 h-6"></i> Doctor </button>
     </div>
     <p class="text-white/40 text-xs mt-10">Puskestren Dulido © 2024</p>
    </div>
   </div><!-- LOGIN PAGE -->
   <div id="page-login" class="page h-full bg-white">
    <div class="p-5 flex items-center gap-3"><button onclick="goHome()" class="w-10 h-10 flex items-center justify-center rounded-xl bg-slate-100 card-hover"> <i data-lucide="arrow-left" class="w-5 h-5 text-slate-600"></i> </button>
     <h2 id="login-title" class="text-lg font-700 text-slate-800">Login</h2>
    </div>
    <div class="flex-1 flex flex-col justify-center px-8 pb-20">
     <div class="fade-in">
      <div id="login-icon-wrap" class="w-20 h-20 mx-auto mb-6 rounded-2xl flex items-center justify-center bg-brand-100"><i data-lucide="user" id="login-icon" class="w-10 h-10 text-brand-600"></i>
      </div>
      <div class="space-y-4">
       <div><label class="text-xs font-600 text-slate-500 mb-1 block">Username</label> <input id="login-user" type="text" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500" placeholder="Enter username">
       </div>
       <div><label class="text-xs font-600 text-slate-500 mb-1 block">Password</label>
        <div class="relative"><input id="login-pass" type="password" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500 pr-12" placeholder="Enter password"> <button onclick="togglePass()" class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-400"> <i data-lucide="eye" id="eye-icon" class="w-5 h-5"></i> </button>
        </div>
       </div>
       <div id="login-error" class="text-red-500 text-sm font-500 hidden">
        Invalid credentials
       </div><button onclick="doLogin()" class="w-full py-3.5 rounded-xl bg-brand-600 text-white font-700 text-lg card-hover shadow-lg shadow-brand-600/30 mt-2"> Sign In </button>
      </div>
     </div>
    </div>
   </div><!-- ADMIN PAGE -->
   <div id="page-admin" class="page h-full bg-slate-50">
    <div class="bg-white p-4 flex items-center justify-between shadow-sm sticky top-0 z-10">
     <div class="flex items-center gap-3"><button onclick="logout()" class="w-10 h-10 flex items-center justify-center rounded-xl bg-slate-100 card-hover"> <i data-lucide="log-out" class="w-5 h-5 text-slate-600"></i> </button>
      <div>
       <h2 class="text-base font-700 text-slate-800">Administration</h2>
       <p class="text-xs text-slate-400">Register new patients</p>
      </div>
     </div><button onclick="showAdminList()" class="relative w-10 h-10 flex items-center justify-center rounded-xl bg-brand-50 card-hover"> <i data-lucide="list" class="w-5 h-5 text-brand-600"></i> <span id="admin-badge" class="absolute -top-1 -right-1 w-5 h-5 bg-red-500 text-white text-[10px] font-700 rounded-full flex items-center justify-center hidden">0</span> </button>
    </div><!-- Admin Form -->
    <div id="admin-form-view" class="flex-1 overflow-auto p-5 fade-in">
     <div class="bg-white rounded-2xl p-5 shadow-sm border border-slate-100">
      <h3 class="font-700 text-slate-800 mb-4 flex items-center gap-2"><i data-lucide="user-plus" class="w-5 h-5 text-brand-600"></i> Register Patient</h3>
      <div class="space-y-4"><!-- Photo -->
       <div class="flex justify-center"><label class="cursor-pointer">
         <div id="photo-preview" class="w-24 h-24 rounded-2xl bg-slate-100 border-2 border-dashed border-slate-300 flex flex-col items-center justify-center text-slate-400 overflow-hidden"><i data-lucide="camera" class="w-8 h-8 mb-1"></i> <span class="text-[10px] font-500">Add Photo</span>
         </div><input type="file" id="photo-input" accept="image/*" class="hidden" onchange="previewPhoto(this)"> </label>
       </div>
       <div><label class="text-xs font-600 text-slate-500 mb-1 block">NIS (Student ID)</label> <input id="admin-nis" type="text" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500" placeholder="Enter NIS">
       </div>
       <div><label class="text-xs font-600 text-slate-500 mb-1 block">Full Name</label> <input id="admin-name" type="text" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500" placeholder="Enter name">
       </div>
       <div class="grid grid-cols-2 gap-3">
        <div><label class="text-xs font-600 text-slate-500 mb-1 block">Class</label> <input id="admin-class" type="text" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500" placeholder="e.g. 10A">
        </div>
        <div><label class="text-xs font-600 text-slate-500 mb-1 block">Room</label> <input id="admin-room" type="text" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500" placeholder="e.g. 201">
        </div>
       </div>
       <div><label class="text-xs font-600 text-slate-500 mb-1 block">Queue Number</label> <input id="admin-queue" type="number" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500" placeholder="Auto" readonly>
       </div><button id="admin-submit-btn" onclick="registerPatient()" class="w-full py-3.5 rounded-xl bg-mint-600 text-white font-700 text-base card-hover shadow-lg shadow-mint-600/30 flex items-center justify-center gap-2"> <i data-lucide="check-circle" class="w-5 h-5"></i> Register Patient </button>
      </div>
     </div>
    </div><!-- Admin Patient List -->
    <div id="admin-list-view" class="flex-1 overflow-auto p-5 hidden fade-in">
     <div class="flex items-center justify-between mb-4">
      <h3 class="font-700 text-slate-800">Registered Patients</h3><button onclick="showAdminForm()" class="text-sm text-brand-600 font-600 flex items-center gap-1"> <i data-lucide="plus" class="w-4 h-4"></i> New </button>
     </div>
     <div id="admin-patient-list" class="space-y-3"></div>
     <div id="admin-empty" class="text-center py-16 text-slate-400 hidden"><i data-lucide="inbox" class="w-12 h-12 mx-auto mb-3 opacity-50"></i>
      <p class="font-500">No patients registered yet</p>
     </div>
    </div>
   </div><!-- DOCTOR PAGE -->
   <div id="page-doctor" class="page h-full bg-slate-50">
    <div class="bg-white p-4 flex items-center justify-between shadow-sm sticky top-0 z-10">
     <div class="flex items-center gap-3"><button onclick="logout()" class="w-10 h-10 flex items-center justify-center rounded-xl bg-slate-100 card-hover"> <i data-lucide="log-out" class="w-5 h-5 text-slate-600"></i> </button>
      <div>
       <h2 class="text-base font-700 text-slate-800">Doctor Panel</h2>
       <p class="text-xs text-slate-400">Examine patients</p>
      </div>
     </div><button onclick="showUntreated()" class="relative w-10 h-10 flex items-center justify-center rounded-xl bg-brand-50 card-hover"> <i data-lucide="message-circle" class="w-5 h-5 text-brand-600"></i> <span id="doc-badge" class="absolute -top-1 -right-1 w-5 h-5 bg-red-500 text-white text-[10px] font-700 rounded-full flex items-center justify-center pulse-badge hidden">0</span> </button>
    </div><!-- Doctor Queue List -->
    <div id="doc-queue-view" class="flex-1 overflow-auto p-5 fade-in">
     <h3 class="font-700 text-slate-800 mb-4">Patient Queue</h3>
     <div id="doc-patient-list" class="space-y-3"></div>
     <div id="doc-empty" class="text-center py-16 text-slate-400 hidden"><i data-lucide="heart-pulse" class="w-12 h-12 mx-auto mb-3 opacity-50"></i>
      <p class="font-500">No patients waiting</p>
     </div>
    </div><!-- Doctor Untreated View -->
    <div id="doc-untreated-view" class="flex-1 overflow-auto p-5 hidden fade-in">
     <div class="flex items-center justify-between mb-4">
      <h3 class="font-700 text-slate-800">Untreated Patients</h3><button onclick="showDocQueue()" class="text-sm text-brand-600 font-600 flex items-center gap-1"> <i data-lucide="arrow-left" class="w-4 h-4"></i> Back </button>
     </div>
     <div id="doc-untreated-list" class="space-y-3"></div>
     <div id="doc-untreated-empty" class="text-center py-16 text-slate-400 hidden"><i data-lucide="check-circle" class="w-12 h-12 mx-auto mb-3 opacity-50 text-mint-500"></i>
      <p class="font-500">All patients have been treated!</p>
     </div>
    </div><!-- Doctor Examine View -->
    <div id="doc-examine-view" class="flex-1 overflow-auto p-5 hidden fade-in"><button onclick="showDocQueue()" class="text-sm text-brand-600 font-600 flex items-center gap-1 mb-4"> <i data-lucide="arrow-left" class="w-4 h-4"></i> Back to Queue </button>
     <div class="bg-white rounded-2xl p-5 shadow-sm border border-slate-100 mb-4">
      <div class="flex items-center gap-4 mb-4">
       <div id="exam-photo" class="w-16 h-16 rounded-xl bg-brand-100 flex items-center justify-center text-brand-600 overflow-hidden flex-shrink-0"><i data-lucide="user" class="w-8 h-8"></i>
       </div>
       <div>
        <p id="exam-name" class="font-700 text-slate-800 text-lg"></p>
        <p id="exam-nis" class="text-xs text-slate-400"></p>
        <div class="flex gap-3 mt-1"><span id="exam-class" class="text-xs bg-brand-50 text-brand-700 px-2 py-0.5 rounded-lg font-600"></span> <span id="exam-room" class="text-xs bg-mint-50 text-mint-700 px-2 py-0.5 rounded-lg font-600"></span>
        </div>
       </div>
      </div>
     </div>
     <div class="bg-white rounded-2xl p-5 shadow-sm border border-slate-100">
      <h3 class="font-700 text-slate-800 mb-4 flex items-center gap-2"><i data-lucide="clipboard-list" class="w-5 h-5 text-brand-600"></i> Medical Examination</h3>
      <div class="space-y-4">
       <div><label class="text-xs font-600 text-slate-500 mb-1 block">Symptoms / Diagnosis</label> <textarea id="exam-symptoms" rows="3" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500 resize-none" placeholder="Describe symptoms..."></textarea>
       </div>
       <div><label class="text-xs font-600 text-slate-500 mb-1 block">Medication</label> <input id="exam-medication" type="text" class="w-full px-4 py-3 rounded-xl border border-slate-200 bg-slate-50 text-slate-800 font-500" placeholder="Prescribed medication">
       </div>
       <div><label class="text-xs font-600 text-slate-500 mb-1 block">Referral</label>
        <div class="grid grid-cols-2 gap-3"><button id="ref-home" onclick="selectReferral('home')" class="py-3 rounded-xl border-2 border-slate-200 text-slate-600 font-600 text-sm card-hover flex flex-col items-center gap-1"> <i data-lucide="home" class="w-5 h-5"></i> Go Home </button> <button id="ref-rest" onclick="selectReferral('rest')" class="py-3 rounded-xl border-2 border-slate-200 text-slate-600 font-600 text-sm card-hover flex flex-col items-center gap-1"> <i data-lucide="bed-double" class="w-5 h-5"></i> Rest at Boarding </button>
        </div>
       </div><button id="exam-submit-btn" onclick="submitExamination()" class="w-full py-3.5 rounded-xl bg-brand-600 text-white font-700 text-base card-hover shadow-lg shadow-brand-600/30 flex items-center justify-center gap-2"> <i data-lucide="save" class="w-5 h-5"></i> Save Examination </button>
      </div>
     </div>
    </div><!-- Permission Letter View -->
    <div id="doc-letter-view" class="flex-1 overflow-auto p-5 hidden fade-in"><button onclick="showDocQueue()" class="text-sm text-brand-600 font-600 flex items-center gap-1 mb-4"> <i data-lucide="arrow-left" class="w-4 h-4"></i> Back to Queue </button>
     <div id="letter-content" class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
      <div class="p-6 border-b-2 border-slate-800 mx-6 mt-4 text-center">
       <h3 class="font-bold text-lg tracking-wide">PUSKESTREN DULIDO</h3>
       <p class="text-xs text-slate-500 mt-1">Student Health Clinic</p>
      </div>
      <div class="p-6" style="font-family: 'Times New Roman', serif;">
       <h4 class="text-center font-bold underline mb-4 text-base">SURAT IZIN PULANG</h4>
       <p class="text-sm leading-relaxed mb-3">Yang bertanda tangan di bawah ini, Dokter Puskestren Dulido, menerangkan bahwa:</p>
       <table class="text-sm w-full mb-4">
        <tbody>
         <tr>
          <td class="py-1 w-24 align-top">NIS</td>
          <td class="py-1 align-top">: <span id="letter-nis"></span></td>
         </tr>
         <tr>
          <td class="py-1 align-top">Nama</td>
          <td class="py-1 align-top">: <span id="letter-name"></span></td>
         </tr>
         <tr>
          <td class="py-1 align-top">Kelas</td>
          <td class="py-1 align-top">: <span id="letter-class"></span></td>
         </tr>
         <tr>
          <td class="py-1 align-top">Kamar</td>
          <td class="py-1 align-top">: <span id="letter-room"></span></td>
         </tr>
         <tr>
          <td class="py-1 align-top">Diagnosis</td>
          <td class="py-1 align-top">: <span id="letter-symptoms"></span></td>
         </tr>
         <tr>
          <td class="py-1 align-top">Obat</td>
          <td class="py-1 align-top">: <span id="letter-medication"></span></td>
         </tr>
        </tbody>
       </table>
       <p class="text-sm leading-relaxed mb-6">Diberikan izin untuk <strong>pulang ke rumah</strong> guna pemulihan kesehatan.</p>
       <div class="text-right text-sm">
        <p id="letter-date"></p>
        <p class="mt-1">Dokter Puskestren,</p>
        <p class="mt-16 font-bold underline">DOKTER Dulido</p>
       </div>
      </div>
     </div><button onclick="printLetter()" class="w-full mt-4 py-3.5 rounded-xl bg-slate-800 text-white font-700 text-base card-hover flex items-center justify-center gap-2"> <i data-lucide="printer" class="w-5 h-5"></i> Print Letter </button>
    </div>
   </div>
  </div>
  <script>
// State
let currentRole = '';
let allPatients = [];
let selectedPatient = null;
let selectedReferral = '';
let photoDataUrl = '';
let nextQueue = 1;

const defaultConfig = {
  app_title: 'SIZKA',
  app_subtitle: 'Sistem Izin Kesehatan',
  background_color: '#1059e1',
  surface_color: '#ffffff',
  text_color: '#1e293b',
  primary_action_color: '#1570f5',
  secondary_action_color: '#0aad49'
};

// Element SDK
window.elementSdk.init({
  defaultConfig,
  onConfigChange: async (config) => {
    const c = { ...defaultConfig, ...config };
    const t = document.getElementById('home-title');
    if (t) t.textContent = c.app_title;
    const s = document.getElementById('home-subtitle');
    if (s) s.textContent = c.app_subtitle;

    // Colors
    const homeEl = document.getElementById('page-home');
    if (homeEl) homeEl.style.background = `linear-gradient(135deg, ${c.background_color} 0%, ${c.secondary_action_color} 100%)`;

    document.querySelectorAll('.bg-brand-600, [class*="bg-brand-600"]').forEach(el => {
      if (el.classList.contains('bg-brand-600')) el.style.backgroundColor = c.primary_action_color;
    });

    document.querySelectorAll('.bg-mint-600, [class*="bg-mint-600"]').forEach(el => {
      if (el.classList.contains('bg-mint-600')) el.style.backgroundColor = c.secondary_action_color;
    });

    document.querySelectorAll('.text-slate-800').forEach(el => {
      el.style.color = c.text_color;
    });

    // Font
    if (c.font_family) {
      document.body.style.fontFamily = `${c.font_family}, 'Plus Jakarta Sans', sans-serif`;
    }
    if (c.font_size) {
      document.body.style.fontSize = `${c.font_size}px`;
    }
  },
  mapToCapabilities: (config) => {
    const c = { ...defaultConfig, ...config };
    return {
      recolorables: [
        { get: () => c.background_color, set: v => { c.background_color = v; window.elementSdk.setConfig({ background_color: v }); } },
        { get: () => c.surface_color, set: v => { c.surface_color = v; window.elementSdk.setConfig({ surface_color: v }); } },
        { get: () => c.text_color, set: v => { c.text_color = v; window.elementSdk.setConfig({ text_color: v }); } },
        { get: () => c.primary_action_color, set: v => { c.primary_action_color = v; window.elementSdk.setConfig({ primary_action_color: v }); } },
        { get: () => c.secondary_action_color, set: v => { c.secondary_action_color = v; window.elementSdk.setConfig({ secondary_action_color: v }); } }
      ],
      borderables: [],
      fontEditable: {
        get: () => c.font_family || defaultConfig.font_family || 'Plus Jakarta Sans',
        set: v => { c.font_family = v; window.elementSdk.setConfig({ font_family: v }); }
      },
      fontSizeable: {
        get: () => c.font_size || 14,
        set: v => { c.font_size = v; window.elementSdk.setConfig({ font_size: v }); }
      }
    };
  },
  mapToEditPanelValues: (config) => {
    const c = { ...defaultConfig, ...config };
    return new Map([
      ['app_title', c.app_title],
      ['app_subtitle', c.app_subtitle]
    ]);
  }
});

// Data SDK
const dataHandler = {
  onDataChanged(data) {
    allPatients = data;
    updateNextQueue();
    renderCurrentView();
  }
};
window.dataSdk.init(dataHandler);

function updateNextQueue() {
  const maxQ = allPatients.reduce((m, p) => Math.max(m, p.queue_number || 0), 0);
  nextQueue = maxQ + 1;
  const qEl = document.getElementById('admin-queue');
  if (qEl) qEl.value = nextQueue;
}

function renderCurrentView() {
  if (currentRole === 'admin') renderAdminList();
  if (currentRole === 'doctor') {
    renderDocQueue();
    renderUntreatedList();
  }
  updateBadges();
}

// Navigation
function showPage(id) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  const page = document.getElementById(id);
  if (page) { page.classList.add('active'); page.classList.add('fade-in'); }
  setTimeout(() => lucide.createIcons(), 50);
}

function goHome() { showPage('page-home'); currentRole = ''; }

function logout() {
  currentRole = '';
  selectedPatient = null;
  goHome();
}

// Login
function showLogin(role) {
  currentRole = role;
  document.getElementById('login-user').value = '';
  document.getElementById('login-pass').value = '';
  document.getElementById('login-error').classList.add('hidden');
  document.getElementById('login-title').textContent = role === 'admin' ? 'Admin Login' : 'Doctor Login';
  showPage('page-login');
}

function togglePass() {
  const inp = document.getElementById('login-pass');
  inp.type = inp.type === 'password' ? 'text' : 'password';
}

function doLogin() {
  const user = document.getElementById('login-user').value.trim();
  const pass = document.getElementById('login-pass').value.trim();
  let valid = false;

  if (currentRole === 'admin' && user === 'PUSKESTREN Dulido' && pass === '24061996') valid = true;
  if (currentRole === 'doctor' && user === 'DOKTER Dulido' && pass === '24061996') valid = true;

  if (valid) {
    if (currentRole === 'admin') {
      updateNextQueue();
      showPage('page-admin');
      showAdminForm();
    } else {
      showPage('page-doctor');
      showDocQueue();
    }
    renderCurrentView();
  } else {
    document.getElementById('login-error').classList.remove('hidden');
  }
}

// Toast
function showToast(msg, type = 'success') {
  const container = document.getElementById('toast-container');
  const t = document.createElement('div');
  t.className = `toast px-4 py-3 rounded-xl text-white font-600 text-sm shadow-lg ${type === 'success' ? 'bg-mint-600' : 'bg-red-500'}`;
  t.textContent = msg;
  container.appendChild(t);
  setTimeout(() => t.remove(), 3000);
}

// Photo preview
function previewPhoto(input) {
  if (input.files && input.files[0]) {
    const reader = new FileReader();
    reader.onload = e => {
      photoDataUrl = e.target.result;
      document.getElementById('photo-preview').innerHTML = `<img src="${photoDataUrl}" class="w-full h-full object-cover">`;
    };
    reader.readAsDataURL(input.files[0]);
  }
}

// Admin
function showAdminForm() {
  document.getElementById('admin-form-view').classList.remove('hidden');
  document.getElementById('admin-list-view').classList.add('hidden');
  updateNextQueue();
}

function showAdminList() {
  document.getElementById('admin-form-view').classList.add('hidden');
  document.getElementById('admin-list-view').classList.remove('hidden');
  renderAdminList();
}

async function registerPatient() {
  const nis = document.getElementById('admin-nis').value.trim();
  const name = document.getElementById('admin-name').value.trim();
  const cls = document.getElementById('admin-class').value.trim();
  const room = document.getElementById('admin-room').value.trim();

  if (!nis || !name || !cls || !room) {
    showToast('Please fill all fields', 'error');
    return;
  }

  if (allPatients.length >= 999) {
    showToast('Maximum patient limit reached!', 'error');
    return;
  }

  const btn = document.getElementById('admin-submit-btn');
  btn.disabled = true;
  btn.innerHTML = '<span class="animate-spin">⏳</span> Registering...';

  const result = await window.dataSdk.create({
    nis, name, class_name: cls, room,
    queue_number: nextQueue,
    photo_url: photoDataUrl || '',
    symptoms: '', medication: '', referral: '',
    treated: false,
    treated_date: '',
    registered_date: new Date().toISOString()
  });

  btn.disabled = false;
  btn.innerHTML = '<i data-lucide="check-circle" class="w-5 h-5"></i> Register Patient';
  lucide.createIcons();

  if (result.isOk) {
    showToast(`Patient ${name} registered - Queue #${nextQueue}`);
    document.getElementById('admin-nis').value = '';
    document.getElementById('admin-name').value = '';
    document.getElementById('admin-class').value = '';
    document.getElementById('admin-room').value = '';
    photoDataUrl = '';
    document.getElementById('photo-preview').innerHTML = '<i data-lucide="camera" class="w-8 h-8 mb-1"></i><span class="text-[10px] font-500">Add Photo</span>';
    lucide.createIcons();
  } else {
    showToast('Failed to register patient', 'error');
  }
}

function renderAdminList() {
  const container = document.getElementById('admin-patient-list');
  const empty = document.getElementById('admin-empty');
  const badge = document.getElementById('admin-badge');

  const patients = allPatients.filter(p => !p.treated);
  badge.textContent = patients.length;
  badge.classList.toggle('hidden', patients.length === 0);

  if (patients.length === 0) {
    container.innerHTML = '';
    empty.classList.remove('hidden');
    return;
  }
  empty.classList.add('hidden');

  const existingIds = new Set([...container.children].map(el => el.dataset.id));
  const dataIds = new Set(patients.map(p => p.__backendId));

  // Remove deleted
  [...container.children].forEach(el => { if (!dataIds.has(el.dataset.id)) el.remove(); });

  patients.sort((a, b) => (a.queue_number || 0) - (b.queue_number || 0)).forEach(p => {
    const existing = container.querySelector(`[data-id="${p.__backendId}"]`);
    if (existing) {
      existing.querySelector('.p-name').textContent = p.name;
      existing.querySelector('.p-nis').textContent = `NIS: ${p.nis}`;
      existing.querySelector('.p-meta').textContent = `Class ${p.class_name} • Room ${p.room}`;
      existing.querySelector('.p-queue').textContent = `#${p.queue_number}`;
    } else {
      const card = document.createElement('div');
      card.dataset.id = p.__backendId;
      card.className = 'bg-white rounded-xl p-4 shadow-sm border border-slate-100 flex items-center gap-3';
      const photoHtml = p.photo_url
        ? `<img src="${p.photo_url}" class="w-12 h-12 rounded-xl object-cover flex-shrink-0">`
        : `<div class="w-12 h-12 rounded-xl bg-brand-100 flex items-center justify-center flex-shrink-0"><i data-lucide="user" class="w-6 h-6 text-brand-600"></i></div>`;
      card.innerHTML = `
        ${photoHtml}
        <div class="flex-1 min-w-0">
          <p class="p-name font-600 text-slate-800 truncate">${p.name}</p>
          <p class="p-nis text-xs text-slate-400">NIS: ${p.nis}</p>
          <p class="p-meta text-xs text-slate-400">Class ${p.class_name} • Room ${p.room}</p>
        </div>
        <span class="p-queue bg-brand-50 text-brand-700 text-sm font-700 px-3 py-1 rounded-lg">#${p.queue_number}</span>
      `;
      container.appendChild(card);
    }
  });
  lucide.createIcons();
}

// Doctor
function showDocQueue() {
  document.getElementById('doc-queue-view').classList.remove('hidden');
  document.getElementById('doc-untreated-view').classList.add('hidden');
  document.getElementById('doc-examine-view').classList.add('hidden');
  document.getElementById('doc-letter-view').classList.add('hidden');
  renderDocQueue();
}

function showUntreated() {
  document.getElementById('doc-queue-view').classList.add('hidden');
  document.getElementById('doc-untreated-view').classList.remove('hidden');
  document.getElementById('doc-examine-view').classList.add('hidden');
  document.getElementById('doc-letter-view').classList.add('hidden');
  renderUntreatedList();
}

function renderDocQueue() {
  const container = document.getElementById('doc-patient-list');
  const empty = document.getElementById('doc-empty');
  const patients = allPatients.filter(p => !p.treated).sort((a, b) => (a.queue_number || 0) - (b.queue_number || 0));

  if (patients.length === 0) { container.innerHTML = ''; empty.classList.remove('hidden'); return; }
  empty.classList.add('hidden');

  // Remove stale
  [...container.children].forEach(el => {
    if (!patients.find(p => p.__backendId === el.dataset.id)) el.remove();
  });

  patients.forEach(p => {
    let card = container.querySelector(`[data-id="${p.__backendId}"]`);
    if (!card) {
      card = document.createElement('div');
      card.dataset.id = p.__backendId;
      card.className = 'bg-white rounded-xl p-4 shadow-sm border border-slate-100 flex items-center gap-3 card-hover cursor-pointer';
      card.onclick = () => examinePatient(p.__backendId);
      container.appendChild(card);
    }
    const photoHtml = p.photo_url
      ? `<img src="${p.photo_url}" class="w-14 h-14 rounded-xl object-cover flex-shrink-0">`
      : `<div class="w-14 h-14 rounded-xl bg-brand-100 flex items-center justify-center flex-shrink-0"><i data-lucide="user" class="w-7 h-7 text-brand-600"></i></div>`;
    card.innerHTML = `
      ${photoHtml}
      <div class="flex-1 min-w-0">
        <p class="font-600 text-slate-800 truncate">${p.name}</p>
        <p class="text-xs text-slate-400">NIS: ${p.nis}</p>
        <div class="flex gap-2 mt-1">
          <span class="text-[10px] bg-brand-50 text-brand-700 px-2 py-0.5 rounded font-600">Class ${p.class_name}</span>
          <span class="text-[10px] bg-mint-50 text-mint-700 px-2 py-0.5 rounded font-600">Room ${p.room}</span>
        </div>
      </div>
      <div class="text-right flex-shrink-0">
        <span class="bg-brand-600 text-white text-sm font-700 px-3 py-1 rounded-lg">#${p.queue_number}</span>
        <p class="text-[10px] text-slate-400 mt-1">Tap to examine</p>
      </div>
    `;
  });
  lucide.createIcons();
}

function renderUntreatedList() {
  const container = document.getElementById('doc-untreated-list');
  const empty = document.getElementById('doc-untreated-empty');
  const patients = allPatients.filter(p => !p.treated);
  const badge = document.getElementById('doc-badge');

  badge.textContent = patients.length;
  badge.classList.toggle('hidden', patients.length === 0);

  if (patients.length === 0) { container.innerHTML = ''; empty.classList.remove('hidden'); return; }
  empty.classList.add('hidden');
  container.innerHTML = '';

  patients.sort((a, b) => (a.queue_number || 0) - (b.queue_number || 0)).forEach(p => {
    const card = document.createElement('div');
    card.className = 'bg-white rounded-xl p-4 shadow-sm border border-orange-200 flex items-center gap-3';
    const photoHtml = p.photo_url
      ? `<img src="${p.photo_url}" class="w-12 h-12 rounded-xl object-cover flex-shrink-0">`
      : `<div class="w-12 h-12 rounded-xl bg-orange-100 flex items-center justify-center flex-shrink-0"><i data-lucide="user" class="w-6 h-6 text-orange-500"></i></div>`;
    card.innerHTML = `
      ${photoHtml}
      <div class="flex-1 min-w-0">
        <p class="font-600 text-slate-800 truncate">${p.name}</p>
        <p class="text-xs text-slate-400">NIS: ${p.nis} • Class ${p.class_name} • Room ${p.room}</p>
      </div>
      <span class="bg-orange-100 text-orange-600 text-[10px] font-700 px-2 py-1 rounded-lg">Waiting</span>
    `;
    container.appendChild(card);
  });
  lucide.createIcons();
}

function updateBadges() {
  const untreated = allPatients.filter(p => !p.treated).length;
  const db = document.getElementById('doc-badge');
  if (db) { db.textContent = untreated; db.classList.toggle('hidden', untreated === 0); }
  const ab = document.getElementById('admin-badge');
  if (ab) { ab.textContent = untreated; ab.classList.toggle('hidden', untreated === 0); }
}

function examinePatient(backendId) {
  selectedPatient = allPatients.find(p => p.__backendId === backendId);
  if (!selectedPatient) return;
  selectedReferral = '';

  document.getElementById('exam-name').textContent = selectedPatient.name;
  document.getElementById('exam-nis').textContent = `NIS: ${selectedPatient.nis}`;
  document.getElementById('exam-class').textContent = `Class ${selectedPatient.class_name}`;
  document.getElementById('exam-room').textContent = `Room ${selectedPatient.room}`;

  const photoEl = document.getElementById('exam-photo');
  if (selectedPatient.photo_url) {
    photoEl.innerHTML = `<img src="${selectedPatient.photo_url}" class="w-full h-full object-cover">`;
  } else {
    photoEl.innerHTML = '<i data-lucide="user" class="w-8 h-8"></i>';
  }

  document.getElementById('exam-symptoms').value = '';
  document.getElementById('exam-medication').value = '';
  document.getElementById('ref-home').className = 'py-3 rounded-xl border-2 border-slate-200 text-slate-600 font-600 text-sm card-hover flex flex-col items-center gap-1';
  document.getElementById('ref-rest').className = 'py-3 rounded-xl border-2 border-slate-200 text-slate-600 font-600 text-sm card-hover flex flex-col items-center gap-1';

  document.getElementById('doc-queue-view').classList.add('hidden');
  document.getElementById('doc-untreated-view').classList.add('hidden');
  document.getElementById('doc-examine-view').classList.remove('hidden');
  document.getElementById('doc-letter-view').classList.add('hidden');
  lucide.createIcons();
}

function selectReferral(type) {
  selectedReferral = type;
  const homeBtn = document.getElementById('ref-home');
  const restBtn = document.getElementById('ref-rest');

  homeBtn.className = type === 'home'
    ? 'py-3 rounded-xl border-2 border-brand-500 bg-brand-50 text-brand-700 font-600 text-sm card-hover flex flex-col items-center gap-1'
    : 'py-3 rounded-xl border-2 border-slate-200 text-slate-600 font-600 text-sm card-hover flex flex-col items-center gap-1';
  restBtn.className = type === 'rest'
    ? 'py-3 rounded-xl border-2 border-mint-500 bg-mint-50 text-mint-700 font-600 text-sm card-hover flex flex-col items-center gap-1'
    : 'py-3 rounded-xl border-2 border-slate-200 text-slate-600 font-600 text-sm card-hover flex flex-col items-center gap-1';
  lucide.createIcons();
}

async function submitExamination() {
  if (!selectedPatient) return;
  const symptoms = document.getElementById('exam-symptoms').value.trim();
  const medication = document.getElementById('exam-medication').value.trim();

  if (!symptoms || !medication || !selectedReferral) {
    showToast('Please fill all fields and select referral', 'error');
    return;
  }

  const btn = document.getElementById('exam-submit-btn');
  btn.disabled = true;
  btn.innerHTML = '<span class="animate-spin">⏳</span> Saving...';

  const updated = { ...selectedPatient, symptoms, medication, referral: selectedReferral, treated: true, treated_date: new Date().toISOString() };
  const result = await window.dataSdk.update(updated);

  btn.disabled = false;
  btn.innerHTML = '<i data-lucide="save" class="w-5 h-5"></i> Save Examination';
  lucide.createIcons();

  if (result.isOk) {
    showToast('Examination saved!');
    if (selectedReferral === 'home') {
      showPermissionLetter(updated);
    } else {
      showDocQueue();
    }
  } else {
    showToast('Failed to save examination', 'error');
  }
}

function showPermissionLetter(patient) {
  document.getElementById('letter-nis').textContent = patient.nis;
  document.getElementById('letter-name').textContent = patient.name;
  document.getElementById('letter-class').textContent = patient.class_name;
  document.getElementById('letter-room').textContent = patient.room;
  document.getElementById('letter-symptoms').textContent = patient.symptoms;
  document.getElementById('letter-medication').textContent = patient.medication;

  const now = new Date();
  const months = ['Januari','Februari','Maret','April','Mei','Juni','Juli','Agustus','September','Oktober','November','Desember'];
  document.getElementById('letter-date').textContent = `Dulido, ${now.getDate()} ${months[now.getMonth()]} ${now.getFullYear()}`;

  document.getElementById('doc-queue-view').classList.add('hidden');
  document.getElementById('doc-examine-view').classList.add('hidden');
  document.getElementById('doc-untreated-view').classList.add('hidden');
  document.getElementById('doc-letter-view').classList.remove('hidden');
}

function printLetter() {
  const content = document.getElementById('letter-content').innerHTML;
  const w = window.open('', '_blank');
  w.document.write(`<html><head><title>Surat Izin Pulang</title><style>body{font-family:'Times New Roman',serif;padding:40px;max-width:600px;margin:auto}table{border-collapse:collapse}td{padding:4px 0}</style></head><body>${content}<script>window.print();<\/script></body></html>`);
  w.document.close();
}

// Init
document.addEventListener('DOMContentLoaded', () => {
  lucide.createIcons();
  updateNextQueue();
});
</script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9ee892dd909dfcde',t:'MTc3NjU2NjczMi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>

