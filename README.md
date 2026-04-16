<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GradeVault — CGPA Calculator</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&display=swap" rel="stylesheet">
<style>
:root {
  --c1: #FF6B6B; --c2: #FFD93D; --c3: #6BCB77; --c4: #4D96FF; --c5: #C77DFF;
  --c1d: #E63946; --c2d: #F4A825; --c3d: #2D9E44; --c4d: #1A6FD4; --c5d: #9B59B6;
  --bg: #0D0D16; --bg2: #13131F; --bg3: #1A1A2E;
  --surface: rgba(255,255,255,0.04); --surface2: rgba(255,255,255,0.08);
  --border: rgba(255,255,255,0.1); --border2: rgba(255,255,255,0.2);
  --text: #F0F0FF; --text2: rgba(240,240,255,0.6); --text3: rgba(240,240,255,0.35);
  --r: 16px; --r2: 12px; --r3: 8px;
}
*{box-sizing:border-box;margin:0;padding:0;}
html{scroll-behavior:smooth;}
body{
  font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);
  min-height:100vh;overflow-x:hidden;
  background-image:
    radial-gradient(ellipse 80% 50% at 20% 10%, rgba(255,107,107,0.07) 0%, transparent 60%),
    radial-gradient(ellipse 60% 40% at 80% 80%, rgba(77,150,255,0.08) 0%, transparent 60%),
    radial-gradient(ellipse 40% 30% at 50% 50%, rgba(199,125,255,0.05) 0%, transparent 60%);
}

/* NOISE OVERLAY */
body::before{
  content:'';position:fixed;inset:0;pointer-events:none;z-index:0;opacity:0.03;
  background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
}

.page{max-width:900px;margin:0 auto;padding:2.5rem 1.5rem 5rem;position:relative;z-index:1;}

