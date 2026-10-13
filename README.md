<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>منصة الكيمياء التفاعلية</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
:root{
  --primary:#6366f1;
  --primary-dark:#4f46e5;
  --secondary:#8b5cf6;
  --accent:#06d6a0;
  --cyan:#22d3ee;
  --pink:#ec4899;
  --dark:#0f172a;
  --success:#10b981;
  --warning:#f59e0b;
  --danger:#ef4444;
  --glass-border:rgba(255,255,255,0.18);
  --transition:all .35s cubic-bezier(.4,0,.2,1);
  --font-size:16px;
}

*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,sans-serif}
html{font-size:var(--font-size)}
body{background:#050b1e;color:#e2e8f0;overflow-x:hidden;position:relative;min-height:100vh}
body.no-anim *{animation:none !important;transition:none !important}

#bg-canvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-2;display:block}
.bg-gradient-overlay{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;background:radial-gradient(ellipse at 20% 10%,rgba(99,102,241,.35) 0%,transparent 55%),radial-gradient(ellipse at 80% 90%,rgba(139,92,246,.3) 0%,transparent 55%),radial-gradient(ellipse at 50% 50%,rgba(6,214,160,.08) 0%,transparent 70%);pointer-events:none}

/* ===== Light Theme ===== */
body.light{background:#f1f5f9;color:#0f172a}
body.light .bg-gradient-overlay{background:radial-gradient(ellipse at 20% 10%,rgba(99,102,241,.15) 0%,transparent 55%),radial-gradient(ellipse at 80% 90%,rgba(139,92,246,.12) 0%,transparent 55%)}
body.light #bg-canvas{opacity:.4}
body.light .sidebar,body.light .header,body.light .card,body.light .settings-container,body.light .quiz-setup,body.light .quiz-content,body.light .whiteboard-container,body.light .progress-history,body.light .stat-card,body.light .research-section,body.light .research-toc,body.light .notifications-container,body.light .profile-container,body.light .simulation-container,body.light .leaderboard-container,body.light .advanced-report{background:rgba(255,255,255,.85);border-color:rgba(0,0,0,.08);color:#0f172a}
body.light .header h2,body.light .card h3,body.light .settings-group h3,body.light .stat-card h3,body.light .welcome-screen h1,body.light .research-hero h1,body.light .research-section h3,body.light .subsection h4,body.light .element-info-title h2,body.light .profile-name,body.light .notification-title{color:#0f172a}
body.light .card p,body.light .welcome-screen p,body.light .setting-info p,body.light .stat-card h3,body.light .subsection p,body.light .subsection li,body.light .section-header-element p{color:#475569}
body.light .nav-link{color:#475569}
body.light .nav-link:hover,body.light .nav-link.active{color:#fff}
body.light .form-control,body.light select,body.light input,body.light textarea{background:rgba(0,0,0,.04);border-color:rgba(0,0,0,.1);color:#0f172a}
body.light .form-control::placeholder{color:#94a3b8}
body.light .option{background:rgba(0,0,0,.03);border-color:rgba(0,0,0,.08);color:#0f172a}
body.light .question{background:rgba(99,102,241,.06);color:#0f172a}
body.light .user-info{background:rgba(99,102,241,.05)}
body.light .leaderboard-item{background:rgba(0,0,0,.03)}
body.light .toc-link,body.light .team-member{background:rgba(0,0,0,.03)}
body.light .history-table th{background:rgba(0,0,0,.04);color:#0f172a}
body.light .history-table td{color:#334155;border-bottom-color:rgba(0,0,0,.05)}
body.light .info-box p,body.light .info-box{color:#334155}
body.light .comparison-table td{background:rgba(0,0,0,.02);color:#334155}
body.light .conclusion-box{color:#065f46}
body.light .references-list li{background:rgba(0,0,0,.03);color:#334155}

/* ===== Sidebar compact mode ===== */
body.compact .card{padding:16px}
body.compact .nav-link{padding:9px 14px}
body.compact .header{padding:12px 20px}

.main-content::-webkit-scrollbar{width:8px}
.main-content::-webkit-scrollbar-track{background:rgba(255,255,255,.03)}
.main-content::-webkit-scrollbar-thumb{background:linear-gradient(180deg,var(--primary),var(--secondary));border-radius:10px}

/* ===== Auth Screen ===== */
.auth-screen{display:none;min-height:100vh;align-items:center;justify-content:center;padding:20px;position:relative;z-index:10}
.auth-screen.active{display:flex}

.auth-container{background:rgba(15,23,42,.75);backdrop-filter:blur(28px);-webkit-backdrop-filter:blur(28px);border:1px solid var(--glass-border);border-radius:28px;padding:40px 36px;width:100%;max-width:520px;box-shadow:0 30px 80px rgba(0,0,0,.6),0 0 0 1px rgba(255,255,255,.05) inset;animation:authIn .7s cubic-bezier(.4,0,.2,1);position:relative;overflow:hidden}
body.light .auth-container{background:rgba(255,255,255,.92);border-color:rgba(0,0,0,.08)}
.auth-container::before{content:'';position:absolute;top:-50%;left:-50%;width:200%;height:200%;background:conic-gradient(from 0deg,transparent,rgba(99,102,241,.12),transparent 30%);animation:rotateConic 10s linear infinite;z-index:-1}
@keyframes rotateConic{to{transform:rotate(360deg)}}
@keyframes authIn{from{opacity:0;transform:translateY(30px) scale(.96)}to{opacity:1;transform:translateY(0) scale(1)}}

.auth-logo{width:96px;height:96px;margin:0 auto 20px;border-radius:50%;background:linear-gradient(135deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;font-size:2.6em;color:#fff;box-shadow:0 0 40px rgba(99,102,241,.6),0 0 80px rgba(139,92,246,.3);animation:pulseIcon 2.5s ease-in-out infinite;position:relative}
.auth-logo::before{content:'';position:absolute;inset:-15px;border-radius:50%;border:2px solid rgba(99,102,241,.3);animation:ringPulse 2.5s ease-out infinite}
@keyframes pulseIcon{0%,100%{transform:scale(1)}50%{transform:scale(1.05)}}
@keyframes ringPulse{0%{transform:scale(.85);opacity:1}100%{transform:scale(1.35);opacity:0}}

.auth-container h1{text-align:center;font-size:1.75em;margin-bottom:8px;background:linear-gradient(135deg,#fff,var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;font-weight:800}
body.light .auth-container h1{background:linear-gradient(135deg,#0f172a,var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent}
.auth-subtitle{text-align:center;color:#94a3b8;margin-bottom:26px;font-size:.92em}

.auth-tabs{display:flex;background:rgba(255,255,255,.04);border-radius:14px;padding:5px;margin-bottom:22px;gap:4px}
.auth-tab{flex:1;padding:11px;text-align:center;border-radius:10px;cursor:pointer;font-weight:600;color:#94a3b8;transition:var(--transition);font-size:.92em;border:none;background:transparent}
.auth-tab.active{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;box-shadow:0 6px 18px rgba(99,102,241,.4)}

.auth-panel{display:none;animation:fadeIn .35s ease}
.auth-panel.active{display:block}

.form-group{margin-bottom:16px;text-align:right}
.form-group label{display:block;margin-bottom:8px;color:#cbd5e1;font-weight:600;font-size:.88em}
body.light .form-group label{color:#475569}
.form-control{width:100%;padding:14px 16px;border:2px solid rgba(255,255,255,.1);border-radius:12px;font-size:.95em;background:rgba(255,255,255,.05);color:#fff;transition:var(--transition);font-family:inherit}
.form-control::placeholder{color:#64748b}
.form-control:focus{border-color:var(--primary);outline:none;background:rgba(255,255,255,.08);box-shadow:0 0 0 4px rgba(99,102,241,.2)}
.form-control option{background:#1e293b;color:#fff}

.input-with-icon{position:relative}
.input-with-icon .form-control{padding-left:44px}
.input-with-icon > i{position:absolute;left:16px;top:50%;transform:translateY(-50%);color:#64748b;pointer-events:none}
.pass-toggle{position:absolute;left:14px;top:50%;transform:translateY(-50%);background:none;border:none;color:#64748b;cursor:pointer;font-size:1em;padding:4px}
.pass-toggle:hover{color:var(--cyan)}

.btn{padding:12px 22px;background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;border:none;border-radius:12px;cursor:pointer;font-size:.93em;font-weight:700;transition:var(--transition);display:inline-flex;align-items:center;justify-content:center;gap:8px;box-shadow:0 6px 18px rgba(99,102,241,.35);position:relative;overflow:hidden;white-space:nowrap;font-family:inherit}
.btn:hover{transform:translateY(-2px);box-shadow:0 10px 24px rgba(99,102,241,.5)}
.btn:active{transform:translateY(0)}
.btn:disabled{opacity:.5;cursor:not-allowed;transform:none}
.btn-success{background:linear-gradient(135deg,var(--success),#059669);box-shadow:0 6px 18px rgba(16,185,129,.35)}
.btn-warning{background:linear-gradient(135deg,var(--warning),#d97706);box-shadow:0 6px 18px rgba(245,158,11,.35)}
.btn-danger{background:linear-gradient(135deg,var(--danger),#dc2626);box-shadow:0 6px 18px rgba(239,68,68,.35)}
.btn-cyan{background:linear-gradient(135deg,var(--cyan),#0891b2);box-shadow:0 6px 18px rgba(34,211,238,.35)}
.btn-outline{background:rgba(255,255,255,.05);border:1.5px solid rgba(255,255,255,.15);color:#e2e8f0;box-shadow:none}
.btn-outline:hover{background:rgba(255,255,255,.1);border-color:var(--primary);color:#fff}
body.light .btn-outline{background:rgba(0,0,0,.03);border-color:rgba(0,0,0,.12);color:#334155}
body.light .btn-outline:hover{background:rgba(99,102,241,.1);border-color:var(--primary);color:var(--primary)}

.auth-error{background:rgba(239,68,68,.1);color:#fca5a5;padding:10px 14px;border-radius:10px;margin-bottom:12px;font-size:.88em;border-right:3px solid var(--danger);display:none}
.auth-error.show{display:block}
.auth-success{background:rgba(16,185,129,.1);color:#6ee7b7;padding:10px 14px;border-radius:10px;margin-bottom:12px;font-size:.88em;border-right:3px solid var(--success);display:none}
.auth-success.show{display:block}
.auth-footer{text-align:center;margin-top:16px;font-size:.85em;color:#94a3b8}

/* ===== App Container ===== */
.app-container{display:none;min-height:100vh}
.app-container.active{display:flex}

/* ===== Sidebar ===== */
.sidebar{width:280px;background:rgba(15,23,42,.78);backdrop-filter:blur(24px);-webkit-backdrop-filter:blur(24px);border-left:1px solid var(--glass-border);padding:20px 0;position:sticky;top:0;height:100vh;overflow-y:auto;z-index:100;display:flex;flex-direction:column}
body.light .sidebar{background:rgba(255,255,255,.85);border-color:rgba(0,0,0,.06)}
.sidebar::-webkit-scrollbar{width:5px}
.sidebar::-webkit-scrollbar-thumb{background:rgba(99,102,241,.4);border-radius:10px}

.logo{text-align:center;padding:12px 20px 20px;border-bottom:1px solid rgba(255,255,255,.08);margin-bottom:16px}
.logo h1{font-size:1.35em;display:flex;align-items:center;justify-content:center;gap:10px;background:linear-gradient(135deg,var(--primary),var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;font-weight:800}
.logo i{font-size:1.6em;background:linear-gradient(135deg,var(--primary),var(--secondary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;animation:spinLogo 8s linear infinite}
@keyframes spinLogo{0%,100%{transform:rotate(0deg)}50%{transform:rotate(180deg)}}

.nav-menu{list-style:none;padding:0 14px;flex-shrink:0}
.nav-item{margin-bottom:4px}
.nav-link{display:flex;align-items:center;gap:14px;padding:12px 16px;color:#cbd5e1;text-decoration:none;border-radius:12px;transition:var(--transition);font-size:.92em;position:relative;overflow:hidden;font-weight:500}
.nav-link:hover{color:#fff;transform:translateX(-4px);background:rgba(255,255,255,.05)}
.nav-link.active{color:#fff;background:linear-gradient(135deg,rgba(99,102,241,.9),rgba(139,92,246,.9));box-shadow:0 8px 24px rgba(99,102,241,.4)}
.nav-link i{font-size:1.05em;width:22px;text-align:center}

.team-section{padding:16px 14px;border-top:1px solid rgba(255,255,255,.08);margin-top:12px}
.team-section h3{margin-bottom:12px;font-size:.85em;color:#94a3b8;display:flex;align-items:center;gap:8px;padding:0 6px;font-weight:600;letter-spacing:.5px;text-transform:uppercase}
.team-section h3 i{color:var(--primary)}

.team-member{display:flex;align-items:center;gap:12px;padding:11px 12px;margin-bottom:6px;border-radius:12px;transition:var(--transition);background:linear-gradient(135deg,rgba(251,191,36,.15),rgba(245,158,11,.1));border:1px solid rgba(251,191,36,.2)}
.team-member:hover{transform:translateX(-3px);border-color:rgba(251,191,36,.4)}
.team-avatar{width:38px;height:38px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:.95em;color:#fff;flex-shrink:0;background:linear-gradient(135deg,#fbbf24,#f59e0b);box-shadow:0 4px 12px rgba(251,191,36,.4)}
.team-member-info{flex:1;min-width:0}
.team-member-name{font-size:.88em;font-weight:700;color:#fbbf24;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.team-member-role{font-size:.72em;color:#94a3b8;margin-top:2px}

.user-section{padding:16px;border-top:1px solid rgba(255,255,255,.08);margin-top:auto}
.user-info{display:flex;align-items:center;gap:12px;padding:12px;background:rgba(255,255,255,.04);border-radius:14px;border:1px solid rgba(255,255,255,.06)}
.user-avatar{width:46px;height:46px;border-radius:50%;background:linear-gradient(135deg,var(--accent),var(--cyan));display:flex;align-items:center;justify-content:center;font-size:1.3em;color:#fff;flex-shrink:0;box-shadow:0 4px 12px rgba(6,214,160,.35);overflow:hidden}
.user-avatar img{width:100%;height:100%;object-fit:cover}
.user-info h4{font-size:.9em;color:#f1f5f9;margin-bottom:3px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.student-info p{font-size:.75em;color:#94a3b8;margin-bottom:2px}
.logout-btn{margin-top:10px;width:100%;padding:10px;background:rgba(239,68,68,.12);color:#fca5a5;border:1px solid rgba(239,68,68,.25);border-radius:10px;cursor:pointer;font-size:.85em;font-weight:600;transition:var(--transition);display:flex;align-items:center;justify-content:center;gap:8px;font-family:inherit}
.logout-btn:hover{background:rgba(239,68,68,.3);color:#fff;transform:translateY(-2px)}

/* ===== Main Content ===== */
.main-content{flex:1;padding:20px;overflow-y:auto;height:100vh;position:relative}

.header{background:rgba(15,23,42,.68);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:16px 24px;margin-bottom:20px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:14px;box-shadow:0 8px 32px rgba(0,0,0,.25)}
.header h2{color:#f1f5f9;display:flex;align-items:center;gap:14px;font-size:1.2em;font-weight:700}
.header h2 i{background:linear-gradient(135deg,var(--primary),var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;font-size:1.25em}
.user-actions{display:flex;gap:10px;flex-wrap:wrap}

.screen{display:none}
.screen.active{display:block;animation:fadeIn .5s ease}
@keyframes fadeIn{from{opacity:0;transform:translateY(15px)}to{opacity:1;transform:translateY(0)}}

/* ===== Dashboard ===== */
.dashboard{padding:8px 0}
.dashboard-hero{background:linear-gradient(135deg,rgba(99,102,241,.25),rgba(139,92,246,.2));border:1px solid rgba(99,102,241,.3);border-radius:22px;padding:30px;margin-bottom:22px;position:relative;overflow:hidden;backdrop-filter:blur(20px)}
.dashboard-hero::before{content:'';position:absolute;top:-50%;right:-30%;width:400px;height:400px;background:radial-gradient(circle,rgba(34,211,238,.2) 0%,transparent 70%);animation:rotateSlow 25s linear infinite;pointer-events:none}
@keyframes rotateSlow{to{transform:rotate(360deg)}}

.greeting{display:flex;align-items:center;gap:20px;flex-wrap:wrap;position:relative;z-index:1}
.greeting-avatar{width:72px;height:72px;border-radius:50%;background:linear-gradient(135deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;font-size:1.8em;color:#fff;flex-shrink:0;box-shadow:0 10px 30px rgba(99,102,241,.5);overflow:hidden}
.greeting-avatar img{width:100%;height:100%;object-fit:cover}
.greeting-text h1{font-size:1.5em;font-weight:800;margin-bottom:6px;color:#f1f5f9}
.greeting-text h1 span{background:linear-gradient(135deg,var(--cyan),var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent}
.greeting-text p{color:#94a3b8;font-size:.9em}

.quick-stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:12px;margin-top:20px;position:relative;z-index:1}
.quick-stat{background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.1);border-radius:14px;padding:14px;text-align:center;transition:var(--transition)}
.quick-stat:hover{transform:translateY(-3px);background:rgba(255,255,255,.1)}
.quick-stat-value{font-size:1.5em;font-weight:800;background:linear-gradient(135deg,var(--cyan),var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:4px}
.quick-stat-label{font-size:.78em;color:#94a3b8}

.section-title{display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;flex-wrap:wrap;gap:10px}
.section-title h2{color:#f1f5f9;font-size:1.15em;display:flex;align-items:center;gap:10px;font-weight:700}
.section-title h2 i{color:var(--cyan)}
.section-title a{color:var(--cyan);text-decoration:none;font-size:.85em;cursor:pointer}
.section-title a:hover{text-decoration:underline}

.cards-container{display:grid;grid-template-columns:repeat(auto-fill,minmax(270px,1fr));gap:18px;margin-bottom:26px}
.card{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:18px;padding:22px 20px;transition:var(--transition);cursor:pointer;position:relative;overflow:hidden}
.card::before{content:'';position:absolute;top:0;left:0;right:0;height:4px;background:linear-gradient(90deg,var(--primary),var(--cyan),var(--secondary));transform:scaleX(0);transform-origin:right;transition:transform .4s}
.card:hover::before{transform:scaleX(1);transform-origin:left}
.card:hover{transform:translateY(-6px);border-color:rgba(99,102,241,.4);box-shadow:0 20px 50px rgba(99,102,241,.25)}

.card-header{display:flex;align-items:center;gap:14px;margin-bottom:14px}
.card-icon{width:52px;height:52px;border-radius:14px;display:flex;align-items:center;justify-content:center;font-size:1.5em;color:#fff;flex-shrink:0;box-shadow:0 8px 20px rgba(0,0,0,.3)}
.card h3{color:#f1f5f9;font-size:1.05em;font-weight:700}
.card p{color:#94a3b8;line-height:1.7;margin-bottom:16px;font-size:.88em}
.card .btn{width:100%;padding:10px;font-size:.88em}

.card-1 .card-icon{background:linear-gradient(135deg,#6366f1,#8b5cf6)}
.card-2 .card-icon{background:linear-gradient(135deg,#06d6a0,#0891b2)}
.card-3 .card-icon{background:linear-gradient(135deg,#f59e0b,#dc2626)}
.card-4 .card-icon{background:linear-gradient(135deg,#ec4899,#8b5cf6)}
.card-5 .card-icon{background:linear-gradient(135deg,#0ea5e9,#6366f1)}

/* ===== Settings ===== */
.settings-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:26px}
.settings-nav{display:flex;gap:6px;flex-wrap:wrap;padding:6px;background:rgba(255,255,255,.04);border-radius:14px;margin-bottom:22px;overflow-x:auto}
.settings-nav-btn{padding:10px 18px;background:transparent;border:none;color:#94a3b8;border-radius:10px;cursor:pointer;font-size:.88em;font-weight:600;font-family:inherit;transition:var(--transition);white-space:nowrap;display:flex;align-items:center;gap:8px}
.settings-nav-btn:hover{color:#fff;background:rgba(255,255,255,.05)}
.settings-nav-btn.active{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;box-shadow:0 6px 18px rgba(99,102,241,.4)}

.settings-panel{display:none;animation:fadeIn .35s ease}
.settings-panel.active{display:block}

.settings-group{margin-bottom:26px}
.settings-group:last-child{margin-bottom:0}
.settings-group h3{color:#e2e8f0;margin-bottom:16px;padding-bottom:12px;border-bottom:1px solid rgba(255,255,255,.08);display:flex;align-items:center;gap:10px;font-size:1em;font-weight:700}
.settings-group h3 i{color:var(--cyan)}

.setting-item{display:flex;justify-content:space-between;align-items:center;gap:16px;padding:14px 0;border-bottom:1px solid rgba(255,255,255,.05);flex-wrap:wrap}
.setting-item:last-child{border-bottom:none}
.setting-info{flex:1;min-width:200px}
.setting-info h4{color:#e2e8f0;margin-bottom:4px;font-size:.92em;font-weight:600}
.setting-info p{color:#94a3b8;font-size:.8em;line-height:1.5}

select,input[type="number"],input[type="text"],input[type="email"],input[type="password"],textarea{padding:10px 14px;border:1.5px solid rgba(255,255,255,.12);border-radius:11px;font-size:.88em;background:rgba(255,255,255,.05);color:#e2e8f0;transition:var(--transition);font-family:inherit;min-width:120px}
select:focus,input:focus,textarea:focus{border-color:var(--primary);outline:none;background:rgba(255,255,255,.08);box-shadow:0 0 0 3px rgba(99,102,241,.2)}
select option{background:#1e293b;color:#e2e8f0}

.toggle-switch{position:relative;display:inline-block;width:50px;height:26px;flex-shrink:0}
.toggle-switch input{opacity:0;width:0;height:0}
.slider{position:absolute;cursor:pointer;inset:0;background:rgba(255,255,255,.15);transition:.3s;border-radius:34px}
.slider:before{content:'';position:absolute;height:18px;width:18px;left:4px;bottom:4px;background:#fff;transition:.3s;border-radius:50%}
input:checked + .slider{background:linear-gradient(135deg,var(--success),#059669)}
input:checked + .slider:before{transform:translateX(24px)}

/* Theme picker */
.theme-picker{display:flex;gap:10px;flex-wrap:wrap}
.theme-color{width:36px;height:36px;border-radius:50%;cursor:pointer;border:3px solid transparent;transition:var(--transition);position:relative}
.theme-color:hover{transform:scale(1.1)}
.theme-color.active{border-color:#fff;box-shadow:0 0 20px currentColor}

.font-size-picker{display:flex;gap:8px}
.font-size-btn{width:38px;height:38px;border-radius:10px;background:rgba(255,255,255,.06);border:1.5px solid rgba(255,255,255,.1);color:#e2e8f0;cursor:pointer;font-family:inherit;font-weight:700;transition:var(--transition)}
.font-size-btn:hover{background:rgba(255,255,255,.1)}
.font-size-btn.active{background:linear-gradient(135deg,var(--primary),var(--secondary));border-color:transparent;color:#fff}

.avatar-picker{display:flex;gap:10px;flex-wrap:wrap}
.avatar-opt{width:52px;height:52px;border-radius:50%;cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:1.4em;color:#fff;transition:var(--transition);border:3px solid transparent}
.avatar-opt:hover{transform:scale(1.08)}
.avatar-opt.active{border-color:#fff;box-shadow:0 0 20px rgba(255,255,255,.5)}

/* ===== Quiz ===== */
.quiz-setup{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:30px;max-width:640px;margin:0 auto}
.quiz-setup h3{text-align:center;margin-bottom:24px;color:#f1f5f9;font-size:1.1em}
.quiz-options{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:24px}
.quiz-option{padding:20px 16px;border:2px solid rgba(255,255,255,.1);border-radius:16px;cursor:pointer;transition:var(--transition);text-align:center;background:rgba(255,255,255,.03)}
.quiz-option i{font-size:1.8em;margin-bottom:10px;color:var(--primary);display:block;transition:var(--transition)}
.quiz-option h4{color:#e2e8f0;margin-bottom:6px;font-size:.95em}
.quiz-option p{color:#94a3b8;font-size:.78em;line-height:1.5}
.quiz-option:hover{border-color:var(--primary);transform:translateY(-4px);background:rgba(99,102,241,.1)}
.quiz-option.selected{border-color:var(--primary);background:linear-gradient(135deg,rgba(99,102,241,.2),rgba(139,92,246,.2));box-shadow:0 10px 30px rgba(99,102,241,.25)}

.quiz-controls{display:flex;gap:10px;margin-top:20px;flex-wrap:wrap}
.quiz-controls .btn{flex:1;min-width:130px}

.quiz-container{display:flex;gap:18px;flex-wrap:wrap}
.quiz-content,.whiteboard-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:24px}
.quiz-content{flex:1;min-width:320px}
.whiteboard-container{flex:1;min-width:320px;display:flex;flex-direction:column}
.whiteboard-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;padding-bottom:12px;border-bottom:1px solid rgba(255,255,255,.08)}
.whiteboard-header h3{color:#e2e8f0;font-size:.95em;display:flex;align-items:center;gap:8px}
.whiteboard-frame{width:100%;height:460px;border:none;border-radius:14px;background:#fff}
.whiteboard-info{margin-top:12px;text-align:center;font-size:.78em;color:#94a3b8}

.quiz-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:18px;padding-bottom:14px;border-bottom:1px solid rgba(255,255,255,.08);gap:12px;flex-wrap:wrap}
.quiz-info{display:flex;gap:16px;font-size:.85em;color:#cbd5e1;flex-wrap:wrap}
.quiz-timer{background:linear-gradient(135deg,var(--warning),#d97706);color:#fff;padding:8px 14px;border-radius:12px;font-weight:700;font-size:.85em;box-shadow:0 4px 14px rgba(245,158,11,.4);display:flex;align-items:center;gap:8px}

.progress-bar{height:8px;background:rgba(255,255,255,.08);border-radius:6px;overflow:hidden}
.progress{height:100%;background:linear-gradient(90deg,var(--primary),var(--cyan));width:0%;transition:width .5s ease;border-radius:6px;box-shadow:0 0 12px rgba(99,102,241,.6)}

.question{font-size:1.1em;margin:18px 0;color:#f1f5f9;line-height:1.75;padding:20px;background:rgba(99,102,241,.08);border-radius:14px;border-right:4px solid var(--primary)}
.options{display:grid;gap:10px;margin-bottom:20px}
.option{padding:14px 18px;background:rgba(255,255,255,.04);border:1.5px solid rgba(255,255,255,.1);border-radius:13px;cursor:pointer;transition:var(--transition);display:flex;align-items:center;justify-content:space-between;color:#e2e8f0;font-size:.92em}
.option:hover{border-color:var(--primary);background:rgba(99,102,241,.1);transform:translateX(-3px)}
.option.selected{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;border-color:transparent}
.option.correct{background:linear-gradient(135deg,var(--success),#059669);color:#fff;border-color:transparent}
.option.wrong{background:linear-gradient(135deg,var(--danger),#dc2626);color:#fff;border-color:transparent}
.option.disabled{cursor:not-allowed}
.option-number{display:inline-flex;align-items:center;justify-content:center;width:28px;height:28px;background:rgba(255,255,255,.08);border-radius:50%;font-weight:700;font-size:.8em;flex-shrink:0}
.selected .option-number,.correct .option-number,.wrong .option-number{background:rgba(255,255,255,.25)}

.explanation{background:rgba(16,185,129,.1);padding:18px;border-radius:14px;margin-top:16px;border-right:4px solid var(--success);display:none;color:#d1fae5;line-height:1.75;font-size:.9em}
.explanation h3{color:#6ee7b7;margin-bottom:10px;display:flex;align-items:center;gap:10px;font-size:.98em}

/* ===== Results ===== */
.result-container{text-align:center;padding:22px;max-width:1000px;margin:0 auto}
.result-score{font-size:3.5em;font-weight:800;background:linear-gradient(135deg,var(--cyan),var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;margin:14px 0}
.result-chart{width:230px;height:230px;margin:0 auto 20px}
.comparison{background:rgba(139,92,246,.1);padding:20px;border-radius:14px;margin:20px 0;border:1px solid rgba(139,92,246,.25);color:#e2e8f0}
.comparison h3{margin-bottom:10px;color:#c4b5fd;font-size:1em}

.weak-lessons{text-align:right;margin-top:22px}
.weak-lessons > h3{color:#f1f5f9;margin-bottom:14px;display:flex;align-items:center;gap:10px;font-size:1em}
.lesson-item{background:rgba(239,68,68,.1);padding:14px;margin:8px 0;border-radius:12px;border-right:4px solid var(--danger);color:#fecaca;line-height:1.7;font-size:.88em}
.lesson-item strong{color:#fca5a5}
.lesson-item p{margin-top:6px}

.advanced-report{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:22px;margin-top:22px}
.report-section h3{color:#f1f5f9;margin-bottom:14px;padding-bottom:10px;border-bottom:1px solid rgba(255,255,255,.08);display:flex;align-items:center;gap:10px;font-size:1em}
.report-chart{width:100%;height:270px;margin:14px 0}
.improvement-tips{background:rgba(16,185,129,.1);padding:18px;border-radius:14px;margin-top:14px;border-right:4px solid var(--success);color:#d1fae5;font-size:.9em}
.improvement-tips h4{color:#6ee7b7;margin-bottom:10px;display:flex;align-items:center;gap:10px;font-size:.95em}
.improvement-tips ul{padding-right:20px}
.improvement-tips li{margin-bottom:6px;line-height:1.7}

/* ===== Progress ===== */
.progress-stats{display:grid;grid-template-columns:repeat(auto-fill,minmax(210px,1fr));gap:16px;margin-bottom:22px}
.stat-card{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:18px;padding:20px;text-align:center;transition:var(--transition)}
.stat-card:hover{transform:translateY(-4px);border-color:rgba(99,102,241,.4);box-shadow:0 15px 40px rgba(99,102,241,.2)}
.stat-card i{font-size:2em;background:linear-gradient(135deg,var(--primary),var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:10px}
.stat-card h3{color:#94a3b8;margin-bottom:8px;font-size:.82em;font-weight:600}
.stat-card .stat-value{font-size:1.7em;font-weight:800;background:linear-gradient(135deg,var(--cyan),var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent}

.progress-history{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:22px;overflow-x:auto}
.progress-history h3{color:#f1f5f9;margin-bottom:16px;display:flex;align-items:center;gap:10px;font-size:1em}
.history-table{width:100%;border-collapse:collapse;min-width:600px}
.history-table th,.history-table td{padding:12px;text-align:right;border-bottom:1px solid rgba(255,255,255,.06);font-size:.85em;color:#cbd5e1}
.history-table th{background:rgba(255,255,255,.03);color:#f1f5f9;font-weight:700}
.history-table tr:hover{background:rgba(99,102,241,.05)}
.history-table tr:last-child td{border-bottom:none}
.score-badge{padding:4px 10px;border-radius:20px;font-weight:700;color:#fff;font-size:.8em;display:inline-block}
.score-high{background:linear-gradient(135deg,var(--success),#059669)}
.score-medium{background:linear-gradient(135deg,var(--warning),#d97706)}
.score-low{background:linear-gradient(135deg,var(--danger),#dc2626)}

/* ===== Notifications ===== */
.notifications-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:20px;max-height:600px;overflow-y:auto}
.notification-item{padding:14px;border-bottom:1px solid rgba(255,255,255,.06);display:flex;align-items:flex-start;gap:12px;transition:var(--transition);cursor:pointer;border-radius:10px}
.notification-item:hover{background:rgba(255,255,255,.04)}
.notification-item:last-child{border-bottom:none}
.notification-icon{font-size:1.05em;color:var(--cyan);flex-shrink:0;width:32px;height:32px;border-radius:50%;background:rgba(34,211,238,.15);display:flex;align-items:center;justify-content:center}
.notification-content{flex:1;min-width:0}
.notification-title{font-weight:700;margin-bottom:4px;color:#f1f5f9;font-size:.88em}
.notification-message{color:#cbd5e1;font-size:.82em;line-height:1.6;margin-bottom:5px}
.notification-time{font-size:.72em;color:#64748b}

/* ===== Profile ===== */
.profile-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:26px}
.profile-header{display:flex;align-items:center;gap:20px;margin-bottom:24px;flex-wrap:wrap}
.profile-avatar{width:96px;height:96px;border-radius:50%;background:linear-gradient(135deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;font-size:2.3em;color:#fff;flex-shrink:0;box-shadow:0 10px 30px rgba(99,102,241,.4);overflow:hidden}
.profile-avatar img{width:100%;height:100%;object-fit:cover}
.profile-info{flex:1;min-width:240px}
.profile-name{font-size:1.2em;font-weight:700;margin-bottom:8px;display:flex;align-items:center;gap:12px;flex-wrap:wrap;color:#f1f5f9}
.profile-info p{color:#94a3b8;font-size:.86em;margin-bottom:4px}

.profile-stats{display:grid;grid-template-columns:repeat(auto-fill,minmax(170px,1fr));gap:14px;margin-top:22px}
.profile-stat{background:rgba(99,102,241,.08);padding:18px;border-radius:14px;text-align:center;border:1px solid rgba(99,102,241,.15);transition:var(--transition)}
.profile-stat:hover{transform:translateY(-3px);border-color:rgba(99,102,241,.4)}
.profile-stat-value{font-size:1.6em;font-weight:800;background:linear-gradient(135deg,var(--cyan),var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:6px}
.profile-stat-label{color:#94a3b8;font-size:.78em}

.rank-badge{display:inline-flex;align-items:center;padding:4px 12px;border-radius:20px;font-weight:700;font-size:.78em;color:#fff;white-space:nowrap}
.rank-inert{background:linear-gradient(135deg,#64748b,#475569)}
.rank-active{background:linear-gradient(135deg,#f59e0b,#d97706)}
.rank-good{background:linear-gradient(135deg,#10b981,#059669)}
.rank-genius{background:linear-gradient(135deg,#8b5cf6,#6366f1);animation:glowBadge 2s infinite alternate}
@keyframes glowBadge{from{box-shadow:0 0 8px rgba(139,92,246,.6)}to{box-shadow:0 0 22px rgba(99,102,241,.9)}}

/* ===== Simulation ===== */
.simulation-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:24px;text-align:center}
.simulation-container h2{color:#f1f5f9;margin-bottom:10px;font-size:1.15em}
.simulation-container > p{color:#94a3b8;margin-bottom:18px;font-size:.88em}
.simulation-frame{width:100%;height:600px;border:none;border-radius:14px;background:#fff}
.simulation-info{margin-top:18px;text-align:right}
.simulation-info h3{color:#f1f5f9;margin-bottom:10px;font-size:1em;display:flex;align-items:center;gap:10px}
.simulation-info p{color:#94a3b8;line-height:1.8;font-size:.85em}

/* ===== Research ===== */
.research-container{max-width:1100px;margin:0 auto;padding:8px}
.research-hero{background:linear-gradient(135deg,rgba(99,102,241,.25),rgba(139,92,246,.25));border:1px solid rgba(99,102,241,.3);border-radius:22px;padding:36px 28px;text-align:center;margin-bottom:24px;position:relative;overflow:hidden;backdrop-filter:blur(20px)}
.research-hero::before{content:'';position:absolute;top:-50%;right:-50%;width:200%;height:200%;background:radial-gradient(circle,rgba(34,211,238,.15) 0%,transparent 60%);animation:rotateSlow 30s linear infinite;pointer-events:none}
.research-hero h1{font-size:1.75em;margin-bottom:12px;position:relative;z-index:1;background:linear-gradient(135deg,#fff,var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;font-weight:800;line-height:1.4}
.research-hero > p{color:#cbd5e1;font-size:.95em;max-width:680px;margin:0 auto;position:relative;z-index:1;line-height:1.75}
.research-elements-showcase{display:flex;justify-content:center;gap:10px;flex-wrap:wrap;margin-top:22px;position:relative;z-index:1}
.element-chip{background:rgba(255,255,255,.08);backdrop-filter:blur(10px);padding:9px 16px;border-radius:30px;font-weight:700;border:1px solid rgba(255,255,255,.15);display:flex;align-items:center;gap:8px;transition:var(--transition);color:#e2e8f0;font-size:.85em}
.element-chip:hover{background:rgba(99,102,241,.25);transform:translateY(-3px);border-color:rgba(99,102,241,.5)}

.research-toc{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:18px;padding:22px;margin-bottom:22px}
.research-toc h3{color:#f1f5f9;margin-bottom:16px;padding-bottom:10px;border-bottom:1px solid rgba(255,255,255,.08);display:flex;align-items:center;gap:10px;font-size:1em}
.research-toc h3 i{color:var(--cyan)}
.toc-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(190px,1fr));gap:10px}
.toc-link{display:flex;align-items:center;gap:10px;padding:11px 14px;background:rgba(255,255,255,.04);border-radius:11px;color:#cbd5e1;text-decoration:none;transition:var(--transition);border-right:3px solid var(--primary);font-size:.85em;cursor:pointer}
.toc-link:hover{background:linear-gradient(135deg,rgba(99,102,241,.2),rgba(139,92,246,.2));transform:translateX(-4px);color:#fff;border-right-color:var(--cyan)}
.toc-link i{color:var(--primary);font-size:.95em;width:18px;text-align:center}
.toc-link:hover i{color:var(--cyan)}

.research-section{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:18px;padding:26px;margin-bottom:20px;scroll-margin-top:20px;transition:var(--transition)}
.research-section:hover{border-color:rgba(99,102,241,.3);box-shadow:0 15px 40px rgba(99,102,241,.15)}
.section-header-element{display:flex;align-items:center;gap:16px;padding:20px;border-radius:16px;margin-bottom:20px;color:#fff;position:relative;overflow:hidden}
.section-header-element::before{content:'';position:absolute;top:0;left:0;width:100%;height:100%;background:radial-gradient(circle at 20% 50%,rgba(255,255,255,.15) 0%,transparent 60%);pointer-events:none}
.element-badge{width:72px;height:72px;border-radius:50%;background:rgba(255,255,255,.22);display:flex;align-items:center;justify-content:center;font-size:1.9em;font-weight:900;backdrop-filter:blur(10px);border:3px solid rgba(255,255,255,.4);flex-shrink:0;box-shadow:0 8px 20px rgba(0,0,0,.3);position:relative;z-index:1}
.element-info-title{position:relative;z-index:1}
.element-info-title h2{font-size:1.25em;margin-bottom:5px;line-height:1.4;font-weight:800}
.element-info-title p{opacity:.9;font-size:.8em}

.element-fe{background:linear-gradient(135deg,#b91c1c,#7f1d1d)}
.element-zn{background:linear-gradient(135deg,#0891b2,#155e75)}
.element-mg{background:linear-gradient(135deg,#16a34a,#15803d)}
.element-i{background:linear-gradient(135deg,#7c3aed,#5b21b6)}
.element-se{background:linear-gradient(135deg,#ea580c,#c2410c)}
.element-intro{background:linear-gradient(135deg,#4f46e5,#7c3aed)}

.subsection{margin-bottom:22px;padding-bottom:18px;border-bottom:1px solid rgba(255,255,255,.06)}
.subsection:last-child{border-bottom:none;margin-bottom:0;padding-bottom:0}
.subsection h4{color:#a5b4fc;font-size:.98em;margin-bottom:12px;display:flex;align-items:center;gap:10px;padding-right:12px;border-right:3px solid var(--primary);font-weight:700}
.subsection p{line-height:1.85;color:#cbd5e1;margin-bottom:10px;text-align:justify;font-size:.88em}
.subsection ul{padding-right:20px;margin-bottom:12px}
.subsection li{line-height:1.8;color:#cbd5e1;margin-bottom:7px;font-size:.87em}
.subsection strong{color:#f1f5f9;font-weight:700}

.formula-box{background:linear-gradient(135deg,#0f172a,#020617);color:#6ee7b7;padding:20px;border-radius:14px;text-align:center;font-family:'Courier New',monospace;font-size:1.05em;font-weight:bold;margin:14px 0;box-shadow:0 8px 24px rgba(0,0,0,.4);border:1px solid rgba(6,214,160,.35);direction:ltr;letter-spacing:1.5px;position:relative;overflow:hidden}
.formula-box::before{content:'';position:absolute;top:0;left:-100%;width:100%;height:100%;background:linear-gradient(90deg,transparent,rgba(6,214,160,.15),transparent);animation:shine 3s infinite}
@keyframes shine{0%{left:-100%}100%{left:100%}}

.info-box{padding:14px 16px;border-radius:12px;margin:12px 0;border-right:4px solid;line-height:1.8;font-size:.87em}
.info-box h5{margin-bottom:8px;display:flex;align-items:center;gap:8px;font-size:.94em;font-weight:700}
.info-box p{color:#cbd5e1;font-size:.9em}
.info-box.warning{background:rgba(245,158,11,.1);border-color:#f59e0b}
.info-box.warning h5{color:#fbbf24}
.info-box.danger{background:rgba(239,68,68,.1);border-color:#ef4444}
.info-box.danger h5{color:#fca5a5}
.info-box.success{background:rgba(16,185,129,.1);border-color:#10b981}
.info-box.success h5{color:#6ee7b7}
.info-box.info{background:rgba(59,130,246,.1);border-color:#3b82f6}
.info-box.info h5{color:#93c5fd}
.info-box.purple{background:rgba(139,92,246,.1);border-color:#8b5cf6}
.info-box.purple h5{color:#c4b5fd}

.comparison-table-wrapper{overflow-x:auto;border-radius:14px;border:1px solid rgba(255,255,255,.08);margin:16px 0}
.comparison-table{width:100%;border-collapse:collapse;min-width:750px}
.comparison-table th{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;padding:12px;text-align:right;font-weight:700;font-size:.85em}
.comparison-table td{padding:12px;border-bottom:1px solid rgba(255,255,255,.06);text-align:right;line-height:1.7;color:#cbd5e1;font-size:.83em;background:rgba(255,255,255,.02)}
.comparison-table tr:hover td{background:rgba(99,102,241,.08)}
.comparison-table tr:last-child td{border-bottom:none}
.comparison-table .element-cell{font-weight:700;color:#f1f5f9;display:flex;align-items:center;gap:8px}
.element-mini-badge{display:inline-flex;align-items:center;justify-content:center;width:28px;height:28px;border-radius:50%;color:#fff;font-weight:800;font-size:.78em;flex-shrink:0}

.references-list{list-style:none;padding:0;counter-reset:ref}
.references-list li{counter-increment:ref;padding:12px 16px 12px 56px;margin-bottom:10px;background:rgba(255,255,255,.03);border-radius:12px;position:relative;line-height:1.7;border-right:3px solid var(--primary);transition:var(--transition);font-size:.83em;direction:ltr;text-align:left;color:#cbd5e1}
.references-list li:hover{background:rgba(99,102,241,.08);transform:translateX(-4px)}
.references-list li::before{content:counter(ref);position:absolute;left:14px;top:50%;transform:translateY(-50%);width:26px;height:26px;background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:700;font-size:.78em}
.references-list a{color:#93c5fd;text-decoration:none;word-break:break-all}
.references-list a:hover{color:#60a5fa;text-decoration:underline}

.conclusion-box{background:linear-gradient(135deg,rgba(16,185,129,.12),rgba(6,214,160,.12));border-radius:16px;padding:24px;border-right:5px solid var(--success);line-height:1.85;color:#d1fae5;font-size:.88em}
.conclusion-box h3{color:#6ee7b7;margin-bottom:12px;display:flex;align-items:center;gap:10px;font-size:1em}

.back-to-top{position:fixed;bottom:22px;left:22px;width:44px;height:44px;background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;border:none;border-radius:50%;cursor:pointer;box-shadow:0 8px 24px rgba(99,102,241,.5);display:none;align-items:center;justify-content:center;font-size:1em;z-index:999;transition:var(--transition)}
.back-to-top.visible{display:flex}
.back-to-top:hover{transform:translateY(-4px);box-shadow:0 12px 30px rgba(99,102,241,.7)}

.url-indicator{position:fixed;bottom:16px;left:50%;transform:translateX(-50%);background:rgba(15,23,42,.85);backdrop-filter:blur(12px);color:var(--cyan);padding:7px 16px;border-radius:20px;font-size:.72em;font-family:'Courier New',monospace;z-index:9999;border:1px solid rgba(34,211,238,.3);cursor:pointer;display:none;max-width:80%;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;transition:var(--transition)}
.url-indicator:hover{background:rgba(15,23,42,.95);color:#fff}
.url-indicator.visible{display:block}

/* Toast */
.toast{position:fixed;top:20px;left:50%;transform:translateX(-50%) translateY(-100px);background:rgba(15,23,42,.95);backdrop-filter:blur(20px);color:#fff;padding:14px 24px;border-radius:14px;border:1px solid var(--glass-border);box-shadow:0 10px 40px rgba(0,0,0,.5);z-index:10000;transition:transform .4s cubic-bezier(.4,0,.2,1);display:flex;align-items:center;gap:12px;font-size:.9em;font-weight:600;max-width:90%}
.toast.show{transform:translateX(-50%) translateY(0)}
.toast i{font-size:1.2em}
.toast.success i{color:var(--success)}
.toast.error i{color:var(--danger)}
.toast.info i{color:var(--cyan)}

/* Modal */
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.7);backdrop-filter:blur(8px);z-index:10000;display:none;align-items:center;justify-content:center;padding:20px}
.modal-overlay.active{display:flex;animation:fadeIn .3s ease}
.modal{background:rgba(15,23,42,.95);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:22px;padding:28px;max-width:480px;width:100%;box-shadow:0 30px 80px rgba(0,0,0,.6)}
.modal h3{color:#f1f5f9;margin-bottom:14px;display:flex;align-items:center;gap:10px;font-size:1.1em}
.modal p{color:#94a3b8;margin-bottom:20px;line-height:1.7;font-size:.9em}
.modal-actions{display:flex;gap:10px;justify-content:flex-end;flex-wrap:wrap}
.modal-actions .btn{min-width:100px}

/* Responsive */
@media(max-width:900px){
  .sidebar{width:100%;height:auto;position:relative;border-left:none;border-bottom:1px solid var(--glass-border)}
  .app-container.active{flex-direction:column}
  .main-content{height:auto;min-height:calc(100vh - 200px)}
  .nav-menu{display:flex;overflow-x:auto;gap:6px;padding:0 10px 10px}
  .nav-item{flex-shrink:0;margin-bottom:0}
  .nav-link{white-space:nowrap;padding:9px 12px;font-size:.85em}
  .quiz-options{grid-template-columns:1fr}
  .quiz-container{flex-direction:column}
  .whiteboard-frame{height:380px}
  .research-hero h1{font-size:1.3em}
  .section-header-element{flex-direction:column;text-align:center}
  .greeting{justify-content:center;text-align:center}
  .greeting-text h1{font-size:1.2em}
}

@media(max-width:500px){
  .header{padding:12px 16px}
  .header h2{font-size:.95em}
  .btn{padding:9px 14px;font-size:.82em}
  .result-score{font-size:2.5em}
  .auth-container{padding:26px 20px}
  .auth-container h1{font-size:1.4em}
  .greeting-avatar{width:60px;height:60px;font-size:1.5em}
}
</style>
</head>
<body>

<canvas id="bg-canvas"></canvas>
<div class="bg-gradient-overlay"></div>

<!-- ============ شاشة المصادقة ============ -->
<div class="auth-screen active" id="auth">
  <div class="auth-container">
    <div class="auth-logo"><i class="fas fa-atom"></i></div>
    <h1>منصة الكيمياء التفاعلية</h1>
    <p class="auth-subtitle">سجل دخولك أو أنشئ حساباً جديداً للبدء</p>

    <div class="auth-tabs">
      <button class="auth-tab active" data-tab="login"><i class="fas fa-sign-in-alt"></i> تسجيل الدخول</button>
      <button class="auth-tab" data-tab="signup"><i class="fas fa-user-plus"></i> حساب جديد</button>
    </div>

    <div class="auth-error" id="auth-error"></div>
    <div class="auth-success" id="auth-success"></div>

    <!-- تسجيل الدخول -->
    <div class="auth-panel active" id="panel-login">
      <form id="login-form">
        <div class="form-group">
          <label><i class="fas fa-user-circle"></i> البريد الإلكتروني أو اسم المستخدم</label>
          <div class="input-with-icon">
            <i class="fas fa-user"></i>
            <input type="text" id="login-identifier" class="form-control" placeholder="example@mail.com أو username" required>
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-lock"></i> كلمة المرور</label>
          <div class="input-with-icon">
            <i class="fas fa-lock"></i>
            <input type="password" id="login-password" class="form-control" placeholder="••••••••" required>
            <button type="button" class="pass-toggle" onclick="togglePass('login-password', this)"><i class="fas fa-eye"></i></button>
          </div>
        </div>
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:16px;font-size:.85em;">
          <label style="display:flex;align-items:center;gap:6px;cursor:pointer;color:#94a3b8;">
            <input type="checkbox" id="remember-me" style="width:auto;padding:0;"> تذكرني
          </label>
          <a href="#" style="color:var(--cyan);text-decoration:none;" onclick="showForgot(event)">نسيت كلمة المرور؟</a>
        </div>
        <button type="submit" class="btn" style="width:100%;padding:13px;">
          <i class="fas fa-sign-in-alt"></i> تسجيل الدخول
        </button>
      </form>
    </div>

    <!-- إنشاء حساب -->
    <div class="auth-panel" id="panel-signup">
      <form id="signup-form">
        <div class="form-group">
          <label><i class="fas fa-id-card"></i> الاسم الكامل</label>
          <div class="input-with-icon">
            <i class="fas fa-user"></i>
            <input type="text" id="signup-name" class="form-control" placeholder="أدخل اسمك الكامل" required>
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-at"></i> اسم المستخدم</label>
          <div class="input-with-icon">
            <i class="fas fa-user-tag"></i>
            <input type="text" id="signup-username" class="form-control" placeholder="username (بالإنجليزية والأرقام)" required pattern="[a-zA-Z0-9_]{3,20}">
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-envelope"></i> البريد الإلكتروني</label>
          <div class="input-with-icon">
            <i class="fas fa-envelope"></i>
            <input type="email" id="signup-email" class="form-control" placeholder="example@mail.com" required>
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-users"></i> الشعبة</label>
          <select id="signup-section" class="form-control" required style="padding-right:16px;">
            <option value="" disabled selected>اختر شعبتك</option>
            <option value="الشعبة 1">الشعبة 1</option>
            <option value="الشعبة 2">الشعبة 2</option>
            <option value="الشعبة 3">الشعبة 3</option>
            <option value="الشعبة 4">الشعبة 4</option>
            <option value="الشعبة 5">الشعبة 5</option>
            <option value="الشعبة 6">الشعبة 6</option>
            <option value="الشعبة 7">الشعبة 7</option>
          </select>
        </div>
        <div class="form-group">
          <label><i class="fas fa-lock"></i> كلمة المرور</label>
          <div class="input-with-icon">
            <i class="fas fa-lock"></i>
            <input type="password" id="signup-password" class="form-control" placeholder="6 أحرف على الأقل" required minlength="6">
            <button type="button" class="pass-toggle" onclick="togglePass('signup-password', this)"><i class="fas fa-eye"></i></button>
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-lock"></i> تأكيد كلمة المرور</label>
          <div class="input-with-icon">
            <i class="fas fa-check-double"></i>
            <input type="password" id="signup-password2" class="form-control" placeholder="أعد كتابة كلمة المرور" required>
          </div>
        </div>
        <button type="submit" class="btn" style="width:100%;padding:13px;">
          <i class="fas fa-user-plus"></i> إنشاء الحساب
        </button>
      </form>
    </div>
  </div>
</div>

<!-- ============ التطبيق ============ -->
<div class="app-container" id="main-app">
  <aside class="sidebar">
    <div class="logo"><h1><i class="fas fa-atom"></i> كيمياء</h1></div>

    <ul class="nav-menu">
      <li class="nav-item"><a href="#/welcome" class="nav-link" data-screen="welcome"><i class="fas fa-home"></i><span>الرئيسية</span></a></li>
      <li class="nav-item"><a href="#/chemistry-human" class="nav-link" data-screen="chemistry-human"><i class="fas fa-dna"></i><span>الكيمياء والانسان</span></a></li>
      <li class="nav-item"><a href="#/quiz-setup" class="nav-link" data-screen="quiz-setup"><i class="fas fa-file-alt"></i><span>الاختبارات</span></a></li>
      <li class="nav-item"><a href="#/simulation" class="nav-link" data-screen="simulation"><i class="fas fa-flask"></i><span>المحاكاة</span></a></li>
      <li class="nav-item"><a href="#/progress" class="nav-link" data-screen="progress"><i class="fas fa-chart-line"></i><span>التقدم</span></a></li>
      <li class="nav-item"><a href="#/settings" class="nav-link" data-screen="settings"><i class="fas fa-cog"></i><span>الإعدادات</span></a></li>
    </ul>

    <div class="team-section">
      <h3><i class="fas fa-star"></i> المشرف العام</h3>
      <div class="team-member">
        <div class="team-avatar"><i class="fas fa-crown"></i></div>
        <div class="team-member-info">
          <div class="team-member-name">تميم الزهراني</div>
          <div class="team-member-role">القائد والمطوّر</div>
        </div>
      </div>
    </div>

    <div class="user-section">
      <div class="user-info">
        <div class="user-avatar" id="sidebar-avatar"><i class="fas fa-user"></i></div>
        <div style="flex:1;min-width:0;">
          <h4 id="username-display">-</h4>
          <div class="student-info">
            <p id="section-display">الشعبة: -</p>
            <p id="user-level">المستوى: مبتدئ</p>
          </div>
        </div>
      </div>
      <button class="logout-btn" id="logout-btn"><i class="fas fa-sign-out-alt"></i> تسجيل الخروج</button>
    </div>
  </aside>

  <main class="main-content">

    <!-- ===== الرئيسية ===== -->
    <div class="screen active" id="welcome">
      <div class="header">
        <h2><i class="fas fa-home"></i> الرئيسية</h2>
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('notifications')"><i class="fas fa-bell"></i> الإشعارات <span id="notif-count" style="background:var(--danger);color:#fff;border-radius:50%;padding:1px 7px;font-size:.75em;margin-right:4px;display:none;">0</span></button>
          <button class="btn btn-outline" onclick="navigateTo('profile')"><i class="fas fa-user"></i> الملف الشخصي</button>
        </div>
      </div>

      <div class="dashboard">
        <div class="dashboard-hero">
          <div class="greeting">
            <div class="greeting-avatar" id="welcome-avatar"><i class="fas fa-user"></i></div>
            <div class="greeting-text">
              <h1>مرحباً، <span id="welcome-name">طالب</span> 👋</h1>
              <p id="welcome-subtitle">لنبدأ رحلة الكيمياء التفاعلية معاً</p>
            </div>
          </div>
          <div class="quick-stats">
            <div class="quick-stat"><div class="quick-stat-value" id="qs-xp">0</div><div class="quick-stat-label"><i class="fas fa-bolt"></i> نقاط الخبرة</div></div>
            <div class="quick-stat"><div class="quick-stat-value" id="qs-quizzes">0</div><div class="quick-stat-label"><i class="fas fa-file-alt"></i> اختباراتك</div></div>
            <div class="quick-stat"><div class="quick-stat-value" id="qs-avg">0%</div><div class="quick-stat-label"><i class="fas fa-chart-line"></i> متوسطك</div></div>
            <div class="quick-stat"><div class="quick-stat-value" id="qs-streak">0</div><div class="quick-stat-label"><i class="fas fa-fire"></i> أيام متتالية</div></div>
          </div>
        </div>

        <div class="section-title">
          <h2><i class="fas fa-th-large"></i> الأقسام الرئيسية</h2>
        </div>

        <div class="cards-container">
          <div class="card card-2" onclick="navigateTo('chemistry-human')">
            <div class="card-header">
              <div class="card-icon"><i class="fas fa-dna"></i></div>
              <h3>الكيمياء والانسان</h3>
            </div>
            <p>دراسة كيميائية حيوية شاملة للعناصر المهمة في جسم الإنسان وآليات عملها على المستوى الجزيئي</p>
            <button class="btn btn-success"><i class="fas fa-book-open"></i> اقرأ البحث</button>
          </div>

          <div class="card card-1" onclick="navigateTo('quiz-setup')">
            <div class="card-header">
              <div class="card-icon"><i class="fas fa-file-alt"></i></div>
              <h3>الاختبارات التفاعلية</h3>
            </div>
            <p>اختبر معرفتك من خلال اختبارات تفاعلية مع تصحيح فوري وتحليل مفصل لأدائك</p>
            <button class="btn"><i class="fas fa-play"></i> ابدأ الاختبار</button>
          </div>

          <div class="card card-5" onclick="navigateTo('simulation')">
            <div class="card-header">
              <div class="card-icon"><i class="fas fa-flask"></i></div>
              <h3>المحاكاة والتجارب</h3>
            </div>
            <p>قم بتجربة التفاعلات الكيميائية افتراضياً في مختبر آمن مع شرح مفصل للنتائج</p>
            <button class="btn btn-cyan"><i class="fas fa-vial"></i> ابدأ المحاكاة</button>
          </div>

          <div class="card card-3" onclick="navigateTo('progress')">
            <div class="card-header">
              <div class="card-icon"><i class="fas fa-chart-line"></i></div>
              <h3>تتبع التقدم</h3>
            </div>
            <p>راجع إحصائيات أدائك، تتبع تقدمك التعليمي، واحصل على توصيات مخصصة</p>
            <button class="btn btn-warning"><i class="fas fa-chart-bar"></i> عرض التقارير</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ===== الكيمياء والانسان ===== -->
    <div class="screen" id="chemistry-human">
      <div class="header">
        <h2><i class="fas fa-dna"></i> الكيمياء والانسان</h2>
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button>
        </div>
      </div>

      <div class="research-container">
        <div class="research-hero">
          <h1><i class="fas fa-atom"></i> العناصر الكيميائية المهمة في جسم الإنسان</h1>
          <p>دراسة كيميائية حيوية شاملة لخصائص العناصر، آليات عملها على المستوى الجزئي، ووظائفها داخل الجسم</p>
          <div class="research-elements-showcase">
            <div class="element-chip"><span>Fe</span> الحديد</div>
            <div class="element-chip"><span>Zn</span> الزنك</div>
            <div class="element-chip"><span>Mg</span> المغنيسيوم</div>
            <div class="element-chip"><span>I</span> اليود</div>
            <div class="element-chip"><span>Se</span> السيلينيوم</div>
          </div>
        </div>

        <div class="research-toc">
          <h3><i class="fas fa-list"></i> فهرس البحث</h3>
          <div class="toc-grid">
            <a href="#res-intro" class="toc-link"><i class="fas fa-book-open"></i> المقدمة والمبادئ</a>
            <a href="#res-fe" class="toc-link"><i class="fas fa-fire"></i> الحديد (Fe)</a>
            <a href="#res-zn" class="toc-link"><i class="fas fa-shield-alt"></i> الزنك (Zn)</a>
            <a href="#res-mg" class="toc-link"><i class="fas fa-bolt"></i> المغنيسيوم (Mg)</a>
            <a href="#res-i" class="toc-link"><i class="fas fa-leaf"></i> اليود (I)</a>
            <a href="#res-se" class="toc-link"><i class="fas fa-umbrella"></i> السيلينيوم (Se)</a>
            <a href="#res-table" class="toc-link"><i class="fas fa-table"></i> جدول المقارنة</a>
            <a href="#res-conclusion" class="toc-link"><i class="fas fa-flag-checkered"></i> الخاتمة</a>
            <a href="#res-refs" class="toc-link"><i class="fas fa-book"></i> المراجع</a>
          </div>
        </div>

        <!-- المقدمة -->
        <div class="research-section" id="res-intro">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-flask"></i></div>
            <div class="element-info-title">
              <h2>المقدمة والمبادئ الكيميائية الحيوية</h2>
              <p>الأساس الذري لوظائف الجسم الحيوية</p>
            </div>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-info-circle"></i> المقدمة والأهمية الكيميائية الحيوية</h4>
            <p>لا تعتمد وظائف جسم الإنسان الحيوية على وجود المركبات العضوية الكبرى وحدها، مثل البروتينات، الكربوهيدرات، والدهون، بل تعتمد اعتماداً كلياً على عناصر كيميائية تمتلك خصائص ذرية وإلكترونية فريدة تجعلها ضرورية لحدوث تفاعلات حيوية محددة لا يمكن بدونها استمرار الحياة.</p>
            <p>وتكمن أهمية هذه العناصر الكيميائية في قدرتها على تكوين الأيونات النشطة، والارتباط الانتقائي بالجزيئات الحيوية، وتغيير طاقة التنشيط وسرعة التفاعلات الكيميائية عبر التحفيز الإنزيمي، بالإضافة إلى المشاركة المباشرة في سلاسل انتقال الإلكترونات وتثبيت التراكيب الفراغية ثلاثية الأبعاد للبروتينات والأحماض النووية.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-microscope"></i> الرؤية الكيميائية الحيوية للعناصر</h4>
            <p>من منظور الكيمياء الحيوية، لا تتساوى العناصر في طريقة عملها داخل الجسم؛ فالحديد يستطيع التناوب بين حالات أكسدة متعددة ليشارك في نقل الإلكترونات والأكسجين، بينما يمتاز الزنك بقدرته على تكوين روابط تناسقية ثابتة الجزيئات، ويمتلك المغنيسيوم ألفة عالية لمجموعات الفوسفات السالبة في جزيئات ATP، بينما يدخل اليود بصورة بنيوية دائمة في تركيب الهرمونات، ويشكل السيلينيوم مركزاً حافزاً فريداً في إنزيمات مضادات الأكسدة.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-cogs"></i> المبادئ الكيميائية الحيوية الحاكمة للعناصر</h4>
            <p>تخضع كافة العناصر الكيميائية في جسم الإنسان لمجموعة من القواعد الفيزيوكيميائية التي تحدد نمط وظيفتها داخل الخلايا، ويمكن تلخيص هذه المبادئ في أربعة محاور أساسية:</p>
            <div class="info-box info">
              <h5><i class="fas fa-bolt"></i> 1. جهد الأكسدة والاختزال (Redox Potential)</h5>
              <p>تحديد قدرة العنصر على اكتساب أو فقد الإلكترونات بمرونة دون إحداث إجهاد تأكسدي عشوائي.</p>
            </div>
            <div class="info-box purple">
              <h5><i class="fas fa-cube"></i> 2. الهندسة التناسقية (Coordination Geometry)</h5>
              <p>شكل الروابط الفراغية التي يشكلها أيون الفلز مع المجموعات العضوية المحيطة به داخل الموقع النشط.</p>
            </div>
            <p><strong>وتُصنَّف العناصر الحيوية تبعاً لوظائفها إلى ثلاث فئات رئيسية:</strong></p>
            <div class="toc-grid" style="margin-top:14px;">
              <div class="toc-link" style="cursor:default;"><i class="fas fa-fire" style="color:#ef4444;"></i><div><strong>عناصر التخليق الهرموني</strong><div style="font-size:.78em;opacity:.75;margin-top:3px;">اليود (I)</div></div></div>
              <div class="toc-link" style="cursor:default;"><i class="fas fa-shield-alt" style="color:#10b981;"></i><div><strong>عناصر التثبيت البنيوي</strong><div style="font-size:.78em;opacity:.75;margin-top:3px;">الزنك (Zn) والمغنيسيوم (Mg)</div></div></div>
              <div class="toc-link" style="cursor:default;"><i class="fas fa-exchange-alt" style="color:#3b82f6;"></i><div><strong>عناصر نقل الطاقة والإلكترون</strong><div style="font-size:.78em;opacity:.75;margin-top:3px;">الحديد (Fe) والسيلينيوم (Se)</div></div></div>
            </div>
          </div>
        </div>

        <!-- الحديد -->
        <div class="research-section" id="res-fe">
          <div class="section-header-element element-fe">
            <div class="element-badge">Fe</div>
            <div class="element-info-title"><h2>أولاً: الحديد (Fe) — كيمياء الأكسجين وانتقال الإلكترونات</h2><p>العدد الذري: 26 | الوزن الذري: 55.84</p></div>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية والذرية للحديد</h4>
            <p>الحديد عنصر فلزي انتقالي (Transition Metal) ينتمي للسلسلة الانتقالية الأولى في الجدول الدوري، وعدده الذري 26. يتميز التوزيع الإلكتروني للحديد بقابليته لفقد إلكترونات من المدارين 4s و 3d، مما يمنحه القدرة الفريدة على الوجود في حالتي أكسدة حيوية رئيسيتين ومتوازنتين داخل البيئة المائية للجسم:</p>
            <ul>
              <li><strong>الحديد الثنائي - Ferrous (Fe²⁺):</strong> يتميز بزيادة تفاعليته وقابليته للذوبان، ويُعد الصورة الرئيسية في الامتصاص المعوي والارتباط بالأكسجين.</li>
              <li><strong>الحديد الثلاثي - Ferric (Fe³⁺):</strong> الصورة الأكثر استقراراً في الوسط الأكسجيني، ويشكل الصورة الأساسية للتخزين بروتينياً داخل الخلية.</li>
            </ul>
            <p>تسمح هذه الخاصية المزدوجة للحديد بالمشاركة الفعالة والديناميكية في تفاعلات الأكسدة والاختزال (Redox Reactions)، حيث تنتقل الإلكترونات بسلاسة بين المواد المتفاعلة، وعلى خلاف العناصر ذات حالات الأكسدة الثابتة (مثل الزنك 2+)، يعد الحديد بمثابة "مكون إلكتروني" يحرك التفاعلات الحيوية داخل الخلايا.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-lungs"></i> 2. آلية نقل الأكسجين عبر حلقة الهيم والهيموغلوبين</h4>
            <p>لا يوجد الحديد داخل جسم الإنسان في صورة أيون حر طليق، لأن الأيونات الحرة تمتلك سمية تأكسدية عالية. بدلاً من ذلك، يتم احتجاز الحديد داخل مجموعة كيميائية عضوية معقدة تُعرف بالهيم (Heme). تتكون مجموعة الهيم من حلقة عضوية كبيرة تُعرف باسم البورفيرين (Porphyrin) تتوسطها ذرة حديد مرابطة بأربعة روابط تناسقية مع ذرات نيتروجين الحلقة.</p>
            <div class="info-box info">
              <h5><i class="fas fa-droplet"></i> التكامل الجزيئي في نقل الأكسجين</h5>
              <p>تتيح البيئة الفراغية المنظمة داخل بروتين الهيموغلوبين بذرة الحديد (Fe²⁺) تكوين رابطة سادسة ضعيفة وقابلة للانعكاس مع جزيء الأكسجين (O₂). يتيح هذا التوازن للحديد التقاط الأكسجين في الرئتين حيث يكون الضغط الجزئي للأكسجين مرتفعاً، ثم إطلاقه بسلاسة في الأنسجة التي تعاني من انخفاض الضغط الجزئي.</p>
            </div>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-charging-station"></i> 3. الحديد في السيتوكرومات وسلاسل نقل الإلكترون</h4>
            <p>لا تقتصر كيمياء الحديد الحيوية على نقل الغازات كالأكسجين، بل يمتد دوره الكيميائي إلى الميتوكوندريا حيث يشارك في إنتاج الطاقة. يدخل الحديد في تركيب بروتينات متخصصة تسمى السيتوكرومات (Cytochrome)، والتي تشكل المركبات الأساسية في سلسلة التنفس الخلوي ونقل الإلكترونات (Electron Transport Chain).</p>
            <p>خلال عمليات التنفس الخلوي، تكتسب ذرة الحديد في السيتوكروم إلكتروناً لتحول من الصورة الثلاثية (Fe³⁺) إلى الصورة الثنائية (Fe²⁺)، ثم لا تلبث أن تفقد هذا الإلكترون لتنقله إلى المركب التالي في السلسلة وتعود لـ (Fe³⁺). هذا التأرجح الدوري المستمر يجعل الحديد يعمل كموصل كهربائي جزئي ينقل الطاقة الكيميائية دون أن يتم استهلاكه كمتفاعل نهائي.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-cubes"></i> 4. عناقيد الحديد والكبريت (Iron-Sulfur Clusters)</h4>
            <p>من التراكيب الجزيئية المتقدمة للحديد دخوله في تراكيب مدمجة تُعرف باسم عناقيد الحديد والكبريت (Fe-S Clusters) مثل [2Fe-2S] و [4Fe-4S]. ترتبط هذه العناقيد ببقايا حمض السيستئين الأميني داخل الإنزيمات الحيوية (مثل إنزيم أكونيتاز وإنزيمات مركب الميتوكوندريا الأول).</p>
            <p>وتعمل هذه العناقيد كمستشعرات خلوية لمستويات الأكسجين والتأكسد، بالإضافة إلى أداء دور التحفيز الإنزيمي المباشر.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-exclamation-triangle"></i> 5. الخطورة التأكسدية وتفاعل فنتون (Fenton Reaction)</h4>
            <p>على الرغم من الأهمية المطلقة للحديد، إلا أن وجوده حراً في السيتوبلازم يمثل خطراً جسيماً على سلامة الخلية. يمتلك الحديد الحر القدرة على تحفيز تفاعل كيميائي خطير يُعرف بـ تفاعل فنتون (Fenton Reaction)، حيث يتفاعل الحديد الثنائي مع فوق أكسيد الهيدروجين:</p>
            <div class="formula-box">Fe²⁺ + H₂O₂ → Fe³⁺ + OH⁻ + •OH</div>
            <div class="info-box danger">
              <h5><i class="fas fa-radiation"></i> خطر جذر الهيدروكسيل (•OH)</h5>
              <p>ينتج عن هذا التفاعل جذر الهيدروكسيل (•OH) وهو أحشى وأقوى الجذور الحرة التأكسدية التي تدمر الأغشية الخلوية والـ DNA. لذلك نظم الجسم آليات صارمة لتخزين الحديد ونقله عبر الترانسفيرين (Transferrin) في الدم والفيريتين (Ferritin) بروتين خلايا.</p>
            </div>
            <p><strong>بروتين الفيريتين (Ferritin):</strong> مجمع تخزين كروي مجوف يختزن ما يصل إلى 4500 ذرة حديد داخل نواته البلورية.</p>
            <p><strong>بروتين الترانسفيرين (Transferrin):</strong> ينقل الحديد في مجرى الدم في حالة الأكسدة Fe³⁺ بأمان تام لمنع تفاعل فنتون.</p>
          </div>
        </div>

        <!-- الزنك -->
        <div class="research-section" id="res-zn">
          <div class="section-header-element element-zn">
            <div class="element-badge">Zn</div>
            <div class="element-info-title"><h2>ثانياً: الزنك (Zn) — عنصر التثبيت البنيوي والتحفيز الإنزيمي</h2><p>العدد الذري: 30 | عنصر فلزي انتقالي</p></div>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية والروابط التناسقية للزنك</h4>
            <p>الزنك عنصر فلزي يقع في المجموعة الثانية عشرة من الجدول الدوري، وعدده الذري 30. يوجد الزنك داخل الأنظمة الحيوية حصرياً على صورة أيون موجب ثنائي الشحنة (Zn²⁺). يمتلك الزنك غلافاً إلكترونياً كاملاً من طراز (d10)، مما جعله خافتاً من الناحية التأكسدية؛ فهو لا يتأرجح بين حالات أكسدة مختلفة كالحديد، ولا يشارك في تفاعلات الأكسدة والاختزال المباشرة.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-fingerprint"></i> 2. تثبيت البنية الفراغية وأصابع الزنك (Zinc Fingers)</h4>
            <p>تتمثل القوة الكيميائية الفريدة لأيون الزنك في قدرته الفائقة على تكوين روابط تناسقية رباعية مع ذرات النيتروجين في حمض الهيستيدين (Histidine) وذرات الكبريت في حمض السيستين (Cysteine). يتيح هذا الارتباط إنشاء ثنيات بروتينية صلبة تعرف بـ أصابع الزنك (Zinc Finger Motifs).</p>
            <div class="info-box purple">
              <h5><i class="fas fa-dna"></i> التحكم في التعبير الجيني بواسطة الزنك</h5>
              <p>تعمل أصابع الزنك كقوالب هندسية تسمح لبروتينات العوامل النووية (Transcription Factors) بالقبض الدقيق على مجاري الحمض النووي DNA والتلازم مع تسلسلات قواعده النيتروجينية، مما يعطي الخلايا القدرة على تنظيم قراءة الجينات وتفعيلها بحسب الحاجة الحيوية.</p>
            </div>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-cog"></i> 3. الدور التحفيزي في الإنزيمات: الأنهيدراز الكربوني</h4>
            <p>يدخل الزنك في تركيب أكثر من 300 إنزيم تحفيزي. يعمل أيون الزنك (Zn²⁺) كحمض لويس (Lewis Acid) قوي يكذب جزيء الماء المترابط معه، مما يقلل القيمة الحمضية pKa للماء ويحوله إلى أيون هيدروكسيل نشط عند الرقم الهيدروجيني الفسيولوجي. وأبرز مثال هو إنزيم الأنهيدراز الكربوني (Carbonic Anhydrase):</p>
            <div class="formula-box">CO₂ + H₂O ⇌ HCO₃⁻ + H⁺</div>
          </div>
        </div>

        <!-- المغنيسيوم -->
        <div class="research-section" id="res-mg">
          <div class="section-header-element element-mg">
            <div class="element-badge">Mg</div>
            <div class="element-info-title"><h2>ثالثاً: المغنيسيوم (Mg) — عنصر نقل الطاقة والاستقرار الجيني</h2><p>العدد الذري: 12 | الوزن الذري: 24.30</p></div>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية والشحنة الأيونية للمغنيسيوم</h4>
            <p>المغنيسيوم عنصر فلزي من الفلزات القلوية الترابية (Group 2)، وعدده الذري 12. يوجد في البيئة الخلوية على هيئة أيون موجب ثنائي الشحنة (Mg²⁺). يمتلك أيون المغنيسيوم كثافة شحنة عالية جداً بفضل صغر حجمه الأيوني، مما يجعله محاطاً بغلاف تميه كيميائي مائي كثيف، ويمنحه جلي الألفة الكيميائية للارتباط بالمجموعات الكيميائية سالبة الشحنة، وعلى رأسها مجموعات الفوسفات (Phosphate Groups).</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-bolt"></i> 2. معقد المغنيسيوم وجزيء ATP</h4>
            <p>يُعد جزيء أدينوسين ثالثي الفوسفات (ATP) العملة الأساسية لنقل الطاقة داخل الخلايا. يحمل هذا الجزيء أربع شحنات سالبة كيميائية في الظروف الفسيولوجية، مما يجعل مجموعات الفوسفات تتنافر وتتأثر بالاضطراب الفراغي، وهذا يأتي الدور الكيميائي الحاسم للمغنيسيوم؛ حيث يندمج مع جزيء ATP ليشكل معقداً مخلبياً يعرف بـ [Mg-ATP]²⁻.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-key"></i> 3. المغنيسيوم كالمفتاح الحقيقي لاستخدام الطاقة</h4>
            <p>إن أغلب الإنزيمات المستهلكة للطاقة (مثل الكاينازات Kinases) لا تتفاعل إطلاقاً مع جزيء ATP الحر؛ بل تعرف حصرياً معقد Mg-ATP. يعمل المغنيسيوم على الجانب الإلكترونية للمجموعات، مما يجعل الفوسفات الطرفية (γ-phosphate) محبة للهجوم النيكلوفيلي وسهلة الانفصال والنقل الطاقة.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-dna"></i> 4. تثبيت الهياكل الجينية والريبوسومات</h4>
            <p>يمتد دور المغنيسيوم الكيميائي ليشكل الدعامة الأساسية للأحماض النووية (DNA و RNA). يعمل المغنيسيوم على التعادل الكهرومغناطيسي مع مصفوفة الفوسفات ثنائية السلسلة في الـ DNA، مما يمنع تفكك السلسلتين المزدوجتين تحت تأثير التنافر الكهربائي السالب. كما يعد المغنيسيوم شرطاً لتماسك وحدات الريبوسومات (Ribosomal Subunits) لتخليق وتصنيع البروتينات.</p>
          </div>
        </div>

        <!-- اليود -->
        <div class="research-section" id="res-i">
          <div class="section-header-element element-i">
            <div class="element-badge">I</div>
            <div class="element-info-title"><h2>رابعاً: اليود (I) — عنصر يدخل في بناء الهرمونات ببراعة تساهمية</h2><p>العدد الذري: 53 | الوزن الذري: 126.90</p></div>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية والامتصاص النسيجي لليود</h4>
            <p>اليود عنصر لا فلزي قلوي ينتمي لعائلة الهالوجينات (Group 17) بالجدول الدوري، وعدده الذري 53. يتمتع اليود بنصف قطر ذري كبير وقابلية للاستقطاب الإلكتروني، يُلتقط اليود الغذائي في القناة الهضمية على صورة أيون يوديد (I⁻)، حيث يتم ضخه بنشاط إلى داخل خلايا الغدة الدرقية عبر ناقل اليوديد والصوديوم (NIS Membrane Pump) بكفاءة تزيد مئات المرات عن مستواه في الدم.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-flask"></i> 2. الآلية الإنزيمية لبناء هرمونات الغدة الدرقية</h4>
            <p>على عكس المعادن الأخرى التي تؤدي دور المساعد الإنزيمي المؤقت، يدخل اليود في روابط تساهمية كربونية-يودية (C-I Bonds) دائمة داخل بنية المركب العضوي. تتضمن هذه العملية خطوات كيميائية متعاقبة تحت إشراف إنزيم بيروكسيداز الغدة الدرقية (TPO):</p>
            <ul>
              <li>أكسدة أيون اليوديد (I⁻) باستخدام فوق أكسيد الهيدروجين إلى صورة يودية نشطة.</li>
              <li>إدراج اليود تساهمياً في الحلقات العطرية لحمض التيروزين (Tyrosine) الأميني داخل بروتين الثايروغلوبولين.</li>
              <li>اقتران حلقة أحادي يودوتيروزين (MIT) مع ثنائي يودوتيروزين (DIT) لتشكيل هرمون T₃، أو اقتران حلقتين ثنائيتين لتشكيل هرمون T₄.</li>
            </ul>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-balance-scale"></i> 3. الفارق الذري بين هرموني T3 و T4</h4>
            <p>يحمل هرمون الثيروكسين (T₄) أربع ذرات يود وهو الهرمون الرئيسي المفرز في الدم، بينما يحمل هرمون ثلاثي يودوثايرونين (T₃) ثلاث ذرات يود فقط. وعلى الرغم من حقيقة أن T₄ أكثر وفرة، فإن T₃ يمتلك ألفة ارتباط بالمستقبلات النووية تزيد بمقدار 10 إلى 15 ضعفاً عن T₄، مما يبرز الأثر الفائق لنزع ذرة يود واحدة على الفعالية الفسيولوجية.</p>
          </div>
        </div>

        <!-- السيلينيوم -->
        <div class="research-section" id="res-se">
          <div class="section-header-element element-se">
            <div class="element-badge">Se</div>
            <div class="element-info-title"><h2>خامساً: السيلينيوم (Se) — درع مضاد الأكسدة الحيوي</h2><p>العدد الذري: 34 | الوزن الذري: 78.96</p></div>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية وحمض السيلينوسيستين النادر</h4>
            <p>السيلينيوم عنصر لا فلزي يقع في المجموعة السادسة عشرة (الكالكوجينات Chalcogens) بالجدول الدوري أسفل عنصر الكبريت مباشرة، وعدده الذري 34. يتشابه السيلينيوم كيميائياً مع الكبريت، ولكنه يمتاز بكثافة إلكترونية ونصف قطر ذري أكبر، مما يجعل رابطة (Se-H) أكثر حامضية وأسهل في التأين مقارنة برابطة (S-H). يدمج الجسم السيلينيوم في الحمض الأميني الخاص رقم 21 (سيلينوسيستين).</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-shield-virus"></i> 2. إنزيمات السيلينوبروتين والدفاع المضاد للأكسدة</h4>
            <p>يُشفَّر حمض السيلينوسيستين جينياً عبر شفرة خاصة (UGA Stop Codon) بمساعدة تراكيب SECIS المتقدمة في الـ mRNA. يدخل هذا الحمض في بروتينات حيوية تسمى (Selenoproteins)، وعلى رأسها مجموعة إنزيمات الجلوتاثيون بيروكسيداز (Glutathione Peroxidase - GPx).</p>
            <div class="formula-box">H₂O₂ + 2GSH → 2H₂O + GSSG</div>
            <p>يعمل السيلينيوم في هذا الإنزيم على التفكيك المباشر والآمن لمركبات بيروكسيد الهيدروجين والبيروكسيدات العضوية، وتحويلها إلى ماء وكحول دون إطلاق أي جذور حرة ثانوية، مما يحمي الغشاء الخلوية والدهون الفسفورية من التلف.</p>
          </div>

          <div class="subsection">
            <h4><i class="fas fa-handshake"></i> 3. التكامل الكيميائي الحيوي بين السيلينيوم واليود</h4>
            <p>يمتد التكامل الكيميائي بين العناصر عبر إنزيمات نازعات اليود (Deiodinases)، وهي إنزيمات سيلينوبروتينية تحتوي على السيلينيوم في مواقعها النشطة. تقوم هذه الإنزيمات بنزع ذرة يود من هرمون T₄ الخامل لتحويله إلى T₃ النشط. وبذلك، فإن بناء هرمون الغدة الدرقية يتطلب اليود، بينما تفعيله النسيجي يتطلب السيلينيوم.</p>
          </div>
        </div>

        <!-- جدول المقارنة -->
        <div class="research-section" id="res-table">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-table"></i></div>
            <div class="element-info-title"><h2>جدول المقارنة الشامل للخصائص والوظائف</h2><p>ملخص الخصائص الكيميائية والوظائف الحيوية للعناصر الخمسة</p></div>
          </div>
          <div class="comparison-table-wrapper">
            <table class="comparison-table">
              <thead><tr><th>العنصر</th><th>الصورة الأيونية</th><th>الخاصية الكيميائية</th><th>الوظيفة الجزئية</th></tr></thead>
              <tbody>
                <tr><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#b91c1c,#7f1d1d);">Fe</span> الحديد</div></td><td>Fe²⁺ / Fe³⁺</td><td>التناوب بين حالات الأكسدة وتكوين الهيم</td><td>نقل O₂ عبر الهيموغلوبين ونقل الإلكترونات في السيتوكرومات</td></tr>
                <tr><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#0891b2,#155e75);">Zn</span> الزنك</div></td><td>Zn²⁺ (ثابتة)</td><td>روابط تناسقية رباعية مع Cys/His (حمض لويس)</td><td>تثبيت أشكال أصابع الزنك للـ DNA وتحفيز الأنهيدراز الكربوني</td></tr>
                <tr><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#16a34a,#15803d);">Mg</span> المغنيسيوم</div></td><td>Mg²⁺</td><td>الألفة العالية لشحنات الفوسفات الكثيفة</td><td>تكوين معقد [Mg-ATP]²⁻ وترتيب الـ DNA والريبوسومات</td></tr>
                <tr><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#7c3aed,#5b21b6);">I</span> اليود</div></td><td>I⁻ / يود عضوي</td><td>تكوين روابط تساهمية كربونية-يودية دائمة</td><td>بناء الهرمونات الدرقية (T₃ & T₄) للتحكم بالمستقلب الخلوي</td></tr>
                <tr><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#ea580c,#c2410c);">Se</span> السيلينيوم</div></td><td>سيلينوسيستين</td><td>حمض أميني نادر ذو قدرة اختزال فائقة</td><td>الموقع النشط لإنزيم GPx لمكافحة التأكسد وإنزيمات Deiodinases</td></tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- الخاتمة -->
        <div class="research-section" id="res-conclusion">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-flag-checkered"></i></div>
            <div class="element-info-title"><h2>الخاتمة والتوصيات العلمية</h2><p>ملخص النتائج والتوصيات البحثية</p></div>
          </div>
          <div class="conclusion-box">
            <h3><i class="fas fa-lightbulb"></i> الخاتمة</h3>
            <p>تكشف هذه الدراسة الكيميائية الحيوية الموسعة للعناصر الخمسة (الحديد، الزنك، المغنيسيوم، اليود، والسيلينيوم) عن حقيقة جوهرية: إن المظاهر الفسيولوجية المعقدة لجسم الإنسان تستند كلياً إلى سلوكيات تفاعلية دقيقة تحدث على المستوى الذري والإلكتروني.</p>
            <p>فالتوازنات الدقيقة بين هذه العناصر، وتوافق هندستها التناسقية مع الجزيئات الحيوية الكبرى، هي ما يضمن كفاءة تحويل الطاقة، حماية الخلايا من التلف التأكسدي، والتعبير الجيني المضبوط.</p>
            <h3 style="margin-top:18px;"><i class="fas fa-bullseye"></i> التوصيات</h3>
            <p>نوصي بالتوسع في أبحاث علم الكيمياء التناسقية الحيوية (Bioinorganic Chemistry) لفهم التدخلات المباشرة بين النقص الغذائي لهذه العناصر والاضطرابات الجزئية الناتجة عنها.</p>
          </div>
        </div>

        <!-- المراجع -->
        <div class="research-section" id="res-refs">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-book"></i></div>
            <div class="element-info-title"><h2>المراجع العلمية الأصلية</h2><p>المصادر والدراسات المعتمدة في البحث</p></div>
          </div>
          <ol class="references-list">
            <li>Rolić, T., et al. (2025). Iron Metabolism, Calcium, Magnesium and Trace Elements: A Review. <em>Biological Trace Element Research</em>. <a href="https://doi.org/10.1007/s12011-024-04289-z" target="_blank">https://doi.org/10.1007/s12011-024-04289-z</a></li>
            <li>Teaching the Chemical Elements in Biochemistry: Elemental Biology and Metallomics. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC9303777" target="_blank">https://pmc.ncbi.nlm.nih.gov/articles/PMC9303777</a></li>
            <li>Trace Elements in Human Nutrition (II) – An Update. <a href="https://pmc.ncbi.nlm.nih.gov/articles/6993532" target="_blank">https://pmc.ncbi.nlm.nih.gov/articles/6993532</a></li>
            <li>Biochemistry, Nutrients. NCBI Bookshelf. <a href="https://www.ncbi.nlm.nih.gov/books/NBK554545" target="_blank">https://www.ncbi.nlm.nih.gov/books/NBK554545</a></li>
            <li>Essential Metals in Health and Disease. <em>Chemico-Biological Interactions</em>. <a href="https://doi.org/10.1016/j.cbi.2022.110173" target="_blank">https://doi.org/10.1016/j.cbi.2022.110173</a></li>
          </ol>
        </div>

        <button class="back-to-top" id="back-to-top"><i class="fas fa-chevron-up"></i></button>
      </div>
    </div>

    <!-- إعداد الاختبار -->
    <div class="screen" id="quiz-setup">
      <div class="header">
        <h2><i class="fas fa-file-alt"></i> إعداد الاختبار</h2>
        <div class="user-actions"><button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button></div>
      </div>
      <div class="quiz-setup">
        <h3>اختر نوع الاختبار</h3>
        <div class="quiz-options">
          <div class="quiz-option selected" data-mode="practice">
            <i class="fas fa-graduation-cap"></i>
            <h4>وضع الممارسة</h4>
            <p>تعلم بالسرعة التي تناسبك مع تصحيح فوري</p>
          </div>
          <div class="quiz-option" data-mode="exam">
            <i class="fas fa-clock"></i>
            <h4>وضع الامتحان</h4>
            <p>اختبر نفسك في ظروف مشابهة للامتحان</p>
          </div>
        </div>
        <div class="setting-item">
          <div class="setting-info"><h4>عدد الأسئلة</h4><p>اختر عدد الأسئلة في الاختبار</p></div>
          <select id="question-count"><option value="10">10 أسئلة</option><option value="20">20 سؤالاً</option><option value="30">30 سؤالاً</option><option value="50" selected>50 سؤالاً (كاملة)</option></select>
        </div>
        <div class="setting-item">
          <div class="setting-info"><h4>مستوى الصعوبة</h4><p>اختر مستوى الصعوبة المناسب</p></div>
          <select id="difficulty"><option value="all">جميع المستويات</option><option value="easy">سهل</option><option value="medium" selected>متوسط</option><option value="hard">صعب</option></select>
        </div>
        <div class="setting-item">
          <div class="setting-info"><h4>خلط الأسئلة</h4><p>ترتيب عشوائي للأسئلة في كل مرة</p></div>
          <label class="toggle-switch"><input type="checkbox" id="shuffle-questions" checked><span class="slider"></span></label>
        </div>
        <div class="quiz-controls">
          <button class="btn" id="start-quiz"><i class="fas fa-play"></i> بدء الاختبار</button>
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-times"></i> إلغاء</button>
        </div>
      </div>
    </div>

    <!-- شاشة الاختبار -->
    <div class="screen" id="quiz">
      <div class="header">
        <h2><i class="fas fa-file-alt"></i> اختبار الكيمياء</h2>
        <div class="user-actions">
          <div class="quiz-timer" id="quiz-timer"><i class="fas fa-hourglass-half"></i> 30:00</div>
          <button class="btn btn-danger" id="end-quiz"><i class="fas fa-stop"></i> إنهاء</button>
        </div>
      </div>
      <div class="quiz-container">
        <div class="quiz-content">
          <div class="quiz-header">
            <div class="quiz-info">
              <div id="question-counter">السؤال 1 من 50</div>
              <div id="score-display">النقاط: 0</div>
            </div>
          </div>
          <div class="progress-bar"><div class="progress" id="progress"></div></div>
          <div class="question" id="question" style="margin-top:18px;"></div>
          <div class="options" id="options"></div>
          <button class="btn" id="next-btn" disabled><i class="fas fa-arrow-left"></i> التالي</button>
          <div class="explanation" id="explanation"></div>
        </div>
        <div class="whiteboard-container">
          <div class="whiteboard-header">
            <h3><i class="fas fa-chalkboard"></i> السبورة التفاعلية</h3>
            <button class="btn btn-outline" id="clear-whiteboard" style="padding:7px 12px;font-size:.78em;"><i class="fas fa-eraser"></i> مسح</button>
          </div>
          <iframe src="https://www.canva.com/design/DAG37-MyN6M/31ae22xdkRDIFafdn72jBw/view?embed" class="whiteboard-frame" id="whiteboard" allowfullscreen></iframe>
          <div class="whiteboard-info"><p>استخدم السبورة لحل المسائل وكتابة الملاحظات</p></div>
        </div>
      </div>
    </div>

    <!-- النتائج -->
    <div class="screen" id="results">
      <div class="header">
        <h2><i class="fas fa-chart-bar"></i> نتائج الاختبار</h2>
        <div class="user-actions">
          <button class="btn" onclick="navigateTo('quiz-setup')"><i class="fas fa-redo"></i> اختبار جديد</button>
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-home"></i> الرئيسية</button>
        </div>
      </div>
      <div class="result-container">
        <div class="user-info" style="background:rgba(99,102,241,.1);padding:20px;border-radius:16px;margin-bottom:22px;border:1px solid rgba(99,102,241,.25);text-align:right;">
          <p style="color:#cbd5e1;font-size:.9em;margin-bottom:6px;"><i class="fas fa-user-graduate"></i> <strong style="color:#f1f5f9;">الاسم:</strong> <span id="result-username">-</span></p>
          <p style="color:#cbd5e1;font-size:.9em;margin-bottom:6px;"><i class="fas fa-clock"></i> <strong style="color:#f1f5f9;">الوقت:</strong> <span id="time-taken">--:--</span></p>
          <p style="color:#cbd5e1;font-size:.9em;margin-bottom:6px;"><i class="fas fa-chart-bar"></i> <strong style="color:#f1f5f9;">النتيجة:</strong> <span id="result-score">0</span> من <span id="total-questions">50</span></p>
          <p style="color:#cbd5e1;font-size:.9em;margin-bottom:0;"><i class="fas fa-medal"></i> <strong style="color:#f1f5f9;">رتبتك:</strong> <span id="user-rank">غاز خامل</span></p>
        </div>
        <h2 style="color:#f1f5f9;margin-bottom:12px;display:flex;align-items:center;justify-content:center;gap:10px;font-size:1.2em;"><i class="fas fa-trophy"></i> نتيجة الاختبار</h2>
        <div class="result-score" id="final-score">0%</div>
        <div class="result-chart"><canvas id="result-chart" width="230" height="230"></canvas></div>
        <div class="comparison" id="comparison"></div>
        <div class="weak-lessons" id="weak-lessons"></div>
        <div class="advanced-report">
          <div class="report-section">
            <h3><i class="fas fa-chart-line"></i> تحليل أدائك</h3>
            <div class="report-chart"><canvas id="performance-chart"></canvas></div>
            <div class="improvement-tips">
              <h4><i class="fas fa-lightbulb"></i> نصائح للتحسين</h4>
              <ul id="improvement-tips"></ul>
            </div>
          </div>
        </div>
        <div class="quiz-controls" style="margin-top:22px;">
          <button class="btn" onclick="navigateTo('quiz-setup')"><i class="fas fa-redo"></i> اختبار جديد</button>
          <button class="btn btn-outline" onclick="navigateTo('progress')"><i class="fas fa-chart-line"></i> عرض التقدم</button>
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-home"></i> الرئيسية</button>
        </div>
      </div>
    </div>

    <!-- التقدم -->
    <div class="screen" id="progress">
      <div class="header">
        <h2><i class="fas fa-chart-line"></i> تتبع التقدم</h2>
        <div class="user-actions"><button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button></div>
      </div>
      <div class="progress-stats">
        <div class="stat-card"><i class="fas fa-chart-bar"></i><h3>متوسط النتائج</h3><div class="stat-value" id="average-score">0%</div></div>
        <div class="stat-card"><i class="fas fa-check-circle"></i><h3>أفضل نتيجة</h3><div class="stat-value" id="best-score">0%</div></div>
        <div class="stat-card"><i class="fas fa-list-ol"></i><h3>عدد الاختبارات</h3><div class="stat-value" id="total-quizzes">0</div></div>
        <div class="stat-card"><i class="fas fa-clock"></i><h3>إجمالي الوقت</h3><div class="stat-value" id="total-time">0 د</div></div>
      </div>
      <div class="progress-history">
        <h3><i class="fas fa-history"></i> تاريخ الاختبارات</h3>
        <table class="history-table">
          <thead><tr><th>التاريخ</th><th>النوع</th><th>النتيجة</th><th>الوقت</th><th>الأسئلة</th></tr></thead>
          <tbody id="quiz-history"></tbody>
        </table>
      </div>
    </div>

    <!-- الإعدادات -->
    <div class="screen" id="settings">
      <div class="header">
        <h2><i class="fas fa-cog"></i> الإعدادات</h2>
      </div>
      <div class="settings-container">
        <div class="settings-nav">
          <button class="settings-nav-btn active" data-panel="account"><i class="fas fa-user-cog"></i> الحساب</button>
          <button class="settings-nav-btn" data-panel="appearance"><i class="fas fa-palette"></i> المظهر</button>
          <button class="settings-nav-btn" data-panel="notifications"><i class="fas fa-bell"></i> الإشعارات</button>
          <button class="settings-nav-btn" data-panel="quiz"><i class="fas fa-file-alt"></i> الاختبارات</button>
          <button class="settings-nav-btn" data-panel="language"><i class="fas fa-language"></i> اللغة</button>
          <button class="settings-nav-btn" data-panel="data"><i class="fas fa-database"></i> البيانات</button>
          <button class="settings-nav-btn" data-panel="advanced"><i class="fas fa-sliders-h"></i> متقدم</button>
        </div>

        <!-- الحساب -->
        <div class="settings-panel active" id="panel-account">
          <div class="settings-group">
            <h3><i class="fas fa-id-card"></i> معلومات الحساب</h3>
            <div class="setting-item">
              <div class="setting-info"><h4>الاسم الكامل</h4><p>الاسم المعروض في المنصة</p></div>
              <input type="text" id="username" value="">
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>اسم المستخدم</h4><p>معرف فريد لحسابك</p></div>
              <input type="text" id="user-username" value="" readonly>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>البريد الإلكتروني</h4><p>لاستقبال الإشعارات والتنبيهات</p></div>
              <input type="email" id="user-email" value="">
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>المستوى التعليمي</h4><p>مستواك الحالي في الكيمياء</p></div>
              <select id="user-level-select"><option value="beginner">مبتدئ</option><option value="intermediate">متوسط</option><option value="advanced">متقدم</option></select>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>تغيير كلمة المرور</h4><p>قم بتحديث كلمة مرورك</p></div>
              <button class="btn btn-outline" onclick="openChangePassword()"><i class="fas fa-key"></i> تغيير</button>
            </div>
          </div>

          <div class="settings-group">
            <h3><i class="fas fa-image"></i> الصورة الرمزية</h3>
            <div class="avatar-picker" id="avatar-picker"></div>
          </div>
        </div>

        <!-- المظهر -->
        <div class="settings-panel" id="panel-appearance">
          <div class="settings-group">
            <h3><i class="fas fa-moon"></i> الوضع العام</h3>
            <div class="setting-item">
              <div class="setting-info"><h4>الوضع الليلي</h4><p>مظهر داكن مريح للعيون</p></div>
              <label class="toggle-switch"><input type="checkbox" id="dark-mode" checked><span class="slider"></span></label>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>الوضع المضغوط</h4><p>تقليل الحشو لعرض محتوى أكثر</p></div>
              <label class="toggle-switch"><input type="checkbox" id="compact-mode"><span class="slider"></span></label>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>تفعيل الحركات</h4><p>الرسوم المتحركة والانتقالات السلسة</p></div>
              <label class="toggle-switch"><input type="checkbox" id="animations-enabled" checked><span class="slider"></span></label>
            </div>
          </div>

          <div class="settings-group">
            <h3><i class="fas fa-tint"></i> لون الواجهة</h3>
            <div class="theme-picker" id="theme-picker"></div>
          </div>

          <div class="settings-group">
            <h3><i class="fas fa-font"></i> حجم الخط</h3>
            <div class="font-size-picker">
              <button class="font-size-btn" data-size="14">ص</button>
              <button class="font-size-btn active" data-size="16">ص</button>
              <button class="font-size-btn" data-size="18">ص</button>
            </div>
          </div>
        </div>

        <!-- الإشعارات -->
        <div class="settings-panel" id="panel-notifications">
          <div class="settings-group">
            <h3><i class="fas fa-bell"></i> تفضيلات الإشعارات</h3>
            <div class="setting-item">
              <div class="setting-info"><h4>تفعيل الإشعارات</h4><p>عرض الإشعارات في المنصة</p></div>
              <label class="toggle-switch"><input type="checkbox" id="notifications-enabled" checked><span class="slider"></span></label>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>تنبيهات الاختبارات</h4><p>إشعارات عند إتمام الاختبارات</p></div>
              <label class="toggle-switch"><input type="checkbox" id="quiz-notif" checked><span class="slider"></span></label>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>تنبيهات الإنجازات</h4><p>إشعارات عند تحقيق إنجاز جديد</p></div>
              <label class="toggle-switch"><input type="checkbox" id="achieve-notif" checked><span class="slider"></span></label>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>تذكير يومي</h4><p>تذكير بمواصلة التعلم</p></div>
              <label class="toggle-switch"><input type="checkbox" id="daily-reminder"><span class="slider"></span></label>
            </div>
          </div>
        </div>

        <!-- الاختبارات -->
        <div class="settings-panel" id="panel-quiz">
          <div class="settings-group">
            <h3><i class="fas fa-sliders-h"></i> تفضيلات الاختبارات</h3>
            <div class="setting-item">
              <div class="setting-info"><h4>عدد الأسئلة الافتراضي</h4><p>في كل اختبار جديد</p></div>
              <input type="number" id="default-question-count" value="20" min="5" max="50">
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>مستوى الصعوبة الافتراضي</h4><p>مستوى الصعوبة التلقائي</p></div>
              <select id="default-difficulty">
                <option value="all">جميع المستويات</option>
                <option value="easy">سهل</option>
                <option value="medium" selected>متوسط</option>
                <option value="hard">صعب</option>
              </select>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>الشرح الفوري</h4><p>عرض التفسير مباشرة بعد الإجابة</p></div>
              <label class="toggle-switch"><input type="checkbox" id="instant-explain" checked><span class="slider"></span></label>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>مدة المؤقت</h4><p>بالدقائق في وضع الامتحان</p></div>
              <select id="timer-duration"><option value="15">15 دقيقة</option><option value="30" selected>30 دقيقة</option><option value="45">45 دقيقة</option><option value="60">60 دقيقة</option></select>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>حفظ تلقائي للنتائج</h4><p>حفظ النتائج في السجل تلقائياً</p></div>
              <label class="toggle-switch"><input type="checkbox" id="auto-save" checked><span class="slider"></span></label>
            </div>
          </div>
        </div>

        <!-- اللغة -->
        <div class="settings-panel" id="panel-language">
          <div class="settings-group">
            <h3><i class="fas fa-globe"></i> إعدادات اللغة</h3>
            <div class="setting-item">
              <div class="setting-info"><h4>لغة الواجهة</h4><p>اللغة المعروضة في المنصة</p></div>
              <select id="interface-language">
                <option selected>العربية</option>
                <option>English</option>
              </select>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>لغة المصطلحات العلمية</h4><p>لغة المصطلحات الكيميائية</p></div>
              <select id="terms-language">
                <option selected>العربية</option>
                <option>English</option>
                <option>مختلط</option>
              </select>
            </div>
          </div>
        </div>

        <!-- البيانات -->
        <div class="settings-panel" id="panel-data">
          <div class="settings-group">
            <h3><i class="fas fa-database"></i> إدارة البيانات</h3>
            <div class="setting-item">
              <div class="setting-info"><h4>حجم البيانات المستخدمة</h4><p id="data-size">-- KB</p></div>
              <button class="btn btn-outline" onclick="calculateDataSize()"><i class="fas fa-calculator"></i> حساب</button>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>تصدير البيانات</h4><p>تنزيل نسخة احتياطية كاملة</p></div>
              <button class="btn btn-outline" id="export-data"><i class="fas fa-download"></i> تصدير</button>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>استيراد البيانات</h4><p>استعادة نسخة احتياطية</p></div>
              <label class="btn btn-outline" style="cursor:pointer;"><i class="fas fa-upload"></i> استيراد<input type="file" id="import-file" accept=".json" hidden></label>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>مسح سجل الاختبارات</h4><p>حذف جميع نتائج الاختبارات فقط</p></div>
              <button class="btn btn-warning" onclick="clearResults()"><i class="fas fa-history"></i> مسح السجل</button>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>حذف جميع البيانات</h4><p>مسح كل شيء نهائياً (لا يمكن التراجع)</p></div>
              <button class="btn btn-danger" onclick="clearAllData()"><i class="fas fa-trash"></i> حذف الكل</button>
            </div>
          </div>
        </div>

        <!-- متقدم -->
        <div class="settings-panel" id="panel-advanced">
          <div class="settings-group">
            <h3><i class="fas fa-keyboard"></i> اختصارات لوحة المفاتيح</h3>
            <div class="setting-item"><div class="setting-info"><h4>فتح/إغلاق السبورة</h4><p>في شاشة الاختبار</p></div><kbd style="background:rgba(255,255,255,.1);padding:4px 10px;border-radius:6px;font-size:.85em;">Ctrl + B</kbd></div>
            <div class="setting-item"><div class="setting-info"><h4>السؤال التالي</h4><p>الانتقال للسؤال التالي</p></div><kbd style="background:rgba(255,255,255,.1);padding:4px 10px;border-radius:6px;font-size:.85em;">Enter</kbd></div>
            <div class="setting-item"><div class="setting-info"><h4>اختيار إجابة</h4><p>اختيار الخيارات 1-4</p></div><kbd style="background:rgba(255,255,255,.1);padding:4px 10px;border-radius:6px;font-size:.85em;">1 2 3 4</kbd></div>
          </div>
          <div class="settings-group">
            <h3><i class="fas fa-code"></i> معلومات المطور</h3>
            <div class="setting-item"><div class="setting-info"><h4>الإصدار</h4><p>رقم الإصدار الحالي</p></div><span style="color:var(--cyan);font-weight:700;">v2.0.0</span></div>
            <div class="setting-item"><div class="setting-info"><h4>المطور</h4><p>تميم الزهراني</p></div><span style="color:#fbbf24;font-weight:700;">👑 القائد</span></div>
          </div>
          <div class="settings-group">
            <h3><i class="fas fa-undo"></i> استعادة</h3>
            <div class="setting-item">
              <div class="setting-info"><h4>إعادة تعيين الإعدادات</h4><p>استعادة الإعدادات الافتراضية</p></div>
              <button class="btn btn-warning" onclick="resetSettings()"><i class="fas fa-redo"></i> إعادة تعيين</button>
            </div>
          </div>
        </div>

        <div class="quiz-controls">
          <button class="btn" id="save-settings"><i class="fas fa-save"></i> حفظ الإعدادات</button>
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-times"></i> إلغاء</button>
        </div>
      </div>
    </div>

    <!-- المحاكاة -->
    <div class="screen" id="simulation">
      <div class="header">
        <h2><i class="fas fa-flask"></i> المحاكاة والتجارب</h2>
        <div class="user-actions"><button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button></div>
      </div>
      <div class="simulation-container">
        <h2><i class="fas fa-balance-scale"></i> محاكاة موازنة المعادلات الكيميائية</h2>
        <p>استخدم هذه المحاكاة التفاعلية لموازنة المعادلات الكيميائية واكتشاف مبادئ حفظ الكتلة</p>
        <iframe src="https://phet.colorado.edu/sims/html/balancing-chemical-equations/latest/balancing-chemical-equations_ar.html" class="simulation-frame" allowfullscreen></iframe>
        <div class="simulation-info">
          <h3><i class="fas fa-info-circle"></i> معلومات عن المحاكاة</h3>
          <p>هذه المحاكاة مقدمة من مشروع PhET التفاعلي بجامعة كولورادو. يمكنك استخدامها لموازنة المعادلات الكيميائية، ومشاهدة كيفية تفاعل الذرات، وفهم مبدأ حفظ الكتلة في التفاعلات الكيميائية.</p>
        </div>
      </div>
    </div>

    <!-- الإشعارات -->
    <div class="screen" id="notifications">
      <div class="header">
        <h2><i class="fas fa-bell"></i> الإشعارات</h2>
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button>
          <button class="btn btn-danger" id="clear-notifications"><i class="fas fa-trash"></i> مسح الكل</button>
        </div>
      </div>
      <div class="notifications-container" id="notifications-list"></div>
    </div>

    <!-- الملف الشخصي -->
    <div class="screen" id="profile">
      <div class="header">
        <h2><i class="fas fa-user"></i> الملف الشخصي</h2>
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button>
          <button class="btn" id="save-profile"><i class="fas fa-save"></i> حفظ</button>
        </div>
      </div>
      <div class="profile-container">
        <div class="profile-header">
          <div class="profile-avatar" id="profile-avatar-display"><i class="fas fa-user"></i></div>
          <div class="profile-info">
            <div class="profile-name">
              <span id="profile-username-text">-</span>
              <span id="profile-rank" class="rank-badge rank-inert">غاز خامل</span>
            </div>
            <p id="profile-section">الشعبة: -</p>
            <p id="profile-level">المستوى: مبتدئ</p>
            <p id="profile-email">-</p>
          </div>
        </div>
        <div class="profile-stats">
          <div class="profile-stat"><div class="profile-stat-value" id="profile-total-quizzes">0</div><div class="profile-stat-label">إجمالي الاختبارات</div></div>
          <div class="profile-stat"><div class="profile-stat-value" id="profile-average-score">0%</div><div class="profile-stat-label">متوسط النتائج</div></div>
          <div class="profile-stat"><div class="profile-stat-value" id="profile-best-score">0%</div><div class="profile-stat-label">أفضل نتيجة</div></div>
          <div class="profile-stat"><div class="profile-stat-value" id="profile-total-time">0 د</div><div class="profile-stat-label">إجمالي الوقت</div></div>
        </div>
        <div class="settings-group" style="margin-top:24px;">
          <h3><i class="fas fa-pen"></i> السيرة الذاتية</h3>
          <textarea id="profile-bio" class="form-control" rows="4" placeholder="اكتب نبذة عن نفسك..." style="width:100%;"></textarea>
        </div>
      </div>
    </div>

  </main>
</div>

<!-- Modal -->
<div class="modal-overlay" id="modal-overlay">
  <div class="modal">
    <h3 id="modal-title">تأكيد</h3>
    <p id="modal-message">هل أنت متأكد؟</p>
    <div class="modal-actions">
      <button class="btn btn-outline" onclick="closeModal()">إلغاء</button>
      <button class="btn btn-danger" id="modal-confirm">تأكيد</button>
    </div>
  </div>
</div>

<!-- Toast -->
<div class="toast" id="toast"><i class="fas fa-info-circle"></i><span id="toast-message"></span></div>

<div class="url-indicator" id="url-indicator" title="اضغط لنسخ الرابط"></div>

<audio id="correct-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/257/257-preview.mp3" type="audio/mp3"></audio>
<audio id="fail-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/250/250-preview.mp3" type="audio/mp3"></audio>
<audio id="complete-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/255/255-preview.mp3" type="audio/mp3"></audio>

<script>
/* ============================================================
   🌌 Interactive Background
   ============================================================ */
(function(){
  const canvas = document.getElementById('bg-canvas');
  const ctx = canvas.getContext('2d');
  let W, H, particles = [];
  const isMobile = window.innerWidth < 768;
  const COUNT = isMobile ? 40 : 80;
  const MAX_DIST = isMobile ? 100 : 140;
  function resize(){ W = canvas.width = window.innerWidth; H = canvas.height = window.innerHeight; }
  resize(); window.addEventListener('resize', resize);
  const colors = ['#6366f1','#8b5cf6','#06d6a0','#22d3ee','#ec4899'];
  function Particle(){
    this.x = Math.random()*W; this.y = Math.random()*H;
    this.vx = (Math.random()-0.5)*0.5; this.vy = (Math.random()-0.5)*0.5;
    this.r = Math.random()*2+1; this.color = colors[Math.floor(Math.random()*colors.length)];
    this.pulse = Math.random()*Math.PI*2;
  }
  Particle.prototype.update = function(){
    this.x += this.vx; this.y += this.vy;
    if(this.x<0||this.x>W) this.vx*=-1; if(this.y<0||this.y>H) this.vy*=-1;
    this.pulse += 0.03;
  };
  Particle.prototype.draw = function(){
    const rad = this.r + Math.sin(this.pulse)*0.5;
    ctx.beginPath(); ctx.arc(this.x,this.y,Math.max(0.5,rad),0,Math.PI*2);
    ctx.fillStyle = this.color; ctx.shadowBlur = 12; ctx.shadowColor = this.color; ctx.fill(); ctx.shadowBlur = 0;
  };
  for(let i=0;i<COUNT;i++) particles.push(new Particle());
  const mouse = {x:-1000,y:-1000};
  window.addEventListener('mousemove', e=>{ mouse.x=e.clientX; mouse.y=e.clientY; });
  window.addEventListener('mouseleave', ()=>{ mouse.x=-1000; mouse.y=-1000; });
  function animate(){
    ctx.clearRect(0,0,W,H);
    for(let i=0;i<particles.length;i++){
      const p1 = particles[i];
      for(let j=i+1;j<particles.length;j++){
        const p2 = particles[j];
        const dx = p1.x-p2.x, dy = p1.y-p2.y;
        const dist = Math.sqrt(dx*dx+dy*dy);
        if(dist < MAX_DIST){
          const alpha = (1-dist/MAX_DIST)*0.35;
          ctx.beginPath(); ctx.moveTo(p1.x,p1.y); ctx.lineTo(p2.x,p2.y);
          const grad = ctx.createLinearGradient(p1.x,p1.y,p2.x,p2.y);
          grad.addColorStop(0,p1.color); grad.addColorStop(1,p2.color);
          ctx.strokeStyle = grad; ctx.globalAlpha = alpha; ctx.lineWidth = 0.7; ctx.stroke(); ctx.globalAlpha = 1;
        }
      }
      const mdx = p1.x-mouse.x, mdy = p1.y-mouse.y;
      const mdist = Math.sqrt(mdx*mdx+mdy*mdy);
      if(mdist < 180){
        const alpha = (1-mdist/180)*0.6;
        ctx.beginPath(); ctx.moveTo(p1.x,p1.y); ctx.lineTo(mouse.x,mouse.y);
        ctx.strokeStyle = p1.color; ctx.globalAlpha = alpha; ctx.lineWidth = 1; ctx.stroke(); ctx.globalAlpha = 1;
      }
    }
    particles.forEach(p=>{ p.update(); p.draw(); });
    requestAnimationFrame(animate);
  }
  animate();
})();

/* ============================================================
   🧭 Router
   ============================================================ */
const VALID_SCREENS = ['auth','welcome','chemistry-human','quiz-setup','quiz','results','progress','settings','simulation','notifications','profile'];
function getScreenFromHash(){
  let h = window.location.hash.replace(/^#\/?/, '');
  return VALID_SCREENS.includes(h) ? h : (h ? 'welcome' : 'auth');
}
function navigateTo(screen, replace){
  if(!VALID_SCREENS.includes(screen)) screen = 'welcome';
  const hash = '#/' + screen;
  if(replace){ history.replaceState(null,'',hash); handleRoute(); }
  else { if(window.location.hash === hash) handleRoute(); else window.location.hash = hash; }
}
function handleRoute(){
  const screen = getScreenFromHash();
  const user = currentUser();
  if(!user && screen !== 'auth'){ navigateTo('auth', true); return; }
  if(user && screen === 'auth'){ navigateTo('welcome', true); return; }
  if(screen === 'auth'){
    document.getElementById('auth').classList.add('active');
    document.getElementById('main-app').classList.remove('active');
  } else {
    document.getElementById('auth').classList.remove('active');
    document.getElementById('main-app').classList.add('active');
    applyUserToUI(user);
    showScreenInternal(screen);
  }
  updateUrlIndicator();
}
function showScreenInternal(id){
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
  const t = document.getElementById(id);
  if(t) t.classList.add('active');
  const l = document.querySelector(`.nav-link[data-screen="${id}"]`);
  if(l) l.classList.add('active');
  if(id === 'progress') updateProgressUI();
  if(id === 'notifications') renderNotifications();
  if(id === 'profile') updateProfileUI();
  if(id === 'settings') loadSettingsUI();
  if(id === 'chemistry-human') setupResearchLinks();
  if(id === 'welcome') updateDashboard();
  const mc = document.querySelector('.main-content');
  if(mc) mc.scrollTop = 0;
}
function updateUrlIndicator(){
  const ind = document.getElementById('url-indicator');
  if(!ind) return;
  ind.textContent = '🔗 ' + window.location.href;
  ind.classList.add('visible');
}
document.getElementById('url-indicator').addEventListener('click', function(){
  navigator.clipboard.writeText(window.location.href).then(()=>{
    const o = this.textContent; this.textContent = '✅ تم نسخ الرابط!';
    setTimeout(()=>this.textContent = o, 1500);
  });
});

/* ============================================================
   🔐 Auth System
   ============================================================ */
const USERS_KEY = 'chemUsers';
const SESSION_KEY = 'chemSession';
const SETTINGS_KEY = 'chemSettings';
const RESULTS_KEY = 'chemResults';
const NOTIF_KEY = 'chemNotifs';

function getUsers(){ try{ return JSON.parse(localStorage.getItem(USERS_KEY) || '[]'); }catch{ return []; } }
function saveUsers(arr){ localStorage.setItem(USERS_KEY, JSON.stringify(arr)); }
function currentUser(){
  const id = localStorage.getItem(SESSION_KEY);
  if(!id) return null;
  return getUsers().find(u => u.id === id) || null;
}
function hashPass(p){ // simple hash (demo)
  let h = 0;
  for(let i=0;i<p.length;i++){ h = ((h<<5)-h) + p.charCodeAt(i); h |= 0; }
  return 'h' + Math.abs(h).toString(36) + p.length;
}

// Tabs
document.querySelectorAll('.auth-tab').forEach(tab => {
  tab.addEventListener('click', function(){
    document.querySelectorAll('.auth-tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.auth-panel').forEach(p => p.classList.remove('active'));
    this.classList.add('active');
    document.getElementById('panel-' + this.dataset.tab).classList.add('active');
    hideAuthMessages();
  });
});

function showAuthError(msg){
  const el = document.getElementById('auth-error');
  el.textContent = msg; el.classList.add('show');
  document.getElementById('auth-success').classList.remove('show');
  setTimeout(()=>el.classList.remove('show'), 4000);
}
function showAuthSuccess(msg){
  const el = document.getElementById('auth-success');
  el.textContent = msg; el.classList.add('show');
  document.getElementById('auth-error').classList.remove('show');
  setTimeout(()=>el.classList.remove('show'), 4000);
}
function hideAuthMessages(){
  document.getElementById('auth-error').classList.remove('show');
  document.getElementById('auth-success').classList.remove('show');
}
function togglePass(id, btn){
  const inp = document.getElementById(id);
  if(inp.type === 'password'){ inp.type = 'text'; btn.innerHTML = '<i class="fas fa-eye-slash"></i>'; }
  else { inp.type = 'password'; btn.innerHTML = '<i class="fas fa-eye"></i>'; }
}
function showForgot(e){
  e.preventDefault();
  showToast('يرجى التواصل مع المشرف لإعادة تعيين كلمة المرور', 'info');
}

// Login
document.getElementById('login-form').addEventListener('submit', function(e){
  e.preventDefault();
  const id = document.getElementById('login-identifier').value.trim().toLowerCase();
  const pass = document.getElementById('login-password').value;
  if(!id || !pass){ showAuthError('يرجى إدخال جميع البيانات'); return; }
  const users = getUsers();
  const user = users.find(u => u.email.toLowerCase() === id || u.username.toLowerCase() === id);
  if(!user){ showAuthError('❌ لم يتم العثور على الحساب'); return; }
  if(user.password !== hashPass(pass)){ showAuthError('❌ كلمة المرور غير صحيحة'); return; }
  localStorage.setItem(SESSION_KEY, user.id);
  loadSettings();
  showToast('مرحباً بك، ' + user.name + ' 👋', 'success');
  addNotification('تسجيل الدخول', `مرحباً ${user.name}!`, 'info');
  navigateTo('welcome', true);
});

// Signup
document.getElementById('signup-form').addEventListener('submit', function(e){
  e.preventDefault();
  const name = document.getElementById('signup-name').value.trim();
  const username = document.getElementById('signup-username').value.trim();
  const email = document.getElementById('signup-email').value.trim();
  const section = document.getElementById('signup-section').value;
  const pass = document.getElementById('signup-password').value;
  const pass2 = document.getElementById('signup-password2').value;

  if(!name || !username || !email || !section || !pass){ showAuthError('يرجى ملء جميع الحقول'); return; }
  if(!/^[a-zA-Z0-9_]{3,20}$/.test(username)){ showAuthError('اسم المستخدم يجب أن يكون 3-20 حرفاً (إنجليزي وأرقام و _)'); return; }
  if(!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)){ showAuthError('البريد الإلكتروني غير صحيح'); return; }
  if(pass.length < 6){ showAuthError('كلمة المرور يجب أن تكون 6 أحرف على الأقل'); return; }
  if(pass !== pass2){ showAuthError('كلمتا المرور غير متطابقتين'); return; }

  const users = getUsers();
  if(users.find(u => u.username.toLowerCase() === username.toLowerCase())){ showAuthError('❌ اسم المستخدم مستخدم مسبقاً'); return; }
  if(users.find(u => u.email.toLowerCase() === email.toLowerCase())){ showAuthError('❌ البريد الإلكتروني مستخدم مسبقاً'); return; }

  const newUser = {
    id: 'u_' + Date.now() + '_' + Math.random().toString(36).slice(2,7),
    name, username, email, section,
    password: hashPass(pass),
    avatar: 'fa-user',
    createdAt: Date.now(),
    xp: 0
  };
  users.push(newUser);
  saveUsers(users);
  localStorage.setItem(SESSION_KEY, newUser.id);
  loadSettings();
  showToast('تم إنشاء حسابك بنجاح! 🎉', 'success');
  addNotification('مرحباً بك!', `أهلاً ${name} في منصة الكيمياء التفاعلية`, 'success');
  navigateTo('welcome', true);
});

// Logout
document.getElementById('logout-btn').addEventListener('click', function(){
  confirmModal('تسجيل الخروج', 'هل أنت متأكد من تسجيل الخروج؟', () => {
    localStorage.removeItem(SESSION_KEY);
    navigateTo('auth', true);
    document.getElementById('login-form').reset();
  });
});

function applyUserToUI(user){
  if(!user) return;
  document.getElementById('username-display').textContent = user.name;
  document.getElementById('section-display').textContent = 'الشعبة: ' + user.section;
  document.getElementById('welcome-name').textContent = user.name;
  document.getElementById('result-username').textContent = user.name;
  const avatarHTML = `<i class="fas ${user.avatar || 'fa-user'}"></i>`;
  document.getElementById('sidebar-avatar').innerHTML = avatarHTML;
  document.getElementById('welcome-avatar').innerHTML = avatarHTML;
  document.getElementById('profile-avatar-display').innerHTML = avatarHTML;
}

/* ============================================================
   🎨 Theme & Appearance
   ============================================================ */
const THEMES = [
  { id: 'indigo', primary: '#6366f1', secondary: '#8b5cf6', name: 'بنفسجي' },
  { id: 'blue', primary: '#3b82f6', secondary: '#06b6d4', name: 'أزرق' },
  { id: 'green', primary: '#10b981', secondary: '#06d6a0', name: 'أخضر' },
  { id: 'pink', primary: '#ec4899', secondary: '#8b5cf6', name: 'وردي' },
  { id: 'orange', primary: '#f59e0b', secondary: '#ef4444', name: 'برتقالي' },
  { id: 'cyan', primary: '#06b6d4', secondary: '#3b82f6', name: 'سماوي' }
];

function applySettings(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  // Theme
  const theme = THEMES.find(t => t.id === s.theme) || THEMES[0];
  document.documentElement.style.setProperty('--primary', theme.primary);
  document.documentElement.style.setProperty('--secondary', theme.secondary);
  document.documentElement.style.setProperty('--primary-dark', theme.primary);
  // Dark/Light
  if(s.darkMode === false) document.body.classList.add('light');
  else document.body.classList.remove('light');
  // Compact
  if(s.compactMode) document.body.classList.add('compact');
  else document.body.classList.remove('compact');
  // Animations
  if(s.animations === false) document.body.classList.add('no-anim');
  else document.body.classList.remove('no-anim');
  // Font size
  document.documentElement.style.fontSize = (s.fontSize || 16) + 'px';
  // Sound
  soundEnabled = s.sound !== false;
  // Timer
  defaultTimer = (s.timerDuration || 30) * 60;
  // Auto save
  autoSave = s.autoSave !== false;
  // Instant explain
  instantExplain = s.instantExplain !== false;
}

function loadSettings(){
  applySettings();
}

function loadSettingsUI(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  const user = currentUser();
  if(user){
    document.getElementById('username').value = user.name;
    document.getElementById('user-username').value = user.username;
    document.getElementById('user-email').value = user.email;
  }
  document.getElementById('user-level-select').value = s.userLevel || 'beginner';
  document.getElementById('dark-mode').checked = s.darkMode !== false;
  document.getElementById('compact-mode').checked = !!s.compactMode;
  document.getElementById('animations-enabled').checked = s.animations !== false;
  document.getElementById('notifications-enabled').checked = s.notifications !== false;
  document.getElementById('quiz-notif').checked = s.quizNotif !== false;
  document.getElementById('achieve-notif').checked = s.achieveNotif !== false;
  document.getElementById('daily-reminder').checked = !!s.dailyReminder;
  document.getElementById('default-question-count').value = s.defaultQuestionCount || 20;
  document.getElementById('default-difficulty').value = s.defaultDifficulty || 'medium';
  document.getElementById('instant-explain').checked = s.instantExplain !== false;
  document.getElementById('timer-duration').value = s.timerDuration || 30;
  document.getElementById('auto-save').checked = s.autoSave !== false;
  document.getElementById('interface-language').value = s.interfaceLanguage || 'العربية';
  document.getElementById('terms-language').value = s.termsLanguage || 'العربية';

  renderThemePicker();
  renderAvatarPicker();
}

function renderThemePicker(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  const active = s.theme || 'indigo';
  const el = document.getElementById('theme-picker');
  el.innerHTML = THEMES.map(t => 
    `<div class="theme-color ${active===t.id?'active':''}" style="background:linear-gradient(135deg,${t.primary},${t.secondary});color:${t.primary};" data-theme="${t.id}" title="${t.name}"></div>`
  ).join('');
  el.querySelectorAll('.theme-color').forEach(c => {
    c.addEventListener('click', function(){
      const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
      s.theme = this.dataset.theme;
      localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
      applySettings();
      renderThemePicker();
      showToast('تم تغيير السمة', 'success');
    });
  });
}

const AVATARS = ['fa-user','fa-user-astronaut','fa-user-ninja','fa-user-tie','fa-user-graduate','fa-user-md','fa-user-secret','fa-dragon','fa-cat','fa-dog','fa-atom','fa-flask'];

function renderAvatarPicker(){
  const user = currentUser();
  const active = user?.avatar || 'fa-user';
  const el = document.getElementById('avatar-picker');
  const colors = ['linear-gradient(135deg,#6366f1,#8b5cf6)','linear-gradient(135deg,#06d6a0,#0891b2)','linear-gradient(135deg,#f59e0b,#dc2626)','linear-gradient(135deg,#ec4899,#8b5cf6)','linear-gradient(135deg,#0ea5e9,#6366f1)','linear-gradient(135deg,#10b981,#059669)'];
  el.innerHTML = AVATARS.map((a, i) => 
    `<div class="avatar-opt ${active===a?'active':''}" data-avatar="${a}" style="background:${colors[i % colors.length]}"><i class="fas ${a}"></i></div>`
  ).join('');
  el.querySelectorAll('.avatar-opt').forEach(opt => {
    opt.addEventListener('click', function(){
      const user = currentUser();
      if(!user) return;
      user.avatar = this.dataset.avatar;
      const users = getUsers();
      const idx = users.findIndex(u => u.id === user.id);
      users[idx] = user;
      saveUsers(users);
      applyUserToUI(user);
      renderAvatarPicker();
      showToast('تم تحديث الصورة الرمزية', 'success');
    });
  });
}

// Font size
document.querySelectorAll('.font-size-btn').forEach(btn => {
  btn.addEventListener('click', function(){
    document.querySelectorAll('.font-size-btn').forEach(b => b.classList.remove('active'));
    this.classList.add('active');
    const size = parseInt(this.dataset.size);
    const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
    s.fontSize = size;
    localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
    applySettings();
  });
});

// Dark mode
document.getElementById('dark-mode').addEventListener('change', function(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  s.darkMode = this.checked;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  applySettings();
});

document.getElementById('compact-mode').addEventListener('change', function(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  s.compactMode = this.checked;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  applySettings();
});

document.getElementById('animations-enabled').addEventListener('change', function(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  s.animations = this.checked;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  applySettings();
});

// Settings tabs
document.querySelectorAll('.settings-nav-btn').forEach(btn => {
  btn.addEventListener('click', function(){
    document.querySelectorAll('.settings-nav-btn').forEach(b => b.classList.remove('active'));
    document.querySelectorAll('.settings-panel').forEach(p => p.classList.remove('active'));
    this.classList.add('active');
    document.getElementById('panel-' + this.dataset.panel).classList.add('active');
  });
});

// Save settings
document.getElementById('save-settings').addEventListener('click', function(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  const user = currentUser();
  const newName = document.getElementById('username').value.trim();
  const newEmail = document.getElementById('user-email').value.trim();
  const newLevel = document.getElementById('user-level-select').value;
  
  if(user && newName && newEmail){
    user.name = newName;
    user.email = newEmail;
    user.level = newLevel;
    const users = getUsers();
    const idx = users.findIndex(u => u.id === user.id);
    users[idx] = user;
    saveUsers(users);
    applyUserToUI(user);
  }
  
  s.userLevel = newLevel;
  s.notifications = document.getElementById('notifications-enabled').checked;
  s.quizNotif = document.getElementById('quiz-notif').checked;
  s.achieveNotif = document.getElementById('achieve-notif').checked;
  s.dailyReminder = document.getElementById('daily-reminder').checked;
  s.defaultQuestionCount = parseInt(document.getElementById('default-question-count').value);
  s.defaultDifficulty = document.getElementById('default-difficulty').value;
  s.instantExplain = document.getElementById('instant-explain').checked;
  s.timerDuration = parseInt(document.getElementById('timer-duration').value);
  s.autoSave = document.getElementById('auto-save').checked;
  s.interfaceLanguage = document.getElementById('interface-language').value;
  s.termsLanguage = document.getElementById('terms-language').value;
  s.sound = document.getElementById('sound-enabled')?.checked ?? s.sound ?? true;
  
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  applySettings();
  showToast('✅ تم حفظ الإعدادات بنجاح', 'success');
});

function openChangePassword(){
  const oldPass = prompt('أدخل كلمة المرور الحالية:');
  if(oldPass === null) return;
  const user = currentUser();
  if(!user) return;
  if(user.password !== hashPass(oldPass)){ showToast('كلمة المرور الحالية غير صحيحة', 'error'); return; }
  const newPass = prompt('أدخل كلمة المرور الجديدة (6 أحرف على الأقل):');
  if(!newPass || newPass.length < 6){ showToast('كلمة المرور قصيرة جداً', 'error'); return; }
  const confirm = prompt('أعد كتابة كلمة المرور الجديدة:');
  if(newPass !== confirm){ showToast('كلمتا المرور غير متطابقتين', 'error'); return; }
  user.password = hashPass(newPass);
  const users = getUsers();
  const idx = users.findIndex(u => u.id === user.id);
  users[idx] = user;
  saveUsers(users);
  showToast('✅ تم تغيير كلمة المرور', 'success');
}

function resetSettings(){
  confirmModal('إعادة التعيين', 'هل تريد إعادة جميع الإعدادات للوضع الافتراضي؟', () => {
    localStorage.removeItem(SETTINGS_KEY);
    applySettings();
    loadSettingsUI();
    showToast('تم إعادة تعيين الإعدادات', 'success');
  });
}

function calculateDataSize(){
  let total = 0;
  [SETTINGS_KEY, RESULTS_KEY, NOTIF_KEY, USERS_KEY].forEach(k => {
    const v = localStorage.getItem(k);
    if(v) total += new Blob([v]).size;
  });
  const size = (total / 1024).toFixed(2) + ' KB';
  document.getElementById('data-size').textContent = size;
  showToast('حجم البيانات: ' + size, 'info');
}

/* ============================================================
   📥 Export / Import
   ============================================================ */
document.getElementById('export-data').addEventListener('click', function(){
  const data = {
    user: currentUser(),
    settings: JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}'),
    results: JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]'),
    notifications: JSON.parse(localStorage.getItem(NOTIF_KEY) || '[]'),
    exportedAt: new Date().toISOString()
  };
  const blob = new Blob([JSON.stringify(data, null, 2)], {type: 'application/json'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'chemistry-backup-' + new Date().toISOString().slice(0,10) + '.json';
  a.click();
  showToast('تم تصدير البيانات', 'success');
});

document.getElementById('import-file').addEventListener('change', function(e){
  const file = e.target.files[0];
  if(!file) return;
  const reader = new FileReader();
  reader.onload = function(ev){
    try{
      const data = JSON.parse(ev.target.result);
      if(data.settings) localStorage.setItem(SETTINGS_KEY, JSON.stringify(data.settings));
      if(data.results) localStorage.setItem(RESULTS_KEY, JSON.stringify(data.results));
      if(data.notifications) localStorage.setItem(NOTIF_KEY, JSON.stringify(data.notifications));
      applySettings();
      showToast('✅ تم استيراد البيانات بنجاح', 'success');
    }catch(err){
      showToast('❌ ملف غير صالح', 'error');
    }
  };
  reader.readAsText(file);
});

function clearResults(){
  confirmModal('مسح السجل', 'هل أنت متأكد من مسح جميع نتائج الاختبارات؟', () => {
    localStorage.removeItem(RESULTS_KEY);
    updateProgressUI();
    showToast('تم مسح السجل', 'success');
  });
}
function clearAllData(){
  confirmModal('حذف شامل', 'هل أنت متأكد من حذف جميع البيانات؟ لا يمكن التراجع!', () => {
    localStorage.clear();
    location.reload();
  });
}

/* ============================================================
   🔔 Notifications
   ============================================================ */
let notifications = [];
function loadNotifications(){ try{ notifications = JSON.parse(localStorage.getItem(NOTIF_KEY) || '[]'); }catch{ notifications = []; } }
function saveNotifications(){ localStorage.setItem(NOTIF_KEY, JSON.stringify(notifications)); updateNotifBadge(); }
function addNotification(title, msg, type='info'){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  if(s.notifications === false) return;
  notifications.unshift({ id: Date.now(), title, msg, type, time: new Date().toLocaleTimeString('ar-SA'), date: new Date().toLocaleDateString('ar-SA'), read: false });
  if(notifications.length > 50) notifications = notifications.slice(0, 50);
  saveNotifications();
  if(document.getElementById('notifications').classList.contains('active')) renderNotifications();
}
function updateNotifBadge(){
  const unread = notifications.filter(n => !n.read).length;
  const badge = document.getElementById('notif-count');
  if(unread > 0){ badge.textContent = unread; badge.style.display = 'inline-block'; }
  else badge.style.display = 'none';
}
function renderNotifications(){
  const list = document.getElementById('notifications-list');
  if(!notifications.length){ list.innerHTML = '<p style="text-align:center;padding:30px;color:#94a3b8;">لا توجد إشعارات</p>'; return; }
  list.innerHTML = notifications.map(n => `
    <div class="notification-item" data-id="${n.id}">
      <div class="notification-icon"><i class="fas fa-${iconFor(n.type)}"></i></div>
      <div class="notification-content">
        <div class="notification-title">${n.title}</div>
        <div class="notification-message">${n.msg}</div>
        <div class="notification-time">${n.date} - ${n.time}</div>
      </div>
    </div>
  `).join('');
  list.querySelectorAll('.notification-item').forEach(el => {
    el.addEventListener('click', function(){
      const id = parseInt(this.dataset.id);
      const n = notifications.find(x => x.id === id);
      if(n) n.read = true;
      saveNotifications();
      renderNotifications();
    });
  });
}
function iconFor(t){
  return t === 'success' ? 'check-circle' : t === 'warning' ? 'exclamation-triangle' : t === 'error' ? 'times-circle' : 'info-circle';
}
document.getElementById('clear-notifications').addEventListener('click', function(){
  confirmModal('مسح الإشعارات', 'هل تريد مسح جميع الإشعارات؟', () => {
    notifications = []; saveNotifications(); renderNotifications();
    showToast('تم مسح الإشعارات', 'success');
  });
});

/* ============================================================
   📊 Dashboard
   ============================================================ */
function updateDashboard(){
  const arr = JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]');
  const user = currentUser();
  if(arr.length){
    const total = arr.reduce((s,r) => s + r.score, 0);
    const avg = Math.round(total / arr.length);
    document.getElementById('qs-quizzes').textContent = arr.length;
    document.getElementById('qs-avg').textContent = avg + '%';
  } else {
    document.getElementById('qs-quizzes').textContent = '0';
    document.getElementById('qs-avg').textContent = '0%';
  }
  // XP
  const xp = arr.reduce((sum, r) => sum + (r.score * 10), 0);
  document.getElementById('qs-xp').textContent = xp;
  // Streak (demo - based on unique days)
  const days = new Set(arr.map(r => r.date));
  document.getElementById('qs-streak').textContent = Math.min(days.size, 30);
  // Greeting subtitle
  const hour = new Date().getHours();
  let greet = 'صباح الخير';
  if(hour >= 12 && hour < 17) greet = 'مساء الخير';
  else if(hour >= 17) greet = 'مساء الخير';
  document.getElementById('welcome-subtitle').textContent = greet + '، لنبدأ رحلة الكيمياء التفاعلية معاً';
}

/* ============================================================
   ❓ Questions Bank
   ============================================================ */
const allQuestions = [
{q:"العملية التي يعاد فيها ترتيب الذرات في مادة أو أكثر لتكوين مواد جديدة تسمى:",o:["التغير الفيزيائي","التفاعل النووي","التفاعل الكيميائي"],c:2,e:"التعريف المباشر للتفاعل الكيميائي هو عملية يعاد فيها ترتيب الذرات لتكوين مواد جديدة",l:"التفاعلات والمعادلات",d:"easy"},
{q:"المعادلة التي توضح الجسيمات (الذرات أو الأيونات) المشاركة في التفاعل بشكل مفصل تسمى:",o:["المعادلة الكيميائية","المعادلة الأيونية","المعادلة الموزونة"],c:1,e:"المعادلة الأيونية تمثل المواد في المحاليل المائية على شكل أيونات",l:"التفاعلات والمعادلات",d:"medium"},
{q:"ما هو العدد الأقصى من الإلكترونات الذي يستوعبه مستوى الطاقة الرئيسي الرابع (n=4)?",o:["18","8","32"],c:2,e:"حسب قاعدة 2n²: 2 × (4)² = 32 إلكترون",l:"الترتيب الإلكتروني",d:"medium"},
{q:"أي من الرموز التالية يمثل الصيغة الأيونية الصحيحة لكلوريد الصوديوم؟",o:["Na²⁺ Cl⁻","Na⁺ Cl⁻","Na²⁺ Cl²⁻"],c:1,e:"الصوديوم يفقد إلكترون ليصبح Na⁺ والكلور يكتسب ليصبح Cl⁻",l:"التفاعلات والمعادلات",d:"easy"},
{q:"الأيون OH⁻ يسمى أيون:",o:["الكربوكسيل","الهيدروكسيد","الكربونات"],c:1,e:"الاسم الشائع لأيون الهيدروكسيد",l:"التفاعلات والمعادلات",d:"easy"},
{q:"التفاعل الذي تكون نواتجه مادة واحدة فقط يُصنف على أنه تفاعل:",o:["احتراق","إحلال بسيط","تكوين"],c:2,e:"يتحد متفاعلان أو أكثر لتكوين مادة واحدة",l:"التفاعلات والمعادلات",d:"easy"},
{q:"إذا انتهى التوزيع الإلكتروني بـ 3p⁶، فإن العدد الذري هو:",o:["10","12","18"],c:2,e:"1s² 2s² 2p⁶ 3s² 3p⁶ = 18 إلكترون",l:"الترتيب الإلكتروني",d:"medium"},
{q:"الرمز المستخدم للفصل بين المواد المتفاعلة والنواتج:",o:["+","→","(s)"],c:1,e:"السهم يشير لاتجاه التفاعل",l:"التفاعلات والمعادلات",d:"easy"},
{q:"تفاعل مادة مع الأكسجين ينتج ضوء أو حرارة هو تفاعل:",o:["تكوين","احتراق","إحلال مزدوج"],c:1,e:"التعريف المباشر لتفاعل الاحتراق",l:"التفاعلات والمعادلات",d:"easy"},
{q:"الصيغة الصحيحة لهيدروكسيد الألومنيوم:",o:["AlOH","Al(OH)₂","Al(OH)₃"],c:2,e:"شحنة Al 3+ وشحنة OH 1-",l:"التفاعلات والمعادلات",d:"medium"},
{q:"كم عدد المستويات الثانوية في n=3?",o:["2","3","4"],c:1,e:"s, p, d",l:"الترتيب الإلكتروني",d:"medium"},
{q:"أي توزيع إلكتروني صحيح للكلور (17)?",o:["1s² 2s² 2p⁶ 3s² 3p⁵","1s² 2s² 2p⁶ 3s² 3p⁶","1s² 2s² 2p⁶ 3s¹ 3p⁶"],c:0,e:"وفقاً لمبدأ البناء التصاعدي",l:"الترتيب الإلكتروني",d:"hard"},
{q:"ما المستويات الثانوية التي يحتويها n=5?",o:["s, p","s, p, d","s, p, d, f"],c:2,e:"المستوى الخامس يحتوي على s, p, d, f, g",l:"الترتيب الإلكتروني",d:"hard"},
{q:"أي مما يلي ليس من أدلة حدوث التفاعل الكيميائي?",o:["تصاعد غاز","تغير اللون","تغير الشكل"],c:2,e:"تغير الشكل تغير فيزيائي",l:"التفاعلات والمعادلات",d:"easy"},
{q:"في N₂ + 3H₂ → 2NH₃، ما معامل H₂?",o:["1","2","3"],c:2,e:"3 جزئيات من H₂",l:"التفاعلات والمعادلات",d:"medium"},
{q:"عدد الأكسدة للأكسجين في معظم مركباته:",o:["+1","-1","-2"],c:2,e:"-2 في معظم الأكاسيد",l:"التفاعلات والمعادلات",d:"medium"},
{q:"التوزيع الإلكتروني الصحيح للبورون (5):",o:["1s² 2s³","1s² 2s² 2p¹","1s² 2p³"],c:1,e:"1s² 2s² 2p¹",l:"الترتيب الإلكتروني",d:"medium"},
{q:"الصيغة الصحيحة للناتج من Mg²⁺ مع NO₃⁻:",o:["MgNO₃","Mg(NO₃)₂","Mg₂NO₃"],c:1,e:"شحنة Mg 2+ وشحنة NO₃ 1-",l:"التفاعلات والمعادلات",d:"medium"},
{q:"الصيغة الكيميائية لأكسيد الحديد الثنائي:",o:["FeO₂","Fe₂O","FeO"],c:2,e:"نسبة 1:1 لمعادلة الشحنة",l:"التفاعلات والمعادلات",d:"medium"},
{q:"المعادلة الأيونية الكاملة للتفاعل AgNO₃ + KI:",o:["Ag⁺ + NO₃⁻ + K⁺ + I⁻ → AgI + K⁺ + NO₃⁻","Ag⁺ + I⁻ → AgI","AgNO₃ + KI → AgI + KNO₃"],c:0,e:"جميع المواد الذائبة تتفكك",l:"التفاعلات والمعادلات",d:"hard"},
{q:"المعادلة الأيونية النهائية للتفاعل AgNO₃ + KI:",o:["Ag⁺ + NO₃⁻ + K⁺ + I⁻ → AgI + K⁺ + NO₃⁻","Ag⁺ + I⁻ → AgI","K⁺ + NO₃⁻ → KNO₃"],c:1,e:"الأيونات المتفرجة تحذف",l:"التفاعلات والمعادلات",d:"hard"},
{q:"قانون حفظ الكتلة ينص على:",o:["كتلة المتفاعلة = كتلة الناتجة","فقدان كتلة","زيادة كتلة"],c:0,e:"المادة لا تفنى ولا تستحدث",l:"التفاعلات والمعادلات",d:"easy"},
{q:"الغرض من المعاملات في المعادلة:",o:["توضيح النسب بحيث يتساوى عدد الذرات","سهولة القراءة","زيادة السرعة"],c:0,e:"لتحقيق قانون حفظ الكتلة",l:"التفاعلات والمعادلات",d:"easy"},
{q:"تفاعل CH₄ مع O₂ لإنتاج CO₂ و H₂O هو:",o:["تكوين","إحلال بسيط","احتراق"],c:2,e:"احتراق المركبات العضوية",l:"التفاعلات والمعادلات",d:"medium"},
{q:"لماذا يحل Mg محل H في HCl?",o:["أكثر لمعاناً","أكثر نشاطاً في سلسلة النشاط","أرخص"],c:1,e:"حسب سلسلة النشاط الكيميائي",l:"التفاعلات والمعادلات",d:"medium"},
{q:"أي التفاعلات التالية إحلال مزدوج?",o:["Na + Cl₂ → 2NaCl","Zn + 2HCl → ZnCl₂ + H₂","AgNO₃ + NaCl → AgCl + NaNO₃"],c:2,e:"تبادل الأيونات",l:"التفاعلات والمعادلات",d:"medium"},
{q:"الخطأ في H₂ + O₂ = H₂O:",o:["الصيغ خاطئة","غير موزونة","اتجاه السهم"],c:1,e:"عدد ذرات الأكسجين غير متساو",l:"التفاعلات والمعادلات",d:"easy"},
{q:"الرمز (s) يدل على:",o:["محلول مائي","غاز","صلب"],c:2,e:"(s) للصلب",l:"التفاعلات والمعادلات",d:"easy"},
{q:"أي التغيرات التالية كيميائي؟",o:["ذوبان السكر","صدأ الحديد","تجمّد الماء"],c:1,e:"صدأ الحديد تفاعل كيميائي",l:"التفاعلات والمعادلات",d:"easy"},
{q:"النسبة بين H₂ و O₂ في 2H₂ + O₂ → 2H₂O:",o:["1:1","2:1","1:2"],c:1,e:"المعاملات 2:1",l:"التفاعلات والمعادلات",d:"medium"},
{q:"الصيغة الصحيحة لكبريتات الألومنيوم:",o:["AlSO₄","Al₂(SO₄)₃","Al₃(SO₄)₂"],c:1,e:"شحنة Al 3+ و SO₄ 2-",l:"التفاعلات والمعادلات",d:"hard"},
{q:"4غ هيدروجين + 32غ أكسجين = ؟ ماء:",o:["28غ","32غ","36غ"],c:2,e:"قانون حفظ الكتلة: 4 + 32 = 36غ",l:"التفاعلات والمعادلات",d:"medium"},
{q:"المعادلة الصحيحة لاحتراق المغنيسيوم:",o:["Mg + O → MgO","Mg + O₂ → 2MgO","2Mg + O₂ → 2MgO"],c:2,e:"موزونة وصحيحة",l:"التفاعلات والمعادلات",d:"medium"},
{q:"هل تغير حالة المادة دليل على تفاعل كيميائي؟",o:["صحيحة تماماً","خاطئة تماماً","غير دقيقة"],c:2,e:"قد يكون تغير فيزيائي",l:"التفاعلات والمعادلات",d:"easy"},
{q:"الرمز (g) يدل على:",o:["صلب","سائل","غاز"],c:2,e:"(g) للغاز",l:"التفاعلات والمعادلات",d:"easy"},
{q:"لماذا لا يحل Cu محل Fe?",o:["أقل لمعاناً","أقل نشاطاً","أكثر تكلفة"],c:1,e:"حسب سلسلة النشاط",l:"التفاعلات والمعادلات",d:"medium"},
{q:"عدد ذرات الأكسجين في Al₂(SO₄)₃:",o:["4","7","12"],c:2,e:"3 × 4 = 12",l:"التفاعلات والمعادلات",d:"hard"},
{q:"أي التفاعلات إحلال مزدوج؟",o:["CaCO₃ → CaO + CO₂","Zn + 2HCl → ZnCl₂ + H₂","AgNO₃ + NaCl → AgCl + NaNO₃"],c:2,e:"تبادل الأيونات",l:"التفاعلات والمعادلات",d:"medium"},
{q:"المعادلة الموزونة لـ HCl + NaOH:",o:["HCl + NaOH → NaCl + H₂O","2HCl + NaOH → 2NaCl + H₂O","HCl + 2NaOH → NaCl + 2H₂O"],c:0,e:"موزونة أصلاً",l:"التفاعلات والمعادلات",d:"medium"},
{q:"سبب استخدام السهم في المعادلات:",o:["اتجاه التفاعل","سرعة التفاعل","التمييز"],c:0,e:"يرمز لـ 'ينتج'",l:"التفاعلات والمعادلات",d:"easy"},
{q:"عدد الإلكترونات التي يفقدها Mg (12):",o:["إلكترون واحد","إلكترونان","ثلاثة"],c:1,e:"3s² يفقد إلكترونين",l:"الترتيب الإلكتروني",d:"medium"},
{q:"أيون توزيعه 1s² 2s² 2p⁶:",o:["Na⁺","F⁻","O²⁻","جميعها صحيحة"],c:3,e:"كلها لها 10 إلكترونات",l:"الترتيب الإلكتروني",d:"hard"},
{q:"المعادلة اللفظية لنترات الفضة + كلوريد الصوديوم:",o:["→ كلوريد الفضة (راسب) + نترات الصوديوم","→ نترات الصوديوم + كلوريد الفضة","كلاهما صحيح"],c:2,e:"كلا الوصفين صحيح",l:"التفاعلات والمعادلات",d:"easy"},
{q:"أي المشاهدات تدل على تفاعل كيميائي؟",o:["ذوبان السكر","تغير لون عباد الشمس","تكوين راسب"],c:2,e:"الراسب دليل قوي",l:"التفاعلات والمعادلات",d:"medium"},
{q:"الغرض من وزن المعادلات:",o:["سهولة القراءة","تطبيق حفظ الكتلة","زيادة السرعة"],c:1,e:"تحقيق قانون حفظ الكتلة",l:"التفاعلات والمعادلات",d:"easy"},
{q:"معامل النيتروجين في 4NH₃ + 5O₂ → 4NO + 6H₂O:",o:["4","5","1"],c:2,e:"معامل ضمني",l:"التفاعلات والمعادلات",d:"hard"},
{q:"لماذا نكتب حالة المادة في المعادلة؟",o:["اللون","تؤثر على المعادلة الأيونية","الصيغة"],c:1,e:"مهمة للمعادلة الأيونية",l:"التفاعلات والمعادلات",d:"medium"},
{q:"المعادلة الموزونة لـ Ca + H₂O:",o:["Ca + H₂O → Ca(OH)₂ + H₂","Ca + 2H₂O → Ca(OH)₂ + H₂","2Ca + 2H₂O → 2Ca(OH)₂ + H₂"],c:1,e:"موزونة بشكل صحيح",l:"التفاعلات والمعادلات",d:"medium"},
{q:"هل H₂ + Cl₂ → 2HCl موزونة؟",o:["نعم","لا","لا، الناتج خطأ"],c:0,e:"عدد الذرات متساو",l:"التفاعلات والمعادلات",d:"easy"},
{q:"المعادلة الموزونة لتفاعل Na مع Cl₂:",o:["Na + Cl → NaCl","2Na + Cl₂ → 2NaCl","2Na + 2Cl → 2NaCl"],c:1,e:"الكلور ثنائي الذرة",l:"التفاعلات والمعادلات",d:"medium"}
];

/* ============================================================
   🎮 Quiz Logic
   ============================================================ */
let currentQuestions = [], currentQuestion = 0, score = 0, userAnswers = [], weakLessons = {};
let quizMode = 'practice', quizTime = 1800, timerInterval;
let soundEnabled = true, autoSave = true, instantExplain = true, defaultTimer = 1800;

const correctSound = document.getElementById('correct-sound');
const failSound = document.getElementById('fail-sound');
const completeSound = document.getElementById('complete-sound');

document.querySelectorAll('.quiz-option').forEach(o => {
  o.addEventListener('click', function(){
    document.querySelectorAll('.quiz-option').forEach(x => x.classList.remove('selected'));
    this.classList.add('selected');
    quizMode = this.dataset.mode;
  });
});

document.getElementById('start-quiz').addEventListener('click', function(){
  const count = parseInt(document.getElementById('question-count').value);
  const diff = document.getElementById('difficulty').value;
  const shuffleOn = document.getElementById('shuffle-questions').checked;
  let pool = allQuestions;
  if(diff !== 'all') pool = allQuestions.filter(q => q.d === diff);
  if(pool.length === 0) pool = allQuestions;
  const arr = shuffleOn ? shuffle([...pool]) : [...pool];
  currentQuestions = arr.slice(0, Math.min(count, arr.length));
  currentQuestion = 0; score = 0; userAnswers = []; weakLessons = {};
  if(quizMode === 'exam'){
    quizTime = defaultTimer;
    startTimer();
  }
  navigateTo('quiz');
  showQuestion();
});

function shuffle(arr){
  const a = [...arr];
  for(let i = a.length - 1; i > 0; i--){
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

function showQuestion(){
  const q = currentQuestions[currentQuestion];
  document.getElementById('question').textContent = q.q;
  document.getElementById('question-counter').textContent = `السؤال ${currentQuestion + 1} من ${currentQuestions.length}`;
  document.getElementById('progress').style.width = ((currentQuestion + 1) / currentQuestions.length * 100) + '%';
  const box = document.getElementById('options');
  box.innerHTML = '';
  q.o.forEach((op, i) => {
    const el = document.createElement('div');
    el.className = 'option';
    el.innerHTML = `${op}<span class="option-number">${String.fromCharCode(65 + i)}</span>`;
    el.onclick = () => pickOption(i);
    box.appendChild(el);
  });
  document.getElementById('explanation').style.display = 'none';
  document.getElementById('next-btn').disabled = true;
  document.getElementById('next-btn').innerHTML = currentQuestion === currentQuestions.length - 1
    ? '<i class="fas fa-flag-checkered"></i> إنهاء الاختبار'
    : '<i class="fas fa-arrow-left"></i> التالي';
  document.getElementById('score-display').textContent = 'النقاط: ' + score;
}

function pickOption(idx){
  const opts = document.querySelectorAll('.option');
  const q = currentQuestions[currentQuestion];
  opts.forEach(o => { o.classList.add('disabled'); o.style.pointerEvents = 'none'; });
  opts[idx].classList.add('selected');
  const correct = idx === q.c;
  if(correct){
    opts[idx].classList.add('correct');
    score++;
    if(soundEnabled) correctSound.play().catch(()=>{});
  } else {
    opts[idx].classList.add('wrong');
    opts[q.c].classList.add('correct');
    if(soundEnabled) failSound.play().catch(()=>{});
    weakLessons[q.l] = (weakLessons[q.l] || 0) + 1;
  }
  userAnswers.push({ selected: idx, correct: q.c, isCorrect: correct, q: q.q, explanation: q.e, lesson: q.l });
  if(quizMode === 'practice' && instantExplain){
    const ex = document.getElementById('explanation');
    ex.innerHTML = `
      <h3><i class="fas ${correct ? 'fa-check-circle' : 'fa-times-circle'}"></i> ${correct ? 'إجابة صحيحة!' : 'إجابة خاطئة!'}</h3>
      <p><strong>التعليل:</strong> ${q.e}</p>
      ${!correct ? `<div style="background:rgba(245,158,11,.15);padding:14px;border-radius:10px;margin-top:14px;border-right:4px solid #f59e0b;color:#fef3c7;"><strong>الإجابة الصحيحة:</strong> ${q.o[q.c]}</div>` : ''}
      <div style="background:rgba(59,130,246,.15);padding:14px;border-radius:10px;margin-top:14px;border-right:4px solid #3b82f6;color:#dbeafe;"><strong>الدرس:</strong> ${q.l}</div>
    `;
    ex.style.display = 'block';
  }
  document.getElementById('next-btn').disabled = false;
  document.getElementById('score-display').textContent = 'النقاط: ' + score;
}

document.getElementById('next-btn').addEventListener('click', function(){
  if(currentQuestion < currentQuestions.length - 1){ currentQuestion++; showQuestion(); }
  else endQuiz();
});

document.getElementById('end-quiz').addEventListener('click', function(){
  confirmModal('إنهاء الاختبار', 'هل أنت متأكد من إنهاء الاختبار؟', endQuiz);
});

function startTimer(){
  updateTimer();
  clearInterval(timerInterval);
  timerInterval = setInterval(() => {
    quizTime--; updateTimer();
    if(quizTime <= 0) endQuiz();
  }, 1000);
}
function updateTimer(){
  const m = Math.floor(quizTime / 60).toString().padStart(2, '0');
  const s = (quizTime % 60).toString().padStart(2, '0');
  document.getElementById('quiz-timer').innerHTML = `<i class="fas fa-hourglass-half"></i> ${m}:${s}`;
}

function endQuiz(){
  clearInterval(timerInterval);
  if(soundEnabled) completeSound.play().catch(()=>{});
  navigateTo('results');
  showResults();
  if(autoSave) saveResults();
  const p = Math.round(score / currentQuestions.length * 100);
  addNotification('انتهى الاختبار!', `حصلت على ${p}%`, 'success');
  if(p >= 90) addNotification('إنجاز جديد! 🏆', 'حصلت على العالم العبقري!', 'success');
}

function showResults(){
  const total = currentQuestions.length;
  const p = Math.round(score / total * 100);
  document.getElementById('final-score').textContent = p + '%';
  document.getElementById('result-score').textContent = score;
  document.getElementById('total-questions').textContent = total;
  const rank = getRank(p);
  document.getElementById('user-rank').textContent = rank.text;
  document.getElementById('user-rank').className = 'rank-badge ' + rank.cls;
  const timeStr = quizMode === 'exam' ? document.getElementById('quiz-timer').textContent.replace(/[^\d:]/g, '').trim() : '--:--';
  document.getElementById('time-taken').textContent = timeStr;
  // Chart
  const canvas = document.getElementById('result-chart');
  const ctx = canvas.getContext('2d');
  ctx.clearRect(0, 0, 230, 230);
  ctx.beginPath();
  ctx.arc(115, 115, 92, 0, 2 * Math.PI);
  ctx.strokeStyle = 'rgba(255,255,255,.1)';
  ctx.lineWidth = 16;
  ctx.stroke();
  const color = p >= 70 ? '#10b981' : p >= 50 ? '#f59e0b' : '#ef4444';
  ctx.beginPath();
  ctx.arc(115, 115, 92, -0.5 * Math.PI, (2 * p / 100 - 0.5) * Math.PI);
  ctx.strokeStyle = color;
  ctx.lineWidth = 16;
  ctx.lineCap = 'round';
  ctx.stroke();
  ctx.font = 'bold 30px Arial';
  ctx.fillStyle = color;
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  ctx.fillText(p + '%', 115, 115);
  // Comparison
  const randPct = Math.floor(Math.random() * 30) + 60;
  document.getElementById('comparison').innerHTML = `<h3><i class="fas fa-chart-line"></i> مقارنة مع الآخرين</h3><p>${randPct}% من الطلاب حصلوا على نفس نتيجتك تقريباً</p>`;
  // Weak lessons
  const wl = document.getElementById('weak-lessons');
  const keys = Object.keys(weakLessons);
  if(keys.length > 0){
    wl.innerHTML = '<h3><i class="fas fa-book"></i> الدروس التي تحتاج إلى مراجعة:</h3>' +
      keys.map(k => `<div class="lesson-item"><strong>${k}</strong><p>راجع هذا الدرس لتحسين أدائك</p></div>`).join('');
  } else {
    wl.innerHTML = '<h3><i class="fas fa-star"></i> ممتاز! لا توجد دروس تحتاج إلى مراجعة</h3>';
  }
  createPerfChart(p);
  showTips(p);
  if(quizMode === 'exam') showExamDetails();
}

function getRank(p){
  if(p < 35) return { text:'غاز خامل', cls:'rank-inert' };
  if(p <= 50) return { text:'متفاعل نشيط', cls:'rank-active' };
  if(p <= 89) return { text:'كيميائي جيد', cls:'rank-good' };
  return { text:'العالم العبقري', cls:'rank-genius' };
}

let perfChartInst;
function createPerfChart(currentP){
  if(perfChartInst) perfChartInst.destroy();
  const ctx = document.getElementById('performance-chart').getContext('2d');
  const labels = ['الاختبار 1','الاختبار 2','الاختبار 3','الاختبار 4','الاختبار 5','هذا الاختبار'];
  const data = [55,62,58,70,65,currentP];
  perfChartInst = new Chart(ctx, {
    type: 'line',
    data: {
      labels,
      datasets: [{
        label: 'النتائج %',
        data,
        borderColor: '#22d3ee',
        backgroundColor: 'rgba(34,211,238,.15)',
        borderWidth: 3,
        fill: true,
        tension: .4,
        pointBackgroundColor: '#6366f1',
        pointBorderColor: '#fff',
        pointBorderWidth: 2,
        pointRadius: 6
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: { legend: { labels: { color: '#e2e8f0' } } },
      scales: {
        y: { beginAtZero: true, max: 100, ticks: { color: '#94a3b8', callback: v => v + '%' }, grid: { color: 'rgba(255,255,255,.05)' } },
        x: { ticks: { color: '#94a3b8' }, grid: { color: 'rgba(255,255,255,.05)' } }
      }
    }
  });
}

function showTips(p){
  const el = document.getElementById('improvement-tips');
  let html = '';
  if(p < 50){
    html = '<li>ركز على فهم الأساسيات قبل الانتقال للمواضيع المتقدمة</li><li>استخدم وضع الممارسة للتعلم من أخطائك</li><li>جرب قسم المحاكاة لفهم المفاهيم بشكل عملي</li><li>خصص وقتاً منتظماً للمراجعة</li>';
  } else if(p < 80){
    html = '<li>ركز على تحسين نقاط ضعفك المحددة</li><li>جرب وضع الامتحان لتحسين إدارة الوقت</li><li>راجع قسم الكيمياء والانسان لفهم التطبيقات</li><li>تنافس مع نفسك لتحطيم رقمك القياسي</li>';
  } else {
    html = '<li>تهانينا! أنت على المسار الصحيح</li><li>جرب اختبارات المستوى المتقدم</li><li>ساعد زملائك في فهم المواضيع الصعبة</li><li>استمر في استكشاف مواضيع جديدة</li>';
  }
  el.innerHTML = html;
}

function showExamDetails(){
  const wl = document.getElementById('weak-lessons');
  let html = '<h3 style="margin-top:20px;"><i class="fas fa-clipboard-list"></i> تفاصيل الأخطاء:</h3>';
  userAnswers.forEach((a, i) => {
    if(!a.isCorrect){
      html += `<div class="lesson-item"><strong>سؤال ${i+1}:</strong> ${a.q}<p><strong>إجابتك:</strong> ${currentQuestions[i].o[a.selected]}</p><p><strong>الصحيحة:</strong> ${currentQuestions[i].o[a.correct]}</p><p><strong>الشرح:</strong> ${a.explanation}</p></div>`;
    }
  });
  wl.innerHTML += html;
}

function saveResults(){
  const p = Math.round(score / currentQuestions.length * 100);
  const arr = JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]');
  arr.unshift({
    date: new Date().toLocaleDateString('ar-SA'),
    time: new Date().toLocaleTimeString('ar-SA'),
    score: p,
    total: currentQuestions.length,
    correct: score,
    mode: quizMode,
    weak: weakLessons
  });
  if(arr.length > 100) arr.splice(100);
  localStorage.setItem(RESULTS_KEY, JSON.stringify(arr));
}

/* ============================================================
   📊 Progress
   ============================================================ */
function updateProgressUI(){
  const arr = JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]');
  if(arr.length){
    const total = arr.reduce((s, r) => s + r.score, 0);
    const avg = Math.round(total / arr.length);
    const best = Math.max(...arr.map(r => r.score));
    document.getElementById('average-score').textContent = avg + '%';
    document.getElementById('best-score').textContent = best + '%';
    document.getElementById('total-quizzes').textContent = arr.length;
    document.getElementById('total-time').textContent = (arr.length * 30) + ' د';
  } else {
    document.getElementById('average-score').textContent = '0%';
    document.getElementById('best-score').textContent = '0%';
    document.getElementById('total-quizzes').textContent = '0';
    document.getElementById('total-time').textContent = '0 د';
  }
  const tb = document.getElementById('quiz-history');
  tb.innerHTML = '';
  arr.forEach(r => {
    const cls = r.score >= 70 ? 'score-high' : r.score >= 50 ? 'score-medium' : 'score-low';
    const row = document.createElement('tr');
    row.innerHTML = `
      <td>${r.date} - ${r.time}</td>
      <td>${r.mode === 'practice' ? 'تدريب' : 'امتحان'}</td>
      <td><span class="score-badge ${cls}">${r.score}%</span></td>
      <td>--:--</td>
      <td>${r.total}</td>
    `;
    tb.appendChild(row);
  });
}

/* ============================================================
   👤 Profile
   ============================================================ */
function updateProfileUI(){
  const user = currentUser();
  if(!user) return;
  const arr = JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]');
  if(arr.length){
    const total = arr.reduce((s, r) => s + r.score, 0);
    const avg = Math.round(total / arr.length);
    const best = Math.max(...arr.map(r => r.score));
    document.getElementById('profile-total-quizzes').textContent = arr.length;
    document.getElementById('profile-average-score').textContent = avg + '%';
    document.getElementById('profile-best-score').textContent = best + '%';
    document.getElementById('profile-total-time').textContent = (arr.length * 30) + ' د';
    const rank = getRank(avg);
    document.getElementById('profile-rank').textContent = rank.text;
    document.getElementById('profile-rank').className = 'rank-badge ' + rank.cls;
  }
  document.getElementById('profile-username-text').textContent = user.name;
  document.getElementById('profile-section').textContent = 'الشعبة: ' + user.section;
  document.getElementById('profile-email').innerHTML = '<i class="fas fa-envelope"></i> ' + user.email;
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  document.getElementById('profile-level').textContent = 'المستوى: ' + (s.userLevel === 'beginner' ? 'مبتدئ' : s.userLevel === 'intermediate' ? 'متوسط' : 'متقدم');
  if(s.bio) document.getElementById('profile-bio').value = s.bio;
}

document.getElementById('save-profile').addEventListener('click', function(){
  const bio = document.getElementById('profile-bio').value;
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  s.bio = bio;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  showToast('✅ تم حفظ الملف الشخصي', 'success');
});

/* ============================================================
   🧪 Research Navigation
   ============================================================ */
function setupResearchLinks(){
  document.querySelectorAll('.research-toc .toc-link[href^="#"]').forEach(link => {
    link.onclick = function(e){
      e.preventDefault();
      const id = this.getAttribute('href').substring(1);
      const el = document.getElementById(id);
      const mc = document.querySelector('.main-content');
      if(el && mc) mc.scrollTo({ top: el.offsetTop - 20, behavior: 'smooth' });
    };
  });
  const bt = document.getElementById('back-to-top');
  if(bt) bt.onclick = () => document.querySelector('.main-content').scrollTo({ top: 0, behavior: 'smooth' });
}

document.querySelector('.main-content').addEventListener('scroll', function(){
  const bt = document.getElementById('back-to-top');
  if(!bt) return;
  if(this.scrollTop > 400) bt.classList.add('visible');
  else bt.classList.remove('visible');
});

document.getElementById('clear-whiteboard').addEventListener('click', function(){
  showToast('استخدم أدوات Canva داخل السبورة للمسح أو الرسم', 'info');
});

/* ============================================================
   🛠️ Utilities
   ============================================================ */
function showToast(msg, type='info'){
  const t = document.getElementById('toast');
  const icon = type === 'success' ? 'check-circle' : type === 'error' ? 'times-circle' : 'info-circle';
  t.innerHTML = `<i class="fas fa-${icon}"></i><span>${msg}</span>`;
  t.className = 'toast ' + type;
  t.classList.add('show');
  clearTimeout(window._toastTO);
  window._toastTO = setTimeout(() => t.classList.remove('show'), 3000);
}

function confirmModal(title, msg, onConfirm){
  document.getElementById('modal-title').textContent = title;
  document.getElementById('modal-message').textContent = msg;
  document.getElementById('modal-overlay').classList.add('active');
  const btn = document.getElementById('modal-confirm');
  const newBtn = btn.cloneNode(true);
  btn.parentNode.replaceChild(newBtn, btn);
  newBtn.onclick = () => { closeModal(); onConfirm(); };
}
function closeModal(){ document.getElementById('modal-overlay').classList.remove('active'); }

/* ============================================================
   ⌨️ Keyboard Shortcuts
   ============================================================ */
document.addEventListener('keydown', function(e){
  if(!document.getElementById('quiz').classList.contains('active')) return;
  const q = currentQuestions[currentQuestion];
  if(!q) return;
  const opts = document.querySelectorAll('.option');
  if(opts[0] && opts[0].classList.contains('disabled')) {
    if(e.key === 'Enter' && !document.getElementById('next-btn').disabled){
      document.getElementById('next-btn').click();
    }
    return;
  }
  const map = { '1': 0, '2': 1, '3': 2, '4': 3 };
  if(map[e.key] !== undefined && opts[map[e.key]]){ opts[map[e.key]].click(); }
});

/* ============================================================
   🚀 Initialization
   ============================================================ */
window.addEventListener('hashchange', handleRoute);
window.addEventListener('load', function(){
  loadSettings();
  loadNotifications();
  applySettings();
  updateNotifBadge();
  handleRoute();
});
</script>
</body>
</html>
