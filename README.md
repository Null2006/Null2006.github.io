<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#0a0d13">
<title>反物质维度 Lite · 增量放置游戏</title>
<style>
  :root{
    --bg:#0a0d13;
    --panel:#131a26;
    --panel-2:#18202e;
    --line:#243044;
    --text:#e7eef7;
    --muted:#8b9ab2;
    --accent:#8d7cff;
    --accent-2:#33d6e8;
    --gold:#f6c453;
    --danger:#ff5f6d;
    --ok:#4ade80;
    --radius:14px;
  }
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
  html,body{margin:0;padding:0}
  body{
    background:
      radial-gradient(1100px 520px at 12% -12%, #1b1450 0%, transparent 62%),
      radial-gradient(820px 460px at 102% -6%, #0a2b34 0%, transparent 58%),
      var(--bg);
    background-attachment:fixed;
    color:var(--text);
    font-family:-apple-system,BlinkMacSystemFont,"Segoe UI","PingFang SC","Hiragino Sans GB","Microsoft YaHei",sans-serif;
    font-size:15px;
    line-height:1.5;
    min-height:100vh;
    display:flex;
    flex-direction:column;
    overflow-x:hidden;
  }
  button{font-family:inherit;font-size:inherit;color:inherit;border:0;background:none;cursor:pointer;touch-action:manipulation}
  b{font-weight:600}
  .mono{font-family:ui-monospace,SFMono-Regular,Consolas,"Liberation Mono",Menlo,monospace;font-variant-numeric:tabular-nums}

  /* ---------- 顶栏 ---------- */
  .topwrap{
    position:sticky;top:0;z-index:30;
    background:rgba(10,13,19,.94);
    backdrop-filter:blur(14px);
    border-bottom:1px solid var(--line);
    padding-top:env(safe-area-inset-top);
  }
  header{
    padding:10px 14px 9px;
    background:linear-gradient(180deg,rgba(20,26,40,.55),transparent);
  }
  .bar-top{display:flex;align-items:center;justify-content:space-between;gap:10px;margin-bottom:8px}
  .brand{display:flex;align-items:center;gap:8px;font-weight:700;letter-spacing:.3px;font-size:15px}
  .brand .orb{
    width:14px;height:14px;border-radius:50%;
    background:radial-gradient(circle at 32% 30%, #fff, var(--accent) 45%, #3a2a9e 100%);
    box-shadow:0 0 14px rgba(141,124,255,.85);
    animation:pulse 2.6s ease-in-out infinite;
  }
  @keyframes pulse{0%,100%{transform:scale(1);opacity:1}50%{transform:scale(1.18);opacity:.75}}
  .brand small{color:var(--muted);font-weight:500;font-size:11px;letter-spacing:1.4px;text-transform:uppercase}
  .mult-badge{
    font-size:12px;color:var(--muted);white-space:nowrap;
    border:1px solid var(--line);border-radius:999px;
    padding:3px 10px;background:var(--panel);
  }
  .mult-badge b{color:var(--accent-2)}

  .res-row{display:grid;grid-template-columns:1.5fr 1fr 1fr;gap:8px}
  .res{
    background:var(--panel);border:1px solid var(--line);border-radius:12px;
    padding:6px 10px;min-width:0;
  }
  .res label{display:block;font-size:10.5px;letter-spacing:1.1px;color:var(--muted);text-transform:uppercase}
  .res b{display:block;font-size:17px;line-height:1.25;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .res.am b{color:#fff}
  .res.rate b{color:var(--accent-2)}
  .res.dp b{color:var(--gold)}

  /* ---------- 标签栏 ---------- */
  nav.tabs{
    display:flex;gap:6px;padding:0 10px 8px;
    overflow-x:auto;scrollbar-width:none;
  }
  nav.tabs::-webkit-scrollbar{display:none}
  .tab{
    flex:1 0 auto;min-width:64px;
    padding:8px 12px;border-radius:10px;
    font-size:13.5px;color:var(--muted);
    border:1px solid transparent;transition:.18s;
    white-space:nowrap;
  }
  .tab:hover{color:var(--text)}
  .tab.on{
    color:#fff;background:linear-gradient(180deg,rgba(141,124,255,.22),rgba(141,124,255,.08));
    border-color:rgba(141,124,255,.5);
    box-shadow:0 0 0 1px rgba(141,124,255,.15) inset;
  }
  .tab .dot-badge{display:inline-block;width:6px;height:6px;border-radius:50%;background:var(--gold);margin-left:5px;vertical-align:middle}

  /* ---------- 主区 ---------- */
  main{flex:1;padding:12px 12px 40px;max-width:1080px;width:100%;margin:0 auto}
  .panel{display:none;animation:fade .18s ease}
  .panel.on{display:block}
  @keyframes fade{from{opacity:0;transform:translateY(4px)}to{opacity:1;transform:none}}

  .card{
    background:linear-gradient(180deg,var(--panel),var(--panel-2));
    border:1px solid var(--line);border-radius:var(--radius);
    padding:14px;margin-bottom:12px;
  }
  .card h3{margin:0 0 4px;font-size:15px;display:flex;align-items:center;gap:8px}
  .card .hint{color:var(--muted);font-size:12.5px;margin:0 0 12px;line-height:1.6}
  .sec-title{
    display:flex;align-items:center;justify-content:space-between;gap:10px;
    margin:4px 2px 10px;font-size:12px;letter-spacing:1.2px;color:var(--muted);text-transform:uppercase;
  }

  /* 购买数量 */
  .seg{display:inline-flex;background:#0e1420;border:1px solid var(--line);border-radius:10px;overflow:hidden}
  .seg button{padding:6px 12px;font-size:13px;color:var(--muted);transition:.15s}
  .seg button.on{background:rgba(141,124,255,.2);color:#fff}

  /* 维度卡片 */
  .dim-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}
  @media (max-width:640px){.dim-grid{grid-template-columns:1fr}}
  .dim{
    background:linear-gradient(180deg,#131b28,#111825);
    border:1px solid var(--line);border-radius:12px;padding:10px 12px 9px;
    transition:border-color .2s, box-shadow .2s;
  }
  .dim.can{border-color:rgba(141,124,255,.42);box-shadow:0 0 0 1px rgba(141,124,255,.12), 0 6px 20px -14px rgba(141,124,255,.9)}
  .dim-top{display:flex;align-items:baseline;gap:8px}
  .dim-tag{
    font-size:11px;font-weight:700;letter-spacing:.5px;
    color:#0a0d13;background:linear-gradient(180deg,var(--accent),#6f5cff);
    border-radius:6px;padding:1px 6px;
  }
  .dim-title{font-size:13.5px;font-weight:600;flex:1;min-width:0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .dim-count{font-size:13px;color:#fff;font-weight:600}
  .dim-count em{font-style:normal;color:var(--muted);font-size:11.5px;margin-left:3px}
  .dim-info{display:flex;justify-content:space-between;gap:8px;color:var(--muted);font-size:12px;margin:4px 0 8px}
  .dim-info span{white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .dim-info .out{color:var(--accent-2)}

  .buy{
    width:100%;display:flex;align-items:center;justify-content:space-between;gap:8px;
    padding:8px 11px;border-radius:9px;
    background:#0d1420;border:1px solid var(--line);
    color:#5d6c85;font-size:13px;transition:.15s;
  }
  .buy b{font-weight:600;font-size:13px}
  .buy.can{
    color:#fff;border-color:rgba(141,124,255,.55);
    background:linear-gradient(180deg,rgba(141,124,255,.24),rgba(141,124,255,.1));
  }
  .buy.can:hover{background:linear-gradient(180deg,rgba(141,124,255,.34),rgba(141,124,255,.16))}
  .buy.can:active{transform:scale(.985)}
  .buy b{font-family:ui-monospace,Consolas,monospace}

  .bar{height:3px;border-radius:2px;background:#0b111b;overflow:hidden;margin-top:6px}
  .bar i{display:block;height:100%;width:0;background:linear-gradient(90deg,var(--accent),var(--accent-2));transition:width .18s linear}

  /* 通用按钮 */
  .btn{
    display:inline-flex;align-items:center;justify-content:center;gap:8px;
    padding:10px 16px;border-radius:10px;font-size:14px;font-weight:600;
    background:#0d1420;border:1px solid var(--line);color:var(--muted);
    transition:.15s;width:100%;
  }
  .btn.can{color:#fff;border-color:rgba(141,124,255,.55);background:linear-gradient(180deg,rgba(141,124,255,.26),rgba(141,124,255,.1))}
  .btn.can:hover{filter:brightness(1.15)}
  .btn.gold.can{border-color:rgba(246,196,83,.55);background:linear-gradient(180deg,rgba(246,196,83,.24),rgba(246,196,83,.08))}
  .btn.ghost{background:#0b111b}
  .btn.ghost:hover{color:var(--text);border-color:#33425a}
  .btn.danger{color:#ff9aa2;border-color:rgba(255,95,109,.35);background:rgba(255,95,109,.08)}
  .btn.danger:hover{background:rgba(255,95,109,.16)}
  .btn[disabled]{opacity:.55;cursor:not-allowed}
  .btn-row{display:flex;gap:8px;flex-wrap:wrap}
  .btn-row .btn{width:auto;flex:1 1 130px}

  /* 升级 */
  .up{display:flex;gap:12px;align-items:flex-start;padding:12px;border:1px solid var(--line);border-radius:12px;margin-bottom:9px;background:#111825;transition:.18s}
  .up.can{border-color:rgba(246,196,83,.42);box-shadow:0 0 0 1px rgba(246,196,83,.1)}
  .up.maxed{opacity:.75}
  .up-body{flex:1;min-width:0}
  .up-name{font-size:14px;font-weight:600;display:flex;align-items:center;gap:8px;flex-wrap:wrap}
  .lvl{font-size:11px;color:var(--gold);background:rgba(246,196,83,.12);border:1px solid rgba(246,196,83,.28);border-radius:999px;padding:0 7px}
  .up-desc{color:var(--muted);font-size:12.5px;margin-top:3px}
  .up-buy{flex:0 0 auto;align-self:center;min-width:96px;padding:9px 12px;border-radius:10px;background:#0d1420;border:1px solid var(--line);color:#5d6c85;font-size:13px;font-weight:600}
  .up-buy.can{color:var(--gold);border-color:rgba(246,196,83,.55);background:rgba(246,196,83,.1)}
  .up-buy.can:hover{background:rgba(246,196,83,.18)}

  /* 成就 */
  .ach-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(158px,1fr));gap:9px}
  .ach{
    border:1px solid var(--line);border-radius:11px;padding:10px 11px;background:#0f1622;
    opacity:.55;transition:.2s;
  }
  .ach.got{
    opacity:1;border-color:rgba(74,222,128,.4);
    background:linear-gradient(180deg,rgba(74,222,128,.12),rgba(74,222,128,.03));
  }
  .ach-t{font-size:13px;font-weight:600;display:flex;align-items:center;gap:6px}
  .ach-t .tick{color:var(--ok);font-size:12px}
  .ach-d{font-size:11.5px;color:var(--muted);margin-top:4px;line-height:1.45}
  .ach.got .ach-d{color:#9fb3a9}
  .progress{height:6px;border-radius:3px;background:#0b111b;overflow:hidden;margin:10px 0 4px}
  .progress i{display:block;height:100%;background:linear-gradient(90deg,var(--ok),var(--accent-2));width:0;transition:width .3s}

  /* 统计 */
  .stats{display:grid;grid-template-columns:repeat(auto-fill,minmax(140px,1fr));gap:9px}
  .stat{background:#0f1622;border:1px solid var(--line);border-radius:10px;padding:9px 11px}
  .stat label{display:block;font-size:11px;color:var(--muted);letter-spacing:.6px}
  .stat b{font-size:14px}

  /* 存档 */
  textarea{
    width:100%;min-height:110px;resize:vertical;border-radius:10px;
    background:#0a1018;border:1px solid var(--line);color:var(--text);
    padding:10px;font-family:ui-monospace,Consolas,monospace;font-size:12px;line-height:1.5;
  }
  .warn{color:#ffb3ba;font-size:12.5px;line-height:1.6}

  /* Toast */
  #toasts{position:fixed;left:50%;bottom:18px;transform:translateX(-50%);z-index:60;display:flex;flex-direction:column;gap:8px;pointer-events:none;width:min(92vw,420px)}
  .toast{
    background:rgba(24,32,46,.97);border:1px solid rgba(141,124,255,.45);
    border-radius:11px;padding:10px 14px;font-size:13px;
    box-shadow:0 12px 30px -12px #000;animation:tin .25s ease;
  }
  .toast.gold{border-color:rgba(246,196,83,.5)}
  .toast.ok{border-color:rgba(74,222,128,.5)}
  @keyframes tin{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:none}}
  .toast.out{opacity:0;transform:translateY(8px);transition:.3s}

  footer{text-align:center;color:#4b5a70;font-size:11.5px;padding:0 14px 22px}
  @media (max-width:520px){
    body{font-size:14px}
    header{padding:7px 10px 7px}
    .res b{font-size:15px}
    .brand{font-size:13px}
    .brand small{display:none}
    .bar-top{margin-bottom:6px}
    .res-row{grid-template-columns:1.4fr 1fr 1fr;gap:6px}
    .res{padding:5px 8px}
    .res label{font-size:9.5px;letter-spacing:.6px}
    nav.tabs{padding:0 8px 7px}
    .tab{padding:7px 10px;font-size:13px;min-width:58px}
    main{padding:10px 10px 32px}
    .card{padding:12px}
    .toast{font-size:12.5px;padding:9px 12px}
  }
</style>
</head>
<body>

<div class="topwrap">
<header>
  <div class="bar-top">
    <div class="brand"><span class="orb"></span>反物质维度 <small>LITE</small></div>
    <div class="mult-badge">全局倍率 <b id="multVal" class="mono">1.00×</b></div>
  </div>
  <div class="res-row">
    <div class="res am"><label>反物质</label><b id="amVal" class="mono">0</b></div>
    <div class="res rate"><label>产出 / 秒</label><b id="rateVal" class="mono">0</b></div>
    <div class="res dp"><label>维度点数</label><b id="dpVal" class="mono">0</b></div>
  </div>
</header>

<nav class="tabs" id="tabs">
  <button class="tab on" data-tab="gen">生成器</button>
  <button class="tab" data-tab="dim">维度</button>
  <button class="tab" data-tab="up">升级</button>
  <button class="tab" data-tab="ach">成就<span class="dot-badge" id="achDot" style="display:none"></span></button>
  <button class="tab" data-tab="save">存档</button>
</nav>
</div>

<main>
  
  <section class="panel on" id="panel-gen">
    <div class="card">
      <h3>反物质生成器</h3>
      <p class="hint">
        第一维度直接产出<b>反物质</b>；更高的维度会以每秒 <b id="highProdTxt">0.2</b> 个单位的速度生产下一层维度。
        生产的维度不需要花钱，但只有购买才会推高价格等级。价格随购买次数指数增长。
      </p>
      <div class="sec-title">
        <span>购买数量</span>
        <div class="seg" id="buySeg">
          <button data-amt="1" class="on">×1</button>
          <button data-amt="10">×10</button>
          <button data-amt="max">最大</button>
        </div>
      </div>
      <div class="dim-grid" id="dimGrid"></div>
    </div>
  </section>

  
  <section class="panel" id="panel-dim">
    <div class="card">
      <h3>维度重置 <span class="lvl">BIG CRUNCH</span></h3>
      <p class="hint">
        清空反物质、所有维度与维度提升，换取<b>维度点数</b>。
        维度点数永久生效：每 1 点提供 <b>+50%</b> 全局产能（按累计获得计算，花掉也不掉）。<br>
        重置门槛：持有 <b class="mono" id="reqTxt">1.00e6</b> 反物质。
      </p>
      <div class="stats" style="margin-bottom:12px">
        <div class="stat"><label>本次可获得</label><b class="mono" id="gainVal" style="color:var(--gold)">0</b></div>
        <div class="stat"><label>累计维度点数</label><b class="mono" id="totalDpVal">0</b></div>
        <div class="stat"><label>重置次数</label><b class="mono" id="resetVal">0</b></div>
      </div>
      <button class="btn gold" id="resetBtn">维度重置</button>
    </div>

    <div class="card">
      <h3>维度提升 <span class="lvl">BOOST</span></h3>
      <p class="hint">消耗反物质，永久（本局内）提升全局产能。每次提升后价格 ×25，效果 <b id="boostEffTxt">×2</b>。</p>
      <button class="btn" id="boostBtn">
        <span>维度提升</span><b class="mono" id="boostCost">1.00e4</b>
      </button>
      <div style="color:var(--muted);font-size:12.5px;margin-top:8px">已提升 <b id="boostCount" class="mono">0</b> 次</div>
    </div>

    <div class="card">
      <h3>统计</h3>
      <div class="stats" id="statsBox"></div>
    </div>
  </section>

  
  <section class="panel" id="panel-up">
    <div class="card">
      <h3>永久升级</h3>
      <p class="hint">用维度点数购买，<b>维度重置不会清空</b>。可重复购买的升级每次价格递增。</p>
      <div id="upList"></div>
    </div>
  </section>

  
  <section class="panel" id="panel-ach">
    <div class="card">
      <h3>成就 <span class="lvl" id="achCount">0 / 0</span></h3>
      <p class="hint">每个成就提供 <b id="achBonusTxt">+2%</b> 全局产能（可被「成就共鸣」升级提升）。</p>
      <div class="progress"><i id="achBar"></i></div>
      <div class="ach-grid" id="achGrid" style="margin-top:12px"></div>
    </div>
  </section>

  
  <section class="panel" id="panel-save">
    <div class="card">
      <h3>存档</h3>
      <p class="hint">
        游戏每 10 秒自动保存到浏览器本地（localStorage）。关闭页面后再回来会结算离线收益：
        按当前产出速率 × 离线效率（基础 50%，可用「离线协议」升级提升）× 最长 8 小时。
      </p>
      <div class="btn-row" style="margin-bottom:12px">
        <button class="btn ghost" id="saveNow">立即保存</button>
        <button class="btn ghost" id="exportBtn">导出存档</button>
        <button class="btn ghost" id="copyBtn">复制到剪贴板</button>
      </div>
      <textarea id="saveArea" placeholder="导出的存档会显示在这里；也可以把存档字符串粘贴进来后点击「导入存档」。"></textarea>
      <div class="btn-row" style="margin-top:10px">
        <button class="btn ghost" id="importBtn">导入存档</button>
        <button class="btn danger" id="wipeBtn">硬重置（清空全部进度）</button>
      </div>
      <p class="warn" style="margin-top:12px">硬重置会删除全部进度且无法撤销，会连续确认两次。</p>
    </div>
    <div class="card">
      <h3>关于</h3>
      <p class="hint" style="margin:0">
        反物质维度 Lite · 单文件原生 HTML/CSS/JS 增量放置游戏，无任何外部依赖。<br>
        8 层生成器 / 指数涨价 / 维度重置 / 永久升级 / 成就 / 离线收益 / 自动存档。<br>
        灵感来自《Antimatter Dimensions》，数值与机制为独立设计。
      </p>
    </div>
  </section>
</main>

<footer>反物质维度 Lite · v1.0 · 数据保存在本地浏览器</footer>
<div id="toasts"></div>

<script>
(function () {
  'use strict';

  /* =========================================================
     配置区（想改平衡改这里就行）
     ========================================================= */
  var SAVE_KEY      = 'adlite_save_v1';
  var DIM_COUNT     = 8;
  var DIM_BASE      = [10, 25, 62.5, 156.25, 390.625, 976.5625, 2441.40625, 6103.515625];
  var GROWTH_BASE   = 1.8;     // 每次购买价格 ×1.8
  var HIGH_PROD     = 0.2;     // 高层维度每秒生产下一层的数量
  var RESET_REQ     = 1e6;     // 维度重置门槛
  var OFFLINE_CAP   = 8 * 3600;// 离线收益上限（秒）
  var OFFLINE_BASE  = 0.5;     // 离线效率
  var MAX_AM        = 1e308;   // 反物质软上限
  var SAVE_EVERY    = 10000;   // 自动存档间隔(ms)

  var ACH_BASE_BONUS = 0.02;
  var BOOST_COST_BASE = 1e4;
  var BOOST_COST_MULT = 25;
  var BOOST_EFFECT_BASE = 2;

  /* =========================================================
     工具
     ========================================================= */
  function trimNum(s){ return s.indexOf('.') >= 0 ? s.replace(/0+$/, '').replace(/\.$/, '') : s; }

  function fmt(n) {
    if (n === Infinity) return '∞';
    if (!isFinite(n)) return '0';
    if (n < 0) return '-' + fmt(-n);
    if (n === 0) return '0';
    if (n < 1000) {
      if (n < 10) return trimNum(n.toFixed(2));
      if (n < 100) return trimNum(n.toFixed(1));
      return String(Math.floor(n));
    }
    if (n < 1e6) return Math.floor(n).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
    var e = Math.floor(Math.log10(n));
    var m = n / Math.pow(10, e);
    if (m >= 9.9995) { m = 1; e += 1; }
    return m.toFixed(2) + 'e' + e;
  }

  function fmtInt(n){ return Math.floor(n).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ','); }

  function dur(sec) {
    sec = Math.max(0, Math.floor(sec));
    var d = Math.floor(sec / 86400), h = Math.floor(sec % 86400 / 3600),
        m = Math.floor(sec % 3600 / 60), s = sec % 60;
    if (d > 0) return d + '天' + h + '小时';
    if (h > 0) return h + '小时' + m + '分';
    if (m > 0) return m + '分' + s + '秒';
    return s + '秒';
  }

  function clamp(v, a, b){ return v < a ? a : (v > b ? b : v); }

  /* =========================================================
     状态
     ========================================================= */
  function newState() {
    var dims = [];
    for (var i = 0; i < DIM_COUNT; i++) dims.push({ b: 0, f: 0 });
    return {
      v: 1,
      am: 10,
      dims: dims,
      boosts: 0,
      dp: 0,
      totalDP: 0,
      resets: 0,
      up: {},
      ach: {},
      stats: {
        totalAm: 10,
        bestAm: 10,
        runStart: Date.now(),
        playTime: 0,
        maxBoosts: 0,
        upBought: 0,
        offlineBest: 0,
        bestRate: 0
      },
      lastSave: Date.now()
    };
  }

  var S = newState();
  var buyAmt = 1;              // 1 | 10 | 'max'
  var lastTick = Date.now();
  var saveTimer = 0;
  var autoTimer = 0;

  /* =========================================================
     派生数值
     ========================================================= */
  function upLv(id){ return S.up[id] || 0; }

  function growth() {
    return Math.max(1.3, GROWTH_BASE - 0.02 * upLv('cost'));
  }

  function dimBaseCost(i){ return DIM_BASE[i]; }

  function dimCost(i) {
    return dimBaseCost(i) * Math.pow(growth(), S.dims[i].b);
  }

  function costFor(i, k) {
    var g = growth();
    if (k <= 0) return 0;
    var base = dimBaseCost(i) * Math.pow(g, S.dims[i].b);
    if (Math.abs(g - 1) < 1e-9) return base * k;
    return base * (Math.pow(g, k) - 1) / (g - 1);
  }

  function maxBuy(i, budget) {
    var g = growth();
    var base = dimBaseCost(i) * Math.pow(g, S.dims[i].b);
    if (budget < base) return 0;
    var k = Math.floor(Math.log(1 + budget * (g - 1) / base) / Math.log(g) + 1e-9);
    return Math.max(0, k);
  }

  function dimCount(i){ return S.dims[i].b + S.dims[i].f; }

  function achCount(){ return Object.keys(S.ach).length; }

  function achBonus(){ return ACH_BASE_BONUS + 0.01 * upLv('achv'); }

  function boostEffect(){ return BOOST_EFFECT_BASE + upLv('boost'); }

  function boostCost(){ return BOOST_COST_BASE * Math.pow(BOOST_COST_MULT, S.boosts); }

  function globalMult() {
    var m = 1;
    m *= 1 + 0.5 * S.totalDP;                 // 维度点数
    m *= 1 + 0.25 * upLv('dp');               // 点数共振
    m *= Math.pow(1.5, upLv('dimMult'));      // 维度共鸣
    m *= 1 + achBonus() * achCount();         // 成就
    m *= Math.pow(boostEffect(), S.boosts);   // 维度提升
    return m;
  }

  function amRate(){ return dimCount(0) * globalMult(); }

  function dimOutput(i) {
    if (i === 0) return dimCount(0) * globalMult();
    return dimCount(i) * HIGH_PROD;
  }

  function prestigeGain() {
    if (S.am < RESET_REQ) return 0;
    // 对数平方曲线：1e6→1 点，1e8→9 点，1e12→49 点，1e20→225 点
    var raw = Math.floor(Math.pow(Math.log10(S.am) - 5, 2));
    return Math.max(1, Math.floor(raw * (1 + 0.25 * upLv('gain'))));
  }

  function startAm(){ return 10 * Math.pow(100, upLv('start')); }

  function offlineEff(){ return clamp(OFFLINE_BASE + 0.1 * upLv('offline'), 0, 1); }

  /* =========================================================
     升级定义
     ========================================================= */
  var UPGRADES = [
    { id:'dimMult', name:'维度共鸣', base:1,  growth:2.2, max:25,
      desc:'所有维度产量 ×1.5',
      effect:function(l){ return '当前 ×' + fmt(Math.pow(1.5, l)); } },
    { id:'cost',    name:'成本优化', base:3,  growth:2.6, max:25,
      desc:'维度涨价系数 −0.02（最低 1.3）',
      effect:function(l){ return '当前 ×' + (GROWTH_BASE - 0.02 * l).toFixed(2); } },
    { id:'dp',      name:'点数共振', base:2,  growth:2.4, max:20,
      desc:'维度点数效果 +25%（每级）',
      effect:function(l){ return '当前 +' + (25 * l) + '%'; } },
    { id:'start',   name:'启动资本', base:5,  growth:3.2, max:9,
      desc:'重置后初始反物质 ×100',
      effect:function(l){ return '初始 ' + fmt(10 * Math.pow(100, l)); } },
    { id:'boost',   name:'提升强化', base:8,  growth:3.5, max:8,
      desc:'维度提升效果 +1 倍（2→3→4…）',
      effect:function(l){ return '当前 ×' + (BOOST_EFFECT_BASE + l); } },
    { id:'offline', name:'离线协议', base:4,  growth:2.8, max:5,
      desc:'离线效率 +10%（上限 100%）',
      effect:function(l){ return '当前 ' + Math.round(offlineEffAt(l) * 100) + '%'; } },
    { id:'achv',    name:'成就共鸣', base:6,  growth:3.0, max:10,
      desc:'每个成就的加成 +1%',
      effect:function(l){ return '当前 每个 +' + ((ACH_BASE_BONUS + 0.01 * l) * 100).toFixed(0) + '%'; } },
    { id:'gain',    name:'重置协议', base:12, growth:3.2, max:10,
      desc:'维度点数获取量 +25%（每级）',
      effect:function(l){ return '当前 +' + (25 * l) + '%'; } },
    { id:'auto',    name:'自动化核心', base:30, growth:1, max:1,
      desc:'自动购买可负担的最高维度',
      effect:function(l){ return l ? '已启用' : '未启用'; } }
  ];

  function offlineEffAt(l){ return clamp(OFFLINE_BASE + 0.1 * l, 0, 1); }

  function upCost(u) {
    var lv = upLv(u.id);
    return Math.ceil(u.base * Math.pow(u.growth, lv));
  }

  /* =========================================================
     成就定义
     ========================================================= */
  var ACHS = [
    { id:'a01', name:'初识反物质', desc:'累计持有 10 反物质',        test:function(s){ return s.stats.bestAm >= 10; } },
    { id:'a02', name:'小有规模',   desc:'累计持有 1,000 反物质',     test:function(s){ return s.stats.bestAm >= 1e3; } },
    { id:'a03', name:'百万级',     desc:'累计持有 1e6 反物质',       test:function(s){ return s.stats.bestAm >= 1e6; } },
    { id:'a04', name:'十亿级',     desc:'累计持有 1e9 反物质',       test:function(s){ return s.stats.bestAm >= 1e9; } },
    { id:'a05', name:'天文数字',   desc:'累计持有 1e12 反物质',      test:function(s){ return s.stats.bestAm >= 1e12; } },
    { id:'a06', name:'深空深处',   desc:'累计持有 1e20 反物质',      test:function(s){ return s.stats.bestAm >= 1e20; } },
    { id:'a07', name:'第一桶金',   desc:'购买 10 个第一维度',        test:function(s){ return s.dims[0].b >= 10; } },
    { id:'a08', name:'流水线',     desc:'购买 30 个第一维度',        test:function(s){ return s.dims[0].b >= 30; } },
    { id:'a09', name:'八维齐开',   desc:'八个维度全部至少 1 个',     test:function(s){ var i; for (i=0;i<DIM_COUNT;i++) if (s.dims[i].b < 1) return false; return true; } },
    { id:'a10', name:'均衡发展',   desc:'八个维度全部至少 8 个',     test:function(s){ var i; for (i=0;i<DIM_COUNT;i++) if (s.dims[i].b < 8) return false; return true; } },
    { id:'a11', name:'维度工厂',   desc:'维度购买总数达到 100',      test:function(s){ return totalBought(s) >= 100; } },
    { id:'a12', name:'维度工厂 II',desc:'维度购买总数达到 300',      test:function(s){ return totalBought(s) >= 300; } },
    { id:'a13', name:'第一次提升', desc:'完成 1 次维度提升',         test:function(s){ return s.boosts >= 1; } },
    { id:'a14', name:'提升狂热',   desc:'单局完成 5 次维度提升',     test:function(s){ return s.stats.maxBoosts >= 5; } },
    { id:'a15', name:'崩塌',       desc:'完成 1 次维度重置',         test:function(s){ return s.resets >= 1; } },
    { id:'a16', name:'轮回',       desc:'完成 10 次维度重置',        test:function(s){ return s.resets >= 10; } },
    { id:'a17', name:'点数积累',   desc:'累计获得 10 维度点数',      test:function(s){ return s.totalDP >= 10; } },
    { id:'a18', name:'点数巨富',   desc:'累计获得 500 维度点数',     test:function(s){ return s.totalDP >= 500; } },
    { id:'a19', name:'第一次升级', desc:'购买任意永久升级',          test:function(s){ return s.stats.upBought >= 1; } },
    { id:'a20', name:'升级成瘾',   desc:'累计购买 15 级升级',        test:function(s){ return s.stats.upBought >= 15; } },
    { id:'a21', name:'产能爆炸',   desc:'产出速度达到 1e6 /秒',      test:function(s){ return s.stats.bestRate >= 1e6; } },
    { id:'a22', name:'时间沉淀',   desc:'累计游玩 10 分钟',          test:function(s){ return s.stats.playTime >= 600; } },
    { id:'a23', name:'离线也疯狂', desc:'单次离线收益 ≥ 1e6',        test:function(s){ return s.stats.offlineBest >= 1e6; } },
    { id:'a24', name:'收藏家',     desc:'解锁 12 个成就',            test:function(s){ return Object.keys(s.ach).length >= 12; } }
  ];

  function totalBought(s) {
    var t = 0;
    for (var i = 0; i < DIM_COUNT; i++) t += s.dims[i].b;
    return t;
  }

  /* =========================================================
     操作
     ========================================================= */
  function buyDim(i, amount) {
    var k = amount === 'max' ? maxBuy(i, S.am) : amount;
    if (k <= 0) return;
    var c = costFor(i, k);
    if (c > S.am) return;
    S.am -= c;
    S.dims[i].b += k;
    if (S.am < 0) S.am = 0;
  }

  function buyBoost() {
    var c = boostCost();
    if (S.am < c) return;
    S.am -= c;
    S.boosts++;
    if (S.boosts > S.stats.maxBoosts) S.stats.maxBoosts = S.boosts;
    toast('维度提升 ×' + fmt(boostEffect()) + '（共 ' + S.boosts + ' 次）', 'gold');
  }

  function doReset() {
    var gain = prestigeGain();
    if (gain <= 0) return;
    S.resets++;
    S.dp += gain;
    S.totalDP += gain;
    S.am = startAm();
    for (var i = 0; i < DIM_COUNT; i++) { S.dims[i].b = 0; S.dims[i].f = 0; }
    S.boosts = 0;
    S.stats.runStart = Date.now();
    S.stats.maxBoosts = 0;
    toast('维度重置完成，获得 ' + fmtInt(gain) + ' 维度点数', 'gold');
    save();
    checkAch();
    render();
  }

  function buyUpgrade(id) {
    var u = null;
    for (var i = 0; i < UPGRADES.length; i++) if (UPGRADES[i].id === id) u = UPGRADES[i];
    if (!u) return;
    var lv = upLv(id);
    if (lv >= u.max) return;
    var c = upCost(u);
    if (S.dp < c) return;
    S.dp -= c;
    S.up[id] = lv + 1;
    S.stats.upBought++;
    toast('已购买：' + u.name + ' Lv.' + (lv + 1), 'ok');
    checkAch();
    render();
  }

  /* =========================================================
     循环
     ========================================================= */
  function tick(dt) {
    if (dt <= 0) return;

    // 反物质产出
    S.am += amRate() * dt;
    if (S.am > MAX_AM) S.am = MAX_AM;

    // 维度链：高层维度生产下一层
    for (var i = DIM_COUNT - 1; i >= 1; i--) {
      var out = dimCount(i) * HIGH_PROD * dt;
      var f = S.dims[i - 1].f + out;
      S.dims[i - 1].f = f > MAX_AM ? MAX_AM : f;
    }

    // 统计
    S.stats.playTime += dt;
    if (S.am > S.stats.bestAm) S.stats.bestAm = S.am;
    S.stats.totalAm += amRate() * dt;
    var r = amRate();
    if (r > S.stats.bestRate) S.stats.bestRate = r;

    // 自动购买
    if (upLv('auto')) {
      autoTimer += dt;
      if (autoTimer >= 0.25) {
        autoTimer = 0;
        for (var g = 0; g < 40; g++) {
          var idx = -1;
          for (var j = DIM_COUNT - 1; j >= 0; j--) {
            if (S.am >= dimCost(j)) { idx = j; break; }
          }
          if (idx < 0) break;
          buyDim(idx, 1);
        }
      }
    }

    checkAch();
  }

  function applyOffline(seconds) {
    seconds = Math.min(seconds, OFFLINE_CAP);
    if (seconds < 30) { tick(seconds); return; }
    var eff = offlineEff();
    var gain = amRate() * seconds * eff;
    if (gain > 0) {
      S.am += gain;
      if (S.am > MAX_AM) S.am = MAX_AM;
      S.stats.totalAm += gain;
      if (gain > S.stats.offlineBest) S.stats.offlineBest = gain;
      if (S.am > S.stats.bestAm) S.stats.bestAm = S.am;
      toast('离线 ' + dur(seconds) + ' · 效率 ' + Math.round(eff * 100) + '% · 获得 ' + fmt(gain) + ' 反物质', 'gold');
    }
    checkAch();
  }

  function loop() {
    var now = Date.now();
    var dt = (now - lastTick) / 1000;
    lastTick = now;
    if (!isFinite(dt) || dt < 0) dt = 0;

    if (dt > 60) {
      applyOffline(dt);          // 切后台很久 / 电脑睡眠
    } else {
      tick(dt);                  // 正常推进（最小化时也会按真实时间补算）
    }

    render();

    saveTimer += dt * 1000;
    if (saveTimer >= SAVE_EVERY) { saveTimer = 0; save(); }
  }

  /* =========================================================
     成就检查
     ========================================================= */
  function checkAch() {
    for (var i = 0; i < ACHS.length; i++) {
      var a = ACHS[i];
      if (S.ach[a.id]) continue;
      var ok = false;
      try { ok = a.test(S); } catch (e) { ok = false; }
      if (ok) {
        S.ach[a.id] = 1;
        toast('成就解锁：' + a.name + '  +' + (achBonus() * 100).toFixed(0) + '% 产能', 'ok');
        var dot = document.getElementById('achDot');
        if (dot) dot.style.display = '';
      }
    }
  }

  /* =========================================================
     DOM 构建
     ========================================================= */
  var el = {};
  var dimCards = [];
  var upRows = [];
  var achTiles = [];

  function h(tag, cls, html) {
    var e = document.createElement(tag);
    if (cls) e.className = cls;
    if (html !== undefined) e.innerHTML = html;
    return e;
  }

  function buildDims() {
    var grid = document.getElementById('dimGrid');
    grid.innerHTML = '';
    var names = ['第一维度','第二维度','第三维度','第四维度','第五维度','第六维度','第七维度','第八维度'];
    for (var i = 0; i < DIM_COUNT; i++) {
      var card = h('article', 'dim');
      card.innerHTML =
        '<div class="dim-top">' +
          '<span class="dim-tag">D' + (i + 1) + '</span>' +
          '<span class="dim-title">' + names[i] + '</span>' +
          '<span class="dim-count mono"><b data-c>0</b><em data-f></em></span>' +
        '</div>' +
        '<div class="dim-info">' +
          '<span class="out" data-out>+0 /秒</span>' +
          '<span data-sub>未解锁</span>' +
        '</div>' +
        '<button class="buy" data-buy><span data-lbl>购买 ×1</span><b data-cost>—</b></button>' +
        '<div class="bar"><i data-bar></i></div>';
      (function (idx) {
        card.querySelector('[data-buy]').addEventListener('click', function () {
          buyDim(idx, buyAmt);
          render();
        });
      })(i);
      grid.appendChild(card);
      dimCards.push({
        root: card,
        c: card.querySelector('[data-c]'),
        f: card.querySelector('[data-f]'),
        out: card.querySelector('[data-out]'),
        sub: card.querySelector('[data-sub]'),
        lbl: card.querySelector('[data-lbl]'),
        cost: card.querySelector('[data-cost]'),
        bar: card.querySelector('[data-bar]'),
        btn: card.querySelector('[data-buy]')
      });
    }
  }

  function buildUps() {
    var list = document.getElementById('upList');
    list.innerHTML = '';
    UPGRADES.forEach(function (u) {
      var row = h('div', 'up');
      row.innerHTML =
        '<div class="up-body">' +
          '<div class="up-name">' + u.name + ' <span class="lvl" data-lvl>Lv.0</span></div>' +
          '<div class="up-desc">' + u.desc + ' · <span data-eff></span></div>' +
        '</div>' +
        '<button class="up-buy" data-btn><b data-price>1</b> DP</button>';
      row.querySelector('[data-btn]').addEventListener('click', function () {
        buyUpgrade(u.id);
      });
      list.appendChild(row);
      upRows.push({
        u: u,
        root: row,
        lvl: row.querySelector('[data-lvl]'),
        eff: row.querySelector('[data-eff]'),
        btn: row.querySelector('[data-btn]'),
        price: row.querySelector('[data-price]')
      });
    });
  }

  function buildAchs() {
    var grid = document.getElementById('achGrid');
    grid.innerHTML = '';
    ACHS.forEach(function (a) {
      var t = h('div', 'ach');
      t.innerHTML = '<div class="ach-t"><span class="tick" data-tick>◇</span>' + a.name + '</div>' +
                    '<div class="ach-d">' + a.desc + '</div>';
      grid.appendChild(t);
      achTiles.push({ a: a, root: t, tick: t.querySelector('[data-tick]') });
    });
  }

  function buildStats() {
    var box = document.getElementById('statsBox');
    var items = [
      ['本局时长', 'statRun'],
      ['总游玩时长', 'statPlay'],
      ['历史最高反物质', 'statBest'],
      ['历史总产出', 'statTotal'],
      ['单局最多提升', 'statBoost'],
      ['离线效率', 'statOff']
    ];
    box.innerHTML = '';
    items.forEach(function (it) {
      var d = h('div', 'stat');
      d.innerHTML = '<label>' + it[0] + '</label><b class="mono" id="' + it[1] + '">-</b>';
      box.appendChild(d);
      el[it[1]] = d.querySelector('b');
    });
  }

  /* =========================================================
     渲染
     ========================================================= */
  function render() {
    var mult = globalMult();
    var rate = amRate();

    el.amVal.textContent = fmt(S.am);
    el.rateVal.textContent = fmt(rate);
    el.dpVal.textContent = fmtInt(S.dp);
    el.multVal.textContent = fmt(mult) + '×';

    // 维度卡
    for (var i = 0; i < DIM_COUNT; i++) {
      var c = dimCards[i];
      var unlocked = i === 0 || S.dims[i].b >= 1 || S.dims[i - 1].b > 0;
      var k = buyAmt === 'max' ? maxBuy(i, S.am) : buyAmt;
      var cost = k > 0 ? costFor(i, k) : dimCost(i);
      var can = k > 0 && S.am >= cost;

      c.c.textContent = fmt(dimCount(i));
      c.f.textContent = S.dims[i].f >= 0.01 ? '(+' + fmt(S.dims[i].f) + ')' : '';
      c.out.textContent = '+' + fmt(dimOutput(i)) + (i === 0 ? ' 反物质/秒' : ' D' + i + '/秒');
      c.sub.textContent = '已购 ' + fmtInt(S.dims[i].b);

      if (can) {
        c.lbl.textContent = '购买 ×' + (buyAmt === 'max' ? Math.max(1, k) : buyAmt);
        c.cost.textContent = fmt(cost);
        c.btn.classList.add('can');
      } else {
        c.lbl.textContent = '购买 ×' + (buyAmt === 'max' ? 1 : buyAmt);
        c.cost.textContent = fmt(dimCost(i));
        c.btn.classList.remove('can');
      }
      c.root.classList.toggle('can', can);
      c.root.style.opacity = unlocked || S.dims[i].b > 0 ? '1' : '0.6';

      var pct = clamp(S.am / dimCost(i), 0, 1) * 100;
      c.bar.style.width = pct.toFixed(1) + '%';
    }

    // 重置
    var gain = prestigeGain();
    el.gainVal.textContent = fmtInt(gain);
    el.totalDpVal.textContent = fmtInt(S.totalDP);
    el.resetVal.textContent = fmtInt(S.resets);
    el.resetBtn.className = 'btn gold' + (gain > 0 ? ' can' : '');
    el.resetBtn.textContent = gain > 0 ? ('维度重置 · +' + fmtInt(gain) + ' DP') : ('维度重置（需 ' + fmt(RESET_REQ) + ' 反物质）');

    // 提升
    var bc = boostCost();
    el.boostCost.textContent = fmt(bc);
    el.boostCount.textContent = fmtInt(S.boosts);
    el.boostEffTxt.textContent = '×' + (BOOST_EFFECT_BASE + upLv('boost'));
    el.boostBtn.className = 'btn' + (S.am >= bc ? ' can' : '');

    // 升级
    upRows.forEach(function (r) {
      var lv = upLv(r.u.id);
      var maxed = lv >= r.u.max;
      var c = upCost(r.u);
      r.lvl.textContent = maxed ? 'MAX' : ('Lv.' + lv + ' / ' + r.u.max);
      r.eff.textContent = r.u.effect(lv);
      r.root.classList.toggle('maxed', maxed);
      if (maxed) {
        r.btn.textContent = '已满级';
        r.btn.disabled = true;
        r.btn.classList.remove('can');
        r.root.classList.remove('can');
      } else {
        r.btn.textContent = fmtInt(c) + ' DP';
        r.btn.disabled = false;
        var ok = S.dp >= c;
        r.btn.classList.toggle('can', ok);
        r.root.classList.toggle('can', ok);
      }
    });

    // 成就
    el.achCount.textContent = achCount() + ' / ' + ACHS.length;
    el.achBar.style.width = (achCount() / ACHS.length * 100).toFixed(1) + '%';
    el.achBonusTxt.textContent = '+' + (achBonus() * 100).toFixed(0) + '%';
    achTiles.forEach(function (t) {
      var got = !!S.ach[t.a.id];
      t.root.classList.toggle('got', got);
      t.tick.textContent = got ? '✔' : '◇';
    });

    // 统计
    if (el.statRun) {
      el.statRun.textContent = dur((Date.now() - S.stats.runStart) / 1000);
      el.statPlay.textContent = dur(S.stats.playTime);
      el.statBest.textContent = fmt(S.stats.bestAm);
      el.statTotal.textContent = fmt(S.stats.totalAm);
      el.statBoost.textContent = fmtInt(S.stats.maxBoosts);
      el.statOff.textContent = Math.round(offlineEff() * 100) + '%';
    }

    // 成就页红点：有新解锁且还没看过成就页时显示
    var dot = document.getElementById('achDot');
    if (dot) dot.style.display = (achCount() > 0 && !dot.dataset.seen) ? '' : 'none';
  }

  /* =========================================================
     Toast
     ========================================================= */
  function toast(msg, kind) {
    var box = document.getElementById('toasts');
    var t = h('div', 'toast' + (kind ? ' ' + kind : ''), msg);
    box.appendChild(t);
    setTimeout(function () {
      t.classList.add('out');
      setTimeout(function () { if (t.parentNode) t.parentNode.removeChild(t); }, 320);
    }, 2600);
    while (box.children.length > 4) box.removeChild(box.firstChild);
  }

  /* =========================================================
     存档
     ========================================================= */
  function save() {
    S.lastSave = Date.now();
    try {
      localStorage.setItem(SAVE_KEY, b64encode(JSON.stringify(S)));
    } catch (e) { /* 隐私模式下可能失败，忽略 */ }
  }

  function b64encode(str) {
    return btoa(encodeURIComponent(str).replace(/%([0-9A-F]{2})/g, function (m, p) {
      return String.fromCharCode('0x' + p);
    }));
  }
  function b64decode(str) {
    return decodeURIComponent(Array.prototype.map.call(atob(str), function (c) {
      return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
    }).join(''));
  }

  function applySave(data) {
    S = newState();
    if (data && typeof data === 'object') {
      if (typeof data.am === 'number' && isFinite(data.am)) S.am = Math.max(0, data.am);
      if (Array.isArray(data.dims)) {
        for (var i = 0; i < DIM_COUNT; i++) {
          var d = data.dims[i];
          if (d && typeof d === 'object') {
            S.dims[i].b = Math.max(0, Math.floor(d.b || 0));
            S.dims[i].f = Math.max(0, Number(d.f) || 0);
          }
        }
      }
      S.boosts = Math.max(0, Math.floor(data.boosts || 0));
      S.dp = Math.max(0, Math.floor(data.dp || 0));
      S.totalDP = Math.max(0, Math.floor(data.totalDP || 0));
      S.resets = Math.max(0, Math.floor(data.resets || 0));
      if (data.up && typeof data.up === 'object') {
        for (var k in data.up) if (Object.prototype.hasOwnProperty.call(data.up, k)) {
          S.up[k] = Math.max(0, Math.floor(data.up[k] || 0));
        }
      }
      if (data.ach && typeof data.ach === 'object') {
        for (var a in data.ach) if (data.ach[a]) S.ach[a] = 1;
      }
      if (data.stats && typeof data.stats === 'object') {
        for (var s in S.stats) {
          if (typeof data.stats[s] === 'number' && isFinite(data.stats[s])) S.stats[s] = data.stats[s];
        }
        if (!data.stats.runStart) S.stats.runStart = Date.now();
      }
      if (data.lastSave) S.lastSave = data.lastSave;
    }
    lastTick = Date.now();
    checkAch();
    render();
  }

  function load() {
    var raw = null;
    try { raw = localStorage.getItem(SAVE_KEY); } catch (e) { raw = null; }
    if (!raw) return false;
    try {
      var json = raw.charAt(0) === '{' ? raw : b64decode(raw);
      var data = JSON.parse(json);
      var elapsed = (Date.now() - (data.lastSave || Date.now())) / 1000;
      applySave(data);
      if (elapsed > 60) applyOffline(elapsed);
      return true;
    } catch (e) {
      console.warn('存档读取失败', e);
      return false;
    }
  }

  function hardReset() {
    try { localStorage.removeItem(SAVE_KEY); } catch (e) {}
    S = newState();
    lastTick = Date.now();
    render();
    toast('已清空全部进度', 'gold');
  }

  /* =========================================================
     事件绑定
     ========================================================= */
  function bind() {
    var tabs = document.querySelectorAll('#tabs .tab');
    Array.prototype.forEach.call(tabs, function (t) {
      t.addEventListener('click', function () {
        Array.prototype.forEach.call(tabs, function (x) { x.classList.remove('on'); });
        t.classList.add('on');
        var panels = document.querySelectorAll('.panel');
        Array.prototype.forEach.call(panels, function (p) { p.classList.remove('on'); });
        var target = document.getElementById('panel-' + t.dataset.tab);
        if (target) target.classList.add('on');
        if (t.dataset.tab === 'ach') {
          var dot = document.getElementById('achDot');
          if (dot) { dot.dataset.seen = '1'; dot.style.display = 'none'; }
        }
        render();
      });
    });

    var seg = document.querySelectorAll('#buySeg button');
    Array.prototype.forEach.call(seg, function (b) {
      b.addEventListener('click', function () {
        Array.prototype.forEach.call(seg, function (x) { x.classList.remove('on'); });
        b.classList.add('on');
        buyAmt = b.dataset.amt === 'max' ? 'max' : parseInt(b.dataset.amt, 10);
        render();
      });
    });

    el.resetBtn.addEventListener('click', doReset);
    el.boostBtn.addEventListener('click', function () { buyBoost(); render(); });

    document.getElementById('saveNow').addEventListener('click', function () {
      save(); toast('已保存到本地', 'ok');
    });

    document.getElementById('exportBtn').addEventListener('click', function () {
      save();
      var txt = b64encode(JSON.stringify(S));
      document.getElementById('saveArea').value = txt;
      toast('存档已生成，请复制文本框内容', 'ok');
    });

    document.getElementById('copyBtn').addEventListener('click', function () {
      var area = document.getElementById('saveArea');
      if (!area.value) area.value = b64encode(JSON.stringify(S));
      area.select();
      var done = function () { toast('已复制到剪贴板', 'ok'); };
      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(area.value).then(done, function () { toast('复制失败，请手动复制', 'gold'); });
      } else {
        try { document.execCommand('copy'); done(); } catch (e) { toast('复制失败，请手动复制', 'gold'); }
      }
    });

    document.getElementById('importBtn').addEventListener('click', function () {
      var area = document.getElementById('saveArea');
      var txt = (area.value || '').trim();
      if (!txt) { toast('请先粘贴存档字符串', 'gold'); return; }
      try {
        var json = txt.charAt(0) === '{' ? txt : b64decode(txt);
        var data = JSON.parse(json);
        if (!data || typeof data !== 'object' || !data.dims) throw new Error('格式不正确');
        applySave(data);
        save();
        toast('存档导入成功', 'ok');
        area.value = '';
      } catch (e) {
        toast('导入失败：存档格式不正确', 'gold');
      }
    });

    var wipe = document.getElementById('wipeBtn');
    var armed = false, armTimer = null;
    wipe.addEventListener('click', function () {
      if (!armed) {
        armed = true;
        wipe.textContent = '再次点击确认清空！';
        toast('再次点击「确认清空」将删除全部进度', 'gold');
        armTimer = setTimeout(function () {
          armed = false;
          wipe.textContent = '硬重置（清空全部进度）';
        }, 4000);
        return;
      }
      clearTimeout(armTimer);
      armed = false;
      wipe.textContent = '硬重置（清空全部进度）';
      hardReset();
    });

    window.addEventListener('beforeunload', save);
    document.addEventListener('visibilitychange', function () {
      if (document.hidden) save();
      else lastTick = Date.now();
    });

    el.highProdTxt = document.getElementById('highProdTxt');
    el.highProdTxt.textContent = HIGH_PROD;
  }

  /* =========================================================
     启动
     ========================================================= */
  function init() {
    el.amVal = document.getElementById('amVal');
    el.rateVal = document.getElementById('rateVal');
    el.dpVal = document.getElementById('dpVal');
    el.multVal = document.getElementById('multVal');
    el.gainVal = document.getElementById('gainVal');
    el.totalDpVal = document.getElementById('totalDpVal');
    el.resetVal = document.getElementById('resetVal');
    el.resetBtn = document.getElementById('resetBtn');
    el.boostBtn = document.getElementById('boostBtn');
    el.boostCost = document.getElementById('boostCost');
    el.boostCount = document.getElementById('boostCount');
    el.boostEffTxt = document.getElementById('boostEffTxt');
    el.achCount = document.getElementById('achCount');
    el.achBar = document.getElementById('achBar');
    el.achBonusTxt = document.getElementById('achBonusTxt');
    el.reqTxt = document.getElementById('reqTxt');
    el.reqTxt.textContent = fmt(RESET_REQ);

    buildDims();
    buildUps();
    buildAchs();
    buildStats();
    bind();

    var loadedOk = load();
    render();
    lastTick = Date.now();
    if (!loadedOk) toast('点击「第一维度」买入，开始产出反物质', 'ok');
    setInterval(loop, 50);
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();
</script>
</body>
</html>