/* HEADER */
.header{text-align:center;margin-bottom:3rem;animation:fadeUp 0.7s ease both;}
.logo{display:inline-flex;align-items:center;gap:10px;margin-bottom:1.5rem;}
.logo-mark{width:42px;height:42px;border-radius:12px;background:linear-gradient(135deg,var(--c1),var(--c5));display:flex;align-items:center;justify-content:center;font-family:'Syne',sans-serif;font-weight:800;font-size:18px;color:#fff;}
.logo-text{font-family:'Syne',sans-serif;font-weight:700;font-size:20px;letter-spacing:-0.5px;}
.logo-text span{color:var(--c4);}
h1{font-family:'Syne',sans-serif;font-size:clamp(2rem,5vw,3.2rem);font-weight:800;line-height:1.1;letter-spacing:-1.5px;margin-bottom:0.75rem;}
h1 .hi{color:var(--c2);}
h1 .hii{color:var(--c4);}
.subtitle{font-size:15px;color:var(--text2);max-width:420px;margin:0 auto;line-height:1.6;}

/* COUNTRY SELECTOR */
.country-bar{
  display:flex;align-items:center;gap:12px;margin-bottom:1.75rem;
  background:var(--surface);border:1px solid var(--border);border-radius:var(--r);
  padding:14px 18px;flex-wrap:wrap;
}
.country-bar label{font-size:11px;font-weight:500;color:var(--text3);text-transform:uppercase;letter-spacing:1px;white-space:nowrap;}
.flag-select{
  flex:1;min-width:200px;height:40px;
  background:var(--surface2);border:1px solid var(--border2);
  border-radius:var(--r3);padding:0 12px;
  font-family:'DM Sans',sans-serif;font-size:14px;color:var(--text);
  cursor:pointer;outline:none;transition:border 0.2s;
}
.flag-select:focus{border-color:var(--c4);}
.scale-tag{
  padding:5px 14px;border-radius:20px;font-size:11px;font-weight:500;white-space:nowrap;
  background:rgba(77,150,255,0.15);color:var(--c4);border:1px solid rgba(77,150,255,0.3);
}

/* TABS */
.tabs{
  display:flex;gap:6px;margin-bottom:1.5rem;
  background:var(--surface);border:1px solid var(--border);
  border-radius:var(--r);padding:6px;width:fit-content;
}
.tab{
  padding:9px 22px;border-radius:var(--r3);border:none;
  background:transparent;cursor:pointer;
  font-family:'DM Sans',sans-serif;font-size:13px;font-weight:500;
  color:var(--text2);transition:all 0.2s;
}
.tab.active{background:var(--surface2);color:var(--text);border:1px solid var(--border2);}
.tab:hover:not(.active){color:var(--text);}

/* SECTION */
.sec{display:none;animation:fadeUp 0.3s ease both;}
.sec.active{display:block;}

/* MODE TOGGLE */
.mode-row{display:flex;gap:8px;margin-bottom:1.25rem;}
.mode-btn{
  padding:7px 18px;border-radius:20px;border:1px solid var(--border);
  background:transparent;cursor:pointer;font-family:'DM Sans',sans-serif;
  font-size:12px;font-weight:500;color:var(--text2);transition:all 0.2s;
}
.mode-btn.active{background:rgba(199,125,255,0.15);color:var(--c5);border-color:rgba(199,125,255,0.4);}

/* PANEL */
.panel{
  background:var(--surface);border:1px solid var(--border);
  border-radius:var(--r);padding:1.5rem;margin-bottom:1rem;
}
.panel-title{
  font-family:'Syne',sans-serif;font-size:13px;font-weight:700;
  color:var(--text3);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:1.25rem;
  display:flex;align-items:center;gap:8px;
}
.panel-title::before{content:'';width:3px;height:14px;border-radius:2px;background:var(--c5);}

/* COURSE TABLE */
.tbl-head{
  display:grid;gap:8px;margin-bottom:8px;
  grid-template-columns: minmax(0,2fr) 68px minmax(100px,140px) 64px 36px;
}
.tbl-head span{
  font-size:10px;font-weight:500;color:var(--text3);
  text-transform:uppercase;letter-spacing:1px;padding:0 4px;
}
.course-row{
  display:grid;gap:8px;align-items:center;margin-bottom:8px;
  grid-template-columns: minmax(0,2fr) 68px minmax(100px,140px) 64px 36px;
  animation:rowIn 0.25s ease both;
}
@keyframes rowIn{from{opacity:0;transform:translateY(-6px);}to{opacity:1;transform:translateY(0);}}

.course-row input, .course-row select{
  height:40px;background:var(--surface2);
  border:1px solid var(--border);border-radius:var(--r3);
  padding:0 12px;font-family:'DM Sans',sans-serif;font-size:13px;color:var(--text);
  outline:none;transition:all 0.2s;
}
.course-row input::placeholder{color:var(--text3);}
.course-row input:focus,.course-row select:focus{border-color:var(--c5);background:rgba(199,125,255,0.08);}
.course-row select option{background:#1A1A2E;color:var(--text);}

.gp-chip{
  height:40px;border-radius:var(--r3);
  background:rgba(199,125,255,0.12);border:1px solid rgba(199,125,255,0.25);
  display:flex;align-items:center;justify-content:center;
  font-size:13px;font-weight:500;color:var(--c5);
}
.rm-btn{
  width:36px;height:36px;border-radius:8px;border:1px solid var(--border);
  background:transparent;cursor:pointer;color:var(--text3);font-size:18px;
  display:flex;align-items:center;justify-content:center;transition:all 0.2s;
}
.rm-btn:hover{background:rgba(255,107,107,0.15);border-color:rgba(255,107,107,0.4);color:var(--c1);}

/* ADD BTN */
.add-btn{
  display:inline-flex;align-items:center;gap:8px;
  padding:9px 18px;border-radius:var(--r3);
  border:1px dashed var(--border2);background:transparent;cursor:pointer;
  font-family:'DM Sans',sans-serif;font-size:13px;color:var(--text2);
  transition:all 0.2s;margin-top:6px;
}
.add-btn:hover{background:var(--surface2);border-color:var(--c3);color:var(--c3);}
.add-btn .plus{font-size:20px;line-height:1;color:var(--c3);}

/* CALC BTN */
.calc-btn{
  width:100%;height:50px;border-radius:var(--r2);border:none;cursor:pointer;
  font-family:'Syne',sans-serif;font-size:15px;font-weight:700;letter-spacing:0.5px;
  color:#fff;margin-top:8px;position:relative;overflow:hidden;transition:all 0.2s;
  background:linear-gradient(90deg,var(--c5),var(--c4));
}
.calc-btn::after{
  content:'';position:absolute;inset:0;
  background:linear-gradient(90deg,transparent,rgba(255,255,255,0.15),transparent);
  transform:translateX(-100%);transition:transform 0.5s ease;
}
.calc-btn:hover::after{transform:translateX(100%);}
.calc-btn:hover{transform:translateY(-1px);}
.calc-btn:active{transform:translateY(0);}

/* RESULT */
.result-box{
  border-radius:var(--r);padding:1.75rem;margin-top:1rem;
  border:1px solid var(--border);overflow:hidden;position:relative;
  animation:fadeUp 0.4s ease both;
}
.result-box::before{
  content:'';position:absolute;inset:0;pointer-events:none;
  background:radial-gradient(ellipse 60% 60% at 80% 20%, rgba(77,150,255,0.1) 0%, transparent 60%),
             radial-gradient(ellipse 40% 40% at 10% 80%, rgba(199,125,255,0.08) 0%, transparent 60%);
}
.result-main{display:flex;gap:1.5rem;flex-wrap:wrap;align-items:flex-start;margin-bottom:1.5rem;position:relative;}
.cgpa-hero{flex:0 0 auto;}
.cgpa-big{font-family:'Syne',sans-serif;font-size:5rem;font-weight:800;letter-spacing:-3px;line-height:1;}
.cgpa-max{font-size:1.2rem;color:var(--text3);font-weight:300;margin-top:4px;}
.cgpa-bar-wrap{height:4px;border-radius:2px;background:rgba(255,255,255,0.08);width:180px;margin-top:12px;overflow:hidden;}
.cgpa-bar-fill{height:100%;border-radius:2px;transition:width 0.8s cubic-bezier(0.4,0,0.2,1);}
.result-info{flex:1;min-width:160px;display:flex;flex-direction:column;gap:10px;padding-top:8px;}
.grade-pill{
  display:inline-flex;align-items:center;gap:8px;
  padding:8px 18px;border-radius:20px;font-size:14px;font-weight:500;width:fit-content;
}
.remark-txt{font-size:13px;color:var(--text2);}
.result-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;position:relative;}
.stat-card{
  background:var(--surface2);border-radius:var(--r2);padding:14px 16px;
  border:1px solid var(--border);
}
.stat-label{font-size:10px;font-weight:500;color:var(--text3);text-transform:uppercase;letter-spacing:1px;margin-bottom:6px;}
.stat-val{font-size:1.4rem;font-weight:600;font-family:'Syne',sans-serif;}

/* BREAKDOWN */
.bk-title{font-family:'Syne',sans-serif;font-size:12px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:1.5px;margin:1.25rem 0 10px;}
.bk-row{
  display:flex;align-items:center;justify-content:space-between;
  padding:11px 14px;border-radius:var(--r3);margin-bottom:6px;
  background:var(--surface);border:1px solid var(--border);transition:all 0.2s;
}
.bk-row:hover{background:var(--surface2);}
.bk-label{font-size:13px;color:var(--text2);}
.bk-right{display:flex;align-items:center;gap:10px;}
.bk-val{font-size:14px;font-weight:600;font-family:'Syne',sans-serif;}

/* BADGE */
.badge{padding:3px 12px;border-radius:20px;font-size:11px;font-weight:600;font-family:'Syne',sans-serif;}
.bA{background:rgba(107,203,119,0.15);color:var(--c3);border:1px solid rgba(107,203,119,0.3);}
.bB{background:rgba(77,150,255,0.15);color:var(--c4);border:1px solid rgba(77,150,255,0.3);}
.bC{background:rgba(255,217,61,0.15);color:var(--c2);border:1px solid rgba(255,217,61,0.3);}
.bD{background:rgba(255,107,107,0.15);color:var(--c1);border:1px solid rgba(255,107,107,0.3);}
.bF{background:rgba(150,50,50,0.2);color:#ff8080;border:1px solid rgba(150,50,50,0.4);}

/* CUSTOM MODE */
.custom-row{display:grid;grid-template-columns:1fr 110px 36px;gap:8px;align-items:center;margin-bottom:8px;}
.custom-row input{
  height:40px;background:var(--surface2);border:1px solid var(--border);
  border-radius:var(--r3);padding:0 12px;font-family:'DM Sans',sans-serif;
  font-size:13px;color:var(--text);outline:none;transition:all 0.2s;
}
.custom-row input::placeholder{color:var(--text3);}
.custom-row input:focus{border-color:var(--c5);background:rgba(199,125,255,0.08);}

/* YEAR BLOCK */
.year-block{margin-bottom:1.5rem;}
.year-label{
  display:inline-flex;align-items:center;gap:8px;
  font-family:'Syne',sans-serif;font-size:12px;font-weight:700;
  color:var(--text3);text-transform:uppercase;letter-spacing:1px;margin-bottom:10px;
}
.year-dot{width:8px;height:8px;border-radius:50%;}
.y1 .year-dot{background:var(--c1);}
.y2 .year-dot{background:var(--c2);}
.y3 .year-dot{background:var(--c3);}
.y4 .year-dot{background:var(--c4);}
.y5 .year-dot{background:var(--c5);}
.y1 .year-label{color:var(--c1);}
.y2 .year-label{color:var(--c2);}
.y3 .year-label{color:var(--c3);}
.y4 .year-label{color:var(--c4);}
.y5 .year-label{color:var(--c5);}
.divider{height:1px;background:var(--border);margin:1.5rem 0;}

/* SUMMARY TABLE */
.sum-wrap{overflow-x:auto;}
.sum-table{width:100%;border-collapse:separate;border-spacing:0 4px;font-size:13px;margin-top:8px;}
.sum-table th{
  text-align:left;padding:8px 12px;
  font-size:10px;font-weight:500;color:var(--text3);
  text-transform:uppercase;letter-spacing:1px;
}
.sum-table td{
  padding:12px 12px;
  background:var(--surface);border-top:1px solid var(--border);border-bottom:1px solid var(--border);
  color:var(--text);
}
.sum-table td:first-child{border-left:1px solid var(--border);border-radius:8px 0 0 8px;}
.sum-table td:last-child{border-right:1px solid var(--border);border-radius:0 8px 8px 0;}
.sum-table .total-row td{background:var(--surface2);}

/* MINI METRICS */
.mini-metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-top:1rem;}

/* REF GRID */
.ref-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(190px,1fr));gap:10px;margin-bottom:1.5rem;}
.ref-card{
  background:var(--surface);border:1px solid var(--border);
  border-radius:var(--r2);padding:14px 16px;transition:all 0.2s;cursor:default;
  border-left:3px solid transparent;
}
.ref-card:nth-child(1){border-left-color:var(--c1);}
.ref-card:nth-child(2){border-left-color:var(--c2);}
.ref-card:nth-child(3){border-left-color:var(--c3);}
.ref-card:nth-child(4){border-left-color:var(--c4);}
.ref-card:nth-child(5){border-left-color:var(--c5);}
.ref-card:nth-child(6){border-left-color:var(--c1);}
.ref-card:nth-child(7){border-left-color:var(--c2);}
.ref-card:nth-child(8){border-left-color:var(--c3);}
.ref-card:nth-child(9){border-left-color:var(--c4);}
.ref-card:nth-child(10){border-left-color:var(--c5);}
.ref-card:hover{background:var(--surface2);}
.ref-card-title{font-family:'Syne',sans-serif;font-size:13px;font-weight:700;color:var(--text);margin-bottom:5px;}
.ref-card-sub{font-size:11px;color:var(--text3);line-height:1.55;}
.ref-detail-table{width:100%;border-collapse:separate;border-spacing:0 3px;font-size:13px;}
.ref-detail-table th{text-align:left;padding:6px 10px;font-size:10px;font-weight:500;color:var(--text3);text-transform:uppercase;letter-spacing:1px;}
.ref-detail-table td{padding:9px 12px;background:var(--surface);color:var(--text);}
.ref-detail-table td:first-child{border-radius:6px 0 0 6px;}
.ref-detail-table td:last-child{border-radius:0 6px 6px 0;}

