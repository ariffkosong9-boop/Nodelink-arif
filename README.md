<!DOCTYPE html>
<html lang="id" data-theme="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NodeLink Marketplace — Next Gen</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700;800&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
<style>
  :root[data-theme="dark"]{
    --bg:#060A14; --bg-panel:rgba(17,26,46,.72); --bg-elev:rgba(24,35,66,.85);
    --accent:#22D3EE; --accent2:#A78BFA; --accent-dim:#0E7490;
    --warm:#F59E0B; --text:#E7ECF5; --text-dim:#8C9AC0;
    --success:#34D399; --danger:#F87171; --border:rgba(255,255,255,.09);
    --glow:0 0 24px rgba(34,211,238,.25);
  }
  :root[data-theme="light"]{
    --bg:#EEF2FA; --bg-panel:rgba(255,255,255,.75); --bg-elev:rgba(255,255,255,.9);
    --accent:#0891B2; --accent2:#7C3AED; --accent-dim:#67E8F9;
    --warm:#D97706; --text:#16213E; --text-dim:#5B6785;
    --success:#059669; --danger:#DC2626; --border:rgba(15,23,42,.08);
    --glow:0 0 24px rgba(8,145,178,.18);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{background:var(--bg);color:var(--text);font-family:'Inter',sans-serif;min-height:100vh;transition:background .25s,color .25s;-webkit-tap-highlight-color:transparent;overflow-x:hidden;}
  h1,h2,h3,.display{font-family:'Space Grotesk',sans-serif;}
  .mono{font-family:'JetBrains Mono',monospace;}
  a{color:inherit;text-decoration:none;}
  button{font-family:inherit;cursor:pointer;border:none;}
  input,textarea,select{font-family:inherit;}

  #bgCanvas{position:fixed;inset:0;z-index:0;pointer-events:none;}
  #confettiCanvas{position:fixed;inset:0;z-index:300;pointer-events:none;}

  .grad-text{background:linear-gradient(90deg,var(--accent),var(--accent2));-webkit-background-clip:text;background-clip:text;color:transparent;}

  header{position:sticky;top:0;z-index:50;background:color-mix(in srgb, var(--bg) 78%, transparent);backdrop-filter:blur(14px);border-bottom:1px solid var(--border);}
  header::after{content:"";position:absolute;bottom:-1px;left:0;right:0;height:1px;background:linear-gradient(90deg,transparent,var(--accent),var(--accent2),transparent);opacity:.6;}
  .nav-wrap{max-width:1100px;margin:0 auto;display:flex;align-items:center;justify-content:space-between;padding:12px 16px;gap:10px;position:relative;z-index:2;}
  .logo{display:flex;align-items:center;gap:9px;font-weight:800;font-size:19px;flex-shrink:0;letter-spacing:.2px;}
  .logo .dot{width:10px;height:10px;border-radius:50%;background:var(--accent);box-shadow:0 0 4px var(--accent),0 0 16px var(--accent);animation:pulseDot 2s ease-in-out infinite;}
  @keyframes pulseDot{0%,100%{opacity:1;}50%{opacity:.4;}}
  .header-actions{display:flex;align-items:center;gap:8px;}
  .icon-btn{position:relative;width:38px;height:38px;border-radius:11px;background:var(--bg-elev);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:16px;backdrop-filter:blur(8px);transition:.15s;}
  .icon-btn:active{transform:scale(.92);}
  .badge{position:absolute;top:-4px;right:-4px;background:var(--danger);color:#fff;font-size:10px;font-weight:700;min-width:16px;height:16px;border-radius:8px;display:flex;align-items:center;justify-content:center;padding:0 3px;}
  .avatar-btn{width:38px;height:38px;border-radius:11px;background:linear-gradient(135deg,var(--accent),var(--accent2));color:#04222A;font-weight:700;display:flex;align-items:center;justify-content:center;font-size:14px;box-shadow:var(--glow);}

  nav.tabs{display:flex;gap:2px;background:var(--bg-elev);padding:4px;border-radius:11px;border:1px solid var(--border);overflow-x:auto;max-width:100%;backdrop-filter:blur(8px);}
  nav.tabs button{background:transparent;color:var(--text-dim);padding:7px 11px;border-radius:8px;font-size:12.5px;font-weight:600;white-space:nowrap;transition:.15s;}
  nav.tabs button.active{background:var(--bg-panel);color:var(--accent);box-shadow:inset 0 0 0 1px var(--accent-dim);}
  .tabs-wrap{max-width:1100px;margin:0 auto;padding:0 16px 12px;overflow-x:auto;position:relative;z-index:2;}

  main{max-width:1100px;margin:0 auto;padding:20px 16px 90px;position:relative;z-index:1;}
  .view{display:none;} .view.active{display:block;animation:fade .35s ease;}
  @keyframes fade{from{opacity:0;transform:translateY(8px);}to{opacity:1;transform:translateY(0);}}

  /* hero */
  .hero{padding:14px 0 6px;}
  .eyebrow{display:inline-flex;align-items:center;gap:6px;color:var(--accent);font-size:11px;letter-spacing:2px;text-transform:uppercase;font-family:'JetBrains Mono',monospace;margin-bottom:10px;}
  .hero h1{font-size:clamp(26px,6vw,40px);line-height:1.15;margin:0 0 8px;font-weight:800;}
  .typecursor{display:inline-block;width:3px;height:.9em;background:var(--accent);margin-left:2px;animation:blink 1s step-end infinite;vertical-align:middle;}
  @keyframes blink{50%{opacity:0;}}

  .ticker-wrap{background:var(--bg-panel);border:1px solid var(--border);border-radius:14px;padding:10px 0;margin:14px 0 22px;overflow:hidden;backdrop-filter:blur(10px);box-shadow:var(--glow);}
  .ticker-track{display:flex;gap:34px;white-space:nowrap;animation:scrollTicker 22s linear infinite;padding-left:20px;}
  .ticker-track span{font-size:12px;color:var(--text-dim);font-family:'JetBrains Mono',monospace;}
  .ticker-track span b{color:var(--accent);}
  @keyframes scrollTicker{from{transform:translateX(0);}to{transform:translateX(-50%);}}

  .stat-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:10px;margin-bottom:22px;}
  .stat{background:var(--bg-panel);border:1px solid var(--border);border-radius:16px;padding:15px;backdrop-filter:blur(10px);position:relative;overflow:hidden;}
  .stat::before{content:"";position:absolute;top:-30%;right:-30%;width:60%;height:60%;background:radial-gradient(circle,var(--accent),transparent 70%);opacity:.12;}
  .stat .n{font-family:'Space Grotesk',sans-serif;font-size:24px;font-weight:800;color:var(--accent);position:relative;}
  .stat .l{font-size:11.5px;color:var(--text-dim);margin-top:2px;position:relative;}

  .section-title{display:flex;align-items:baseline;justify-content:space-between;margin:8px 0 14px;flex-wrap:wrap;gap:8px;}
  .section-title h2{font-size:18px;margin:0;}
  .section-title span{color:var(--text-dim);font-size:12px;}

  .toolbar{display:flex;gap:8px;margin-bottom:16px;flex-wrap:wrap;}
  .toolbar input[type="text"]{flex:1;min-width:160px;background:var(--bg-elev);border:1px solid var(--border);color:var(--text);padding:10px 12px;border-radius:10px;font-size:13px;backdrop-filter:blur(8px);}
  .toolbar select{background:var(--bg-elev);border:1px solid var(--border);color:var(--text);padding:10px 12px;border-radius:10px;font-size:12.5px;backdrop-filter:blur(8px);}

  .badge-verified{display:inline-flex;align-items:center;gap:3px;background:color-mix(in srgb, var(--success) 18%, transparent);color:var(--success);font-size:10px;font-weight:600;padding:2px 7px;border-radius:20px;}
  .badge-pending{display:inline-flex;align-items:center;gap:3px;background:color-mix(in srgb, var(--warm) 18%, transparent);color:var(--warm);font-size:10px;font-weight:600;padding:2px 7px;border-radius:20px;}
  .badge-trending{display:inline-flex;align-items:center;gap:3px;background:linear-gradient(90deg,rgba(245,158,11,.2),rgba(248,113,113,.2));color:var(--warm);font-size:10px;font-weight:700;padding:2px 8px;border-radius:20px;}

  .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(215px,1fr));gap:13px;}
  .card{background:var(--bg-panel);border:1px solid var(--border);border-radius:16px;padding:15px;display:flex;flex-direction:column;gap:7px;transition:.2s;backdrop-filter:blur(10px);position:relative;}
  .card:hover{border-color:var(--accent-dim);transform:translateY(-2px);box-shadow:var(--glow);}
  .card-top{display:flex;justify-content:space-between;align-items:flex-start;}
  .card .icon{width:38px;height:38px;border-radius:10px;background:var(--bg-elev);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:18px;}
  .heart-btn{background:transparent;font-size:17px;color:var(--text-dim);transition:.15s;}
  .heart-btn.active{color:var(--danger);}
  .heart-btn:active{transform:scale(1.3);}
  .card h3{font-size:14.5px;margin:0;}
  .card p.desc{color:var(--text-dim);font-size:12px;line-height:1.5;margin:0;flex-grow:1;}
  .card .seller{font-size:11px;color:var(--text-dim);display:flex;align-items:center;gap:5px;flex-wrap:wrap;}
  .stars{color:var(--warm);font-size:12px;letter-spacing:1px;}
  .stars .cnt{color:var(--text-dim);font-size:10.5px;margin-left:3px;}
  .card .price{font-family:'JetBrains Mono',monospace;font-weight:700;font-size:16px;background:linear-gradient(90deg,var(--accent),var(--accent2));-webkit-background-clip:text;background-clip:text;color:transparent;}
  .card .stock{font-size:11px;color:var(--text-dim);}

  .btn{background:linear-gradient(135deg,var(--accent),var(--accent-dim));color:#04222A;font-weight:700;font-size:12.5px;padding:9px 12px;border-radius:10px;white-space:nowrap;transition:.15s;}
  :root[data-theme="light"] .btn{color:#fff;}
  .btn.ghost{background:var(--bg-elev);color:var(--text);border:1px solid var(--border);}
  .btn.warm{background:linear-gradient(135deg,var(--warm),#EF4444);color:#fff;}
  .btn.small{padding:6px 10px;font-size:11.5px;}
  .btn.danger{background:transparent;color:var(--danger);border:1px solid var(--danger);}
  .btn.block{width:100%;}
  .btn:active{transform:scale(.96);}
  .btn:disabled{opacity:.45;cursor:not-allowed;}

  .panel{background:var(--bg-panel);border:1px solid var(--border);border-radius:16px;padding:18px;margin-bottom:16px;backdrop-filter:blur(10px);}
  .panel h3{margin:0 0 14px;font-size:15px;}
  .form-row{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:10px;}
  .form-row.full{grid-template-columns:1fr;}
  label{display:block;font-size:11.5px;color:var(--text-dim);margin-bottom:5px;}
  input,textarea,select{width:100%;background:var(--bg-elev);border:1px solid var(--border);color:var(--text);padding:9px 10px;border-radius:9px;font-size:13px;}
  textarea{resize:vertical;min-height:52px;}

  .list-item{display:flex;align-items:center;gap:10px;padding:11px 0;border-bottom:1px solid var(--border);}
  .list-item:last-child{border-bottom:none;}
  .list-item .info{flex:1;min-width:0;}
  .list-item .info .t{font-size:13.5px;font-weight:600;}
  .list-item .info .m{font-size:11.5px;color:var(--text-dim);}
  .list-actions{display:flex;gap:6px;flex-wrap:wrap;}

  .status-pill{font-size:10.5px;font-weight:700;padding:3px 9px;border-radius:20px;white-space:nowrap;}
  .status-menunggu{background:color-mix(in srgb, var(--warm) 18%, transparent);color:var(--warm);}
  .status-dibayar{background:color-mix(in srgb, var(--accent) 18%, transparent);color:var(--accent);}
  .status-selesai{background:color-mix(in srgb, var(--success) 18%, transparent);color:var(--success);}
  .status-dibatalkan{background:color-mix(in srgb, var(--danger) 18%, transparent);color:var(--danger);}

  .empty{text-align:center;color:var(--text-dim);font-size:13px;padding:28px 10px;border:1px dashed var(--border);border-radius:16px;}

  .level-badge{display:inline-flex;align-items:center;gap:6px;padding:7px 16px;border-radius:20px;font-weight:700;font-size:13px;background:linear-gradient(135deg,var(--accent),var(--accent2));color:#04222A;box-shadow:var(--glow);}
  :root[data-theme="light"] .level-badge{color:#fff;}

  .gate{max-width:340px;margin:50px auto;text-align:center;background:var(--bg-panel);border:1px solid var(--border);border-radius:16px;padding:28px 22px;backdrop-filter:blur(10px);}
  .gate .icon{font-size:28px;margin-bottom:8px;}
  .gate p{color:var(--text-dim);font-size:13px;margin-bottom:14px;}
  .hint{font-size:11px;color:var(--text-dim);margin-top:10px;}
  .hint b{color:var(--warm);}

  .overlay{position:fixed;inset:0;background:rgba(2,5,12,.78);backdrop-filter:blur(6px);display:flex;align-items:center;justify-content:center;z-index:100;padding:16px;}
  .overlay.hidden{display:none;}
  .modal{background:var(--bg-panel);border:1px solid var(--border);border-radius:18px;padding:24px 20px;max-width:380px;width:100%;max-height:88vh;overflow-y:auto;backdrop-filter:blur(16px);box-shadow:0 20px 60px rgba(0,0,0,.5);}
  .modal h3{margin:0 0 6px;font-size:17px;}
  .modal p{color:var(--text-dim);font-size:12.5px;margin:0 0 14px;line-height:1.5;}
  .modal-tabs{display:flex;gap:4px;background:var(--bg-elev);padding:3px;border-radius:10px;margin-bottom:14px;}
  .modal-tabs button{flex:1;background:transparent;color:var(--text-dim);padding:7px;border-radius:8px;font-size:12.5px;font-weight:700;}
  .modal-tabs button.active{background:var(--bg-panel);color:var(--accent);}

  .qr-box{text-align:center;background:#fff;border-radius:14px;padding:14px;margin:14px 0;box-shadow:var(--glow);}
  .qr-box img{width:170px;height:170px;display:block;margin:0 auto;}
  .method-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:8px;margin:12px 0;}
  .method-btn{background:var(--bg-elev);border:1px solid var(--border);border-radius:11px;padding:10px 4px;text-align:center;font-size:11px;color:var(--text-dim);}
  .method-btn.active{border-color:var(--accent);color:var(--accent);background:color-mix(in srgb, var(--accent) 10%, transparent);box-shadow:var(--glow);}
  .method-btn .em{font-size:20px;display:block;margin-bottom:4px;}

  .star-picker{display:flex;gap:4px;font-size:26px;margin:8px 0;}
  .star-picker span{cursor:pointer;color:var(--border);transition:.1s;}
  .star-picker span.on{color:var(--warm);}

  .notif-item{display:flex;gap:10px;padding:13px 0;border-bottom:1px solid var(--border);cursor:pointer;}
  .notif-item:last-child{border-bottom:none;}
  .notif-item .ico{width:34px;height:34px;border-radius:10px;background:var(--bg-elev);display:flex;align-items:center;justify-content:center;font-size:15px;flex-shrink:0;}
  .notif-item .body{flex:1;min-width:0;}
  .notif-item .body .t{font-size:13px;font-weight:700;display:flex;align-items:center;gap:6px;}
  .notif-item .body .t .unread-dot{width:7px;height:7px;border-radius:50%;background:var(--accent);flex-shrink:0;box-shadow:0 0 6px var(--accent);}
  .notif-item .body .m{font-size:12px;color:var(--text-dim);margin-top:2px;line-height:1.4;}
  .notif-item .body .time{font-size:10.5px;color:var(--text-dim);margin-top:4px;font-family:'JetBrains Mono',monospace;}

  .toast{position:fixed;bottom:22px;left:50%;transform:translateX(-50%) translateY(20px);background:var(--bg-elev);border:1px solid var(--accent-dim);color:var(--text);padding:11px 20px;border-radius:11px;font-size:13px;opacity:0;transition:.25s;z-index:200;pointer-events:none;max-width:90vw;text-align:center;backdrop-filter:blur(10px);box-shadow:var(--glow);}
  .toast.show{opacity:1;transform:translateX(-50%) translateY(0);}

  footer{text-align:center;color:var(--text-dim);font-size:11px;padding:26px 16px;border-top:1px solid var(--border);position:relative;z-index:1;}
  .disclaimer{background:var(--bg-elev);border:1px solid var(--border);border-radius:12px;padding:11px 13px;font-size:11.5px;color:var(--text-dim);margin-bottom:16px;line-height:1.5;backdrop-filter:blur(8px);}

  /* ---- wallet ---- */
  .wallet-card{background:linear-gradient(135deg, color-mix(in srgb, var(--accent) 20%, var(--bg-panel)), color-mix(in srgb, var(--accent2) 20%, var(--bg-panel)));border:1px solid var(--border);border-radius:18px;padding:22px;margin-bottom:16px;position:relative;overflow:hidden;}
  .wallet-card::after{content:"";position:absolute;top:-40%;right:-20%;width:70%;height:140%;background:radial-gradient(circle,var(--accent),transparent 65%);opacity:.15;}
  .wallet-card .lbl{font-size:12px;color:var(--text-dim);position:relative;}
  .wallet-card .amt{font-family:'Space Grotesk',sans-serif;font-size:32px;font-weight:800;margin:4px 0 14px;position:relative;}
  .wallet-actions{display:flex;gap:8px;position:relative;flex-wrap:wrap;}

  .tx-item{display:flex;align-items:center;gap:10px;padding:11px 0;border-bottom:1px solid var(--border);}
  .tx-item:last-child{border-bottom:none;}
  .tx-item .ico{width:32px;height:32px;border-radius:9px;display:flex;align-items:center;justify-content:center;font-size:14px;flex-shrink:0;background:var(--bg-elev);}
  .tx-item .info{flex:1;min-width:0;}
  .tx-item .info .t{font-size:13px;font-weight:600;}
  .tx-item .info .m{font-size:11px;color:var(--text-dim);}
  .tx-amt{font-family:'JetBrains Mono',monospace;font-weight:700;font-size:13px;white-space:nowrap;}
  .tx-amt.plus{color:var(--success);}
  .tx-amt.minus{color:var(--danger);}

  .btn.wa{background:linear-gradient(135deg,#25D366,#128C7E);color:#fff;}

  .prod-tile{width:100%;aspect-ratio:1.6/1;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:32px;margin-bottom:2px;position:relative;overflow:hidden;}
  .prod-tile::after{content:"";position:absolute;inset:0;background:radial-gradient(circle at 30% 20%, rgba(255,255,255,.25), transparent 60%);}

  .ref-row{display:flex;align-items:center;gap:8px;background:var(--bg-elev);border:1px solid var(--border);border-radius:9px;padding:8px 10px;margin-top:4px;flex-wrap:wrap;}
  .ref-row .code{font-family:'JetBrains Mono',monospace;color:var(--warm);font-weight:700;font-size:12.5px;}

  .review-item{padding:10px 0;border-bottom:1px solid var(--border);}
  .review-item:last-child{border-bottom:none;}
  .review-item .rstars{color:var(--warm);font-size:12px;}
  .review-item .rname{font-size:12.5px;font-weight:600;margin-left:6px;}
  .review-item .rtext{font-size:12px;color:var(--text-dim);margin-top:3px;}

  @media(max-width:520px){ .form-row{grid-template-columns:1fr;} .method-grid{grid-template-columns:repeat(2,1fr);} }
</style>
</head>
<body>

<canvas id="bgCanvas"></canvas>
<canvas id="confettiCanvas"></canvas>

<header>
  <div class="nav-wrap">
    <div class="logo"><span class="dot"></span> Node<span class="grad-text">Link</span></div>
    <div class="header-actions">
      <button class="icon-btn" id="themeToggle">🌙</button>
      <button class="icon-btn" id="wishlistBtn">🤍<span class="badge" id="wishBadge" style="display:none;">0</span></button>
      <button class="icon-btn" id="bellBtn">🔔<span class="badge" id="notifBadge" style="display:none;">0</span></button>
      <button class="avatar-btn" id="avatarBtn">?</button>
    </div>
  </div>
  <div class="tabs-wrap">
    <nav class="tabs">
      <button data-view="market" class="active">Marketplace</button>
      <button data-view="wallet">Dompet</button>
      <button data-view="wishlist">Wishlist</button>
      <button data-view="store">Toko Saya</button>
      <button data-view="orders">Pesanan</button>
      <button data-view="notif">Notifikasi</button>
      <button data-view="profile">Profil</button>
      <button data-view="admin">Admin</button>
    </nav>
  </div>
</header>

<main>

  <!-- ============ MARKETPLACE ============ -->
  <section id="view-market" class="view active">
    <div class="hero">
      <div class="eyebrow">● jaringan transaksi real-time</div>
      <h1>Belanja teknologi masa depan.<span class="typecursor"></span></h1>
    </div>
    <div class="ticker-wrap"><div class="ticker-track" id="tickerTrack"></div></div>

    <div class="stat-grid">
      <div class="stat"><div class="n" data-count="0" id="stTotalProd">0</div><div class="l">Produk aktif</div></div>
      <div class="stat"><div class="n" data-count="0" id="stTotalUsers">0</div><div class="l">Anggota</div></div>
      <div class="stat"><div class="n" data-count="0" id="stTotalOrders">0</div><div class="l">Transaksi</div></div>
      <div class="stat"><div class="n" id="stTotalGmv">Rp0</div><div class="l">Nilai transaksi</div></div>
    </div>

    <div class="disclaimer">⚡ Demo eksperimen: akun, pesanan &amp; notifikasi tersimpan lokal di browser ini. Pembayaran QRIS/e-wallet <b>simulasi tampilan</b>, bukan transaksi uang sungguhan.</div>

    <div class="section-title"><h2>Semua produk</h2><span id="marketCount">0 produk</span></div>
    <div class="toolbar">
      <input type="text" id="searchInput" placeholder="Cari produk...">
      <select id="sortSelect">
        <option value="terbaru">Terbaru</option>
        <option value="terlaris">Terlaris</option>
        <option value="murah">Harga terendah</option>
        <option value="mahal">Harga tertinggi</option>
        <option value="rating">Rating tertinggi</option>
      </select>
    </div>
    <div class="grid" id="marketGrid"></div>
  </section>

  <!-- ============ DOMPET / WALLET ============ -->
  <section id="view-wallet" class="view">
    <div id="walletLoggedOut" class="gate">
      <div class="icon">💳</div><h3>Belum masuk akun</h3><p>Masuk untuk melihat saldo & transfer.</p>
      <button class="btn warm block" id="walletOpenAuthBtn">Masuk / Daftar</button>
    </div>
    <div id="walletLoggedIn" style="display:none;">
      <div class="wallet-card">
        <div class="lbl">Saldo NodeLink</div>
        <div class="amt" id="walletBalance">Rp0</div>
        <div class="wallet-actions">
          <button class="btn small" id="topupBtn">+ Isi saldo (simulasi)</button>
          <button class="btn ghost small" id="openTransferBtn">Transfer ke pengguna lain</button>
        </div>
      </div>
      <div class="disclaimer">💡 Saldo di sini <b>uang simulasi lokal</b>, bukan uang sungguhan. Tapi transfer antar akun demo & pembayaran pakai saldo ini <b>benar-benar mengubah angka saldo real-time</b> di antara akun-akun di perangkat ini — cocok buat latihan alur dompet digital.</div>

      <div class="panel" id="transferPanel" style="display:none;">
        <h3>Transfer saldo</h3>
        <div class="form-row full"><div><label>Email penerima</label><input type="text" id="transferEmail" placeholder="email@penerima.com"></div></div>
        <div class="form-row">
          <div><label>Jumlah (Rp)</label><input type="number" id="transferAmount" placeholder="50000"></div>
          <div><label>Catatan (opsional)</label><input type="text" id="transferNote" placeholder="cth. Bayar utang"></div>
        </div>
        <button class="btn warm block" id="submitTransferBtn">Kirim transfer</button>
      </div>

      <div class="panel">
        <h3>Riwayat transaksi</h3>
        <div id="walletTxList"></div>
      </div>
    </div>
  </section>

  <!-- ============ WISHLIST ============ -->
  <section id="view-wishlist" class="view">
    <div class="section-title"><h2>Wishlist</h2></div>
    <div class="grid" id="wishlistGrid"></div>
  </section>

  <!-- ============ TOKO SAYA ============ -->
  <section id="view-store" class="view">
    <div class="section-title"><h2>Toko saya</h2><span id="storeStatusBadge"></span></div>
    <div class="panel">
      <h3 id="storeFormTitle">Tambah produk untuk dijual</h3>
      <input type="hidden" id="editProdId">
      <div class="form-row">
        <div><label>Nama produk</label><input id="pName" placeholder="cth. Voucher Diamond MLBB"></div>
        <div><label>Ikon (emoji)</label><input id="pIcon" placeholder="💎" maxlength="2"></div>
      </div>
      <div class="form-row full">
        <div>
          <label>Deskripsi</label>
          <textarea id="pDesc" placeholder="Deskripsi produk..."></textarea>
          <button class="btn ghost small" id="genDescBtn" type="button" style="margin-top:6px;">✨ Bantu tulis deskripsi</button>
        </div>
      </div>
      <div id="descGenBox" style="display:none;background:var(--bg-elev);border:1px solid var(--border);border-radius:10px;padding:12px;margin-bottom:10px;">
        <label>Pilih gaya & kelebihan produk, lalu klik "Buat teks"</label>
        <select id="genTone" style="margin-bottom:8px;">
          <option value="santai">Gaya santai</option>
          <option value="profesional">Gaya profesional</option>
          <option value="menjual">Gaya menjual (persuasif)</option>
        </select>
        <div style="display:flex;gap:6px;flex-wrap:wrap;margin-bottom:10px;" id="genTraits"></div>
        <button class="btn small" id="applyGenDescBtn" type="button">Buat teks</button>
      </div>
      <div class="form-row">
        <div><label>Harga (Rp)</label><input id="pPrice" type="number" placeholder="150000"></div>
        <div><label>Stok</label><input id="pStock" type="number" placeholder="10"></div>
      </div>
      <div style="display:flex;gap:8px;margin-top:6px;">
        <button class="btn warm" id="saveProdBtn">Simpan produk</button>
        <button class="btn ghost" id="cancelProdBtn" style="display:none;">Batal</button>
      </div>
    </div>
    <div class="panel">
      <h3>Produk saya (<span id="myProdCount">0</span>)</h3>
      <div id="myProdList"></div>
    </div>
  </section>

  <!-- ============ PESANAN ============ -->
  <section id="view-orders" class="view">
    <div class="section-title"><h2>Pesanan</h2></div>
    <div class="modal-tabs" style="max-width:280px;">
      <button data-order-tab="buy" class="active">Sebagai pembeli</button>
      <button data-order-tab="sell">Sebagai penjual</button>
    </div>
    <div id="ordersList"></div>
  </section>

  <!-- ============ NOTIFIKASI ============ -->
  <section id="view-notif" class="view">
    <div class="section-title"><h2>Notifikasi</h2><button class="btn ghost small" id="markAllReadBtn">Tandai semua dibaca</button></div>
    <div class="disclaimer">📩 Kotak notifikasi <b>di dalam aplikasi</b>, mensimulasikan email "sudah beli"/"pembayaran masuk". Email sungguhan butuh layanan seperti EmailJS + server.</div>
    <div id="notifList"></div>
  </section>

  <!-- ============ PROFIL ============ -->
  <section id="view-profile" class="view">
    <div id="profileLoggedOut" class="gate">
      <div class="icon">👤</div><h3>Belum masuk akun</h3><p>Masuk atau daftar untuk mulai jual-beli.</p>
      <button class="btn warm block" id="openAuthBtn">Masuk / Daftar</button>
    </div>
    <div id="profileLoggedIn" style="display:none;">
      <div class="panel" style="text-align:center;">
        <div class="avatar-btn" style="width:56px;height:56px;font-size:22px;margin:0 auto 10px;border-radius:16px;" id="profileAvatar">?</div>
        <h3 id="profileName" style="margin:0 0 4px;"></h3>
        <div id="profileEmail" class="mono" style="color:var(--text-dim);font-size:12px;margin-bottom:10px;"></div>
        <div id="profileLevelBadge" style="margin-bottom:8px;"></div>
        <div id="profileVerifyBadge"></div>
      </div>

      <div class="panel">
        <h3>🔗 Kode affiliate kamu</h3>
        <p style="color:var(--text-dim);font-size:12.5px;margin:0 0 4px;">Bagikan kode ini — kalau orang beli produk apa pun setelah pakai kodemu, kamu dapat komisi 5% otomatis masuk ke saldo.</p>
        <div class="ref-row"><span>Kode:</span> <span class="code mono" id="myRefCode">—</span><button class="btn ghost small" id="copyRefBtn" style="margin-left:auto;">Salin</button></div>
        <div class="hint" id="affiliateEarnings"></div>
      </div>

      <div class="panel">
        <h3>💬 Nomor WhatsApp</h3>
        <p style="color:var(--text-dim);font-size:12.5px;margin:0 0 10px;">Diisi supaya pembeli bisa chat kamu langsung dari halaman produk.</p>
        <div class="form-row full"><div><input type="text" id="waNumberInput" placeholder="cth. 6281234567890 (kode negara, tanpa +/0 di depan)"></div></div>
        <button class="btn ghost small" id="saveWaBtn" style="margin-top:8px;">Simpan nomor</button>
      </div>

      <div class="panel">
        <h3>🔳 QRIS pembayaran asli</h3>
        <p style="color:var(--text-dim);font-size:12.5px;margin:0 0 10px;line-height:1.6;">Tempel link gambar QRIS asli kamu (dari DANA/GoPay/OVO/bank, di-upload ke <span class="mono" style="color:var(--accent);">imgur.com</span> dulu). Kalau diisi, pembeli akan lihat QR ASLI ini saat checkout — bukan QR simulasi.</p>
        <div class="form-row full"><div><input type="text" id="qrisUrlInput" placeholder="https://i.imgur.com/xxxxx.jpg"></div></div>
        <button class="btn ghost small" id="saveQrisBtn" style="margin-top:8px;">Simpan QRIS</button>
        <div class="hint">⚠️ QR ini statis (nominal tidak otomatis) — pembeli scan lalu ketik sendiri nominalnya, dan tetap perlu tap "sudah bayar" secara manual. Uang benar-benar masuk ke akun DANA/GoPay/bank kamu, tapi konfirmasi di web ini masih manual (self-report).</div>
      </div>

      <div class="panel">
        <h3>Verifikasi toko</h3>
        <p style="color:var(--text-dim);font-size:12.5px;margin:0 0 12px;">Toko terverifikasi tampil dengan lencana ✓, meningkatkan kepercayaan pembeli.</p>
        <div id="verifyArea"></div>
      </div>

      <div class="panel">
        <h3>📧 Notifikasi via email asli</h3>
        <p style="color:var(--text-dim);font-size:12.5px;margin:0 0 12px;line-height:1.6;">
          Supaya notifikasi "pembayaran berhasil"/"pesanan baru" juga masuk ke email sungguhan (bukan cuma inbox dalam app), hubungkan akun <b style="color:var(--text);">EmailJS</b> gratis kamu:<br>
          1. Daftar di <span class="mono" style="color:var(--accent);">emailjs.com</span> → hubungkan Gmail/email kamu sebagai "Email Service"<br>
          2. Buat "Email Template" dengan variabel: <span class="mono">{{to_email}}</span> <span class="mono">{{to_name}}</span> <span class="mono">{{subject}}</span> <span class="mono">{{message}}</span><br>
          3. Salin Public Key, Service ID, dan Template ID ke bawah ini
        </p>
        <div class="form-row full"><div><label>Public Key</label><input id="ejPublicKey" placeholder="cth. AbCdEfGhIjKlMnOp"></div></div>
        <div class="form-row">
          <div><label>Service ID</label><input id="ejServiceId" placeholder="cth. service_xxxx"></div>
          <div><label>Template ID</label><input id="ejTemplateId" placeholder="cth. template_xxxx"></div>
        </div>
        <label style="display:flex;align-items:center;gap:8px;margin:10px 0;font-size:12.5px;color:var(--text);cursor:pointer;">
          <input type="checkbox" id="ejEnabled" style="width:auto;"> Aktifkan pengiriman email asli
        </label>
        <div style="display:flex;gap:8px;">
          <button class="btn warm" id="saveEmailCfgBtn">Simpan pengaturan</button>
          <button class="btn ghost" id="testEmailBtn">Kirim email tes</button>
        </div>
        <div class="hint" id="emailCfgStatus"></div>
      </div>
      <div class="panel">
        <h3>Ganti tampilan mode</h3>
        <button class="btn ghost" id="profileThemeBtn">Ganti mode sekarang</button>
      </div>
      <div class="panel"><button class="btn danger block" id="logoutBtn">Keluar akun</button></div>
    </div>
  </section>

  <!-- ============ ADMIN ============ -->
  <section id="view-admin" class="view">
    <div id="adminGate" class="gate">
      <div class="icon">🔒</div><h3>Area admin</h3><p>Kelola verifikasi toko, semua pesanan, dan data platform.</p>
      <input type="password" id="adminPassInput" placeholder="Password admin">
      <button class="btn warm block" id="adminLoginBtn" style="margin-top:10px;">Masuk</button>
      <div class="hint">Default: <b>admin123</b> — bisa diganti setelah login.</div>
    </div>
    <div id="adminPanelWrap" style="display:none;">
      <div class="section-title"><h2>Panel admin</h2><button class="btn ghost small" id="adminLogoutBtn">Keluar</button></div>
      <div class="stat-grid">
        <div class="stat"><div class="n" id="admStatUsers">0</div><div class="l">Pengguna</div></div>
        <div class="stat"><div class="n" id="admStatProd">0</div><div class="l">Produk aktif</div></div>
        <div class="stat"><div class="n" id="admStatOrders">0</div><div class="l">Total pesanan</div></div>
        <div class="stat"><div class="n" id="admStatRevenue">Rp0</div><div class="l">Nilai transaksi dibayar</div></div>
      </div>
      <div class="panel"><h3>Pengajuan verifikasi toko</h3><div id="verifyRequestList"></div></div>
      <div class="panel"><h3>Semua pesanan</h3><div id="adminOrderList"></div></div>
      <div class="panel">
        <h3>Ganti password admin</h3>
        <div class="form-row"><div><input id="newAdminPass" type="password" placeholder="Password baru (min 4 karakter)"></div></div>
        <button class="btn ghost" id="changeAdminPassBtn">Update password</button>
      </div>
      <div class="panel">
        <h3>Reset semua data</h3>
        <p style="color:var(--text-dim);font-size:12.5px;margin:0 0 12px;">Menghapus semua akun, produk, pesanan, dan notifikasi.</p>
        <button class="btn danger" id="resetAllBtn">Hapus semua data</button>
      </div>
    </div>
  </section>

</main>

<footer>NodeLink Marketplace · demo eksperimen next-gen UI, 100% berjalan lokal di browser, tanpa server.</footer>

<div class="overlay hidden" id="authOverlay">
  <div class="modal">
    <div class="modal-tabs">
      <button data-auth-tab="login" class="active">Masuk</button>
      <button data-auth-tab="register">Daftar</button>
    </div>
    <div id="authLoginForm">
      <h3>Masuk ke akun</h3><p>Data akun disimpan lokal di perangkat ini (demo).</p>
      <div class="form-row full"><div><label>Email</label><input id="loginEmail" type="email" placeholder="kamu@email.com"></div></div>
      <div class="form-row full"><div><label>Password</label><input id="loginPass" type="password" placeholder="Password"></div></div>
      <button class="btn warm block" id="doLoginBtn">Masuk</button>
    </div>
    <div id="authRegisterForm" style="display:none;">
      <h3>Buat akun baru</h3><p>Simulasi — jangan gunakan password asli kamu.</p>
      <div class="form-row full"><div><label>Nama</label><input id="regName" placeholder="Nama kamu"></div></div>
      <div class="form-row full"><div><label>Email</label><input id="regEmail" type="email" placeholder="kamu@email.com"></div></div>
      <div class="form-row full"><div><label>Password</label><input id="regPass" type="password" placeholder="Buat password"></div></div>
      <button class="btn warm block" id="doRegisterBtn">Daftar &amp; masuk</button>
    </div>
  </div>
</div>

<div class="overlay hidden" id="checkoutOverlay"><div class="modal" id="checkoutModalBody"></div></div>
<div class="overlay hidden" id="notifOverlay"><div class="modal" id="notifModalBody"></div></div>
<div class="overlay hidden" id="productOverlay"><div class="modal" id="productModalBody"></div></div>

<div class="toast" id="toast"></div>

<script>
(function(){
"use strict";

/* ============ storage helpers ============ */
const LS = { get(k,fb){ try{ const v=localStorage.getItem(k); return v===null?fb:JSON.parse(v);}catch(e){return fb;} }, set(k,v){ localStorage.setItem(k, JSON.stringify(v)); } };
function uid(p){ return p + Math.random().toString(36).slice(2,9); }
function rupiah(n){ return "Rp" + Number(n||0).toLocaleString("id-ID"); }
function escapeHtml(s){ return String(s==null?"":s).replace(/[&<>"']/g, c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c])); }
function timeAgo(ts){ return new Date(ts).toLocaleString("id-ID",{day:"2-digit",month:"short",hour:"2-digit",minute:"2-digit"}); }
function toast(msg){ const t=document.getElementById("toast"); t.textContent=msg; t.classList.add("show"); clearTimeout(t._timer); t._timer=setTimeout(()=>t.classList.remove("show"),1900); }

/* ============ seed data ============ */
let users = LS.get("nlm2_users", null);
let products = LS.get("nlm2_products", null);
let orders = LS.get("nlm2_orders", []);
let notifications = LS.get("nlm2_notifications", []);
let reviews = LS.get("nlm2_reviews", []);
let wishlist = LS.get("nlm2_wishlist", []);
let adminPass = LS.get("nlm2_admin_pass", "admin123");
let currentUserId = LS.get("nlm2_session", null);

if(!users){
  users = [
    {id:"seed-u1", name:"Toko Jaringan Arif", email:"seller1@demo.com", password:"demo1234", verified:true, verifyStatus:"approved", createdAt: Date.now()-1000*60*60*24*20, balance:750000, refCode:"arif-net", whatsapp:"6281234567890"},
    {id:"seed-u2", name:"Warung Diamond", email:"seller2@demo.com", password:"demo1234", verified:false, verifyStatus:"none", createdAt: Date.now()-1000*60*60*24*10, balance:200000, refCode:"warungdiamond", whatsapp:""},
    {id:"seed-u3", name:"Budi Santoso", email:"budi@demo.com", password:"demo1234", verified:false, verifyStatus:"none", createdAt: Date.now()-1000*60*60*24*5, balance:100000, refCode:"budi-santoso", whatsapp:""}
  ];
  LS.set("nlm2_users", users);
}
// migrasi: isi field baru (refCode/balance/whatsapp/qrisUrl) untuk akun lama yang dibuat sebelum fitur ini ada
let _migrated = false;
users.forEach(u=>{
  if(!u.refCode){ const slug = u.name.toLowerCase().replace(/[^a-z0-9]+/g,"-").replace(/(^-|-$)/g,"")||"user"; u.refCode = slug+"-"+Math.floor(Math.random()*900+100); _migrated=true; }
  if(u.balance===undefined){ u.balance = 0; _migrated=true; }
  if(u.whatsapp===undefined){ u.whatsapp = ""; _migrated=true; }
  if(u.qrisUrl===undefined){ u.qrisUrl = ""; _migrated=true; }
});
if(_migrated) LS.set("nlm2_users", users);
let walletTx = LS.get("nlm2_wallet_tx", []);
// capture ?ref= from URL for affiliate attribution
(function captureRef(){
  const params = new URLSearchParams(window.location.search);
  const r = params.get("ref");
  if(r) sessionStorage.setItem("nlm2_active_ref", r);
})();
if(!products){
  products = [
    {id:"seed-p1", sellerId:"seed-u1", name:"Router MikroTik hEX", icon:"📡", desc:"Router entry-level, cocok untuk lab MTCNA & kantor kecil.", price:850000, stock:5, createdAt:Date.now()-1000*60*60*24*9},
    {id:"seed-p2", sellerId:"seed-u1", name:"Kabel UTP Cat6 (305m)", icon:"🔌", desc:"Kabel gigabit untuk instalasi indoor.", price:1250000, stock:8, createdAt:Date.now()-1000*60*60*24*7},
    {id:"seed-p3", sellerId:"seed-u2", name:"Voucher Diamond MLBB", icon:"💎", desc:"Top up diamond, proses instan.", price:150000, stock:99, createdAt:Date.now()-1000*60*60*24*3},
    {id:"seed-p4", sellerId:"seed-u2", name:"E-book Persiapan MTCNA", icon:"📘", desc:"Rangkuman materi & soal latihan MTCNA.", price:75000, stock:50, createdAt:Date.now()-1000*60*60*24*1},
    {id:"seed-p5", sellerId:"seed-u3", name:"Access Point TP-Link", icon:"📶", desc:"AP indoor untuk perluasan jaringan WiFi rumah/kantor.", price:320000, stock:12, createdAt:Date.now()-1000*60*60*12}
  ];
  LS.set("nlm2_products", products);
}
if(orders.length===0){
  // seed a few historical orders so ticker/rating/trending has data
  orders = [
    {id:"seed-o1", buyerId:"seed-u3", sellerId:"seed-u1", productId:"seed-p1", productName:"Router MikroTik hEX", unitPrice:850000, total:850000, method:"qris", status:"selesai", createdAt:Date.now()-1000*60*60*30, paidAt:Date.now()-1000*60*60*30},
    {id:"seed-o2", buyerId:"seed-u2", sellerId:"seed-u2", productId:"seed-p3", productName:"Voucher Diamond MLBB", unitPrice:150000, total:150000, method:"gopay", status:"selesai", createdAt:Date.now()-1000*60*60*5, paidAt:Date.now()-1000*60*60*5},
  ];
  reviews = [
    {id:"seed-r1", productId:"seed-p1", buyerId:"seed-u3", orderId:"seed-o1", rating:5, comment:"Router cepat sampai, sudah aku pakai buat lab MTCNA. Mantap!", createdAt:Date.now()-1000*60*60*20},
  ];
  LS.set("nlm2_orders", orders); LS.set("nlm2_reviews", reviews);
}

function saveAll(){ LS.set("nlm2_users",users); LS.set("nlm2_products",products); LS.set("nlm2_orders",orders); LS.set("nlm2_notifications",notifications); LS.set("nlm2_reviews",reviews); LS.set("nlm2_wishlist",wishlist); LS.set("nlm2_wallet_tx",walletTx); }

/* ============ wallet ============ */
function addWalletTx(userId, type, amount, counterpart, note){
  walletTx.unshift({id:uid("tx"), userId, type, amount, counterpart, note: note||"", createdAt: Date.now()});
  LS.set("nlm2_wallet_tx", walletTx);
}
function creditBalance(userId, amount){ const u=users.find(x=>x.id===userId); if(u) u.balance=(u.balance||0)+amount; }
function debitBalance(userId, amount){ const u=users.find(x=>x.id===userId); if(u) u.balance=Math.max(0,(u.balance||0)-amount); }
function payWithWallet(buyer, seller, amount, note){
  if((buyer.balance||0) < amount) return false;
  debitBalance(buyer.id, amount); creditBalance(seller.id, amount);
  addWalletTx(buyer.id, "purchase", amount, seller.name, note);
  addWalletTx(seller.id, "sale", amount, buyer.name, note);
  return true;
}
function tryAffiliateCommission(order, product){
  const refCode = sessionStorage.getItem("nlm2_active_ref");
  if(!refCode) return;
  const referrer = users.find(u=>u.refCode===refCode);
  if(!referrer || referrer.id===order.buyerId || referrer.id===order.sellerId) return;
  const commission = Math.round(product.price * 0.05);
  creditBalance(referrer.id, commission);
  addWalletTx(referrer.id, "affiliate_commission", commission, product.name, "Komisi affiliate 5%");
  notifyUser(referrer.id, "Komisi affiliate masuk 💸", `Kamu dapat komisi ${rupiah(commission)} dari penjualan "${product.name}" lewat link affiliatemu.`, "payment");
}
function currentUser(){ return users.find(u=>u.id===currentUserId) || null; }
function pushNotif(userId,title,body,kind){ notifications.push({id:uid("n"),userId,title,body,kind:kind||"info",read:false,createdAt:Date.now()}); LS.set("nlm2_notifications",notifications); }

/* ============ email notifications (EmailJS) ============ */
let emailCfg = LS.get("nlm2_emailjs_cfg", {enabled:false, publicKey:"", serviceId:"", templateId:""});
let emailjsReady = false;
function ensureEmailjsInit(){
  if(emailjsReady) return;
  if(typeof emailjs === "undefined" || !emailCfg.publicKey) return;
  try{ emailjs.init({publicKey: emailCfg.publicKey}); emailjsReady = true; }catch(e){ console.warn("EmailJS init gagal", e); }
}
function sendRealEmail(toEmail, toName, subject, message){
  if(!emailCfg.enabled || !emailCfg.publicKey || !emailCfg.serviceId || !emailCfg.templateId) return;
  if(typeof emailjs === "undefined"){ console.warn("EmailJS SDK belum termuat (butuh koneksi internet)."); return; }
  ensureEmailjsInit();
  emailjs.send(emailCfg.serviceId, emailCfg.templateId, { to_email: toEmail, to_name: toName, subject, message })
    .then(()=>{ /* terkirim */ })
    .catch(err=>{ console.warn("Gagal kirim email:", err); toast("Notifikasi dalam-app tersimpan, tapi email gagal terkirim"); });
}
/* Panggil ini di semua titik notifikasi supaya juga terkirim ke email asli (kalau diaktifkan) */
function notifyUser(userId, title, body, kind){
  pushNotif(userId, title, body, kind);
  const u = users.find(x=>x.id===userId);
  if(u) sendRealEmail(u.email, u.name, title, body);
}

/* ============ background particle network canvas ============ */
const bgCanvas = document.getElementById("bgCanvas");
const bctx = bgCanvas.getContext("2d");
let W,H,particles=[];
function themeAccent(){ return getComputedStyle(document.documentElement).getPropertyValue("--accent").trim() || "#22D3EE"; }
function resizeCanvas(){ W=bgCanvas.width=window.innerWidth; H=bgCanvas.height=window.innerHeight; }
window.addEventListener("resize", resizeCanvas); resizeCanvas();
function initParticles(){
  particles=[]; const count = Math.min(60, Math.floor(W*H/22000));
  for(let i=0;i<count;i++){ particles.push({x:Math.random()*W,y:Math.random()*H,vx:(Math.random()-.5)*.35,vy:(Math.random()-.5)*.35}); }
}
initParticles();
function drawBg(){
  bctx.clearRect(0,0,W,H);
  const accent = themeAccent();
  bctx.globalAlpha = .5;
  for(const p of particles){
    p.x+=p.vx; p.y+=p.vy;
    if(p.x<0||p.x>W) p.vx*=-1;
    if(p.y<0||p.y>H) p.vy*=-1;
  }
  for(let i=0;i<particles.length;i++){
    for(let j=i+1;j<particles.length;j++){
      const a=particles[i], b=particles[j];
      const d = Math.hypot(a.x-b.x, a.y-b.y);
      if(d<130){
        bctx.strokeStyle = accent; bctx.globalAlpha = (1-d/130)*.15;
        bctx.lineWidth=1; bctx.beginPath(); bctx.moveTo(a.x,a.y); bctx.lineTo(b.x,b.y); bctx.stroke();
      }
    }
  }
  bctx.globalAlpha=.6;
  for(const p of particles){ bctx.fillStyle=accent; bctx.beginPath(); bctx.arc(p.x,p.y,1.6,0,7); bctx.fill(); }
  requestAnimationFrame(drawBg);
}
drawBg();

/* ============ confetti ============ */
const confettiCanvas = document.getElementById("confettiCanvas");
const cctx = confettiCanvas.getContext("2d");
confettiCanvas.width = window.innerWidth; confettiCanvas.height = window.innerHeight;
window.addEventListener("resize", ()=>{ confettiCanvas.width=window.innerWidth; confettiCanvas.height=window.innerHeight; });
function fireConfetti(){
  const colors = ["#22D3EE","#A78BFA","#F59E0B","#34D399","#F87171"];
  let parts = [];
  for(let i=0;i<80;i++){
    parts.push({x:confettiCanvas.width/2,y:confettiCanvas.height*.3,vx:(Math.random()-.5)*9,vy:Math.random()*-7-2,g:.25,size:Math.random()*5+3,color:colors[Math.floor(Math.random()*colors.length)],life:100});
  }
  function tick(){
    cctx.clearRect(0,0,confettiCanvas.width,confettiCanvas.height);
    parts.forEach(p=>{ p.x+=p.vx; p.y+=p.vy; p.vy+=p.g; p.life--; cctx.globalAlpha=Math.max(p.life/100,0); cctx.fillStyle=p.color; cctx.fillRect(p.x,p.y,p.size,p.size); });
    parts = parts.filter(p=>p.life>0);
    if(parts.length>0) requestAnimationFrame(tick); else cctx.clearRect(0,0,confettiCanvas.width,confettiCanvas.height);
  }
  tick();
}

/* ============ typing hero effect ============ */
(function typeLoop(){
  const words = ["masa depan.", "level berikutnya.", "tanpa batas.", "yang cerdas."];
  const target = document.querySelector(".hero h1");
  const staticPart = "Belanja teknologi ";
  let wi=0, ci=0, deleting=false;
  function step(){
    const w = words[wi];
    ci += deleting? -1 : 1;
    target.innerHTML = staticPart + w.slice(0,ci) + '<span class="typecursor"></span>';
    let delay = deleting? 35 : 70;
    if(!deleting && ci===w.length){ delay=1400; deleting=true; }
    else if(deleting && ci===0){ deleting=false; wi=(wi+1)%words.length; delay=300; }
    setTimeout(step, delay);
  }
  step();
})();

/* ============ theme ============ */
function applyTheme(t){ document.documentElement.setAttribute("data-theme",t); document.getElementById("themeToggle").textContent = t==="dark"?"🌙":"☀️"; LS.set("nlm2_theme",t); }
let theme = LS.get("nlm2_theme","dark"); applyTheme(theme);
document.getElementById("themeToggle").addEventListener("click", ()=>{ theme = theme==="dark"?"light":"dark"; applyTheme(theme); });
document.getElementById("profileThemeBtn").addEventListener("click", ()=>{ theme = theme==="dark"?"light":"dark"; applyTheme(theme); toast("Mode diganti ke "+(theme==="dark"?"gelap":"terang")); });

/* ============ nav ============ */
const tabs = document.querySelectorAll("nav.tabs button");
const views = document.querySelectorAll(".view");
tabs.forEach(btn=>btn.addEventListener("click", ()=>switchTab(btn.dataset.view)));
document.getElementById("bellBtn").addEventListener("click", ()=>switchTab("notif"));
document.getElementById("wishlistBtn").addEventListener("click", ()=>switchTab("wishlist"));
document.getElementById("avatarBtn").addEventListener("click", ()=>switchTab("profile"));
function switchTab(name){ tabs.forEach(b=>b.classList.toggle("active", b.dataset.view===name)); views.forEach(v=>v.classList.remove("active")); document.getElementById("view-"+name).classList.add("active"); renderAll(); }

/* ============ auth ============ */
const authOverlay = document.getElementById("authOverlay");
document.getElementById("openAuthBtn").addEventListener("click", ()=>authOverlay.classList.remove("hidden"));
authOverlay.addEventListener("click", e=>{ if(e.target===authOverlay) authOverlay.classList.add("hidden"); });
document.querySelectorAll("[data-auth-tab]").forEach(btn=>{
  btn.addEventListener("click", ()=>{
    document.querySelectorAll("[data-auth-tab]").forEach(b=>b.classList.remove("active")); btn.classList.add("active");
    const isLogin = btn.dataset.authTab==="login";
    document.getElementById("authLoginForm").style.display = isLogin?"block":"none";
    document.getElementById("authRegisterForm").style.display = isLogin?"none":"block";
  });
});
document.getElementById("doLoginBtn").addEventListener("click", ()=>{
  const email = document.getElementById("loginEmail").value.trim().toLowerCase();
  const pass = document.getElementById("loginPass").value;
  const u = users.find(x=>x.email.toLowerCase()===email && x.password===pass);
  if(!u){ toast("Email atau password salah"); return; }
  currentUserId=u.id; LS.set("nlm2_session",currentUserId); authOverlay.classList.add("hidden");
  toast("Selamat datang, "+u.name); renderAll();
});
document.getElementById("doRegisterBtn").addEventListener("click", ()=>{
  const name=document.getElementById("regName").value.trim();
  const email=document.getElementById("regEmail").value.trim().toLowerCase();
  const pass=document.getElementById("regPass").value;
  if(!name||!email||pass.length<4){ toast("Lengkapi semua field, password min 4 karakter"); return; }
  if(users.some(u=>u.email.toLowerCase()===email)){ toast("Email sudah terdaftar"); return; }
  const refSlug = name.toLowerCase().replace(/[^a-z0-9]+/g,"-").replace(/(^-|-$)/g,"") || "user";
  const u={id:uid("u"),name,email,password:pass,verified:false,verifyStatus:"none",createdAt:Date.now(),balance:0,refCode:refSlug+"-"+Math.floor(Math.random()*900+100),whatsapp:""};
  users.push(u); currentUserId=u.id; LS.set("nlm2_session",currentUserId); saveAll();
  notifyUser(u.id,"Akun berhasil dibuat","Selamat datang di NodeLink Marketplace, "+name+"!","welcome");
  authOverlay.classList.add("hidden"); toast("Akun dibuat, langsung masuk"); renderAll();
});
document.getElementById("logoutBtn").addEventListener("click", ()=>{ currentUserId=null; localStorage.removeItem("nlm2_session"); toast("Berhasil keluar"); switchTab("market"); });
function requireLogin(){ if(!currentUser()){ authOverlay.classList.remove("hidden"); return false; } return true; }

/* ============ derived data helpers ============ */
function productRating(pid){
  const rs = reviews.filter(r=>r.productId===pid);
  if(rs.length===0) return {avg:0,count:0};
  const avg = rs.reduce((s,r)=>s+r.rating,0)/rs.length;
  return {avg, count:rs.length};
}
function productOrderCount(pid){ return orders.filter(o=>o.productId===pid).length; }
function starString(avg){
  const full = Math.round(avg);
  return "★".repeat(full) + "☆".repeat(5-full);
}
function isWished(uid_, pid){ return wishlist.some(w=>w.userId===uid_ && w.productId===pid); }
function toggleWish(pid){
  if(!requireLogin()) return;
  const u = currentUser();
  const idx = wishlist.findIndex(w=>w.userId===u.id && w.productId===pid);
  if(idx>-1){ wishlist.splice(idx,1); toast("Dihapus dari wishlist"); } else { wishlist.push({userId:u.id, productId:pid}); toast("Ditambahkan ke wishlist ❤"); }
  LS.set("nlm2_wishlist", wishlist);
  renderAll();
}
function buyerLevel(uid_){
  const cnt = orders.filter(o=>o.buyerId===uid_ && (o.status==="dibayar"||o.status==="selesai")).length;
  if(cnt>=10) return {label:"Platinum", icon:"💠"};
  if(cnt>=5) return {label:"Gold", icon:"🥇"};
  if(cnt>=2) return {label:"Silver", icon:"🥈"};
  return {label:"Pemula", icon:"🌱"};
}

/* ============ animated counters ============ */
function animateCount(el, target){
  const start = 0; const dur=900; const t0=performance.now();
  function step(t){
    const p = Math.min((t-t0)/dur,1);
    el.textContent = Math.floor(start + (target-start)*(1-Math.pow(1-p,3)));
    if(p<1) requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}

/* ============ ticker ============ */
function renderTicker(){
  const track = document.getElementById("tickerTrack");
  const recent = [...orders].sort((a,b)=>b.createdAt-a.createdAt).slice(0,10);
  if(recent.length===0){ track.innerHTML = '<span>Belum ada transaksi. Jadilah yang pertama! 🚀</span>'; return; }
  const items = recent.map(o=>{
    const buyer = users.find(u=>u.id===o.buyerId);
    return `<span>🛒 <b>${buyer?escapeHtml(buyer.name):"Seseorang"}</b> baru saja membeli ${escapeHtml(o.productName)} · ${rupiah(o.total)}</span>`;
  });
  track.innerHTML = items.join("") + items.join(""); // duplicate for seamless loop
}

/* ============ marketplace ============ */
let searchQ = "", sortBy = "terbaru";
document.getElementById("searchInput").addEventListener("input", e=>{ searchQ = e.target.value.toLowerCase(); renderMarket(); });
document.getElementById("sortSelect").addEventListener("change", e=>{ sortBy = e.target.value; renderMarket(); });

function getSortedFiltered(list){
  let arr = list.filter(p=>p.name.toLowerCase().includes(searchQ));
  if(sortBy==="terbaru") arr.sort((a,b)=>b.createdAt-a.createdAt);
  else if(sortBy==="terlaris") arr.sort((a,b)=>productOrderCount(b.id)-productOrderCount(a.id));
  else if(sortBy==="murah") arr.sort((a,b)=>a.price-b.price);
  else if(sortBy==="mahal") arr.sort((a,b)=>b.price-a.price);
  else if(sortBy==="rating") arr.sort((a,b)=>productRating(b.id).avg-productRating(a.id).avg);
  return arr;
}
function productCardHtml(p){
  const seller = users.find(u=>u.id===p.sellerId);
  const verifiedBadge = seller && seller.verified ? '<span class="badge-verified">✓ terverifikasi</span>' : "";
  const oc = productOrderCount(p.id);
  const trending = oc>=2 ? '<span class="badge-trending">🔥 Trending</span>' : "";
  const rating = productRating(p.id);
  const u = currentUser();
  const wished = u ? isWished(u.id,p.id) : false;
  const gradients = ["linear-gradient(135deg,#22D3EE,#0E7490)","linear-gradient(135deg,#A78BFA,#6D28D9)","linear-gradient(135deg,#F59E0B,#B45309)","linear-gradient(135deg,#34D399,#047857)","linear-gradient(135deg,#F87171,#B91C1C)"];
  const grad = gradients[Math.abs(p.id.split("").reduce((a,c)=>a+c.charCodeAt(0),0)) % gradients.length];
  const myRef = u ? u.refCode : null;
  const affLink = myRef ? `?product=${p.id}&ref=${myRef}` : "";
  return `
  <div class="card">
    <div class="prod-tile" style="background:${grad};">${p.icon||"🔗"}</div>
    <div class="card-top" style="margin-top:2px;">
      <span></span>
      <button class="heart-btn ${wished?'active':''}" data-wish="${p.id}">${wished?'❤':'🤍'}</button>
    </div>
    <h3 data-view-prod="${p.id}" style="cursor:pointer;">${escapeHtml(p.name)}</h3>
    <p class="desc">${escapeHtml(p.desc||"")}</p>
    <div class="seller">${seller?escapeHtml(seller.name):"Toko"} ${verifiedBadge} ${trending}</div>
    ${rating.count>0 ? `<div class="stars">${starString(rating.avg)}<span class="cnt">(${rating.count})</span></div>` : `<div class="stars" style="color:var(--text-dim);">Belum ada ulasan</div>`}
    <div class="price">${rupiah(p.price)}</div>
    <div class="stock">Stok: ${p.stock}</div>
    <button class="btn warm" data-buy="${p.id}" ${p.stock<=0?"disabled":""}>${p.stock<=0?"Stok habis":"Beli sekarang"}</button>
    <div style="display:flex;gap:6px;">
      ${seller&&seller.whatsapp ? `<button class="btn wa small" style="flex:1;" data-wa="${p.id}">💬 Chat</button>` : ""}
      ${u ? `<button class="btn ghost small" style="flex:1;" data-share="${p.id}">📤 Bagikan</button>` : ""}
    </div>
    ${myRef && seller && seller.id!==u.id ? `<div class="hint" style="margin-top:0;">Kode affiliate aktif: <b class="mono" style="color:var(--warm);">${myRef}</b></div>` : ""}
  </div>`;
}
function bindCardEvents(scopeEl){
  scopeEl.querySelectorAll("[data-buy]").forEach(btn=>btn.addEventListener("click", ()=>openCheckout(btn.getAttribute("data-buy"))));
  scopeEl.querySelectorAll("[data-wish]").forEach(btn=>btn.addEventListener("click", (e)=>{ e.stopPropagation(); toggleWish(btn.getAttribute("data-wish")); }));
  scopeEl.querySelectorAll("[data-wa]").forEach(btn=>btn.addEventListener("click", (e)=>{
    e.stopPropagation();
    const p = products.find(x=>x.id===btn.getAttribute("data-wa")); const seller = users.find(x=>x.id===p.sellerId);
    if(!seller.whatsapp){ toast("Penjual belum menambahkan nomor WhatsApp"); return; }
    window.open(waLink(seller.whatsapp, `Halo ${seller.name}, saya tertarik dengan produk "${p.name}" (${rupiah(p.price)}) di NodeLink Marketplace. Masih tersedia?`), "_blank");
  }));
  scopeEl.querySelectorAll("[data-share]").forEach(btn=>btn.addEventListener("click", (e)=>{
    e.stopPropagation();
    if(!requireLogin()) return;
    const p = products.find(x=>x.id===btn.getAttribute("data-share")); const u = currentUser();
    const msg = `Cek produk ini di NodeLink Marketplace: "${p.name}" seharga ${rupiah(p.price)}!\n\nPakai kode affiliate saya saat checkout: ${u.refCode}\n(Catatan: link otomatis hanya aktif kalau aplikasi ini sudah di-hosting online.)`;
    window.open(`https://wa.me/?text=${encodeURIComponent(msg)}`, "_blank");
  }));
  scopeEl.querySelectorAll("[data-view-prod]").forEach(el=>el.addEventListener("click", ()=>openProductDetail(el.getAttribute("data-view-prod"))));
}
function renderMarket(){
  const list = getSortedFiltered(products);
  document.getElementById("marketCount").textContent = list.length + " produk";
  const grid = document.getElementById("marketGrid");
  grid.innerHTML = list.length===0 ? '<div class="empty">Tidak ada produk yang cocok.</div>' : list.map(productCardHtml).join("");
  bindCardEvents(grid);

  animateCount(document.getElementById("stTotalProd"), products.length);
  animateCount(document.getElementById("stTotalUsers"), users.length);
  animateCount(document.getElementById("stTotalOrders"), orders.length);
  document.getElementById("stTotalGmv").textContent = rupiah(orders.filter(o=>o.status==="dibayar"||o.status==="selesai").reduce((s,o)=>s+o.total,0));
}

function openProductDetail(pid){
  const p = products.find(x=>x.id===pid); if(!p) return;
  const rating = productRating(pid);
  const rs = reviews.filter(r=>r.productId===pid).sort((a,b)=>b.createdAt-a.createdAt);
  const body = document.getElementById("productModalBody");
  body.innerHTML = `
    <h3>${p.icon||"🔗"} ${escapeHtml(p.name)}</h3>
    <p>${escapeHtml(p.desc||"")}</p>
    <div class="stars" style="margin-bottom:10px;">${rating.count>0?starString(rating.avg)+`<span class="cnt">(${rating.count} ulasan)</span>`:"Belum ada ulasan"}</div>
    <div class="price" style="font-size:19px;margin-bottom:12px;">${rupiah(p.price)}</div>
    <h3 style="font-size:13.5px;">Ulasan pembeli</h3>
    <div id="prodReviewList">${rs.length===0?'<div class="empty">Belum ada ulasan.</div>':rs.map(r=>{
      const buyer = users.find(u=>u.id===r.buyerId);
      return `<div class="review-item"><span class="rstars">${starString(r.rating)}</span><span class="rname">${buyer?escapeHtml(buyer.name):"Pembeli"}</span><div class="rtext">${escapeHtml(r.comment)}</div></div>`;
    }).join("")}</div>
    <button class="btn warm block" id="closeProdBtn" style="margin-top:14px;">Tutup</button>
  `;
  document.getElementById("productOverlay").classList.remove("hidden");
  document.getElementById("closeProdBtn").addEventListener("click", ()=>document.getElementById("productOverlay").classList.add("hidden"));
}
document.getElementById("productOverlay").addEventListener("click", e=>{ if(e.target.id==="productOverlay") e.currentTarget.classList.add("hidden"); });

/* ============ wishlist view ============ */
function renderWishlist(){
  const u = currentUser();
  const grid = document.getElementById("wishlistGrid");
  if(!u){ grid.innerHTML = '<div class="empty">Masuk akun untuk melihat wishlist.</div>'; return; }
  const wp = wishlist.filter(w=>w.userId===u.id).map(w=>products.find(p=>p.id===w.productId)).filter(Boolean);
  document.getElementById("wishBadge").style.display = wp.length>0 ? "flex":"none";
  document.getElementById("wishBadge").textContent = wp.length>9?"9+":wp.length;
  grid.innerHTML = wp.length===0 ? '<div class="empty">Wishlist kosong. Tap ikon hati 🤍 di produk untuk menyimpannya di sini.</div>' : wp.map(productCardHtml).join("");
  bindCardEvents(grid);
}

/* ============ checkout ============ */
let checkoutState = null;
function openCheckout(productId){
  if(!requireLogin()) return;
  const p = products.find(x=>x.id===productId); if(!p) return;
  if(p.sellerId===currentUserId){ toast("Tidak bisa membeli produk toko sendiri"); return; }
  checkoutState = { productId, method:"qris", step:"method" };
  renderCheckoutModal();
  document.getElementById("checkoutOverlay").classList.remove("hidden");
}
document.getElementById("checkoutOverlay").addEventListener("click", e=>{ if(e.target.id==="checkoutOverlay") e.currentTarget.classList.add("hidden"); });
function renderCheckoutModal(){
  const body = document.getElementById("checkoutModalBody");
  const p = products.find(x=>x.id===checkoutState.productId); if(!p){ body.innerHTML=""; return; }
  const buyer = currentUser();
  const methods = [{id:"saldo",em:"💳",label:"Saldo"},{id:"qris",em:"🔳",label:"QRIS"},{id:"gopay",em:"🟢",label:"GoPay"},{id:"ovo",em:"🟣",label:"OVO"},{id:"dana",em:"🔵",label:"DANA"}];
  if(checkoutState.step==="method"){
    body.innerHTML = `
      <h3>Checkout</h3><p>${escapeHtml(p.name)} · <b class="mono">${rupiah(p.price)}</b></p>
      <label>Pilih metode pembayaran</label>
      <div class="method-grid">${methods.map(m=>`<button class="method-btn ${checkoutState.method===m.id?'active':''}" data-method="${m.id}"><span class="em">${m.em}</span>${m.label}</button>`).join("")}</div>
      ${checkoutState.method==="saldo" ? `<div class="hint">Saldo kamu: <b class="mono">${rupiah(buyer.balance||0)}</b> — dipotong langsung, real-time.</div>` : `<div class="hint">Simulasi tampilan pembayaran — tidak ada uang sungguhan yang berpindah.</div>`}
      <button class="btn warm block" id="proceedPayBtn" style="margin-top:10px;">Lanjut bayar</button>`;
    body.querySelectorAll("[data-method]").forEach(b=>b.addEventListener("click", ()=>{ checkoutState.method=b.getAttribute("data-method"); renderCheckoutModal(); }));
    document.getElementById("proceedPayBtn").addEventListener("click", ()=>{
      if(checkoutState.method==="saldo"){
        if((buyer.balance||0) < p.price){ toast("Saldo tidak cukup, isi saldo dulu di tab Dompet"); return; }
        finalizeOrder(p, "saldo", uid("o"));
        return;
      }
      checkoutState.step="pay"; renderCheckoutModal();
    });
  } else if(checkoutState.step==="pay"){
    const orderId = uid("o"); checkoutState.orderId = orderId;
    const seller = users.find(x=>x.id===p.sellerId);
    const hasRealQris = checkoutState.method==="qris" && seller && seller.qrisUrl;
    const payload = encodeURIComponent(`NODELINK-DEMO|order=${orderId}|item=${p.name}|total=${p.price}|method=${checkoutState.method}`);
    const qrUrl = hasRealQris ? seller.qrisUrl : `https://api.qrserver.com/v1/create-qr-code/?size=220x220&data=${payload}`;
    const methodLabel = methods.find(m=>m.id===checkoutState.method).label;
    body.innerHTML = `
      <h3>Scan untuk bayar (${methodLabel})</h3><p>Total: <b class="mono">${rupiah(p.price)}</b></p>
      <div class="qr-box"><img src="${qrUrl}" alt="QR pembayaran"></div>
      ${hasRealQris
        ? `<div class="hint" style="color:var(--success);">✅ Ini QRIS ASLI milik ${escapeHtml(seller.name)}. Scan pakai e-wallet/m-banking, lalu <b>ketik sendiri nominal ${rupiah(p.price)}</b>, dan bayar sungguhan.</div>`
        : `<div class="hint">QR ini berisi data simulasi demo, bukan kode pembayaran resmi bank/e-wallet.</div>`}
      <button class="btn warm block" id="confirmPayBtn" style="margin-top:12px;">Saya sudah bayar${hasRealQris?"":" (simulasi)"}</button>
      <button class="btn ghost block" id="backMethodBtn" style="margin-top:8px;">← Ganti metode</button>`;
    document.getElementById("backMethodBtn").addEventListener("click", ()=>{ checkoutState.step="method"; renderCheckoutModal(); });
    document.getElementById("confirmPayBtn").addEventListener("click", ()=>finalizeOrder(p, checkoutState.method, orderId));
  }
}
function waLink(number, text){ return `https://wa.me/${number.replace(/[^0-9]/g,"")}?text=${encodeURIComponent(text)}`; }
function finalizeOrder(p, method, orderId){
  const buyer = currentUser(); const seller = users.find(u=>u.id===p.sellerId);
  const order = {id:orderId,buyerId:buyer.id,sellerId:p.sellerId,productId:p.id,productName:p.name,unitPrice:p.price,total:p.price,method,status:"dibayar",createdAt:Date.now(),paidAt:Date.now()};
  orders.push(order);
  const idx = products.findIndex(x=>x.id===p.id); if(idx>-1 && products[idx].stock>0) products[idx].stock -= 1;
  if(method==="saldo") payWithWallet(buyer, seller, p.price, "Pembelian "+p.name);
  tryAffiliateCommission(order, p);
  saveAll();
  notifyUser(buyer.id,"Pembayaran berhasil ✅",`Pembayaranmu untuk "${p.name}" sebesar ${rupiah(p.price)} via ${method.toUpperCase()} sudah dikonfirmasi.`,"payment");
  notifyUser(seller.id,"Pesanan baru masuk 🛒",`${buyer.name} baru saja membeli "${p.name}" senilai ${rupiah(p.price)}.`,"order");
  document.getElementById("checkoutOverlay").classList.add("hidden");
  toast("Pembayaran dikonfirmasi 🎉"); fireConfetti();
  renderAll();
  if(seller.whatsapp){
    setTimeout(()=>{
      const msg = `Halo ${seller.name}, saya baru saja beli "${p.name}" (${rupiah(p.price)}) di NodeLink dengan order ID ${orderId}. Mohon diproses ya. Terima kasih!`;
      if(confirm("Kirim konfirmasi pesanan ke penjual via WhatsApp?")) window.open(waLink(seller.whatsapp, msg), "_blank");
    }, 400);
  }
}

/* ============ toko saya ============ */
function renderStore(){
  const wrap = document.getElementById("storeStatusBadge"); const u = currentUser();
  if(!u){ document.getElementById("myProdList").innerHTML = '<div class="empty">Masuk akun dulu untuk mengelola toko.</div>'; document.getElementById("myProdCount").textContent=0; wrap.innerHTML=""; return; }
  wrap.innerHTML = u.verified ? '<span class="badge-verified">✓ toko terverifikasi</span>' : (u.verifyStatus==="pending" ? '<span class="badge-pending">verifikasi diproses</span>' : "");
  const mine = products.filter(p=>p.sellerId===u.id);
  document.getElementById("myProdCount").textContent = mine.length;
  const list = document.getElementById("myProdList");
  list.innerHTML = mine.length===0 ? '<div class="empty">Belum ada produk. Tambahkan lewat form di atas.</div>' : mine.map(p=>`
    <div class="list-item">
      <div class="icon" style="width:32px;height:32px;font-size:15px;">${p.icon||"🔗"}</div>
      <div class="info"><div class="t">${escapeHtml(p.name)}</div><div class="m">${rupiah(p.price)} · stok ${p.stock} · ${productOrderCount(p.id)} terjual</div></div>
      <div class="list-actions"><button class="btn ghost small" data-edit-prod="${p.id}">Edit</button><button class="btn danger small" data-del-prod="${p.id}">Hapus</button></div>
    </div>`).join("");
  list.querySelectorAll("[data-edit-prod]").forEach(b=>b.addEventListener("click", ()=>editProduct(b.getAttribute("data-edit-prod"))));
  list.querySelectorAll("[data-del-prod]").forEach(b=>b.addEventListener("click", ()=>deleteProduct(b.getAttribute("data-del-prod"))));
}
function clearProdForm(){ document.getElementById("editProdId").value=""; document.getElementById("pName").value=""; document.getElementById("pIcon").value=""; document.getElementById("pDesc").value=""; document.getElementById("pPrice").value=""; document.getElementById("pStock").value=""; document.getElementById("storeFormTitle").textContent="Tambah produk untuk dijual"; document.getElementById("cancelProdBtn").style.display="none"; }
function editProduct(id){ const p=products.find(x=>x.id===id); if(!p) return; document.getElementById("editProdId").value=p.id; document.getElementById("pName").value=p.name; document.getElementById("pIcon").value=p.icon||""; document.getElementById("pDesc").value=p.desc||""; document.getElementById("pPrice").value=p.price; document.getElementById("pStock").value=p.stock; document.getElementById("storeFormTitle").textContent="Edit produk"; document.getElementById("cancelProdBtn").style.display="inline-block"; window.scrollTo({top:0,behavior:"smooth"}); }
function deleteProduct(id){ if(!confirm("Hapus produk ini?")) return; products=products.filter(p=>p.id!==id); saveAll(); renderAll(); }
document.getElementById("cancelProdBtn").addEventListener("click", clearProdForm);
document.getElementById("saveProdBtn").addEventListener("click", ()=>{
  if(!requireLogin()) return;
  const name=document.getElementById("pName").value.trim(); const price=Number(document.getElementById("pPrice").value)||0;
  if(!name||price<=0){ toast("Isi nama produk dan harga yang valid"); return; }
  const editId=document.getElementById("editProdId").value;
  const data={name,icon:document.getElementById("pIcon").value.trim()||"🔗",desc:document.getElementById("pDesc").value.trim(),price,stock:Number(document.getElementById("pStock").value)||0};
  if(editId){ const idx=products.findIndex(p=>p.id===editId); if(idx>-1) products[idx]={...products[idx],...data}; toast("Produk diperbarui"); }
  else { products.push({id:uid("p"),sellerId:currentUserId,createdAt:Date.now(),...data}); toast("Produk ditambahkan ke toko"); }
  saveAll(); clearProdForm(); renderAll();
});

/* ============ pesanan + reviews ============ */
let orderTab = "buy";
document.querySelectorAll("[data-order-tab]").forEach(btn=>btn.addEventListener("click", ()=>{ document.querySelectorAll("[data-order-tab]").forEach(b=>b.classList.remove("active")); btn.classList.add("active"); orderTab=btn.getAttribute("data-order-tab"); renderOrders(); }));
function renderOrders(){
  const u = currentUser(); const wrap = document.getElementById("ordersList");
  if(!u){ wrap.innerHTML = '<div class="empty">Masuk akun untuk melihat pesanan.</div>'; return; }
  const list = orders.filter(o=> orderTab==="buy" ? o.buyerId===u.id : o.sellerId===u.id).sort((a,b)=>b.createdAt-a.createdAt);
  if(list.length===0){ wrap.innerHTML = `<div class="empty">Belum ada pesanan sebagai ${orderTab==="buy"?"pembeli":"penjual"}.</div>`; return; }
  wrap.innerHTML = list.map(o=>{
    const other = users.find(x=> x.id === (orderTab==="buy" ? o.sellerId : o.buyerId));
    const hasReview = reviews.some(r=>r.orderId===o.id);
    const canReview = orderTab==="buy" && !hasReview && (o.status==="dibayar"||o.status==="selesai");
    return `<div class="list-item">
      <div class="icon" style="width:32px;height:32px;font-size:14px;">${o.method==='qris'?'🔳':o.method==='gopay'?'🟢':o.method==='ovo'?'🟣':'🔵'}</div>
      <div class="info"><div class="t">${escapeHtml(o.productName)}</div><div class="m">${orderTab==="buy"?"dari":"ke"} ${other?escapeHtml(other.name):"—"} · ${rupiah(o.total)} · ${timeAgo(o.createdAt)}</div></div>
      <div class="list-actions">
        <span class="status-pill status-${o.status}">${o.status}</span>
        ${canReview ? `<button class="btn small" data-review="${o.id}">Nilai</button>` : ""}
      </div>
    </div>`;
  }).join("");
  wrap.querySelectorAll("[data-review]").forEach(b=>b.addEventListener("click", ()=>openReviewModal(b.getAttribute("data-review"))));
}
function openReviewModal(orderId){
  const o = orders.find(x=>x.id===orderId); if(!o) return;
  const body = document.getElementById("productModalBody");
  let rating = 5;
  body.innerHTML = `
    <h3>Nilai "${escapeHtml(o.productName)}"</h3><p>Bagikan pengalamanmu untuk pembeli lain.</p>
    <div class="star-picker" id="starPicker">${[1,2,3,4,5].map(n=>`<span data-n="${n}" class="${n<=5?'on':''}">★</span>`).join("")}</div>
    <textarea id="reviewText" placeholder="Ceritakan pengalamanmu..."></textarea>
    <button class="btn warm block" id="submitReviewBtn" style="margin-top:12px;">Kirim ulasan</button>`;
  const stars = body.querySelectorAll("#starPicker span");
  stars.forEach(s=>s.addEventListener("click", ()=>{ rating=Number(s.getAttribute("data-n")); stars.forEach(x=>x.classList.toggle("on", Number(x.getAttribute("data-n"))<=rating)); }));
  document.getElementById("productOverlay").classList.remove("hidden");
  document.getElementById("submitReviewBtn").addEventListener("click", ()=>{
    const comment = document.getElementById("reviewText").value.trim() || "(tanpa komentar)";
    reviews.push({id:uid("r"),productId:o.productId,buyerId:o.buyerId,orderId:o.id,rating,comment,createdAt:Date.now()});
    o.status = "selesai";
    saveAll();
    notifyUser(o.sellerId, "Ulasan baru diterima ⭐", `${currentUser().name} memberi rating ${rating}★ untuk "${o.productName}".`, "info");
    document.getElementById("productOverlay").classList.add("hidden");
    toast("Ulasan terkirim, terima kasih!");
    renderAll();
  });
}

/* ============ notifikasi ============ */
function renderNotif(){
  const u = currentUser(); const wrap = document.getElementById("notifList");
  if(!u){ wrap.innerHTML = '<div class="empty">Masuk akun untuk melihat notifikasi.</div>'; updateBell(); return; }
  const mine = notifications.filter(n=>n.userId===u.id).sort((a,b)=>b.createdAt-a.createdAt);
  if(mine.length===0){ wrap.innerHTML = '<div class="empty">Belum ada notifikasi.</div>'; updateBell(); return; }
  const iconMap = {payment:"💰",order:"🛒",welcome:"👋",verify:"✅",info:"🔔"};
  wrap.innerHTML = mine.map(n=>`
    <div class="notif-item" data-notif="${n.id}">
      <div class="ico">${iconMap[n.kind]||"🔔"}</div>
      <div class="body"><div class="t">${n.read?"":'<span class="unread-dot"></span>'}${escapeHtml(n.title)}</div><div class="m">${escapeHtml(n.body)}</div><div class="time">${timeAgo(n.createdAt)}</div></div>
    </div>`).join("");
  wrap.querySelectorAll("[data-notif]").forEach(el=>el.addEventListener("click", ()=>{
    const n = notifications.find(x=>x.id===el.getAttribute("data-notif")); if(!n) return;
    n.read=true; LS.set("nlm2_notifications",notifications); openNotifDetail(n); renderAll();
  }));
  updateBell();
}
function openNotifDetail(n){
  const body = document.getElementById("notifModalBody");
  body.innerHTML = `<h3>${escapeHtml(n.title)}</h3><p>${escapeHtml(n.body)}</p><div class="hint">${timeAgo(n.createdAt)}</div><button class="btn warm block" id="closeNotifBtn" style="margin-top:14px;">Tutup</button>`;
  document.getElementById("notifOverlay").classList.remove("hidden");
  document.getElementById("closeNotifBtn").addEventListener("click", ()=>document.getElementById("notifOverlay").classList.add("hidden"));
}
document.getElementById("notifOverlay").addEventListener("click", e=>{ if(e.target.id==="notifOverlay") e.currentTarget.classList.add("hidden"); });
document.getElementById("markAllReadBtn").addEventListener("click", ()=>{ const u=currentUser(); if(!u) return; notifications.forEach(n=>{ if(n.userId===u.id) n.read=true; }); LS.set("nlm2_notifications",notifications); renderAll(); toast("Semua notifikasi ditandai dibaca"); });
function updateBell(){ const u=currentUser(); const badge=document.getElementById("notifBadge"); if(!u){ badge.style.display="none"; return; } const unread=notifications.filter(n=>n.userId===u.id && !n.read).length; if(unread>0){ badge.style.display="flex"; badge.textContent=unread>9?"9+":unread; } else badge.style.display="none"; }

/* ============ profil ============ */
function renderProfile(){
  const u = currentUser();
  document.getElementById("profileLoggedOut").style.display = u?"none":"block";
  document.getElementById("profileLoggedIn").style.display = u?"block":"none";
  document.getElementById("avatarBtn").textContent = u?u.name.charAt(0).toUpperCase():"?";
  if(!u) return;
  document.getElementById("profileAvatar").textContent = u.name.charAt(0).toUpperCase();
  document.getElementById("profileName").textContent = u.name;
  document.getElementById("profileEmail").textContent = u.email;
  const lvl = buyerLevel(u.id);
  document.getElementById("profileLevelBadge").innerHTML = `<span class="level-badge">${lvl.icon} Level ${lvl.label}</span>`;
  document.getElementById("profileVerifyBadge").innerHTML = u.verified?'<span class="badge-verified">✓ toko terverifikasi</span>':(u.verifyStatus==="pending"?'<span class="badge-pending">menunggu persetujuan</span>':"");

  document.getElementById("myRefCode").textContent = u.refCode;
  const totalAff = walletTx.filter(t=>t.userId===u.id && t.type==="affiliate_commission").reduce((s,t)=>s+t.amount,0);
  document.getElementById("affiliateEarnings").textContent = "Total komisi affiliate terkumpul: " + rupiah(totalAff);
  document.getElementById("waNumberInput").value = u.whatsapp || "";
  document.getElementById("qrisUrlInput").value = u.qrisUrl || "";

  const area = document.getElementById("verifyArea");
  if(u.verified){ area.innerHTML = '<div class="empty" style="border-color:var(--success);color:var(--success);">Toko kamu sudah terverifikasi ✓</div>'; }
  else if(u.verifyStatus==="pending"){ area.innerHTML = '<div class="empty">Pengajuan sedang diperiksa admin.</div>'; }
  else { area.innerHTML = '<button class="btn warm block" id="requestVerifyBtn">Ajukan verifikasi toko</button>'; document.getElementById("requestVerifyBtn").addEventListener("click", ()=>{ u.verifyStatus="pending"; saveAll(); notifyUser(u.id,"Pengajuan verifikasi diterima","Pengajuan verifikasi tokomu sedang diperiksa admin.","verify"); toast("Pengajuan verifikasi terkirim"); renderAll(); }); }

  document.getElementById("ejPublicKey").value = emailCfg.publicKey || "";
  document.getElementById("ejServiceId").value = emailCfg.serviceId || "";
  document.getElementById("ejTemplateId").value = emailCfg.templateId || "";
  document.getElementById("ejEnabled").checked = !!emailCfg.enabled;
  document.getElementById("emailCfgStatus").innerHTML = emailCfg.enabled
    ? '<span style="color:var(--success);">✓ Email asli aktif — notifikasi juga dikirim ke inbox kamu.</span>'
    : 'Belum aktif — notifikasi hanya muncul di dalam app.';
}

/* ============ referral & whatsapp profile actions ============ */
document.getElementById("copyRefBtn").addEventListener("click", ()=>{
  const u = currentUser(); if(!u) return;
  const ta = document.createElement("textarea"); ta.value = u.refCode; document.body.appendChild(ta); ta.select();
  try{ document.execCommand("copy"); toast("Kode affiliate disalin"); }catch(e){ toast("Gagal menyalin"); }
  document.body.removeChild(ta);
});
document.getElementById("saveWaBtn").addEventListener("click", ()=>{
  const u = currentUser(); if(!u) return;
  const v = document.getElementById("waNumberInput").value.trim();
  u.whatsapp = v; saveAll(); toast("Nomor WhatsApp disimpan"); renderAll();
});
document.getElementById("saveQrisBtn").addEventListener("click", ()=>{
  const u = currentUser(); if(!u) return;
  const v = document.getElementById("qrisUrlInput").value.trim();
  u.qrisUrl = v; saveAll(); toast(v ? "QRIS asli disimpan — pembeli sekarang lihat QR asli kamu" : "QRIS dihapus, kembali ke QR simulasi"); renderAll();
});

/* ============ description generator ============ */
const TRAITS = ["Cepat & responsif","Awet / tahan lama","100% original","Bergaransi resmi","Kualitas premium","Harga bersahabat","Stok selalu ready","Pengiriman cepat"];
document.getElementById("genTraits").innerHTML = TRAITS.map((t,i)=>`<label style="display:flex;align-items:center;gap:5px;font-size:11.5px;background:var(--bg-panel);border:1px solid var(--border);border-radius:7px;padding:5px 9px;cursor:pointer;"><input type="checkbox" value="${escapeHtml(t)}" style="width:auto;">${escapeHtml(t)}</label>`).join("");
document.getElementById("genDescBtn").addEventListener("click", ()=>{
  const box = document.getElementById("descGenBox");
  box.style.display = box.style.display==="none" ? "block" : "none";
});
document.getElementById("applyGenDescBtn").addEventListener("click", ()=>{
  const name = document.getElementById("pName").value.trim() || "produk ini";
  const tone = document.getElementById("genTone").value;
  const traits = Array.from(document.querySelectorAll("#genTraits input:checked")).map(c=>c.value);
  let text = "";
  if(traits.length===0){ toast("Pilih minimal 1 kelebihan produk"); return; }
  const traitList = traits.join(", ").toLowerCase();
  if(tone==="santai") text = `${name} nih, cocok banget buat kamu yang cari barang ${traitList}. Udah banyak yang order, kualitas oke, gak bakal nyesel deh!`;
  else if(tone==="profesional") text = `${name} merupakan pilihan tepat dengan keunggulan: ${traitList}. Diproses dengan standar kualitas terbaik untuk kepuasan pelanggan.`;
  else text = `Jangan sampai kehabisan! ${name} hadir dengan ${traitList}. Buruan checkout sekarang sebelum stok habis — kualitas dijamin, kepuasan diutamakan!`;
  document.getElementById("pDesc").value = text;
  document.getElementById("descGenBox").style.display = "none";
  toast("Deskripsi dibuat, silakan edit sesuai kebutuhan");
});

/* ============ wallet UI ============ */
document.getElementById("walletOpenAuthBtn").addEventListener("click", ()=>authOverlay.classList.remove("hidden"));
document.getElementById("topupBtn").addEventListener("click", ()=>{
  const u = currentUser(); if(!u) return;
  const amt = prompt("Isi saldo simulasi berapa (Rp)? cth: 100000");
  const n = Number(amt);
  if(!n || n<=0){ return; }
  creditBalance(u.id, n);
  addWalletTx(u.id, "topup", n, "-", "Isi saldo simulasi");
  saveAll();
  notifyUser(u.id, "Isi saldo berhasil 💳", `Saldomu bertambah ${rupiah(n)} (simulasi).`, "payment");
  toast("Saldo bertambah "+rupiah(n));
  renderAll();
});
document.getElementById("openTransferBtn").addEventListener("click", ()=>{
  const p = document.getElementById("transferPanel");
  p.style.display = p.style.display==="none" ? "block" : "none";
});
document.getElementById("submitTransferBtn").addEventListener("click", ()=>{
  const u = currentUser(); if(!u) return;
  const email = document.getElementById("transferEmail").value.trim().toLowerCase();
  const amount = Number(document.getElementById("transferAmount").value);
  const note = document.getElementById("transferNote").value.trim();
  if(!email || !amount || amount<=0){ toast("Lengkapi email penerima & jumlah"); return; }
  const recipient = users.find(x=>x.email.toLowerCase()===email);
  if(!recipient){ toast("Email penerima tidak ditemukan"); return; }
  if(recipient.id===u.id){ toast("Tidak bisa transfer ke diri sendiri"); return; }
  if((u.balance||0) < amount){ toast("Saldo tidak cukup"); return; }
  debitBalance(u.id, amount); creditBalance(recipient.id, amount);
  addWalletTx(u.id, "transfer_out", amount, recipient.name, note);
  addWalletTx(recipient.id, "transfer_in", amount, u.name, note);
  saveAll();
  notifyUser(recipient.id, "Transfer masuk 💰", `${u.name} mengirim ${rupiah(amount)} ke saldomu.${note?" Catatan: "+note:""}`, "payment");
  toast("Transfer "+rupiah(amount)+" berhasil dikirim");
  document.getElementById("transferEmail").value=""; document.getElementById("transferAmount").value=""; document.getElementById("transferNote").value="";
  document.getElementById("transferPanel").style.display="none";
  renderAll();
});
function renderWallet(){
  const u = currentUser();
  document.getElementById("walletLoggedOut").style.display = u?"none":"block";
  document.getElementById("walletLoggedIn").style.display = u?"block":"none";
  if(!u) return;
  document.getElementById("walletBalance").textContent = rupiah(u.balance||0);
  const mine = walletTx.filter(t=>t.userId===u.id).slice(0,30);
  const list = document.getElementById("walletTxList");
  if(mine.length===0){ list.innerHTML = '<div class="empty">Belum ada transaksi.</div>'; return; }
  const iconMap = {topup:"➕",transfer_in:"⬇️",transfer_out:"⬆️",purchase:"🛒",sale:"💵",affiliate_commission:"🔗"};
  const inTypes = ["topup","transfer_in","sale","affiliate_commission"];
  list.innerHTML = mine.map(t=>{
    const isIn = inTypes.includes(t.type);
    const labelMap = {topup:"Isi saldo",transfer_in:"Transfer masuk",transfer_out:"Transfer keluar",purchase:"Pembelian",sale:"Penjualan",affiliate_commission:"Komisi affiliate"};
    return `<div class="tx-item"><div class="ico">${iconMap[t.type]||"💳"}</div>
      <div class="info"><div class="t">${labelMap[t.type]||t.type}${t.counterpart&&t.counterpart!=="-"?" · "+escapeHtml(t.counterpart):""}</div><div class="m">${t.note?escapeHtml(t.note)+" · ":""}${timeAgo(t.createdAt)}</div></div>
      <div class="tx-amt ${isIn?'plus':'minus'}">${isIn?'+':'-'}${rupiah(t.amount)}</div>
    </div>`;
  }).join("");
}

/* ============ email settings UI ============ */
document.getElementById("saveEmailCfgBtn").addEventListener("click", ()=>{
  emailCfg = {
    enabled: document.getElementById("ejEnabled").checked,
    publicKey: document.getElementById("ejPublicKey").value.trim(),
    serviceId: document.getElementById("ejServiceId").value.trim(),
    templateId: document.getElementById("ejTemplateId").value.trim()
  };
  if(emailCfg.enabled && (!emailCfg.publicKey || !emailCfg.serviceId || !emailCfg.templateId)){
    toast("Lengkapi Public Key, Service ID & Template ID dulu"); return;
  }
  LS.set("nlm2_emailjs_cfg", emailCfg);
  emailjsReady = false; // re-init dengan key baru
  toast("Pengaturan email disimpan");
  renderProfile();
});
document.getElementById("testEmailBtn").addEventListener("click", ()=>{
  const u = currentUser();
  if(!u){ toast("Masuk akun dulu"); return; }
  if(!emailCfg.publicKey || !emailCfg.serviceId || !emailCfg.templateId){ toast("Isi & simpan pengaturan EmailJS dulu"); return; }
  if(typeof emailjs === "undefined"){ toast("SDK EmailJS belum termuat, cek koneksi internet"); return; }
  ensureEmailjsInit();
  emailjs.send(emailCfg.serviceId, emailCfg.templateId, { to_email: u.email, to_name: u.name, subject: "Tes notifikasi NodeLink", message: "Ini email tes dari NodeLink Marketplace. Kalau kamu menerima ini, integrasi email sudah berhasil! 🎉" })
    .then(()=>toast("Email tes terkirim, cek inbox kamu"))
    .catch(err=>{ console.warn(err); toast("Gagal kirim — cek Public Key/Service ID/Template ID"); });
});

/* ============ admin ============ */
let adminAuthed = false;
document.getElementById("adminLoginBtn").addEventListener("click", ()=>{ const v=document.getElementById("adminPassInput").value; if(v===adminPass){ adminAuthed=true; document.getElementById("adminGate").style.display="none"; document.getElementById("adminPanelWrap").style.display="block"; renderAdmin(); } else toast("Password salah"); });
document.getElementById("adminLogoutBtn").addEventListener("click", ()=>{ adminAuthed=false; document.getElementById("adminGate").style.display="block"; document.getElementById("adminPanelWrap").style.display="none"; document.getElementById("adminPassInput").value=""; });
document.getElementById("changeAdminPassBtn").addEventListener("click", ()=>{ const v=document.getElementById("newAdminPass").value; if(v.length<4){ toast("Password minimal 4 karakter"); return; } adminPass=v; LS.set("nlm2_admin_pass",adminPass); document.getElementById("newAdminPass").value=""; toast("Password admin diperbarui"); });
document.getElementById("resetAllBtn").addEventListener("click", ()=>{ if(!confirm("Yakin hapus SEMUA data platform?")) return; ["nlm2_users","nlm2_products","nlm2_orders","nlm2_notifications","nlm2_reviews","nlm2_wishlist","nlm2_admin_pass","nlm2_session"].forEach(k=>localStorage.removeItem(k)); location.reload(); });
function renderAdmin(){
  if(!adminAuthed) return;
  document.getElementById("admStatUsers").textContent = users.length;
  document.getElementById("admStatProd").textContent = products.length;
  document.getElementById("admStatOrders").textContent = orders.length;
  document.getElementById("admStatRevenue").textContent = rupiah(orders.filter(o=>o.status==="dibayar"||o.status==="selesai").reduce((s,o)=>s+o.total,0));
  const pending = users.filter(u=>u.verifyStatus==="pending");
  const vList = document.getElementById("verifyRequestList");
  vList.innerHTML = pending.length===0?'<div class="empty">Tidak ada pengajuan verifikasi.</div>':pending.map(u=>`
    <div class="list-item"><div class="info"><div class="t">${escapeHtml(u.name)}</div><div class="m">${escapeHtml(u.email)}</div></div>
    <div class="list-actions"><button class="btn small" data-approve="${u.id}">Setujui</button><button class="btn ghost small" data-reject="${u.id}">Tolak</button></div></div>`).join("");
  vList.querySelectorAll("[data-approve]").forEach(b=>b.addEventListener("click", ()=>{ const u=users.find(x=>x.id===b.getAttribute("data-approve")); u.verified=true; u.verifyStatus="approved"; saveAll(); notifyUser(u.id,"Toko terverifikasi ✅","Selamat! Pengajuan verifikasi tokomu disetujui admin.","verify"); renderAll(); toast("Toko disetujui"); }));
  vList.querySelectorAll("[data-reject]").forEach(b=>b.addEventListener("click", ()=>{ const u=users.find(x=>x.id===b.getAttribute("data-reject")); u.verifyStatus="rejected"; saveAll(); notifyUser(u.id,"Verifikasi belum disetujui","Pengajuan verifikasi tokomu belum bisa disetujui.","verify"); renderAll(); toast("Pengajuan ditolak"); }));
  const oList = document.getElementById("adminOrderList");
  const sortedOrders = [...orders].sort((a,b)=>b.createdAt-a.createdAt).slice(0,20);
  oList.innerHTML = sortedOrders.length===0?'<div class="empty">Belum ada pesanan.</div>':sortedOrders.map(o=>{ const buyer=users.find(u=>u.id===o.buyerId), seller=users.find(u=>u.id===o.sellerId); return `<div class="list-item"><div class="info"><div class="t">${escapeHtml(o.productName)}</div><div class="m">${buyer?escapeHtml(buyer.name):"?"} → ${seller?escapeHtml(seller.name):"?"} · ${rupiah(o.total)}</div></div><span class="status-pill status-${o.status}">${o.status}</span></div>`; }).join("");
}

/* ============ master render ============ */
function renderAll(){ renderTicker(); renderMarket(); renderWishlist(); renderStore(); renderOrders(); renderNotif(); renderProfile(); if(adminAuthed) renderAdmin(); }
renderAll();
})();
</script>
</body>
</html>
