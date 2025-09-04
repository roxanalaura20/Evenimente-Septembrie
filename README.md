# Evenimente-Septembrie
<!doctype html>
<html lang="ro">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Evenimente Sibiu — Septembrie 2025</title>
  <style>
    :root{
      --card-bg: rgba(14,15,20,0.72);
      --muted: #cbd5e1;
      --accent: #ff6b6b;
      --radius:14px;
    }
    *{box-sizing:border-box}
    body{ margin:0; font-family: Inter, Roboto, system-ui, -apple-system, "Segoe UI", Arial; color:#f5f7fb; background:#07080b; }

    /* Background slider */
    .bg{ position:fixed; inset:0; z-index:-2; overflow:hidden; }
    .bg img{ position:absolute; inset:0; width:100%; height:100%; object-fit:cover; opacity:0; transform:scale(1.04); animation:fadeBg 90s infinite; }
    .bg img:nth-child(1){ animation-delay:0s; } 
    .bg img:nth-child(2){ animation-delay:30s; } 
    .bg img:nth-child(3){ animation-delay:60s; }
    @keyframes fadeBg {
      0% {opacity:0; transform:scale(1.06)}
      3% {opacity:1; transform:scale(1.02)}
      30% {opacity:1; transform:scale(1.0)}
      33% {opacity:0; transform:scale(1.0)}
      100% {opacity:0; transform:scale(1.06)}
    }
    .overlay{ position:fixed; inset:0; z-index:-1; background: linear-gradient(180deg, rgba(0,0,0,0.35), rgba(0,0,0,0.65)); backdrop-filter: blur(2px); }

    header{ display:flex; flex-direction:column; align-items:center; text-align:center; padding:28px 24px; gap:12px; }
    .brand{ font-weight:700; font-size:28px; }
    .small{ font-size:14px; color:var(--muted); }

    main{ max-width:1200px; margin:28px auto; padding:0 18px 60px; position:relative; z-index:1; }
    .grid{ display:grid; gap:18px; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); }

    .card{
      background:var(--card-bg); border-radius:var(--radius); overflow:hidden; display:flex; flex-direction:column;
      border:1px solid rgba(255,255,255,0.04); box-shadow:0 10px 30px rgba(0,0,0,0.45); transition:transform .22s ease, box-shadow .22s ease;
      transform:translateY(0); will-change:transform;
    }
    .card:hover{ transform:translateY(-6px); box-shadow:0 22px 50px rgba(0,0,0,0.55); }
    .media{ width:100%; height:200px; overflow:hidden; background:#0b0c10; position:relative; }
    .media img{ width:100%; height:100%; object-fit:cover; transition:transform .6s ease; transform:scale(1.02); display:block; }
    .card:hover .media img{ transform:scale(1.08); }

    .badge{ position:absolute; left:12px; top:12px; background:rgba(0,0,0,0.45); padding:8px 10px; border-radius:999px; color:#fff; font-weight:700; font-size:13px; border:1px solid rgba(255,255,255,0.06); }
    .body{ padding:14px 16px 16px; display:flex; flex-direction:column; gap:8px; min-height:160px; }
    .title{ font-size:18px; font-weight:700; line-height:1.25; }
    .meta{ color:var(--muted); font-size:14px; display:flex; flex-direction:column; gap:6px; margin-top:4px; }
    .meta .row{ display:flex; gap:10px; align-items:center; }
    .meta strong{ color:#fff; font-weight:700; margin-right:6px; }
    .actions{ margin-top:auto; display:flex; justify-content:space-between; align-items:center; gap:8px; }
    .btn{ padding:8px 12px; border-radius:10px; background:linear-gradient(180deg, rgba(255,255,255,0.04), rgba(255,255,255,0.01)); border:1px solid rgba(255,255,255,0.06); color:#fff; text-decoration:none; font-weight:700; font-size:14px; }
    .small{ font-size:12px; color:var(--muted); }

    footer{ text-align:center; color:var(--muted); margin:36px 0 80px; }

    @media(min-width:1100px){ .media{ height:220px } }
  </style>
</head>
<body>

  <!-- Background images -->
  <div class="bg" aria-hidden="true">
    <img src="./images/bg/bg1.jpg" alt="">
    <img src="./images/bg/bg2.jpg" alt="">
    <img src="./images/bg/bg3.jpg" alt="">
  </div>
  <div class="overlay" aria-hidden="true"></div>

  <header>
    <div class="brand">Evenimente Sibiu — Septembrie 2025</div>
    <div class="small">Demo — Toate evenimentele ordonate cronologic</div>
  </header>

  <main>
    <div id="grid" class="grid" role="list" aria-live="polite"></div>
    <footer>Acesta este un demo. Pune imaginile în <code>/images/events/</code> și bg în <code>/images/bg/</code>.</footer>
  </main>

  <script>
    // --- array generat automat de Python ---
    const events = [
      {
        title: "AntiPORTRET DE FAMILIE",
        startDateTime: "2025-09-04T19:00:00",
        location: "Centrul Cultural „Ion Besoiu\", Sibiu",
        link: "https://sibiucityapp.ro/ro/events/antiportret-de-familie",
        image: "./images/events/AntiPORTRET DE FAMILIE.jpg"
      },
      {
        title: "Festivalul meșteșugarilor romi",
        startDateTime: "2025-09-05T09:00:00",
        location: "Piața Mică, Sibiu",
        link: "https://sibiucityapp.ro/ro/events/festivalul-mestesugarilor-romi-6",
        image: "./images/events/festivalul-mestesugarilor-romi-6.jpg"
      },
      // ... completează cu restul evenimentelor extrase
    ];

    function fmtDate(dstr){
      if(!dstr) return "";
      const d = new Date(dstr);
      return d.toLocaleDateString('ro-RO', { day:'2-digit', month:'short', year:'numeric' });
    }

    function fmtTime(dstr){
      if(!dstr) return "";
      const d = new Date(dstr);
      return d.toLocaleTimeString('ro-RO', { hour:'2-digit', minute:'2-digit' });
    }

    const grid = document.getElementById('grid');

    function renderEvent(ev){
      const card = document.createElement('article');
      card.className = 'card';
      const dateLabel = ev.startDateTime ? fmtDate(ev.startDateTime) : 'Data necunoscută';
      const timeLabel = ev.startDateTime ? fmtTime(ev.startDateTime) : '';
      card.innerHTML = `
        <div class="media">
          <img src="${ev.image}" alt="${ev.title}">
          <div class="badge">${dateLabel}</div>
        </div>
        <div class="body">
          <div class="title">${ev.title}</div>
          <div class="meta">
            <div class="row"><strong>${dateLabel}</strong><span class="small">${timeLabel}</span></div>
            <div class="row">${ev.location}</div>
          </div>
          <div class="actions">
            <a class="btn" href="${ev.link}" target="_blank" rel="noopener noreferrer">Detalii & Bilete</a>
          </div>
        </div>
      `;
      return card;
    }

    events.forEach(ev => grid.appendChild(renderEvent(ev)));
  </script>
</body>
</html>