/* ANIMATIONS */
@keyframes fadeUp{from{opacity:0;transform:translateY(16px);}to{opacity:1;transform:translateY(0);}}

/* FOOTER */
.footer{text-align:center;margin-top:3rem;font-size:12px;color:var(--text3);}
.footer span{color:var(--c1);}

/* SCROLLBAR */
::-webkit-scrollbar{width:6px;height:6px;}
::-webkit-scrollbar-track{background:var(--bg);}
::-webkit-scrollbar-thumb{background:var(--border2);border-radius:3px;}

/* RESPONSIVE */
@media(max-width:600px){
  .result-stats{grid-template-columns:repeat(2,1fr);}
  .mini-metrics{grid-template-columns:repeat(2,1fr);}
  .tabs{width:100%;flex-wrap:wrap;}
  .tab{flex:1;text-align:center;}
  h1{font-size:2rem;}
}
</style>
</head>
<body>
<div class="page">

  <!-- HEADER -->
  <div class="header">
    <div class="logo">
      <div class="logo-mark">GV</div>
      <div class="logo-text">Grade<span>Vault</span></div>
    </div>
    <h1>Calculate your <span class="hi">CGPA</span><br>the <span class="hii">smart way</span></h1>
    <p class="subtitle">10 countries · 3 input modes · year-wise breakdown · instant results</p>
  </div>

  <!-- COUNTRY BAR -->
  <div class="country-bar">
    <label>Country / System</label>
    <select class="flag-select" id="country-sel" onchange="onCountryChange()">
      <option value="BD">🇧🇩 Bangladesh (4.0 — UGC)</option>
      <option value="US">🇺🇸 USA (4.0 GPA)</option>
      <option value="UK">🇬🇧 UK (Honours Classification)</option>
      <option value="IN">🇮🇳 India (10 point CGPA)</option>
      <option value="CA">🇨🇦 Canada (4.0)</option>
      <option value="AU">🇦🇺 Australia (7 point)</option>
      <option value="DE">🇩🇪 Germany (1–5 scale)</option>
      <option value="PK">🇵🇰 Pakistan (4.0 — HEC)</option>
      <option value="MY">🇲🇾 Malaysia (4.0 — MQA)</option>
      <option value="CN">🇨🇳 China (4.0 — 百分制)</option>
    </select>
    <span class="scale-tag" id="scale-tag">4.0 scale · UGC Bangladesh</span>
  </div>

  <!-- TABS -->
  <div class="tabs">
    <button class="tab active" onclick="switchTab('cgpa')">⚡ CGPA Calculator</button>
    <button class="tab" onclick="switchTab('yearly')">📅 Year-wise</button>
    <button class="tab" onclick="switchTab('ref')">📚 Grade Reference</button>
  </div>

  <!-- === CGPA TAB === -->
  <div id="tab-cgpa" class="sec active">
    <div class="panel">
      <div class="panel-title">Input method</div>
      <div class="mode-row">
        <button class="mode-btn active" id="mb-course" onclick="setMode('course')">By course grades</button>
        <button class="mode-btn" id="mb-custom" onclick="setMode('custom')">By custom CGPA values</button>
      </div>

      <!-- COURSE MODE -->
      <div id="course-mode">
        <div class="tbl-head" id="c-col-heads">
          <span>Course name</span><span>Credits</span><span>Grade</span><span>Points</span><span></span>
        </div>
        <div id="course-list"></div>
        <button class="add-btn" onclick="addCourse()"><span class="plus">+</span> Add course</button>
      </div>

      <!-- CUSTOM MODE -->
      <div id="custom-mode" style="display:none;">
        <p style="font-size:12px;color:var(--text3);margin-bottom:12px;">Enter each GPA score directly (e.g. 3.20, 2.69, 2.66). Overall average calculated automatically.</p>
        <div style="display:grid;grid-template-columns:1fr 110px 36px;gap:8px;margin-bottom:8px;">
          <span style="font-size:10px;font-weight:500;color:var(--text3);text-transform:uppercase;letter-spacing:1px;padding:0 4px;">Label / semester</span>
          <span style="font-size:10px;font-weight:500;color:var(--text3);text-transform:uppercase;letter-spacing:1px;padding:0 4px;">CGPA value</span>
          <span></span>
        </div>
        <div id="custom-list"></div>
        <button class="add-btn" onclick="addCustomRow()"><span class="plus">+</span> Add entry</button>
      </div>
    </div>

    <button class="calc-btn" onclick="calcCGPA()">Calculate CGPA →</button>

    <div id="cgpa-result" style="display:none;" class="result-box">
      <div class="result-main">
        <div class="cgpa-hero">
          <div class="cgpa-big" id="r-num" style="color:var(--c3);">0.00</div>
          <div class="cgpa-max" id="r-denom">out of 4.0</div>
          <div class="cgpa-bar-wrap"><div class="cgpa-bar-fill" id="r-bar" style="width:0%;background:var(--c3);"></div></div>
        </div>
        <div class="result-info">
          <div class="grade-pill" id="r-pill" style="background:rgba(107,203,119,0.15);color:var(--c3);border:1px solid rgba(107,203,119,0.3);">
            <span id="r-grade-letter" style="font-family:'Syne',sans-serif;font-size:18px;font-weight:800;">A+</span>
            <span id="r-remark" style="font-size:13px;">Excellent</span>
          </div>
          <div class="remark-txt" id="r-country-info"></div>
        </div>
      </div>
      <div class="result-stats">
        <div class="stat-card">
          <div class="stat-label">Credits / entries</div>
          <div class="stat-val" id="r-credits" style="color:var(--c4);">—</div>
        </div>
        <div class="stat-card">
          <div class="stat-label">Percentage</div>
          <div class="stat-val" id="r-pct" style="color:var(--c2);">—</div>
        </div>
        <div class="stat-card">
          <div class="stat-label">Grade class</div>
          <div class="stat-val" id="r-class" style="color:var(--c5);">—</div>
        </div>
      </div>
      <div id="cgpa-breakdown"></div>
    </div>
  </div>

  <!-- === YEARLY TAB === -->
  <div id="tab-yearly" class="sec">
    <div class="panel">
      <div style="display:flex;align-items:center;gap:12px;margin-bottom:1.25rem;flex-wrap:wrap;">
        <div class="panel-title" style="margin-bottom:0;">Total academic years</div>
        <select class="flag-select" style="max-width:100px;flex:0 0 auto;" id="num-years" onchange="renderYears()">
          <option>2</option><option>3</option><option selected>4</option><option>5</option>
        </select>
      </div>
      <div id="yearly-body"></div>
      <button class="calc-btn" onclick="calcYearly()">Calculate all years →</button>
    </div>
    <div id="yearly-result" style="display:none;" class="result-box">
      <div class="sum-wrap"><table class="sum-table" id="yearly-table"></table></div>
      <div class="mini-metrics">
        <div class="stat-card"><div class="stat-label">Final CGPA</div><div class="stat-val" id="yr-cgpa" style="color:var(--c3);">—</div></div>
        <div class="stat-card"><div class="stat-label">Total credits</div><div class="stat-val" id="yr-credits" style="color:var(--c4);">—</div></div>
        <div class="stat-card"><div class="stat-label">Remark</div><div class="stat-val" id="yr-remark" style="font-size:14px;color:var(--c5);">—</div></div>
      </div>
    </div>
  </div>

  <!-- === REF TAB === -->
  <div id="tab-ref" class="sec">
    <div class="panel">
      <div class="panel-title">All countries at a glance</div>
      <div class="ref-grid" id="ref-grid"></div>
      <div class="divider"></div>
      <div class="panel-title">Detailed grade table</div>
      <select class="flag-select" style="max-width:260px;margin-bottom:1rem;" id="ref-sel" onchange="renderRefTable()">
        <option value="BD">🇧🇩 Bangladesh</option><option value="US">🇺🇸 USA</option>
        <option value="UK">🇬🇧 UK</option><option value="IN">🇮🇳 India</option>
        <option value="CA">🇨🇦 Canada</option><option value="AU">🇦🇺 Australia</option>
        <option value="DE">🇩🇪 Germany</option><option value="PK">🇵🇰 Pakistan</option>
        <option value="MY">🇲🇾 Malaysia</option><option value="CN">🇨🇳 China</option>
      </select>
      <table class="ref-detail-table" id="ref-detail"></table>
    </div>
  </div>

  <div class="footer">Built with <span>♥</span> · GradeVault 2025</div>
