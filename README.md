<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Brokerage Automation — HJS</title>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:opsz,wght@9..40,300;9..40,400;9..40,500;9..40,600;9..40,700;9..40,800&display=swap" rel="stylesheet">
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --brand:       #4FC3F7;
      --brand-mid:   #29B6F6;
      --brand-dark:  #0288D1;
      --brand-pale:  #E1F5FE;
      --text-1:      #0D1B2A;
      --text-2:      #475569;
      --text-3:      #94A3B8;
      --bg:          #FFFFFF;
      --bg-2:        #F8FAFC;
      --bg-3:        #F1F5F9;
      --border:      #E2E8F0;
      --border-2:    #CBD5E1;
      --red-pale:    #FEF2F2;
      --red:         #EF4444;
      --green:       #10B981;
      --green-pale:  #ECFDF5;
      --amber-pale:  #FFFBEB;
      --amber:       #D97706;
      --r:           10px;
      --r-lg:        14px;
      --sh:          0 1px 3px rgba(0,0,0,.07), 0 1px 2px rgba(0,0,0,.05);
      --sh-md:       0 4px 14px rgba(0,0,0,.08), 0 1px 4px rgba(0,0,0,.05);
      --sh-lg:       0 12px 32px rgba(0,0,0,.1), 0 2px 8px rgba(0,0,0,.06);
    }

    html { scroll-behavior: smooth; }

    body {
      font-family: 'DM Sans', -apple-system, BlinkMacSystemFont, sans-serif;
      background: var(--bg);
      color: var(--text-1);
      line-height: 1.6;
      -webkit-font-smoothing: antialiased;
    }

    /* ─── NAV ─── */
    nav {
      position: sticky; top: 0; z-index: 100;
      background: rgba(255,255,255,.92);
      backdrop-filter: blur(14px);
      border-bottom: 1px solid var(--border);
      padding: 0 2rem;
      height: 58px;
      display: flex; align-items: center; justify-content: space-between;
    }
    .nav-brand {
      font-size: .9rem; font-weight: 700; color: var(--text-1);
      letter-spacing: -.02em; display: flex; align-items: center; gap: 8px;
    }
    .nav-dot {
      width: 8px; height: 8px; border-radius: 50%;
      background: var(--brand);
      box-shadow: 0 0 0 3px var(--brand-pale);
    }
    .nav-links { display: flex; gap: 1.5rem; list-style: none; }
    .nav-links a {
      font-size: .82rem; font-weight: 500;
      color: var(--text-2); text-decoration: none;
      transition: color .15s;
    }
    .nav-links a:hover { color: var(--text-1); }

    /* ─── HERO ─── */
    .hero {
      padding: 90px 2rem 64px;
      max-width: 1100px; margin: 0 auto; text-align: center;
    }
    .eyebrow {
      display: inline-flex; align-items: center; gap: 6px;
      background: var(--brand-pale); color: var(--brand-dark);
      font-size: .72rem; font-weight: 700;
      letter-spacing: .07em; text-transform: uppercase;
      padding: 5px 13px; border-radius: 100px; margin-bottom: 1.75rem;
    }
    .hero h1 {
      font-size: clamp(2.4rem, 5.5vw, 3.8rem);
      font-weight: 800; letter-spacing: -.05em; line-height: 1.05;
      color: var(--text-1); margin-bottom: 1.25rem;
    }
    .hero h1 em { font-style: normal; color: var(--brand-dark); }
    .hero-sub {
      font-size: 1.05rem; color: var(--text-2);
      max-width: 520px; margin: 0 auto 2.75rem;
      line-height: 1.7; font-weight: 400;
    }

    /* stat contrast — signature element */
    .stat-contrast {
      display: inline-flex; align-items: center;
      border: 1px solid var(--border);
      border-radius: 100px; overflow: visible;
      margin-bottom: 2.5rem;
      box-shadow: var(--sh-md);
      position: relative;
    }
    .sc-block {
      padding: 14px 26px;
      display: flex; flex-direction: column; align-items: center;
    }
    .sc-block.bad  { background: var(--red-pale); border-radius: 100px 0 0 100px; }
    .sc-block.good { background: var(--brand-pale); border-radius: 0 100px 100px 0; position: relative; }
    .sc-label {
      font-size: .66rem; font-weight: 700;
      letter-spacing: .08em; text-transform: uppercase;
      color: var(--text-3); margin-bottom: 3px;
    }
    .sc-value {
      font-size: 1.65rem; font-weight: 800;
      letter-spacing: -.05em; line-height: 1;
    }
    .sc-block.bad  .sc-value { color: var(--red); }
    .sc-block.good .sc-value { color: var(--brand-dark); }
    .sc-arrow {
      padding: 0 14px; color: var(--text-3);
      font-size: 1.1rem; background: var(--bg-2);
      border-left: 1px solid var(--border);
      border-right: 1px solid var(--border);
      align-self: stretch;
      display: flex; align-items: center;
    }

    /* pulse ring on <60s block */
    .sc-block.good::after {
      content: '';
      position: absolute; inset: -6px;
      border-radius: 100px;
      border: 2px solid var(--brand);
      opacity: 0;
      animation: pulse-ring 2.4s ease-out infinite;
    }
    @keyframes pulse-ring {
      0%   { opacity: .6; transform: scale(1); }
      100% { opacity: 0;  transform: scale(1.07); }
    }

    /* stat pills row */
    .stat-row {
      display: flex; justify-content: center;
      gap: 6px; flex-wrap: wrap;
    }
    .stat-pill {
      background: var(--bg-2); border: 1px solid var(--border);
      border-radius: var(--r);
      padding: 12px 20px;
      display: flex; flex-direction: column; align-items: center;
      min-width: 160px;
    }
    .sp-num {
      font-size: 1.45rem; font-weight: 800;
      color: var(--text-1); letter-spacing: -.04em; line-height: 1;
    }
    .sp-label {
      font-size: .73rem; color: var(--text-2);
      text-align: center; margin-top: 3px; line-height: 1.4;
    }

    /* ─── SHARED LAYOUT ─── */
    .wrap { max-width: 1100px; margin: 0 auto; padding: 0 2rem; }
    .divider { height: 1px; background: var(--border); }

    /* ─── PROBLEM SECTION ─── */
    .problem-section { padding: 72px 0; }
    .section-eyebrow {
      font-size: .7rem; font-weight: 700;
      letter-spacing: .09em; text-transform: uppercase;
      color: var(--text-3); margin-bottom: .6rem;
    }
    .section-h {
      font-size: clamp(1.45rem, 3vw, 2rem);
      font-weight: 700; letter-spacing: -.035em;
      line-height: 1.2; color: var(--text-1); margin-bottom: .6rem;
    }
    .section-sub {
      font-size: .9rem; color: var(--text-2);
      max-width: 480px; margin-bottom: 2rem; line-height: 1.65;
    }
    .pain-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 1rem;
    }
    .pain-card {
      background: var(--bg-2); border: 1px solid var(--border);
      border-radius: var(--r); padding: 1.25rem;
    }
    .pain-icon {
      width: 34px; height: 34px; border-radius: 8px;
      background: var(--red-pale);
      display: flex; align-items: center; justify-content: center;
      font-size: .95rem; margin-bottom: .75rem;
    }
    .pain-card h4 {
      font-size: .88rem; font-weight: 600;
      color: var(--text-1); margin-bottom: .3rem;
      letter-spacing: -.015em;
    }
    .pain-card p { font-size: .8rem; color: var(--text-2); line-height: 1.6; }

    /* ─── TIER SECTIONS ─── */
    .tier-section { padding: 80px 0; border-top: 1px solid var(--border); }

    .tier-badge {
      display: inline-flex; align-items: center; gap: 8px;
      border: 1px solid var(--border-2); border-radius: 100px;
      padding: 4px 14px 4px 4px; margin-bottom: 1.5rem;
    }
    .t-dot {
      width: 26px; height: 26px; border-radius: 50%;
      background: var(--text-1);
      display: flex; align-items: center; justify-content: center;
      font-size: .65rem; font-weight: 700; color: white;
    }
    .t-dot.blue { background: var(--brand-dark); }
    .tier-badge-text {
      font-size: .77rem; font-weight: 600; color: var(--text-1);
    }

    .tier-h {
      font-size: clamp(1.65rem, 3.8vw, 2.4rem);
      font-weight: 800; letter-spacing: -.045em;
      line-height: 1.12; color: var(--text-1); margin-bottom: .65rem;
    }
    .tier-h span { color: var(--brand-dark); }
    .tier-desc {
      font-size: .9rem; color: var(--text-2);
      max-width: 540px; margin-bottom: 2.25rem; line-height: 1.7;
    }

    /* ─── SERVICES GRID ─── */
    .services-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(310px, 1fr));
      gap: 1rem; margin-bottom: 1.5rem;
    }
    .svc-card {
      background: var(--bg); border: 1px solid var(--border);
      border-radius: var(--r-lg); padding: 1.5rem;
      display: flex; flex-direction: column; gap: .8rem;
      transition: box-shadow .18s, border-color .18s;
    }
    .svc-card:hover {
      box-shadow: var(--sh-md);
      border-color: var(--brand);
    }
    .svc-top {
      display: flex; align-items: flex-start;
      justify-content: space-between; gap: .75rem;
    }
    .svc-name {
      font-size: .93rem; font-weight: 700;
      color: var(--text-1); letter-spacing: -.02em; line-height: 1.3;
    }
    .tag {
      flex-shrink: 0;
      font-size: .65rem; font-weight: 700;
      letter-spacing: .04em; text-transform: uppercase;
      padding: 3px 8px; border-radius: 6px;
    }
    .tag.default { background: var(--brand-pale); color: var(--brand-dark); }
    .tag.new     { background: var(--amber-pale); color: var(--amber); }
    .tag.found   { background: var(--green-pale); color: var(--green); }

    .svc-desc {
      font-size: .82rem; color: var(--text-2);
      line-height: 1.65; flex-grow: 1;
    }
    .svc-price {
      border-top: 1px solid var(--border); padding-top: .75rem;
      display: flex; align-items: baseline; gap: .4rem; flex-wrap: wrap;
    }
    .p-setup  { font-size: .9rem; font-weight: 700; color: var(--text-1); }
    .p-sep    { font-size: .75rem; color: var(--text-3); }
    .p-month  { font-size: .78rem; font-weight: 500; color: var(--text-2); }

    /* ─── PACKAGE BOX ─── */
    .pkg-box {
      background: var(--text-1); border-radius: var(--r-lg);
      padding: 2rem 2.25rem;
      display: grid; grid-template-columns: 1fr auto;
      gap: 2rem; align-items: center;
    }
    .pkg-eyebrow {
      font-size: .68rem; font-weight: 700;
      letter-spacing: .09em; text-transform: uppercase;
      color: var(--brand); margin-bottom: .45rem;
    }
    .pkg-title {
      font-size: 1.2rem; font-weight: 700;
      color: white; letter-spacing: -.03em; margin-bottom: .45rem;
    }
    .pkg-desc { font-size: .8rem; color: #7B92A9; line-height: 1.6; }
    .pkg-price { text-align: right; flex-shrink: 0; }
    .pkg-setup-val {
      font-size: 1.8rem; font-weight: 800;
      color: white; letter-spacing: -.05em; line-height: 1;
    }
    .pkg-setup-label { font-size: .7rem; color: #4B6070; margin-bottom: .5rem; }
    .pkg-mo-val { font-size: 1.05rem; font-weight: 700; color: var(--brand); letter-spacing: -.03em; }
    .pkg-mo-label { font-size: .68rem; color: #4B6070; }

    /* ─── CLOSE ─── */
    .close-section {
      padding: 80px 2rem;
      max-width: 680px; margin: 0 auto; text-align: center;
      border-top: 1px solid var(--border);
    }
    .close-section h2 {
      font-size: clamp(1.5rem, 3vw, 2rem);
      font-weight: 700; letter-spacing: -.04em;
      color: var(--text-1); margin-bottom: 1rem;
    }
    .close-section p {
      font-size: .92rem; color: var(--text-2);
      line-height: 1.75; margin-bottom: .75rem;
    }
    .close-line {
      font-size: .82rem; color: var(--text-3);
      font-weight: 500; margin-top: 1.25rem;
      display: block;
    }

    /* ─── FOOTER ─── */
    footer {
      border-top: 1px solid var(--border);
      padding: 1.5rem 2rem;
      max-width: 1100px; margin: 0 auto;
      display: flex; justify-content: space-between; align-items: center;
    }
    footer p { font-size: .75rem; color: var(--text-3); }

    /* ─── RESPONSIVE ─── */
    @media (max-width: 768px) {
      nav { padding: 0 1rem; }
      .hero { padding: 56px 1rem 44px; }
      .wrap { padding: 0 1rem; }
      .services-grid { grid-template-columns: 1fr; }
      .pkg-box { grid-template-columns: 1fr; }
      .pkg-price { text-align: left; }
      .stat-row { gap: .5rem; }
      .stat-pill { min-width: 140px; }
      .pain-grid { grid-template-columns: 1fr 1fr; }
    }
    @media (max-width: 520px) {
      .stat-contrast { flex-direction: column; border-radius: var(--r); }
      .sc-block.bad  { border-radius: var(--r) var(--r) 0 0; }
      .sc-block.good { border-radius: 0 0 var(--r) var(--r); }
      .sc-arrow { border: none; border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); padding: 8px 0; }
      .nav-links { display: none; }
      .pain-grid { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

<!-- NAV -->
<nav>
  <div class="nav-brand">
    <div class="nav-dot"></div>
    HJS Automation
  </div>
  <ul class="nav-links">
    <li><a href="#franchise">Franchise Brokers</a></li>
    <li><a href="#independent">Independent Brokers</a></li>
  </ul>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="eyebrow">
    For Principal Broker-Owners · US Real Estate
  </div>

  <h1>Your leads die<br>in the <em>response gap.</em></h1>

  <p class="hero-sub">
    The average brokerage takes 917 minutes to follow up on a new lead.
    Most deals are gone in 5. I build the systems that close that gap permanently.
  </p>

  <div class="stat-contrast">
    <div class="sc-block bad">
      <span class="sc-label">Industry average</span>
      <span class="sc-value">917 min</span>
    </div>
    <div class="sc-arrow">→</div>
    <div class="sc-block good">
      <span class="sc-label">With this system</span>
      <span class="sc-value">&lt;60 sec</span>
    </div>
  </div>

  <div class="stat-row">
    <div class="stat-pill">
      <span class="sp-num">48%</span>
      <span class="sp-label">of real estate leads go completely unanswered</span>
    </div>
    <div class="stat-pill">
      <span class="sp-num">78%</span>
      <span class="sp-label">of buyers work with the first agent who responds</span>
    </div>
    <div class="stat-pill">
      <span class="sp-num">$7,500+</span>
      <span class="sp-label">in lost commission per uncontacted lead</span>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- PROBLEM -->
<section class="problem-section">
  <div class="wrap">
    <div class="section-eyebrow">What's actually breaking</div>
    <h2 class="section-h">This isn't an agent problem.<br>It's an infrastructure problem.</h2>
    <p class="section-sub">Agents don't ignore leads on purpose. The systems just weren't built to make them respond.</p>

    <div class="pain-grid">
      <div class="pain-card">
        <div class="pain-icon">⏱</div>
        <h4>Leads sit for hours</h4>
        <p>Agents see the notification, mean to call back, and the lead goes to whoever was faster. No automation = no urgency.</p>
      </div>
      <div class="pain-card">
        <div class="pain-icon">📊</div>
        <h4>SLA violations go untracked</h4>
        <p>Your franchise requires a &lt;5-minute response. You have no visibility into who's actually hitting that — until a deal is already dead.</p>
      </div>
      <div class="pain-card">
        <div class="pain-icon">🔧</div>
        <h4>Automation breaks silently</h4>
        <p>Zapier stops. Leads don't route. Agents aren't notified. Nobody finds out until a broker checks manually a week later.</p>
      </div>
      <div class="pain-card">
        <div class="pain-icon">💸</div>
        <h4>No idea which lead spend works</h4>
        <p>You're paying for Zillow, Realtor.com, and FB leads with no report showing which source converts and which wastes budget.</p>
      </div>
    </div>
  </div>
</section>

<!-- FRANCHISE TIER -->
<section class="tier-section" id="franchise">
  <div class="wrap">
    <div class="tier-badge">
      <div class="t-dot blue">F</div>
      <span class="tier-badge-text">Franchise Broker-Owners — RE/MAX · KW · EXP · Coldwell Banker</span>
    </div>

    <h2 class="tier-h">Mandated CRM.<br><span>No mandate to actually use it.</span></h2>
    <p class="tier-desc">
      Your franchise picked the tech. Your agents are ignoring the SLA. I build the layer between them that enforces response times, surfaces violations, and keeps your pipeline moving — without replacing a single tool your franchise requires.
    </p>

    <div class="services-grid">

      <div class="svc-card">
        <div class="svc-top">
          <span class="svc-name">CRM Automation Audit + Fix</span>
          <span class="tag default">Diagnostic</span>
        </div>
        <p class="svc-desc">Find every broken workflow inside your franchise CRM. I map your current setup, identify failure points, and rebuild 5–7 working automations from scratch — confirmed live before handoff. Includes a 3-page audit report.</p>
        <div class="svc-price">
          <span class="p-setup">$3,000</span>
          <span class="p-month">one-time project</span>
        </div>
      </div>

      <div class="svc-card">
        <div class="svc-top">
          <span class="svc-name">SLA Dashboard + Violation Tracking</span>
          <span class="tag default">Accountability</span>
        </div>
        <p class="svc-desc">Real-time dashboard showing which agents are missing your franchise's response SLA — by lead, by hour, by agent. You see violations as they happen, not in a weekly report after the deal is gone.</p>
        <div class="svc-price">
          <span class="p-setup">$3,000 setup</span>
          <span class="p-sep">·</span>
          <span class="p-month">$1,500/month</span>
        </div>
      </div>

      <div class="svc-card">
        <div class="svc-top">
          <span class="svc-name">Speed-to-Lead (&lt;60 Seconds)</span>
          <span class="tag default">Response</span>
        </div>
        <p class="svc-desc">Instant SMS + email the moment a lead hits from Zillow, your website, or Facebook. Auto-creates a CRM record. Fires before any agent sees the notification. Works on top of your existing franchise CRM — nothing replaced.</p>
        <div class="svc-price">
          <span class="p-setup">$2,000 setup</span>
          <span class="p-sep">·</span>
          <span class="p-month">$997/month</span>
        </div>
      </div>

      <div class="svc-card">
        <div class="svc-top">
          <span class="svc-name">ISA-Level Follow-Up (14-touch)</span>
          <span class="tag default">Nurture</span>
        </div>
        <p class="svc-desc">A 2-week automated sequence with AI-powered lead qualification. Runs 24/7. Detects replies and pauses. Warms the lead and hands it off to your agent only when it's ready to talk — no human ISA required for initial qualification.</p>
        <div class="svc-price">
          <span class="p-setup">$2,500 setup</span>
          <span class="p-sep">·</span>
          <span class="p-month">$1,200/month</span>
        </div>
      </div>

      <div class="svc-card">
        <div class="svc-top">
          <span class="svc-name">Lead Routing + SLA Alerts</span>
          <span class="tag default">Routing</span>
        </div>
        <p class="svc-desc">Every lead auto-assigned to the right agent based on territory, price range, or availability — then SMS notification in under 5 minutes. If the agent doesn't respond, an escalation alert goes to you. No manual assignment.</p>
        <div class="svc-price">
          <span class="p-setup">$1,000 setup</span>
          <span class="p-sep">·</span>
          <span class="p-month">$800/month</span>
        </div>
      </div>

      <div class="svc-card">
        <div class="svc-top">
          <span class="svc-name">After-Hours Lead Response</span>
          <span class="tag new">New</span>
        </div>
        <p class="svc-desc">A dedicated system for leads that arrive evenings, weekends, and holidays when your agents are offline. Qualifies automatically, keeps the lead warm overnight, and delivers a briefed handoff to the assigned agent first thing in the morning.</p>
        <div class="svc-price">
          <sp