</div>

<script>
const C = {
  BD:{name:'Bangladesh',info:'4.0 scale · UGC Bangladesh',max:4.0,
    grades:[{l:'A+',gp:4.00,min:80,r:'Excellent'},{l:'A',gp:3.75,min:75,r:'Very Good'},{l:'A-',gp:3.50,min:70,r:'Very Good'},{l:'B+',gp:3.25,min:65,r:'Good'},{l:'B',gp:3.00,min:60,r:'Good'},{l:'B-',gp:2.75,min:55,r:'Good'},{l:'C+',gp:2.50,min:50,r:'Average'},{l:'C',gp:2.25,min:45,r:'Average'},{l:'D',gp:2.00,min:40,r:'Pass'},{l:'F',gp:0.00,min:0,r:'Fail'}],
    remark:g=>g>=3.50?'Excellent':g>=2.75?'Good':g>=2.25?'Average':g>=2.00?'Pass':'Fail',summary:'A+ = 4.0, pass at D (2.0)'},
  US:{name:'USA',info:'4.0 GPA · Standard US system',max:4.0,
    grades:[{l:'A',gp:4.0,min:93,r:'Excellent'},{l:'A-',gp:3.7,min:90,r:'Excellent'},{l:'B+',gp:3.3,min:87,r:'Good'},{l:'B',gp:3.0,min:83,r:'Good'},{l:'B-',gp:2.7,min:80,r:'Good'},{l:'C+',gp:2.3,min:77,r:'Average'},{l:'C',gp:2.0,min:73,r:'Average'},{l:'C-',gp:1.7,min:70,r:'Average'},{l:'D',gp:1.0,min:60,r:'Below Avg'},{l:'F',gp:0.0,min:0,r:'Fail'}],
    remark:g=>g>=3.5?"Dean's List":g>=3.0?'Good Standing':g>=2.0?'Satisfactory':'Probation',summary:"Dean's List at 3.5+"},
  UK:{name:'UK',info:'Honours classification',max:4.0,
    grades:[{l:'First (1st)',gp:4.0,min:70,r:'First Class'},{l:'2:1',gp:3.3,min:60,r:'Upper Second'},{l:'2:2',gp:2.7,min:50,r:'Lower Second'},{l:'Third',gp:2.0,min:40,r:'Third Class'},{l:'Fail',gp:0.0,min:0,r:'Fail'}],
    remark:g=>g>=4.0?'First Class':g>=3.3?'Upper Second (2:1)':g>=2.7?'Lower Second (2:2)':g>=2.0?'Third Class':'Fail',summary:'First / 2:1 / 2:2 / Third'},
  IN:{name:'India',info:'10 point CGPA · CBCS system',max:10.0,
    grades:[{l:'O',gp:10,min:90,r:'Outstanding'},{l:'A+',gp:9,min:80,r:'Excellent'},{l:'A',gp:8,min:70,r:'Very Good'},{l:'B+',gp:7,min:60,r:'Good'},{l:'B',gp:6,min:55,r:'Above Avg'},{l:'C',gp:5,min:50,r:'Average'},{l:'P',gp:4,min:45,r:'Pass'},{l:'F',gp:0,min:0,r:'Fail'}],
    remark:g=>g>=9?'Outstanding':g>=7?'Very Good':g>=5?'Average':g>=4?'Pass':'Fail',summary:'O = 10, pass at P (4.0)'},
  CA:{name:'Canada',info:'4.0 scale · varies by province',max:4.0,
    grades:[{l:'A+',gp:4.0,min:90,r:'Exceptional'},{l:'A',gp:4.0,min:85,r:'Excellent'},{l:'A-',gp:3.7,min:80,r:'Very Good'},{l:'B+',gp:3.3,min:77,r:'Good'},{l:'B',gp:3.0,min:73,r:'Good'},{l:'B-',gp:2.7,min:70,r:'Satisfactory'},{l:'C+',gp:2.3,min:67,r:'Satisfactory'},{l:'C',gp:2.0,min:63,r:'Adequate'},{l:'D',gp:1.0,min:50,r:'Marginal'},{l:'F',gp:0.0,min:0,r:'Fail'}],
    remark:g=>g>=3.7?'Distinction':g>=3.0?'Good Standing':g>=2.0?'Satisfactory':'Probation',summary:'Distinction at 3.7+'},
  AU:{name:'Australia',info:'7 point GPA scale',max:7.0,
    grades:[{l:'HD',gp:7,min:85,r:'High Distinction'},{l:'D',gp:6,min:75,r:'Distinction'},{l:'C',gp:5,min:65,r:'Credit'},{l:'P',gp:4,min:50,r:'Pass'},{l:'F',gp:0,min:0,r:'Fail'}],
    remark:g=>g>=6?'Distinction':g>=5?'Credit':g>=4?'Pass':'Fail',summary:'HD=7, Distinction=6, Pass=4'},
  DE:{name:'Germany',info:'1–5 scale (lower is better)',max:1.0,
    grades:[{l:'1.0 Sehr gut',gp:1.0,min:95,r:'Very Good'},{l:'1.3',gp:1.3,min:90,r:'Very Good'},{l:'1.7',gp:1.7,min:85,r:'Good'},{l:'2.0 Gut',gp:2.0,min:80,r:'Good'},{l:'2.3',gp:2.3,min:75,r:'Good'},{l:'2.7',gp:2.7,min:70,r:'Satisfactory'},{l:'3.0',gp:3.0,min:65,r:'Satisfactory'},{l:'3.7',gp:3.7,min:55,r:'Sufficient'},{l:'4.0',gp:4.0,min:50,r:'Min pass'},{l:'5.0',gp:5.0,min:0,r:'Fail'}],
    remark:g=>g<=1.5?'Sehr gut':g<=2.5?'Gut':g<=3.5?'Befriedigend':g<=4.0?'Ausreichend':'Nicht bestanden',summary:'1.0 best, 4.0 min pass'},
  PK:{name:'Pakistan',info:'4.0 scale · HEC Pakistan',max:4.0,
    grades:[{l:'A+',gp:4.0,min:90,r:'Outstanding'},{l:'A',gp:4.0,min:85,r:'Excellent'},{l:'A-',gp:3.7,min:80,r:'Very Good'},{l:'B+',gp:3.3,min:75,r:'Good'},{l:'B',gp:3.0,min:70,r:'Good'},{l:'B-',gp:2.7,min:65,r:'Above Avg'},{l:'C+',gp:2.3,min:60,r:'Average'},{l:'C',gp:2.0,min:55,r:'Average'},{l:'D',gp:1.0,min:45,r:'Pass'},{l:'F',gp:0.0,min:0,r:'Fail'}],
    remark:g=>g>=3.5?'Excellent':g>=3.0?'Good':g>=2.0?'Average':g>=1.0?'Pass':'Fail',summary:'HEC, A+ = 4.0, pass at D'},
  MY:{name:'Malaysia',info:'4.0 CGPA · MQA standard',max:4.0,
    grades:[{l:'A+',gp:4.0,min:90,r:'Excellent'},{l:'A',gp:4.0,min:80,r:'Excellent'},{l:'A-',gp:3.67,min:75,r:'Very Good'},{l:'B+',gp:3.33,min:70,r:'Good'},{l:'B',gp:3.0,min:65,r:'Good'},{l:'B-',gp:2.67,min:60,r:'Satisfactory'},{l:'C+',gp:2.33,min:55,r:'Satisfactory'},{l:'C',gp:2.0,min:50,r:'Pass'},{l:'D',gp:1.0,min:45,r:'Marginal'},{l:'F',gp:0.0,min:0,r:'Fail'}],
    remark:g=>g>=3.67?'Distinction':g>=3.0?'Merit':g>=2.0?'Pass':'Fail',summary:'Distinction at 3.67+'},
  CN:{name:'China',info:'4.0 GPA · 百分制',max:4.0,
    grades:[{l:'A (优秀)',gp:4.0,min:90,r:'Excellent'},{l:'B (良好)',gp:3.0,min:75,r:'Good'},{l:'C (中等)',gp:2.0,min:60,r:'Average'},{l:'D (及格)',gp:1.0,min:50,r:'Pass'},{l:'F (不及格)',gp:0.0,min:0,r:'Fail'}],
    remark:g=>g>=3.5?'Excellent':g>=3.0?'Good':g>=2.0?'Average':g>=1.0?'Pass':'Fail',summary:'A=90+, pass at D (50%)'}
};
const FLAGS={BD:'🇧🇩',US:'🇺🇸',UK:'🇬🇧',IN:'🇮🇳',CA:'🇨🇦',AU:'🇦🇺',DE:'🇩🇪',PK:'🇵🇰',MY:'🇲🇾',CN:'🇨🇳'};
const YEAR_COLORS=['c1','c2','c3','c4','c5'];

let curC='BD', curMode='course', cc=0, crc=0, ysc=0;

function getC(code){return C[code||curC];}
function closest(gp,code){return getC(code).grades.reduce((a,b)=>Math.abs(b.gp-gp)<Math.abs(a.gp-gp)?b:a);}
function badgeClass(gp,code){
  const c=getC(code);
  if(code==='DE'){return gp<=1.5?'bA':gp<=2.5?'bB':gp<=3.5?'bC':gp<=4.0?'bD':'bF';}
  const r=gp/c.max;
  return r>=0.875?'bA':r>=0.7?'bB':r>=0.5?'bC':r>=0.4?'bD':'bF';
}
function gpaColor(gpa,code){
  const c=getC(code);
  if(code==='DE'){return gpa<=2.0?'var(--c3)':gpa<=3.0?'var(--c2)':'var(--c1)';}
  const r=gpa/c.max;
  return r>=0.7?'var(--c3)':r>=0.5?'var(--c2)':'var(--c1)';
}
function remarkClass(gpa,code){
  const r=gpaColor(gpa,code);
  if(r==='var(--c3)')return{bg:'rgba(107,203,119,0.15)',color:'var(--c3)',border:'rgba(107,203,119,0.3)'};
  if(r==='var(--c2)')return{bg:'rgba(255,217,61,0.15)',color:'var(--c2)',border:'rgba(255,217,61,0.3)'};
  return{bg:'rgba(255,107,107,0.15)',color:'var(--c1)',border:'rgba(255,107,107,0.3)'};
}
function gradeOpts(code,selGP){return getC(code).grades.map(g=>`<option value="${g.gp}"${g.gp===selGP?' selected':''}>${g.l}</option>`).join('');}

function onCountryChange(){
  curC=document.getElementById('country-sel').value;
  document.getElementById('scale-tag').textContent=getC(curC).info;
  rebuildCourseList();
  document.getElementById('cgpa-result').style.display='none';
}

function setMode(m){
  curMode=m;
  document.getElementById('mb-course').classList.toggle('active',m==='course');
  document.getElementById('mb-custom').classList.toggle('active',m==='custom');
  document.getElementById('course-mode').style.display=m==='course'?'block':'none';
  document.getElementById('custom-mode').style.display=m==='custom'?'block':'none';
  document.getElementById('cgpa-result').style.display='none';
}

function rebuildCourseList(){
  const list=document.getElementById('course-list');
  const saved=[...list.querySelectorAll('.course-row')].map(r=>({name:r.children[0].value,credit:r.children[1].value}));
  list.innerHTML='';cc=0;
  const def=getC(curC).grades[0].gp;
  if(saved.length)saved.forEach(s=>addCourse(s.name,parseFloat(s.credit)||3,def));
  else{addCourse('',3,def);addCourse('',3,def);addCourse('',3,def);}
}

function addCourse(name='',credit=3,gp=null){
  cc++;const id='c'+cc;
  const defGP=gp!==null?gp:getC(curC).grades[0].gp;
  const div=document.createElement('div');
  div.className='course-row';div.id=id;
  div.innerHTML=`
    <input type="text" placeholder="e.g. Advanced Math" value="${name}">
    <input type="number" min="0.5" max="6" step="0.5" value="${credit}" style="text-align:center;">
    <select onchange="updateGP(this,'${id}')">${gradeOpts(curC,defGP)}</select>
    <div class="gp-chip" id="gp-${id}">${defGP}</div>
    <button class="rm-btn" onclick="document.getElementById('${id}').remove()" title="Remove">×</button>
  `;
  document.getElementById('course-list').appendChild(div);
}

function updateGP(sel,id){document.getElementById('gp-'+id).textContent=parseFloat(sel.value);}

function addCustomRow(label='',val=''){
  crc++;const id='cr'+crc;
  const div=document.createElement('div');div.className='custom-row';div.id=id;
  div.style.animation='rowIn 0.25s ease both';
  div.innerHTML=`
    <input type="text" placeholder="e.g. Semester 1 / Year 1" value="${label}">
    <input type="number" min="0" max="10" step="0.01" placeholder="e.g. 3.20" value="${val}">
    <button class="rm-btn" onclick="document.getElementById('${id}').remove()">×</button>
  `;
  document.getElementById('custom-list').appendChild(div);
}

function calcCGPA(){
  const code=curC;
  let cgpa=0,count=0,custom=null;
  if(curMode==='course'){
    const rows=document.querySelectorAll('#course-list .course-row');
    if(!rows.length){alert('Please add at least one course.');return;}
    let tp=0,tc=0;
    rows.forEach(r=>{const c=parseFloat(r.children[1].value)||0;const g=parseFloat(r.children[2].value)||0;tp+=c*g;tc+=c;});
    cgpa=tc?tp/tc:0;count=tc;
  } else {
    const rows=document.querySelectorAll('#custom-list .custom-row');
    if(!rows.length){alert('Please add at least one entry.');return;}
    let vals=[],labels=[];
    rows.forEach(r=>{const lbl=r.children[0].value||('Entry '+(labels.length+1));const v=parseFloat(r.children[1].value);if(!isNaN(v)){labels.push(lbl);vals.push(v);}});
    if(!vals.length){alert('Please enter valid numeric values.');return;}
    cgpa=vals.reduce((a,b)=>a+b,0)/vals.length;count=vals.length;
    if(vals.length>1)custom={labels,vals};
  }
  showResult(cgpa,count,custom,code);
}

function showResult(cgpa,count,custom,code){
  const c=getC(code);
  const col=gpaColor(cgpa,code);
  const rc=remarkClass(cgpa,code);
  const pct=code==='DE'?((5-cgpa)/4*100).toFixed(1):(cgpa/c.max*100).toFixed(1);
  const barPct=code==='DE'?((5-cgpa)/4*100):cgpa/c.max*100;
  const g=closest(cgpa,code);
  const remarkText=c.remark(cgpa);

  document.getElementById('r-num').textContent=cgpa.toFixed(2);
  document.getElementById('r-num').style.color=col;
  document.getElementById('r-denom').textContent='out of '+c.max;
  document.getElementById('r-bar').style.width=Math.min(barPct,100)+'%';
  document.getElementById('r-bar').style.background=col;
  document.getElementById('r-grade-letter').textContent=g.l;
  document.getElementById('r-remark').textContent=remarkText;
  const pill=document.getElementById('r-pill');
  pill.style.background=rc.bg;pill.style.color=rc.color;pill.style.borderColor=rc.border;
  document.getElementById('r-country-info').textContent=FLAGS[code]+' '+c.name+' · '+c.info;
  document.getElementById('r-credits').textContent=count;
  document.getElementById('r-pct').textContent=pct+'%';
  document.getElementById('r-class').textContent=g.l;

  if(custom&&custom.vals.length>1){
    let bk='<div class="bk-title">Breakdown</div>';
    custom.labels.forEach((lbl,i)=>{
      const v=custom.vals[i];const gl=closest(v,code);
      bk+=`<div class="bk-row"><span class="bk-label">${lbl}</span><div class="bk-right"><span class="bk-val" style="color:${gpaColor(v,code)};">${v.toFixed(2)}</span><span class="badge ${badgeClass(v,code)}">${gl.l}</span></div></div>`;
    });
    document.getElementById('cgpa-breakdown').innerHTML=bk;
  } else {document.getElementById('cgpa-breakdown').innerHTML='';}
  document.getElementById('cgpa-result').style.display='block';
}

function switchTab(tab){
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.sec').forEach(s=>s.classList.remove('active'));
  ['cgpa','yearly','ref'].forEach((t,i)=>{
    if(t===tab){
      document.querySelectorAll('.tab')[i].classList.add('active');
      document.getElementById('tab-'+t).classList.add('active');
    }
  });
  if(tab==='yearly')renderYears();
  if(tab==='ref'){renderRefGrid();renderRefTable();}
}

function renderYears(){
  const n=parseInt(document.getElementById('num-years').value);
  const code=curC;
  const body=document.getElementById('yearly-body');
  const prev={};
  body.querySelectorAll('[data-year]').forEach(el=>{
    const y=el.dataset.year;if(!prev[y])prev[y]=[];
    el.querySelectorAll('.course-row').forEach(r=>prev[y].push({name:r.children[0].value,credit:r.children[1].value,gp:parseFloat(r.children[2].value)}));
  });
  body.innerHTML='';
  for(let y=1;y<=n;y++){
    const cl=YEAR_COLORS[(y-1)%5];
    const wrap=document.createElement('div');
    wrap.className=`year-block y${y}`;wrap.dataset.year=y;
    wrap.innerHTML=`
      <div class="year-label"><span class="year-dot"></span>Year ${y}</div>
      <div class="tbl-head">
        <span>Subject</span>
        <span>Credits</span>
        <span>Grade</span>
        <span>Points</span>
        <span></span>
      </div>
      <div id="ys${y}"></div>
      <button class="add-btn" style="font-size:12px;margin-bottom:6px;" onclick="addYS(${y},'${code}')"><span class="plus">+</span> Add subject</button>
      ${y<n?'<div class="divider"></div>':''}
    `;
    body.appendChild(wrap);
    const saved=prev[y];
    if(saved&&saved.length)saved.forEach(s=>addYS(y,code,s.name,parseFloat(s.credit)||3,s.gp));
    else{addYS(y,code);addYS(y,code);}
  }
}

function addYS(year,code,name='',credit=3,gp=null){
  ysc++;const id='ys'+ysc;
  const defGP=gp!==null?gp:getC(code).grades[0].gp;
  const container=document.getElementById('ys'+year);if(!container)return;
  const div=document.createElement('div');div.className='course-row';div.id=id;
  div.innerHTML=`
    <input type="text" placeholder="Subject name" value="${name}">
    <input type="number" min="1" max="6" step="0.5" value="${credit}" style="text-align:center;">
    <select onchange="updateGP(this,'${id}')">${gradeOpts(code,defGP)}</select>
    <div class="gp-chip" id="gp-${id}">${defGP}</div>
    <button class="rm-btn" onclick="document.getElementById('${id}').remove()">×</button>
  `;
  container.appendChild(div);
}

function calcYearly(){
  const n=parseInt(document.getElementById('num-years').value);
  const code=curC;
  let html='<tr><th>Year</th><th>Credits</th><th>GPA</th><th>Grade</th><th>Remark</th></tr>';
  let gp=0,gc=0;
  for(let y=1;y<=n;y++){
    const subs=document.querySelectorAll(`#ys${y} .course-row`);
    let yp=0,yc=0;
    subs.forEach(r=>{const c=parseFloat(r.children[1].value)||0;const g=parseFloat(r.children[2].value)||0;yp+=c*g;yc+=c;});
    const ygpa=yc?yp/yc:0;gp+=yp;gc+=yc;
    const ltr=closest(ygpa,code).l;
    const col=gpaColor(ygpa,code);
    html+=`<tr><td>Year ${y}</td><td>${yc}</td><td style="font-family:'Syne',sans-serif;font-weight:700;color:${col};">${ygpa.toFixed(2)}</td><td><span class="badge ${badgeClass(ygpa,code)}">${ltr}</span></td><td>${getC(code).remark(ygpa)}</td></tr>`;
  }
  const cgpa=gc?gp/gc:0;
  const fcol=gpaColor(cgpa,code);
  html+=`<tr class="total-row"><td style="font-weight:600;">Overall CGPA</td><td>${gc}</td><td style="font-family:'Syne',sans-serif;font-weight:800;font-size:1.1rem;color:${fcol};">${cgpa.toFixed(2)} / ${getC(code).max}</td><td><span class="badge ${badgeClass(cgpa,code)}">${closest(cgpa,code).l}</span></td><td>${getC(code).remark(cgpa)}</td></tr>`;
  document.getElementById('yearly-table').innerHTML=html;
  document.getElementById('yr-cgpa').textContent=cgpa.toFixed(2);
  document.getElementById('yr-cgpa').style.color=fcol;
  document.getElementById('yr-credits').textContent=gc;
  document.getElementById('yr-remark').textContent=getC(code).remark(cgpa);
  document.getElementById('yearly-result').style.display='block';
}

function renderRefGrid(){
  document.getElementById('ref-grid').innerHTML=Object.keys(C).map(k=>`
    <div class="ref-card">
      <div class="ref-card-title">${FLAGS[k]} ${C[k].name}</div>
      <div class="ref-card-sub">${C[k].summary}<br><span style="color:var(--text2);">Max: ${C[k].max}</span></div>
    </div>
  `).join('');
}

function renderRefTable(){
  const code=document.getElementById('ref-sel').value;
  const c=getC(code);
  let html='<tr><th>Grade</th><th>Points</th><th>Min %</th><th>Remark</th></tr>';
  c.grades.forEach(g=>{
    html+=`<tr><td><span class="badge ${badgeClass(g.gp,code)}">${g.l}</span></td><td style="font-family:'Syne',sans-serif;font-weight:600;">${g.gp}</td><td>${g.min}%+</td><td style="color:var(--text2);">${g.r}</td></tr>`;
  });
  document.getElementById('ref-detail').innerHTML=html;
}

// INIT
document.getElementById('scale-tag').textContent=getC('BD').info;
addCourse('',3,4.0);addCourse('',3,3.75);addCourse('',3,3.50);
addCustomRow('Semester 1','3.20');addCustomRow('Semester 2','2.69');addCustomRow('Semester 3','2.66');
</script>
</body>
</html>
