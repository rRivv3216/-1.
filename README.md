<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>منصة الكيمياء التفاعلية v3.1</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<style>
:root{
  --primary:#6366f1;--primary-dark:#4f46e5;--secondary:#8b5cf6;--accent:#06d6a0;
  --cyan:#22d3ee;--pink:#ec4899;--dark:#0f172a;--success:#10b981;--warning:#f59e0b;
  --danger:#ef4444;--glass-border:rgba(255,255,255,0.18);
  --transition:all .35s cubic-bezier(.4,0,.2,1);--font-size:16px;
}
*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,sans-serif}
html{font-size:var(--font-size)}
body{background:#050b1e;color:#e2e8f0;overflow-x:hidden;position:relative;min-height:100vh}
body.no-anim *{animation:none!important;transition:none!important}
#bg-canvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-2;display:block}
.bg-gradient-overlay{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;background:radial-gradient(ellipse at 20% 10%,rgba(99,102,241,.35) 0%,transparent 55%),radial-gradient(ellipse at 80% 90%,rgba(139,92,246,.3) 0%,transparent 55%),radial-gradient(ellipse at 50% 50%,rgba(6,214,160,.08) 0%,transparent 70%);pointer-events:none}
body.light{background:#f1f5f9;color:#0f172a}
body.light .bg-gradient-overlay{background:radial-gradient(ellipse at 20% 10%,rgba(99,102,241,.15) 0%,transparent 55%),radial-gradient(ellipse at 80% 90%,rgba(139,92,246,.12) 0%,transparent 55%)}
body.light #bg-canvas{opacity:.4}
body.light .sidebar,body.light .header,body.light .card,body.light .settings-container,body.light .quiz-setup,body.light .quiz-content,body.light .whiteboard-container,body.light .stat-card,body.light .research-section,body.light .research-toc,body.light .notifications-container,body.light .profile-container,body.light .simulation-container,body.light .advanced-report,body.light .chat-container,body.light .calc-container,body.light .periodic-container{background:rgba(255,255,255,.85);border-color:rgba(0,0,0,.08);color:#0f172a}
body.light .header h2,body.light .card h3,body.light .settings-group h3,body.light .stat-card h3,body.light .welcome-screen h1,body.light .greeting-text h1,body.light .research-hero h1,body.light .research-section h3,body.light .subsection h4,body.light .element-info-title h2,body.light .profile-name,body.light .notification-title{color:#0f172a}
body.light .card p,body.light .welcome-screen p,body.light .setting-info p,body.light .subsection p,body.light .subsection li{color:#475569}
body.light .nav-link{color:#475569}
body.light .form-control,body.light select,body.light input,body.light textarea{background:rgba(0,0,0,.04);border-color:rgba(0,0,0,.1);color:#0f172a}
body.light .option{background:rgba(0,0,0,.03);border-color:rgba(0,0,0,.08);color:#0f172a}
body.light .question{background:rgba(99,102,241,.06);color:#0f172a}
body.light .chat-msg.bot{background:rgba(0,0,0,.05);color:#0f172a}
body.light .calc-tab{background:rgba(0,0,0,.04);color:#475569}
body.compact .card{padding:16px}
body.compact .nav-link{padding:9px 14px}
body.compact .header{padding:12px 20px}
body.compact .sidebar{width:240px}
::-webkit-scrollbar{width:8px;height:8px}
::-webkit-scrollbar-track{background:rgba(255,255,255,.03)}
::-webkit-scrollbar-thumb{background:linear-gradient(180deg,var(--primary),var(--secondary));border-radius:10px}
.auth-screen{display:none;min-height:100vh;align-items:center;justify-content:center;padding:20px;position:relative;z-index:10}
.auth-screen.active{display:flex}
.auth-container{background:rgba(15,23,42,.75);backdrop-filter:blur(28px);border:1px solid var(--glass-border);border-radius:28px;padding:40px 36px;width:100%;max-width:520px;box-shadow:0 30px 80px rgba(0,0,0,.6);animation:authIn .7s;position:relative;overflow:hidden}
body.light .auth-container{background:rgba(255,255,255,.92);border-color:rgba(0,0,0,.08)}
.auth-container::before{content:'';position:absolute;top:-50%;left:-50%;width:200%;height:200%;background:conic-gradient(from 0deg,transparent,rgba(99,102,241,.12),transparent 30%);animation:rotateConic 10s linear infinite;z-index:-1}
@keyframes rotateConic{to{transform:rotate(360deg)}}
@keyframes authIn{from{opacity:0;transform:translateY(30px) scale(.96)}to{opacity:1;transform:translateY(0) scale(1)}}
.auth-logo{width:96px;height:96px;margin:0 auto 20px;border-radius:50%;background:linear-gradient(135deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;font-size:2.6em;color:#fff;box-shadow:0 0 40px rgba(99,102,241,.6);animation:pulseIcon 2.5s infinite;position:relative}
.auth-logo::before{content:'';position:absolute;inset:-15px;border-radius:50%;border:2px solid rgba(99,102,241,.3);animation:ringPulse 2.5s infinite}
@keyframes pulseIcon{0%,100%{transform:scale(1)}50%{transform:scale(1.05)}}
@keyframes ringPulse{0%{transform:scale(.85);opacity:1}100%{transform:scale(1.35);opacity:0}}
.auth-container h1{text-align:center;font-size:1.75em;margin-bottom:8px;background:linear-gradient(135deg,#fff,var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;font-weight:800}
body.light .auth-container h1{background:linear-gradient(135deg,#0f172a,var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent}
.auth-subtitle{text-align:center;color:#94a3b8;margin-bottom:26px;font-size:.92em}
.auth-tabs{display:flex;background:rgba(255,255,255,.04);border-radius:14px;padding:5px;margin-bottom:22px;gap:4px}
.auth-tab{flex:1;padding:11px;text-align:center;border-radius:10px;cursor:pointer;font-weight:600;color:#94a3b8;transition:var(--transition);font-size:.92em;border:none;background:transparent;font-family:inherit}
.auth-tab.active{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;box-shadow:0 6px 18px rgba(99,102,241,.4)}
.auth-panel{display:none}
.auth-panel.active{display:block;animation:fadeIn .35s}
@keyframes fadeIn{from{opacity:0;transform:translateY(15px)}to{opacity:1;transform:translateY(0)}}
.form-group{margin-bottom:16px;text-align:right}
.form-group label{display:block;margin-bottom:8px;color:#cbd5e1;font-weight:600;font-size:.88em}
body.light .form-group label{color:#475569}
.form-control{width:100%;padding:14px 16px;border:2px solid rgba(255,255,255,.1);border-radius:12px;font-size:.95em;background:rgba(255,255,255,.05);color:#fff;transition:var(--transition);font-family:inherit}
.form-control::placeholder{color:#64748b}
.form-control:focus{border-color:var(--primary);outline:none;background:rgba(255,255,255,.08);box-shadow:0 0 0 4px rgba(99,102,241,.2)}
.form-control option{background:#1e293b;color:#fff}
.input-with-icon{position:relative}
.input-with-icon .form-control{padding-left:44px}
.input-with-icon>i{position:absolute;left:16px;top:50%;transform:translateY(-50%);color:#64748b;pointer-events:none}
.pass-toggle{position:absolute;left:14px;top:50%;transform:translateY(-50%);background:none;border:none;color:#64748b;cursor:pointer;font-size:1em;padding:4px}
.pass-toggle:hover{color:var(--cyan)}
.btn{padding:12px 22px;background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;border:none;border-radius:12px;cursor:pointer;font-size:.93em;font-weight:700;transition:var(--transition);display:inline-flex;align-items:center;justify-content:center;gap:8px;box-shadow:0 6px 18px rgba(99,102,241,.35);white-space:nowrap;font-family:inherit}
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
.btn-sm{padding:7px 14px;font-size:.8em}
.auth-error{background:rgba(239,68,68,.1);color:#fca5a5;padding:10px 14px;border-radius:10px;margin-bottom:12px;font-size:.88em;border-right:3px solid var(--danger);display:none}
.auth-error.show{display:block}
.auth-success{background:rgba(16,185,129,.1);color:#6ee7b7;padding:10px 14px;border-radius:10px;margin-bottom:12px;font-size:.88em;border-right:3px solid var(--success);display:none}
.auth-success.show{display:block}
.app-container{display:none;min-height:100vh}
.app-container.active{display:flex}
.sidebar{width:280px;background:rgba(15,23,42,.78);backdrop-filter:blur(24px);border-left:1px solid var(--glass-border);padding:20px 0;position:sticky;top:0;height:100vh;overflow-y:auto;z-index:100;display:flex;flex-direction:column}
body.light .sidebar{background:rgba(255,255,255,.85);border-color:rgba(0,0,0,.06)}
.logo{text-align:center;padding:12px 20px 20px;border-bottom:1px solid rgba(255,255,255,.08);margin-bottom:16px}
.logo h1{font-size:1.35em;display:flex;align-items:center;justify-content:center;gap:10px;background:linear-gradient(135deg,var(--primary),var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;font-weight:800}
.logo i{font-size:1.6em;background:linear-gradient(135deg,var(--primary),var(--secondary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;animation:spinLogo 8s linear infinite}
@keyframes spinLogo{0%,100%{transform:rotate(0deg)}50%{transform:rotate(180deg)}}
.nav-menu{list-style:none;padding:0 14px;flex-shrink:0}
.nav-item{margin-bottom:4px}
.nav-link{display:flex;align-items:center;gap:14px;padding:12px 16px;color:#cbd5e1;text-decoration:none;border-radius:12px;transition:var(--transition);font-size:.92em;position:relative;font-weight:500}
.nav-link:hover{color:#fff;transform:translateX(-4px);background:rgba(255,255,255,.05)}
.nav-link.active{color:#fff;background:linear-gradient(135deg,rgba(99,102,241,.9),rgba(139,92,246,.9));box-shadow:0 8px 24px rgba(99,102,241,.4)}
.nav-link i{font-size:1.05em;width:22px;text-align:center}
.team-section{padding:16px 14px;border-top:1px solid rgba(255,255,255,.08);margin-top:12px}
.team-section h3{margin-bottom:12px;font-size:.85em;color:#94a3b8;display:flex;align-items:center;gap:8px;padding:0 6px;font-weight:600;text-transform:uppercase}
.team-section h3 i{color:var(--primary)}
.team-member{display:flex;align-items:center;gap:12px;padding:11px 12px;border-radius:12px;background:linear-gradient(135deg,rgba(251,191,36,.15),rgba(245,158,11,.1));border:1px solid rgba(251,191,36,.2)}
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
.main-content{flex:1;padding:20px;overflow-y:auto;height:100vh;position:relative}
.header{background:rgba(15,23,42,.68);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:16px 24px;margin-bottom:20px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:14px;box-shadow:0 8px 32px rgba(0,0,0,.25)}
.header h2{color:#f1f5f9;display:flex;align-items:center;gap:14px;font-size:1.2em;font-weight:700}
.header h2 i{background:linear-gradient(135deg,var(--primary),var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;font-size:1.25em}
.user-actions{display:flex;gap:10px;flex-wrap:wrap}
.screen{display:none}
.screen.active{display:block;animation:fadeIn .5s}
.dashboard-hero{background:linear-gradient(135deg,rgba(99,102,241,.25),rgba(139,92,246,.2));border:1px solid rgba(99,102,241,.3);border-radius:22px;padding:30px;margin-bottom:22px;position:relative;overflow:hidden;backdrop-filter:blur(20px)}
.greeting{display:flex;align-items:center;gap:20px;flex-wrap:wrap;position:relative;z-index:1}
.greeting-avatar{width:72px;height:72px;border-radius:50%;background:linear-gradient(135deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;font-size:1.8em;color:#fff;flex-shrink:0;box-shadow:0 10px 30px rgba(99,102,241,.5)}
.greeting-text h1{font-size:1.5em;font-weight:800;margin-bottom:6px;color:#f1f5f9}
.greeting-text h1 span{background:linear-gradient(135deg,var(--cyan),var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent}
.greeting-text p{color:#94a3b8;font-size:.9em}
.quick-stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:12px;margin-top:20px}
.quick-stat{background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.1);border-radius:14px;padding:14px;text-align:center;transition:var(--transition)}
.quick-stat:hover{transform:translateY(-3px);background:rgba(255,255,255,.1)}
.quick-stat-value{font-size:1.5em;font-weight:800;background:linear-gradient(135deg,var(--cyan),var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:4px}
.quick-stat-label{font-size:.78em;color:#94a3b8}
.section-title{display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;flex-wrap:wrap;gap:10px}
.section-title h2{color:#f1f5f9;font-size:1.15em;display:flex;align-items:center;gap:10px;font-weight:700}
.section-title h2 i{color:var(--cyan)}
.cards-container{display:grid;grid-template-columns:repeat(auto-fill,minmax(270px,1fr));gap:18px;margin-bottom:26px}
.card{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:18px;padding:22px 20px;transition:var(--transition);cursor:pointer;position:relative;overflow:hidden}
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
.card-6 .card-icon{background:linear-gradient(135deg,#8b5cf6,#ec4899)}
.card-7 .card-icon{background:linear-gradient(135deg,#22d3ee,#3b82f6)}
.settings-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:26px}
.settings-nav{display:flex;gap:6px;flex-wrap:wrap;padding:6px;background:rgba(255,255,255,.04);border-radius:14px;margin-bottom:22px;overflow-x:auto}
.settings-nav-btn{padding:10px 18px;background:transparent;border:none;color:#94a3b8;border-radius:10px;cursor:pointer;font-size:.88em;font-weight:600;font-family:inherit;transition:var(--transition);white-space:nowrap;display:flex;align-items:center;gap:8px}
.settings-nav-btn:hover{color:#fff;background:rgba(255,255,255,.05)}
.settings-nav-btn.active{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;box-shadow:0 6px 18px rgba(99,102,241,.4)}
.settings-panel{display:none;animation:fadeIn .35s}
.settings-panel.active{display:block}
.settings-group{margin-bottom:26px}
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
.stat-card{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:18px;padding:20px;text-align:center;transition:var(--transition)}
.stat-card:hover{transform:translateY(-4px);border-color:rgba(99,102,241,.4);box-shadow:0 15px 40px rgba(99,102,241,.2)}
.stat-card i{font-size:2em;background:linear-gradient(135deg,var(--primary),var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:10px}
.stat-card h3{color:#94a3b8;margin-bottom:8px;font-size:.82em;font-weight:600}
.stat-card .stat-value{font-size:1.7em;font-weight:800;background:linear-gradient(135deg,var(--cyan),var(--primary));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent}
.notifications-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:20px;max-height:600px;overflow-y:auto}
.notification-item{padding:14px;border-bottom:1px solid rgba(255,255,255,.06);display:flex;align-items:flex-start;gap:12px;transition:var(--transition);cursor:pointer;border-radius:10px}
.notification-item:hover{background:rgba(255,255,255,.04)}
.notification-item:last-child{border-bottom:none}
.notification-icon{font-size:1.05em;color:var(--cyan);flex-shrink:0;width:32px;height:32px;border-radius:50%;background:rgba(34,211,238,.15);display:flex;align-items:center;justify-content:center}
.notification-content{flex:1;min-width:0}
.notification-title{font-weight:700;margin-bottom:4px;color:#f1f5f9;font-size:.88em}
.notification-message{color:#cbd5e1;font-size:.82em;line-height:1.6;margin-bottom:5px}
.notification-time{font-size:.72em;color:#64748b}
.profile-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:26px}
.profile-header{display:flex;align-items:center;gap:20px;margin-bottom:24px;flex-wrap:wrap}
.profile-avatar{width:96px;height:96px;border-radius:50%;background:linear-gradient(135deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;font-size:2.3em;color:#fff;flex-shrink:0;box-shadow:0 10px 30px rgba(99,102,241,.4)}
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
.simulation-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:24px;text-align:center}
.simulation-container h2{color:#f1f5f9;margin-bottom:10px;font-size:1.15em}
.simulation-container > p{color:#94a3b8;margin-bottom:18px;font-size:.88em}
.simulation-frame{width:100%;height:600px;border:none;border-radius:14px;background:#fff}
.simulation-info{margin-top:18px;text-align:right}
.simulation-info h3{color:#f1f5f9;margin-bottom:10px;font-size:1em;display:flex;align-items:center;gap:10px}
.simulation-info p{color:#94a3b8;line-height:1.8;font-size:.85em}
.research-container{max-width:1100px;margin:0 auto;padding:8px}
.research-hero{background:linear-gradient(135deg,rgba(99,102,241,.25),rgba(139,92,246,.25));border:1px solid rgba(99,102,241,.3);border-radius:22px;padding:36px 28px;text-align:center;margin-bottom:24px;position:relative;overflow:hidden;backdrop-filter:blur(20px)}
.research-hero h1{font-size:1.75em;margin-bottom:12px;background:linear-gradient(135deg,#fff,var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;font-weight:800;line-height:1.4}
.research-hero > p{color:#cbd5e1;font-size:.95em;max-width:680px;margin:0 auto;line-height:1.75}
.research-elements-showcase{display:flex;justify-content:center;gap:10px;flex-wrap:wrap;margin-top:22px}
.element-chip{background:rgba(255,255,255,.08);backdrop-filter:blur(10px);padding:9px 16px;border-radius:30px;font-weight:700;border:1px solid rgba(255,255,255,.15);display:flex;align-items:center;gap:8px;transition:var(--transition);color:#e2e8f0;font-size:.85em;cursor:pointer}
.element-chip:hover{background:rgba(99,102,241,.25);transform:translateY(-3px);border-color:rgba(99,102,241,.5)}
.research-toc{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:18px;padding:22px;margin-bottom:22px}
.research-toc h3{color:#f1f5f9;margin-bottom:16px;padding-bottom:10px;border-bottom:1px solid rgba(255,255,255,.08);display:flex;align-items:center;gap:10px;font-size:1em}
.research-toc h3 i{color:var(--cyan)}
.toc-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(190px,1fr));gap:10px}
.toc-link{display:flex;align-items:center;gap:10px;padding:11px 14px;background:rgba(255,255,255,.04);border-radius:11px;color:#cbd5e1;text-decoration:none;transition:var(--transition);border-right:3px solid var(--primary);font-size:.85em;cursor:pointer}
.toc-link:hover{background:linear-gradient(135deg,rgba(99,102,241,.2),rgba(139,92,246,.2));transform:translateX(-4px);color:#fff;border-right-color:var(--cyan)}
.toc-link i{color:var(--primary);font-size:.95em;width:18px;text-align:center}
.research-section{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:18px;padding:26px;margin-bottom:20px;scroll-margin-top:20px;transition:var(--transition)}
.research-section:hover{border-color:rgba(99,102,241,.3);box-shadow:0 15px 40px rgba(99,102,241,.15)}
.section-header-element{display:flex;align-items:center;gap:16px;padding:20px;border-radius:16px;margin-bottom:20px;color:#fff;position:relative;overflow:hidden;cursor:pointer;transition:var(--transition)}
.section-header-element:hover{transform:scale(1.01);box-shadow:0 15px 40px rgba(0,0,0,.3)}
.section-header-element::after{content:'👆 اضغط لعرض النموذج 3D';position:absolute;bottom:8px;left:14px;font-size:.7em;opacity:.7;background:rgba(0,0,0,.3);padding:3px 8px;border-radius:10px}
.element-badge{width:72px;height:72px;border-radius:50%;background:rgba(255,255,255,.22);display:flex;align-items:center;justify-content:center;font-size:1.9em;font-weight:900;backdrop-filter:blur(10px);border:3px solid rgba(255,255,255,.4);flex-shrink:0;box-shadow:0 8px 20px rgba(0,0,0,.3)}
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
.formula-box{background:linear-gradient(135deg,#0f172a,#020617);color:#6ee7b7;padding:20px;border-radius:14px;text-align:center;font-family:'Courier New',monospace;font-size:1.05em;font-weight:bold;margin:14px 0;box-shadow:0 8px 24px rgba(0,0,0,.4);border:1px solid rgba(6,214,160,.35);direction:ltr;letter-spacing:1.5px}
.info-box{padding:14px 16px;border-radius:12px;margin:12px 0;border-right:4px solid;line-height:1.8;font-size:.87em}
.info-box h5{margin-bottom:8px;display:flex;align-items:center;gap:8px;font-size:.94em;font-weight:700}
.info-box p{color:#cbd5e1;font-size:.9em}
.info-box.warning{background:rgba(245,158,11,.1);border-color:#f59e0b}.info-box.warning h5{color:#fbbf24}
.info-box.danger{background:rgba(239,68,68,.1);border-color:#ef4444}.info-box.danger h5{color:#fca5a5}
.info-box.success{background:rgba(16,185,129,.1);border-color:#10b981}.info-box.success h5{color:#6ee7b7}
.info-box.info{background:rgba(59,130,246,.1);border-color:#3b82f6}.info-box.info h5{color:#93c5fd}
.info-box.purple{background:rgba(139,92,246,.1);border-color:#8b5cf6}.info-box.purple h5{color:#c4b5fd}
.comparison-table-wrapper{overflow-x:auto;border-radius:14px;border:1px solid rgba(255,255,255,.08);margin:16px 0}
.comparison-table{width:100%;border-collapse:collapse;min-width:750px}
.comparison-table th{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;padding:12px;text-align:right;font-weight:700;font-size:.85em}
.comparison-table td{padding:12px;border-bottom:1px solid rgba(255,255,255,.06);text-align:right;line-height:1.7;color:#cbd5e1;font-size:.83em;background:rgba(255,255,255,.02)}
.comparison-table tr:hover td{background:rgba(99,102,241,.08)}
.comparison-table tr:last-child td{border-bottom:none}
.comparison-table .element-cell{font-weight:700;color:#f1f5f9;display:flex;align-items:center;gap:8px}
.element-mini-badge{display:inline-flex;align-items:center;justify-content:center;width:28px;height:28px;border-radius:50%;color:#fff;font-weight:800;font-size:.78em;flex-shrink:0}
.references-list{list-style:none;padding:0;counter-reset:ref}
.references-list li{counter-increment:ref;padding:12px 16px 12px 56px;margin-bottom:10px;background:rgba(255,255,255,.03);border-radius:12px;position:relative;line-height:1.7;border-right:3px solid var(--primary);font-size:.83em;direction:ltr;text-align:left;color:#cbd5e1}
.references-list li::before{content:counter(ref);position:absolute;left:14px;top:50%;transform:translateY(-50%);width:26px;height:26px;background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:700;font-size:.78em}
.references-list a{color:#93c5fd;text-decoration:none;word-break:break-all}
.references-list a:hover{color:#60a5fa;text-decoration:underline}
.conclusion-box{background:linear-gradient(135deg,rgba(16,185,129,.12),rgba(6,214,160,.12));border-radius:16px;padding:24px;border-right:5px solid var(--success);line-height:1.85;color:#d1fae5;font-size:.88em}
.conclusion-box h3{color:#6ee7b7;margin-bottom:12px;display:flex;align-items:center;gap:10px;font-size:1em}
.back-to-top{position:fixed;bottom:22px;left:22px;width:44px;height:44px;background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;border:none;border-radius:50%;cursor:pointer;box-shadow:0 8px 24px rgba(99,102,241,.5);display:none;align-items:center;justify-content:center;font-size:1em;z-index:999;transition:var(--transition)}
.back-to-top.visible{display:flex}
.url-indicator{position:fixed;bottom:16px;left:50%;transform:translateX(-50%);background:rgba(15,23,42,.85);backdrop-filter:blur(12px);color:var(--cyan);padding:7px 16px;border-radius:20px;font-size:.72em;font-family:'Courier New',monospace;z-index:9999;border:1px solid rgba(34,211,238,.3);cursor:pointer;display:none;max-width:80%;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.url-indicator.visible{display:block}
.toast{position:fixed;top:20px;left:50%;transform:translateX(-50%) translateY(-100px);background:rgba(15,23,42,.95);backdrop-filter:blur(20px);color:#fff;padding:14px 24px;border-radius:14px;border:1px solid var(--glass-border);box-shadow:0 10px 40px rgba(0,0,0,.5);z-index:10000;transition:transform .4s cubic-bezier(.4,0,.2,1);display:flex;align-items:center;gap:12px;font-size:.9em;font-weight:600;max-width:90%}
.toast.show{transform:translateX(-50%) translateY(0)}
.toast.success i{color:var(--success)}.toast.error i{color:var(--danger)}.toast.info i{color:var(--cyan)}
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.75);backdrop-filter:blur(8px);z-index:10000;display:none;align-items:center;justify-content:center;padding:20px}
.modal-overlay.active{display:flex;animation:fadeIn .3s}
.modal{background:rgba(15,23,42,.98);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:22px;padding:28px;max-width:480px;width:100%;box-shadow:0 30px 80px rgba(0,0,0,.6)}
.modal h3{color:#f1f5f9;margin-bottom:14px;display:flex;align-items:center;gap:10px;font-size:1.1em}
.modal p{color:#94a3b8;margin-bottom:20px;line-height:1.7;font-size:.9em}
.modal-actions{display:flex;gap:10px;justify-content:flex-end;flex-wrap:wrap}
.chat-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:0;display:flex;flex-direction:column;height:calc(100vh - 220px);min-height:500px;overflow:hidden}
.chat-header{padding:14px 20px;border-bottom:1px solid rgba(255,255,255,.08);display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.chat-header .avatar{width:40px;height:40px;border-radius:50%;background:linear-gradient(135deg,var(--primary),var(--cyan));display:flex;align-items:center;justify-content:center;font-size:1.2em;color:#fff}
.chat-header .info h3{color:#f1f5f9;font-size:.95em;font-weight:700}
.chat-header .info p{color:#94a3b8;font-size:.75em}
.chat-header-actions{margin-right:auto;display:flex;gap:8px;flex-wrap:wrap}
.chat-msgs{flex:1;overflow-y:auto;padding:18px;display:flex;flex-direction:column;gap:12px}
.chat-msg{max-width:80%;padding:12px 16px;border-radius:16px;line-height:1.7;font-size:.9em;word-wrap:break-word;white-space:pre-wrap}
.chat-msg.user{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;align-self:flex-start;border-bottom-right-radius:4px}
.chat-msg.bot{background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.08);color:#e2e8f0;align-self:flex-end;border-bottom-left-radius:4px}
.chat-msg.bot code{background:rgba(0,0,0,.3);padding:2px 6px;border-radius:6px;font-family:'Courier New',monospace;color:#6ee7b7}
.chat-msg.typing{display:flex;gap:4px;align-items:center}
.chat-msg.typing span{width:8px;height:8px;background:var(--cyan);border-radius:50%;animation:typing 1.4s infinite}
.chat-msg.typing span:nth-child(2){animation-delay:.2s}
.chat-msg.typing span:nth-child(3){animation-delay:.4s}
@keyframes typing{0%,60%,100%{opacity:.3;transform:translateY(0)}30%{opacity:1;transform:translateY(-4px)}}
.chat-input-area{padding:14px;border-top:1px solid rgba(255,255,255,.08);display:flex;gap:10px;align-items:flex-end}
.chat-input-area textarea{flex:1;resize:none;min-height:44px;max-height:120px;padding:12px 14px;border:1.5px solid rgba(255,255,255,.12);border-radius:14px;background:rgba(255,255,255,.05);color:#e2e8f0;font-family:inherit;font-size:.9em}
.chat-input-area textarea:focus{border-color:var(--primary);outline:none;box-shadow:0 0 0 3px rgba(99,102,241,.2)}
.chat-send-btn{width:46px;height:46px;border-radius:50%;padding:0}
.chat-suggestions{display:flex;gap:8px;padding:10px 14px;overflow-x:auto;border-top:1px solid rgba(255,255,255,.05);flex-wrap:wrap}
.chat-suggestion{padding:7px 14px;background:rgba(99,102,241,.12);border:1px solid rgba(99,102,241,.25);border-radius:20px;color:#c7d2fe;font-size:.78em;cursor:pointer;white-space:nowrap;transition:var(--transition)}
.chat-suggestion:hover{background:rgba(99,102,241,.25);color:#fff}
.calc-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:24px}
.calc-tabs{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:20px;padding:5px;background:rgba(255,255,255,.04);border-radius:14px;overflow-x:auto}
.calc-tab{padding:10px 16px;background:transparent;border:none;color:#94a3b8;border-radius:10px;cursor:pointer;font-size:.85em;font-weight:600;font-family:inherit;transition:var(--transition);white-space:nowrap;display:flex;align-items:center;gap:6px}
.calc-tab:hover{color:#fff;background:rgba(255,255,255,.05)}
.calc-tab.active{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;box-shadow:0 6px 18px rgba(99,102,241,.4)}
.calc-panel{display:none}
.calc-panel.active{display:block;animation:fadeIn .35s}
.calc-panel h3{color:#f1f5f9;margin-bottom:6px;font-size:1.05em;display:flex;align-items:center;gap:10px}
.calc-panel h3 i{color:var(--cyan)}
.calc-desc{color:#94a3b8;font-size:.82em;margin-bottom:18px;line-height:1.6}
.calc-form{display:grid;gap:14px}
.calc-form-row{display:grid;grid-template-columns:1fr 1fr;gap:12px}
@media(max-width:600px){.calc-form-row{grid-template-columns:1fr}}
.calc-field label{display:block;color:#cbd5e1;font-size:.82em;margin-bottom:6px;font-weight:600}
.calc-field input,.calc-field select{width:100%}
.calc-result{margin-top:20px;padding:18px;background:linear-gradient(135deg,rgba(6,214,160,.1),rgba(34,211,238,.1));border:1px solid rgba(6,214,160,.3);border-radius:14px;border-right:4px solid var(--accent);display:none}
.calc-result.show{display:block;animation:fadeIn .4s}
.calc-result-label{color:#94a3b8;font-size:.78em;margin-bottom:6px}
.calc-result-value{color:#6ee7b7;font-size:1.5em;font-weight:800;font-family:'Courier New',monospace;word-break:break-word}
.calc-result-detail{color:#cbd5e1;font-size:.82em;margin-top:10px;line-height:1.7}
.calc-help{font-size:.78em;color:#64748b;margin-top:8px;line-height:1.6}
.periodic-container{background:rgba(15,23,42,.65);backdrop-filter:blur(20px);border:1px solid var(--glass-border);border-radius:20px;padding:20px;overflow-x:auto}
.periodic-legend{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:16px;padding:12px;background:rgba(255,255,255,.03);border-radius:12px}
.legend-item{display:flex;align-items:center;gap:6px;font-size:.75em;color:#cbd5e1;cursor:pointer;padding:4px 8px;border-radius:8px;transition:var(--transition)}
.legend-item:hover{background:rgba(255,255,255,.06)}
.legend-dot{width:14px;height:14px;border-radius:4px}
.periodic-grid{display:grid;grid-template-columns:repeat(18,minmax(44px,1fr));gap:4px;min-width:900px;padding:10px}
.pt-element{aspect-ratio:1;border-radius:8px;display:flex;flex-direction:column;align-items:center;justify-content:center;cursor:pointer;transition:var(--transition);padding:4px;position:relative;color:#fff;font-weight:700;overflow:hidden}
.pt-element:hover{transform:scale(1.15);z-index:10;box-shadow:0 8px 25px rgba(0,0,0,.5);border:2px solid #fff}
.pt-element .symbol{font-size:.95em;line-height:1}
.pt-element .num{font-size:.6em;position:absolute;top:3px;right:5px;opacity:.85}
.pt-element .name{font-size:.5em;opacity:.85;line-height:1;text-align:center;overflow:hidden;white-space:nowrap;max-width:100%;text-overflow:ellipsis}
.pt-spacer{visibility:hidden}
.pt-category-alkali{background:linear-gradient(135deg,#f43f5e,#be123c)}
.pt-category-alkaline{background:linear-gradient(135deg,#fb923c,#c2410c)}
.pt-category-transition{background:linear-gradient(135deg,#facc15,#a16207)}
.pt-category-post-transition{background:linear-gradient(135deg,#84cc16,#3f6212)}
.pt-category-metalloid{background:linear-gradient(135deg,#14b8a6,#0f766e)}
.pt-category-nonmetal{background:linear-gradient(135deg,#3b82f6,#1e40af)}
.pt-category-halogen{background:linear-gradient(135deg,#06b6d4,#0e7490)}
.pt-category-noble{background:linear-gradient(135deg,#a855f7,#6b21a8)}
.pt-category-lanthanide{background:linear-gradient(135deg,#ec4899,#9d174d)}
.pt-category-actinide{background:linear-gradient(135deg,#d946ef,#86198f)}
.pt-category-unknown{background:linear-gradient(135deg,#64748b,#334155)}
.pt-info-panel{background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.08);border-radius:14px;padding:16px;margin-top:16px;min-height:100px;display:flex;align-items:center;justify-content:center;text-align:center;color:#94a3b8;font-size:.9em}
.modal-3d{max-width:920px!important;width:95%!important;padding:0!important;overflow:hidden;background:rgba(8,12,26,.98)!important}
.modal-3d-header{padding:18px 24px;border-bottom:1px solid rgba(255,255,255,.08);display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:12px}
.modal-3d-header h3{color:#f1f5f9;font-size:1.15em;font-weight:700;margin:0;display:flex;align-items:center;gap:10px}
.modal-3d-header .close-btn{background:rgba(239,68,68,.15);color:#fca5a5;border:1px solid rgba(239,68,68,.3);width:36px;height:36px;border-radius:50%;cursor:pointer;font-size:1em;transition:var(--transition)}
.modal-3d-header .close-btn:hover{background:rgba(239,68,68,.35);color:#fff}
.modal-3d-body{display:grid;grid-template-columns:1.3fr 1fr;gap:0}
@media(max-width:800px){.modal-3d-body{grid-template-columns:1fr}}
.atomic-3d-canvas{width:100%;height:460px;background:radial-gradient(circle at center,rgba(99,102,241,.15),transparent 70%);position:relative;overflow:hidden;cursor:grab}
.atomic-3d-canvas:active{cursor:grabbing}
.atomic-3d-canvas canvas{display:block;width:100%;height:100%}
.atomic-3d-hint{position:absolute;bottom:10px;right:12px;font-size:.7em;color:#94a3b8;background:rgba(0,0,0,.5);padding:4px 10px;border-radius:10px}
.atomic-info{padding:22px;overflow-y:auto;max-height:460px}
.atomic-info h2{color:#f1f5f9;font-size:1.5em;margin-bottom:6px;display:flex;align-items:center;gap:12px}
.atomic-info .symbol-box{width:60px;height:60px;border-radius:14px;display:flex;align-items:center;justify-content:center;font-size:1.4em;font-weight:900;color:#fff;background:linear-gradient(135deg,var(--primary),var(--secondary));box-shadow:0 8px 20px rgba(99,102,241,.4)}
.atomic-info .cat-badge{display:inline-block;padding:3px 12px;border-radius:20px;font-size:.75em;font-weight:700;color:#fff;margin-bottom:12px}
.atomic-info .info-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:14px}
.atomic-info .info-item{background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.08);border-radius:10px;padding:10px 12px}
.atomic-info .info-item .label{font-size:.72em;color:#94a3b8;margin-bottom:3px}
.atomic-info .info-item .value{font-size:.92em;color:#f1f5f9;font-weight:700;font-family:'Courier New',monospace;word-break:break-word}
.atomic-info .shells-display{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}
.atomic-info .shell-badge{background:linear-gradient(135deg,rgba(99,102,241,.2),rgba(139,92,246,.2));border:1px solid rgba(99,102,241,.4);border-radius:20px;padding:5px 12px;font-size:.8em;color:#c7d2fe;font-weight:700;font-family:'Courier New',monospace}
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
  .periodic-grid{grid-template-columns:repeat(18,minmax(30px,1fr));min-width:600px}
}
@media(max-width:500px){
  .header{padding:12px 16px}
  .header h2{font-size:.95em}
  .btn{padding:9px 14px;font-size:.82em}
  .result-score{font-size:2.5em}
  .auth-container{padding:26px 20px}
  .auth-container h1{font-size:1.4em}
}
</style>
</head>
<body>

<canvas id="bg-canvas"></canvas>
<div class="bg-gradient-overlay"></div>

<!-- Auth -->
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
    <div class="auth-panel active" id="panel-login">
      <form id="login-form">
        <div class="form-group">
          <label><i class="fas fa-user-circle"></i> البريد الإلكتروني أو اسم المستخدم</label>
          <div class="input-with-icon"><i class="fas fa-user"></i>
            <input type="text" id="login-identifier" class="form-control" placeholder="example@mail.com أو username" required>
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-lock"></i> كلمة المرور</label>
          <div class="input-with-icon"><i class="fas fa-lock"></i>
            <input type="password" id="login-password" class="form-control" placeholder="••••••••" required>
            <button type="button" class="pass-toggle" onclick="togglePass('login-password', this)"><i class="fas fa-eye"></i></button>
          </div>
        </div>
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:16px;font-size:.85em;">
          <label style="display:flex;align-items:center;gap:6px;cursor:pointer;color:#94a3b8;"><input type="checkbox" id="remember-me" style="width:auto;padding:0;"> تذكرني</label>
          <a href="#" style="color:var(--cyan);text-decoration:none;" onclick="showForgot(event)">نسيت كلمة المرور؟</a>
        </div>
        <button type="submit" class="btn" style="width:100%;padding:13px;"><i class="fas fa-sign-in-alt"></i> تسجيل الدخول</button>
      </form>
    </div>
    <div class="auth-panel" id="panel-signup">
      <form id="signup-form">
        <div class="form-group">
          <label><i class="fas fa-id-card"></i> الاسم الكامل</label>
          <div class="input-with-icon"><i class="fas fa-user"></i>
            <input type="text" id="signup-name" class="form-control" placeholder="أدخل اسمك الكامل" required>
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-at"></i> اسم المستخدم</label>
          <div class="input-with-icon"><i class="fas fa-user-tag"></i>
            <input type="text" id="signup-username" class="form-control" placeholder="username (بالإنجليزية والأرقام)" required pattern="[a-zA-Z0-9_]{3,20}">
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-envelope"></i> البريد الإلكتروني</label>
          <div class="input-with-icon"><i class="fas fa-envelope"></i>
            <input type="email" id="signup-email" class="form-control" placeholder="example@mail.com" required>
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-users"></i> الشعبة</label>
          <select id="signup-section" class="form-control" required>
            <option value="" disabled selected>اختر شعبتك</option>
            <option value="الشعبة 1">الشعبة 1</option><option value="الشعبة 2">الشعبة 2</option>
            <option value="الشعبة 3">الشعبة 3</option><option value="الشعبة 4">الشعبة 4</option>
            <option value="الشعبة 5">الشعبة 5</option><option value="الشعبة 6">الشعبة 6</option>
            <option value="الشعبة 7">الشعبة 7</option>
          </select>
        </div>
        <div class="form-group">
          <label><i class="fas fa-lock"></i> كلمة المرور</label>
          <div class="input-with-icon"><i class="fas fa-lock"></i>
            <input type="password" id="signup-password" class="form-control" placeholder="6 أحرف على الأقل" required minlength="6">
            <button type="button" class="pass-toggle" onclick="togglePass('signup-password', this)"><i class="fas fa-eye"></i></button>
          </div>
        </div>
        <div class="form-group">
          <label><i class="fas fa-lock"></i> تأكيد كلمة المرور</label>
          <div class="input-with-icon"><i class="fas fa-check-double"></i>
            <input type="password" id="signup-password2" class="form-control" placeholder="أعد كتابة كلمة المرور" required>
          </div>
        </div>
        <button type="submit" class="btn" style="width:100%;padding:13px;"><i class="fas fa-user-plus"></i> إنشاء الحساب</button>
      </form>
    </div>
  </div>
</div>

<!-- Main App -->
<div class="app-container" id="main-app">
  <aside class="sidebar">
    <div class="logo"><h1><i class="fas fa-atom"></i> كيمياء</h1></div>
    <ul class="nav-menu">
      <li class="nav-item"><a href="#/welcome" class="nav-link" data-screen="welcome"><i class="fas fa-home"></i><span>الرئيسية</span></a></li>
      <li class="nav-item"><a href="#/chemistry-human" class="nav-link" data-screen="chemistry-human"><i class="fas fa-dna"></i><span>الكيمياء والانسان</span></a></li>
      <li class="nav-item"><a href="#/periodic-table" class="nav-link" data-screen="periodic-table"><i class="fas fa-th"></i><span>الجدول الدوري</span></a></li>
      <li class="nav-item"><a href="#/calculator" class="nav-link" data-screen="calculator"><i class="fas fa-calculator"></i><span>الآلة الحاسبة</span></a></li>
      <li class="nav-item"><a href="#/ai-assistant" class="nav-link" data-screen="ai-assistant"><i class="fas fa-robot"></i><span>المساعد الذكي</span></a></li>
      <li class="nav-item"><a href="#/quiz-setup" class="nav-link" data-screen="quiz-setup"><i class="fas fa-file-alt"></i><span>الاختبارات</span></a></li>
      <li class="nav-item"><a href="#/simulation" class="nav-link" data-screen="simulation"><i class="fas fa-flask"></i><span>المحاكاة</span></a></li>
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
    <!-- Welcome -->
    <div class="screen active" id="welcome">
      <div class="header">
        <h2><i class="fas fa-home"></i> الرئيسية</h2>
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('notifications')"><i class="fas fa-bell"></i> الإشعارات <span id="notif-count" style="background:var(--danger);color:#fff;border-radius:50%;padding:1px 7px;font-size:.75em;margin-right:4px;display:none;">0</span></button>
          <button class="btn btn-outline" onclick="navigateTo('profile')"><i class="fas fa-user"></i> الملف الشخصي</button>
        </div>
      </div>
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
      <div class="section-title"><h2><i class="fas fa-th-large"></i> الأقسام الرئيسية</h2></div>
      <div class="cards-container">
        <div class="card card-6" onclick="navigateTo('ai-assistant')">
          <div class="card-header"><div class="card-icon"><i class="fas fa-robot"></i></div><h3>المساعد الذكي</h3></div>
          <p>مساعد ذكاء اصطناعي متقدم للرد على جميع أسئلتك الكيميائية بذكاء واحترافية</p>
          <button class="btn"><i class="fas fa-comments"></i> ابدأ المحادثة</button>
        </div>
        <div class="card card-7" onclick="navigateTo('periodic-table')">
          <div class="card-header"><div class="card-icon"><i class="fas fa-th"></i></div><h3>الجدول الدوري</h3></div>
          <p>تصفح 118 عنصر كيميائي مع نماذج ذرية ثلاثية الأبعاد تفاعلية قابلة للتدوير</p>
          <button class="btn btn-cyan"><i class="fas fa-cube"></i> استكشف</button>
        </div>
        <div class="card card-3" onclick="navigateTo('calculator')">
          <div class="card-header"><div class="card-icon"><i class="fas fa-calculator"></i></div><h3>الآلة الحاسبة</h3></div>
          <p>8 أدوات حسابية: كتلة مولية، pH، تخفيف، غاز مثالي، موازن معادلات والمزيد</p>
          <button class="btn btn-warning"><i class="fas fa-equals"></i> احسب</button>
        </div>
        <div class="card card-2" onclick="navigateTo('chemistry-human')">
          <div class="card-header"><div class="card-icon"><i class="fas fa-dna"></i></div><h3>الكيمياء والانسان</h3></div>
          <p>دراسة كيميائية حيوية شاملة للعناصر المهمة في جسم الإنسان مع نماذج 3D</p>
          <button class="btn btn-success"><i class="fas fa-book-open"></i> اقرأ البحث</button>
        </div>
        <div class="card card-1" onclick="navigateTo('quiz-setup')">
          <div class="card-header"><div class="card-icon"><i class="fas fa-file-alt"></i></div><h3>الاختبارات التفاعلية</h3></div>
          <p>اختبر معرفتك من خلال اختبارات تفاعلية مع تصحيح فوري وتحليل مفصل</p>
          <button class="btn"><i class="fas fa-play"></i> ابدأ الاختبار</button>
        </div>
        <div class="card card-5" onclick="navigateTo('simulation')">
          <div class="card-header"><div class="card-icon"><i class="fas fa-flask"></i></div><h3>المحاكاة والتجارب</h3></div>
          <p>قم بتجربة التفاعلات الكيميائية افتراضياً في مختبر آمن مع شرح مفصل</p>
          <button class="btn btn-cyan"><i class="fas fa-vial"></i> ابدأ المحاكاة</button>
        </div>
      </div>
    </div>

    <!-- Chemistry & Human -->
    <div class="screen" id="chemistry-human">
      <div class="header">
        <h2><i class="fas fa-dna"></i> الكيمياء والانسان</h2>
        <div class="user-actions"><button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button></div>
      </div>
      <div class="research-container">
        <div class="research-hero">
          <h1><i class="fas fa-atom"></i> العناصر الكيميائية المهمة في جسم الإنسان</h1>
          <p>دراسة كيميائية حيوية شاملة لخصائص العناصر، آليات عملها على المستوى الجزئي، ووظائفها داخل الجسم</p>
          <div class="research-elements-showcase">
            <div class="element-chip" onclick="openElement3D(26)"><span>Fe</span> الحديد</div>
            <div class="element-chip" onclick="openElement3D(30)"><span>Zn</span> الزنك</div>
            <div class="element-chip" onclick="openElement3D(12)"><span>Mg</span> المغنيسيوم</div>
            <div class="element-chip" onclick="openElement3D(53)"><span>I</span> اليود</div>
            <div class="element-chip" onclick="openElement3D(34)"><span>Se</span> السيلينيوم</div>
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

        <div class="research-section" id="res-intro">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-flask"></i></div>
            <div class="element-info-title"><h2>المقدمة والمبادئ الكيميائية الحيوية</h2><p>الأساس الذري لوظائف الجسم الحيوية</p></div>
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
            <p>تخضع كافة العناصر الكيميائية في جسم الإنسان لمجموعة من القواعد الفيزيوكيميائية التي تحدد نمط وظيفتها داخل الخلايا:</p>
            <div class="info-box info"><h5><i class="fas fa-bolt"></i> 1. جهد الأكسدة والاختزال (Redox Potential)</h5><p>تحديد قدرة العنصر على اكتساب أو فقد الإلكترونات بمرونة دون إحداث إجهاد تأكسدي عشوائي.</p></div>
            <div class="info-box purple"><h5><i class="fas fa-cube"></i> 2. الهندسة التناسقية (Coordination Geometry)</h5><p>شكل الروابط الفراغية التي يشكلها أيون الفلز مع المجموعات العضوية المحيطة به داخل الموقع النشط.</p></div>
          </div>
        </div>

        <div class="research-section" id="res-fe">
          <div class="section-header-element element-fe" onclick="openElement3D(26)">
            <div class="element-badge">Fe</div>
            <div class="element-info-title"><h2>أولاً: الحديد (Fe) — كيمياء الأكسجين وانتقال الإلكترونات</h2><p>العدد الذري: 26 | الوزن الذري: 55.84</p></div>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية والذرية للحديد</h4>
            <p>الحديد عنصر فلزي انتقالي (Transition Metal) ينتمي للسلسلة الانتقالية الأولى، وعدده الذري 26. يتميز بقابليته لفقد إلكترونات من 4s و 3d، مما يمنحه القدرة على الوجود في حالتي أكسدة:</p>
            <ul>
              <li><strong>الحديد الثنائي (Fe²⁺):</strong> تفاعلي وقابل للذوبان، الصورة الرئيسية في الارتباط بالأكسجين.</li>
              <li><strong>الحديد الثلاثي (Fe³⁺):</strong> الأكثر استقراراً في الوسط الأكسجيني، صورة التخزين.</li>
            </ul>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-lungs"></i> 2. آلية نقل الأكسجين عبر حلقة الهيم والهيموغلوبين</h4>
            <p>يتم احتجاز الحديد داخل الهيم (Heme)، الذي يتكون من حلقة بورفيرين (Porphyrin) تتوسطها ذرة حديد مرابطة بأربع روابط تناسقية مع ذرات نيتروجين.</p>
            <div class="info-box info"><h5><i class="fas fa-droplet"></i> التكامل الجزيئي</h5><p>تتيح البيئة الفراغية للهيموغلوبين بذرة الحديد (Fe²⁺) تكوين رابطة سادسة مع O₂، فيلتقطه في الرئتين ويطلقه في الأنسجة.</p></div>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-exclamation-triangle"></i> 3. الخطورة التأكسدية وتفاعل فنتون</h4>
            <p>على الرغم من أهميته، يمثل الحديد الحر خطراً على الخلية عبر تفاعل فنتون:</p>
            <div class="formula-box">Fe²⁺ + H₂O₂ → Fe³⁺ + OH⁻ + •OH</div>
            <div class="info-box danger"><h5><i class="fas fa-radiation"></i> خطر جذر الهيدروكسيل (•OH)</h5><p>جذر الهيدروكسيل يدمر الأغشية الخلوية والـ DNA. لذلك نظم الجسم بروتينات خاصة للتخزين (فيريتين) والنقل (ترانسفيرين).</p></div>
          </div>
        </div>

        <div class="research-section" id="res-zn">
          <div class="section-header-element element-zn" onclick="openElement3D(30)">
            <div class="element-badge">Zn</div>
            <div class="element-info-title"><h2>ثانياً: الزنك (Zn) — عنصر التثبيت البنيوي والتحفيز الإنزيمي</h2><p>العدد الذري: 30 | عنصر فلزي انتقالي</p></div>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية للزنك</h4>
            <p>يوجد الزنك حصرياً على صورة أيون Zn²⁺. يمتلك غلافاً إلكترونياً كاملاً (d10)، مما جعله خافتاً تأكسدياً ولا يشارك في تفاعلات الأكسدة المباشرة.</p>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-fingerprint"></i> 2. أصابع الزنك (Zinc Fingers)</h4>
            <p>يكوّن الزنك روابط تناسقية رباعية مع النيتروجين في الهيستيدين والكبريت في السيستين، مما ينتج ثنيات بروتينية صلبة (أصابع الزنك) تتحكم في التعبير الجيني.</p>
            <div class="info-box purple"><h5><i class="fas fa-dna"></i> التحكم في التعبير الجيني</h5><p>تعمل أصابع الزنك كقوالب هندسية تسمح للعوامل النووية بالقبض على DNA وتنظيم قراءة الجينات.</p></div>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-cog"></i> 3. الأنهيدراز الكربوني</h4>
            <p>يعمل Zn²⁺ كحمض لويس قوي في أكثر من 300 إنزيم. مثال:</p>
            <div class="formula-box">CO₂ + H₂O ⇌ HCO₃⁻ + H⁺</div>
          </div>
        </div>

        <div class="research-section" id="res-mg">
          <div class="section-header-element element-mg" onclick="openElement3D(12)">
            <div class="element-badge">Mg</div>
            <div class="element-info-title"><h2>ثالثاً: المغنيسيوم (Mg) — عنصر نقل الطاقة والاستقرار الجيني</h2><p>العدد الذري: 12 | الوزن الذري: 24.30</p></div>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية للمغنيسيوم</h4>
            <p>فلز من الفلزات القلوية الترابية (Group 2)، يوجد على هيئة Mg²⁺. يمتلك كثافة شحنة عالية جداً وألفة قوية لمجموعات الفوسفات السالبة.</p>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-bolt"></i> 2. معقد Mg-ATP</h4>
            <p>يحمل ATP أربع شحنات سالبة تجعل مجموعات الفوسفات تتنافر. يندمج المغنيسيوم مع ATP ليشكل معقد [Mg-ATP]²⁻ الضروري لاستخدام الطاقة.</p>
            <div class="formula-box">Mg²⁺ + ATP⁴⁻ → [Mg-ATP]²⁻</div>
          </div>
        </div>

        <div class="research-section" id="res-i">
          <div class="section-header-element element-i" onclick="openElement3D(53)">
            <div class="element-badge">I</div>
            <div class="element-info-title"><h2>رابعاً: اليود (I) — عنصر بناء الهرمونات</h2><p>العدد الذري: 53 | الوزن الذري: 126.90</p></div>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية لليود</h4>
            <p>عنصر لا فلزي من الهالوجينات (Group 17). يُلتقط الغذائي على صورة أيون يوديد (I⁻) ويُضخ إلى الغدة الدرقية عبر NIS Membrane Pump.</p>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-flask"></i> 2. الآلية الإنزيمية لبناء الهرمونات</h4>
            <p>يدخل اليود في روابط تساهمية كربونية-يودية دائمة داخل هرمونات الغدة الدرقية:</p>
            <ul>
              <li>أكسدة I⁻ باستخدام H₂O₂.</li>
              <li>إدراج اليود في حلقات التيروزين.</li>
              <li>اقتران لتشكيل هرمونات T₃ أو T₄.</li>
            </ul>
          </div>
        </div>

        <div class="research-section" id="res-se">
          <div class="section-header-element element-se" onclick="openElement3D(34)">
            <div class="element-badge">Se</div>
            <div class="element-info-title"><h2>خامساً: السيلينيوم (Se) — درع مضاد الأكسدة</h2><p>العدد الذري: 34 | الوزن الذري: 78.96</p></div>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-atom"></i> 1. الخصائص الكيميائية للسيلينيوم</h4>
            <p>يقع في المجموعة السادسة عشرة (Chalcogens). رابطة (Se-H) أكثر حامضية من (S-H). يدمج في الحمض الأميني رقم 21 (سيلينوسيستين).</p>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-shield-virus"></i> 2. إنزيم GPx</h4>
            <p>يدخل السيلينيوم في إنزيم الجلوتاثيون بيروكسيداز الذي يفكك البيروكسيدات:</p>
            <div class="formula-box">H₂O₂ + 2GSH → 2H₂O + GSSG</div>
          </div>
          <div class="subsection">
            <h4><i class="fas fa-handshake"></i> 3. التكامل مع اليود</h4>
            <p>إنزيمات Deiodinases (نازعات اليود) إنزيمات سيلينوبروتينية تحوّل T₄ الخامل إلى T₃ النشط.</p>
          </div>
        </div>

        <div class="research-section" id="res-table">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-table"></i></div>
            <div class="element-info-title"><h2>جدول المقارنة الشامل</h2><p>ملخص الخصائص والوظائف</p></div>
          </div>
          <div class="comparison-table-wrapper">
            <table class="comparison-table">
              <thead><tr><th>العنصر</th><th>الصورة الأيونية</th><th>الخاصية الكيميائية</th><th>الوظيفة الجزئية</th></tr></thead>
              <tbody>
                <tr onclick="openElement3D(26)" style="cursor:pointer"><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#b91c1c,#7f1d1d);">Fe</span> الحديد</div></td><td>Fe²⁺ / Fe³⁺</td><td>التناوب بين حالات الأكسدة</td><td>نقل O₂ ونقل الإلكترونات</td></tr>
                <tr onclick="openElement3D(30)" style="cursor:pointer"><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#0891b2,#155e75);">Zn</span> الزنك</div></td><td>Zn²⁺</td><td>روابط تناسقية رباعية</td><td>تثبيت البنية وتحفيز الإنزيمات</td></tr>
                <tr onclick="openElement3D(12)" style="cursor:pointer"><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#16a34a,#15803d);">Mg</span> المغنيسيوم</div></td><td>Mg²⁺</td><td>الألفة للفوسفات</td><td>معقد [Mg-ATP]²⁻ والريبوسومات</td></tr>
                <tr onclick="openElement3D(53)" style="cursor:pointer"><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#7c3aed,#5b21b6);">I</span> اليود</div></td><td>I⁻ / يود عضوي</td><td>روابط تساهمية دائمة</td><td>هرمونات T₃ و T₄</td></tr>
                <tr onclick="openElement3D(34)" style="cursor:pointer"><td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#ea580c,#c2410c);">Se</span> السيلينيوم</div></td><td>سيلينوسيستين</td><td>حمض أميني نادر</td><td>إنزيم GPx و Deiodinases</td></tr>
              </tbody>
            </table>
          </div>
        </div>

        <div class="research-section" id="res-conclusion">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-flag-checkered"></i></div>
            <div class="element-info-title"><h2>الخاتمة والتوصيات</h2><p>ملخص النتائج</p></div>
          </div>
          <div class="conclusion-box">
            <h3><i class="fas fa-lightbulb"></i> الخاتمة</h3>
            <p>تكشف هذه الدراسة عن حقيقة جوهرية: إن المظاهر الفسيولوجية المعقدة لجسم الإنسان تستند كلياً إلى سلوكيات تفاعلية دقيقة على المستوى الذري والإلكتروني.</p>
            <h3 style="margin-top:18px;"><i class="fas fa-bullseye"></i> التوصيات</h3>
            <p>نوصي بالتوسع في أبحاث علم الكيمياء التناسقية الحيوية (Bioinorganic Chemistry) لفهم التداخلات المباشرة بين النقص الغذائي والاضطرابات الجزئية.</p>
          </div>
        </div>

        <div class="research-section" id="res-refs">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-book"></i></div>
            <div class="element-info-title"><h2>المراجع العلمية</h2><p>المصادر المعتمدة</p></div>
          </div>
          <ol class="references-list">
            <li>Rolić, T., et al. (2025). Iron Metabolism, Calcium, Magnesium and Trace Elements: A Review. <em>Biological Trace Element Research</em>. <a href="https://doi.org/10.1007/s12011-024-04289-z" target="_blank">DOI</a></li>
            <li>Teaching the Chemical Elements in Biochemistry: Elemental Biology and Metallomics. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC9303777" target="_blank">Link</a></li>
            <li>Trace Elements in Human Nutrition (II) – An Update. <a href="https://pmc.ncbi.nlm.nih.gov/articles/6993532" target="_blank">Link</a></li>
            <li>Biochemistry, Nutrients. NCBI Bookshelf. <a href="https://www.ncbi.nlm.nih.gov/books/NBK554545" target="_blank">Link</a></li>
            <li>Essential Metals in Health and Disease. <em>Chemico-Biological Interactions</em>. <a href="https://doi.org/10.1016/j.cbi.2022.110173" target="_blank">DOI</a></li>
          </ol>
        </div>
        <button class="back-to-top" id="back-to-top"><i class="fas fa-chevron-up"></i></button>
      </div>
    </div>

    <!-- Periodic Table -->
    <div class="screen" id="periodic-table">
      <div class="header">
        <h2><i class="fas fa-th"></i> الجدول الدوري التفاعلي</h2>
        <div class="user-actions">
          <input type="text" id="pt-search" placeholder="🔍 ابحث عن عنصر..." style="min-width:200px;padding:9px 14px;border-radius:10px;border:1.5px solid rgba(255,255,255,.12);background:rgba(255,255,255,.05);color:#e2e8f0;">
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button>
        </div>
      </div>
      <div class="periodic-container">
        <div class="periodic-legend" id="pt-legend"></div>
        <div class="periodic-grid" id="pt-grid"></div>
        <div class="pt-info-panel" id="pt-info">👆 اضغط على أي عنصر لعرض نموذج بور ثلاثي الأبعاد ومعلوماته الكاملة</div>
      </div>
    </div>

    <!-- Calculator -->
    <div class="screen" id="calculator">
      <div class="header">
        <h2><i class="fas fa-calculator"></i> الآلة الحاسبة الكيميائية</h2>
        <div class="user-actions"><button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button></div>
      </div>
      <div class="calc-container">
        <div class="calc-tabs">
          <button class="calc-tab active" data-calc="molar"><i class="fas fa-weight"></i> الكتلة المولية</button>
          <button class="calc-tab" data-calc="ph"><i class="fas fa-flask"></i> pH و pOH</button>
          <button class="calc-tab" data-calc="dilution"><i class="fas fa-tint"></i> التخفيف</button>
          <button class="calc-tab" data-calc="gas"><i class="fas fa-wind"></i> الغاز المثالي</button>
          <button class="calc-tab" data-calc="balance"><i class="fas fa-balance-scale"></i> موازن المعادلات</button>
          <button class="calc-tab" data-calc="conc"><i class="fas fa-vial"></i> التركيز</button>
          <button class="calc-tab" data-calc="convert"><i class="fas fa-exchange-alt"></i> تحويلات</button>
          <button class="calc-tab" data-calc="energy"><i class="fas fa-bolt"></i> الطاقة</button>
        </div>

        <div class="calc-panel active" id="calc-molar">
          <h3><i class="fas fa-weight"></i> حاسبة الكتلة المولية</h3>
          <p class="calc-desc">أدخل الصيغة الكيميائية لحساب الكتلة المولية. مثال: H2SO4, C6H12O6, NaCl</p>
          <div class="calc-form">
            <div class="calc-field"><label>الصيغة الكيميائية</label><input type="text" id="molar-formula" placeholder="H2SO4" value="H2SO4"></div>
            <button class="btn" onclick="calcMolar()"><i class="fas fa-equals"></i> احسب الكتلة المولية</button>
          </div>
          <div class="calc-result" id="molar-result">
            <div class="calc-result-label">الكتلة المولية</div>
            <div class="calc-result-value" id="molar-value">-</div>
            <div class="calc-result-detail" id="molar-detail"></div>
          </div>
        </div>

        <div class="calc-panel" id="calc-ph">
          <h3><i class="fas fa-flask"></i> حاسبة pH و pOH</h3>
          <p class="calc-desc">اختر ما تعرفه (تركيز H⁺ أو OH⁻) وسنحسب لك الباقي</p>
          <div class="calc-form">
            <div class="calc-field"><label>نوع القيمة المدخلة</label>
              <select id="ph-type"><option value="h">تركيز [H⁺] بالمولار</option><option value="oh">تركيز [OH⁻] بالمولار</option><option value="ph">قيمة pH</option><option value="poh">قيمة pOH</option></select>
            </div>
            <div class="calc-field"><label>القيمة</label><input type="number" id="ph-value" placeholder="0.001" step="any" value="0.001"></div>
            <button class="btn" onclick="calcPH()"><i class="fas fa-equals"></i> احسب</button>
          </div>
          <div class="calc-result" id="ph-result">
            <div class="calc-result-label">النتيجة</div>
            <div class="calc-result-value" id="ph-value-out">-</div>
            <div class="calc-result-detail" id="ph-detail"></div>
          </div>
        </div>

        <div class="calc-panel" id="calc-dilution">
          <h3><i class="fas fa-tint"></i> حاسبة التخفيف</h3>
          <p class="calc-desc">C₁ × V₁ = C₂ × V₂ — اترك حقل واحد فارغ</p>
          <div class="calc-form">
            <div class="calc-form-row">
              <div class="calc-field"><label>C₁</label><input type="number" id="d-c1" placeholder="M₁" step="any"></div>
              <div class="calc-field"><label>V₁</label><input type="number" id="d-v1" placeholder="L أو mL" step="any"></div>
            </div>
            <div class="calc-form-row">
              <div class="calc-field"><label>C₂</label><input type="number" id="d-c2" placeholder="M₂" step="any"></div>
              <div class="calc-field"><label>V₂</label><input type="number" id="d-v2" placeholder="L أو mL" step="any"></div>
            </div>
            <button class="btn" onclick="calcDilution()"><i class="fas fa-equals"></i> احسب</button>
          </div>
          <div class="calc-result" id="dilution-result">
            <div class="calc-result-label">القيمة المحسوبة</div>
            <div class="calc-result-value" id="dilution-value">-</div>
            <div class="calc-result-detail" id="dilution-detail"></div>
          </div>
        </div>

        <div class="calc-panel" id="calc-gas">
          <h3><i class="fas fa-wind"></i> قانون الغاز المثالي</h3>
          <p class="calc-desc">PV = nRT — اترك حقل واحد فارغ. R = 0.0821</p>
          <div class="calc-form">
            <div class="calc-form-row">
              <div class="calc-field"><label>P (atm)</label><input type="number" id="g-p" step="any"></div>
              <div class="calc-field"><label>V (L)</label><input type="number" id="g-v" step="any"></div>
            </div>
            <div class="calc-form-row">
              <div class="calc-field"><label>n (مول)</label><input type="number" id="g-n" step="any"></div>
              <div class="calc-field"><label>T (K)</label><input type="number" id="g-t" step="any"></div>
            </div>
            <button class="btn" onclick="calcGas()"><i class="fas fa-equals"></i> احسب</button>
          </div>
          <div class="calc-result" id="gas-result">
            <div class="calc-result-label">النتيجة</div>
            <div class="calc-result-value" id="gas-value">-</div>
            <div class="calc-result-detail" id="gas-detail"></div>
          </div>
        </div>

        <div class="calc-panel" id="calc-balance">
          <h3><i class="fas fa-balance-scale"></i> موازن المعادلات</h3>
          <p class="calc-desc">أدخل المعادلة بصيغة: H2 + O2 = H2O</p>
          <div class="calc-form">
            <div class="calc-field"><label>المعادلة</label><input type="text" id="balance-eq" placeholder="H2 + O2 = H2O" value="H2 + O2 = H2O"></div>
            <button class="btn" onclick="calcBalance()"><i class="fas fa-equals"></i> وازن</button>
          </div>
          <div class="calc-help">💡 استخدم '=' أو '->' للفصل</div>
          <div class="calc-result" id="balance-result">
            <div class="calc-result-label">المعادلة الموزونة</div>
            <div class="calc-result-value" id="balance-value">-</div>
            <div class="calc-result-detail" id="balance-detail"></div>
          </div>
        </div>

        <div class="calc-panel" id="calc-conc">
          <h3><i class="fas fa-vial"></i> حاسبة التركيز</h3>
          <p class="calc-desc">احسب المولارية من الكتلة والحجم</p>
          <div class="calc-form">
            <div class="calc-form-row">
              <div class="calc-field"><label>الكتلة (g)</label><input type="number" id="c-mass" step="any"></div>
              <div class="calc-field"><label>الكتلة المولية (g/mol)</label><input type="number" id="c-mw" step="any"></div>
            </div>
            <div class="calc-field"><label>الحجم (L)</label><input type="number" id="c-vol" step="any"></div>
            <button class="btn" onclick="calcConc()"><i class="fas fa-equals"></i> احسب</button>
          </div>
          <div class="calc-result" id="conc-result">
            <div class="calc-result-label">التركيز</div>
            <div class="calc-result-value" id="conc-value">-</div>
            <div class="calc-result-detail" id="conc-detail"></div>
          </div>
        </div>

        <div class="calc-panel" id="calc-convert">
          <h3><i class="fas fa-exchange-alt"></i> تحويلات المول</h3>
          <p class="calc-desc">حوّل بين: g ← mol ← جزيئات</p>
          <div class="calc-form">
            <div class="calc-form-row">
              <div class="calc-field"><label>القيمة</label><input type="number" id="cv-val" step="any" value="1"></div>
              <div class="calc-field"><label>الكتلة المولية</label><input type="number" id="cv-mw" step="any" value="18"></div>
            </div>
            <div class="calc-field"><label>نوع القيمة</label>
              <select id="cv-type"><option value="g">كتلة (g)</option><option value="mol">مول</option><option value="atoms">عدد جزيئات</option></select>
            </div>
            <button class="btn" onclick="calcConvert()"><i class="fas fa-equals"></i> حول</button>
          </div>
          <div class="calc-result" id="convert-result">
            <div class="calc-result-label">النتيجة</div>
            <div class="calc-result-value" id="convert-value">-</div>
            <div class="calc-result-detail" id="convert-detail"></div>
          </div>
        </div>

        <div class="calc-panel" id="calc-energy">
          <h3><i class="fas fa-bolt"></i> حاسبة الطاقة</h3>
          <p class="calc-desc">E = m×c×ΔT أو E = mc² أو E = hf</p>
          <div class="calc-form">
            <div class="calc-field"><label>نوع الحساب</label>
              <select id="e-type" onchange="toggleEnergyFields()">
                <option value="heat">حرارة (q = mcΔT)</option>
                <option value="mass">طاقة الكتلة (E = mc²)</option>
                <option value="photon">طاقة الفوتون (E = hf)</option>
              </select>
            </div>
            <div id="energy-fields"></div>
            <button class="btn" onclick="calcEnergy()"><i class="fas fa-equals"></i> احسب</button>
          </div>
          <div class="calc-result" id="energy-result">
            <div class="calc-result-label">الطاقة</div>
            <div class="calc-result-value" id="energy-value">-</div>
            <div class="calc-result-detail" id="energy-detail"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- AI Assistant -->
    <div class="screen" id="ai-assistant">
      <div class="header">
        <h2><i class="fas fa-robot"></i> المساعد الذكي</h2>
        <div class="user-actions">
          <button class="btn btn-outline" onclick="openAISettings()"><i class="fas fa-key"></i> إعدادات API</button>
          <button class="btn btn-outline" onclick="clearChat()"><i class="fas fa-trash"></i> مسح</button>
        </div>
      </div>
      <div class="chat-container">
        <div class="chat-header">
          <div class="avatar"><i class="fas fa-atom"></i></div>
          <div class="info">
            <h3>كيميائي AI</h3>
            <p id="ai-status">🟢 جاهز — قاعدة معرفة داخلية</p>
          </div>
        </div>
        <div class="chat-msgs" id="chat-msgs"></div>
        <div class="chat-suggestions" id="chat-suggestions">
          <div class="chat-suggestion" onclick="sendSuggestion('ما هي الذرة؟')">ما هي الذرة؟</div>
          <div class="chat-suggestion" onclick="sendSuggestion('اشرح الروابط الكيميائية')">الروابط الكيميائية</div>
          <div class="chat-suggestion" onclick="sendSuggestion('ما هو pH؟')">ما هو pH؟</div>
          <div class="chat-suggestion" onclick="sendSuggestion('ما هي الأحماض والقواعد؟')">الأحماض والقواعد</div>
          <div class="chat-suggestion" onclick="sendSuggestion('اشرح التفاعلات الكيميائية')">التفاعلات</div>
          <div class="chat-suggestion" onclick="sendSuggestion('ما هو المول؟')">المول</div>
        </div>
        <div class="chat-input-area">
          <textarea id="chat-input" rows="1" placeholder="اكتب سؤالك الكيميائي هنا..."></textarea>
          <button class="btn chat-send-btn" onclick="sendChat()"><i class="fas fa-paper-plane"></i></button>
        </div>
      </div>
    </div>

    <!-- Quiz Setup -->
    <div class="screen" id="quiz-setup">
      <div class="header">
        <h2><i class="fas fa-file-alt"></i> إعداد الاختبار</h2>
        <div class="user-actions"><button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button></div>
      </div>
      <div class="quiz-setup">
        <h3>اختر نوع الاختبار</h3>
        <div class="quiz-options">
          <div class="quiz-option selected" data-mode="practice"><i class="fas fa-graduation-cap"></i><h4>وضع الممارسة</h4><p>تعلم بالسرعة التي تناسبك مع تصحيح فوري</p></div>
          <div class="quiz-option" data-mode="exam"><i class="fas fa-clock"></i><h4>وضع الامتحان</h4><p>اختبر نفسك في ظروف مشابهة للامتحان</p></div>
        </div>
        <div class="setting-item">
          <div class="setting-info"><h4>عدد الأسئلة</h4><p>اختر عدد الأسئلة</p></div>
          <select id="question-count"><option value="10">10 أسئلة</option><option value="20">20 سؤالاً</option><option value="30">30 سؤالاً</option><option value="50" selected>50 سؤالاً (كاملة)</option></select>
        </div>
        <div class="setting-item">
          <div class="setting-info"><h4>مستوى الصعوبة</h4><p>اختر المستوى المناسب</p></div>
          <select id="difficulty"><option value="all">جميع المستويات</option><option value="easy">سهل</option><option value="medium" selected>متوسط</option><option value="hard">صعب</option></select>
        </div>
        <div class="quiz-controls">
          <button class="btn" id="start-quiz"><i class="fas fa-play"></i> بدء الاختبار</button>
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-times"></i> إلغاء</button>
        </div>
      </div>
    </div>

    <!-- Quiz -->
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
            <div class="quiz-info"><div id="question-counter">السؤال 1 من 50</div><div id="score-display">النقاط: 0</div></div>
          </div>
          <div class="progress-bar"><div class="progress" id="progress"></div></div>
          <div class="question" id="question"></div>
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
          <div class="whiteboard-info"><p>استخدم السبورة لحل المسائل</p></div>
        </div>
      </div>
    </div>

    <!-- Results -->
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
            <div class="improvement-tips"><h4><i class="fas fa-lightbulb"></i> نصائح للتحسين</h4><ul id="improvement-tips"></ul></div>
          </div>
        </div>
        <div class="quiz-controls" style="margin-top:22px;">
          <button class="btn" onclick="navigateTo('quiz-setup')"><i class="fas fa-redo"></i> اختبار جديد</button>
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-home"></i> الرئيسية</button>
        </div>
      </div>
    </div>

    <!-- Settings -->
    <div class="screen" id="settings">
      <div class="header"><h2><i class="fas fa-cog"></i> الإعدادات</h2></div>
      <div class="settings-container">
        <div class="settings-nav">
          <button class="settings-nav-btn active" data-panel="account"><i class="fas fa-user-cog"></i> الحساب</button>
          <button class="settings-nav-btn" data-panel="appearance"><i class="fas fa-palette"></i> المظهر</button>
          <button class="settings-nav-btn" data-panel="ai"><i class="fas fa-robot"></i> الذكاء الاصطناعي</button>
          <button class="settings-nav-btn" data-panel="notifications"><i class="fas fa-bell"></i> الإشعارات</button>
          <button class="settings-nav-btn" data-panel="quiz"><i class="fas fa-file-alt"></i> الاختبارات</button>
          <button class="settings-nav-btn" data-panel="data"><i class="fas fa-database"></i> البيانات</button>
        </div>

        <div class="settings-panel active" id="panel-account">
          <div class="settings-group">
            <h3><i class="fas fa-id-card"></i> معلومات الحساب</h3>
            <div class="setting-item"><div class="setting-info"><h4>الاسم الكامل</h4><p>الاسم المعروض</p></div><input type="text" id="username"></div>
            <div class="setting-item"><div class="setting-info"><h4>اسم المستخدم</h4><p>معرف فريد</p></div><input type="text" id="user-username" readonly></div>
            <div class="setting-item"><div class="setting-info"><h4>البريد الإلكتروني</h4><p>للاستقبال الإشعارات</p></div><input type="email" id="user-email"></div>
            <div class="setting-item"><div class="setting-info"><h4>المستوى</h4><p>مستواك الحالي</p></div>
              <select id="user-level-select"><option value="beginner">مبتدئ</option><option value="intermediate">متوسط</option><option value="advanced">متقدم</option></select>
            </div>
            <div class="setting-item"><div class="setting-info"><h4>تغيير كلمة المرور</h4><p>تحديث كلمة المرور</p></div><button class="btn btn-outline" onclick="openChangePassword()"><i class="fas fa-key"></i> تغيير</button></div>
          </div>
          <div class="settings-group">
            <h3><i class="fas fa-image"></i> الصورة الرمزية</h3>
            <div class="avatar-picker" id="avatar-picker"></div>
          </div>
        </div>

        <div class="settings-panel" id="panel-appearance">
          <div class="settings-group">
            <h3><i class="fas fa-moon"></i> الوضع العام</h3>
            <div class="setting-item"><div class="setting-info"><h4>الوضع الليلي</h4><p>مظهر داكن</p></div><label class="toggle-switch"><input type="checkbox" id="dark-mode" checked><span class="slider"></span></label></div>
            <div class="setting-item"><div class="setting-info"><h4>الوضع المضغوط</h4><p>تقليل الحشو</p></div><label class="toggle-switch"><input type="checkbox" id="compact-mode"><span class="slider"></span></label></div>
            <div class="setting-item"><div class="setting-info"><h4>تفعيل الحركات</h4><p>الرسوم المتحركة</p></div><label class="toggle-switch"><input type="checkbox" id="animations-enabled" checked><span class="slider"></span></label></div>
          </div>
          <div class="settings-group"><h3><i class="fas fa-tint"></i> لون الواجهة</h3><div class="theme-picker" id="theme-picker"></div></div>
          <div class="settings-group"><h3><i class="fas fa-font"></i> حجم الخط</h3>
            <div class="font-size-picker">
              <button class="font-size-btn" data-size="14">ص</button>
              <button class="font-size-btn active" data-size="16">ص</button>
              <button class="font-size-btn" data-size="18">ص</button>
            </div>
          </div>
        </div>

        <div class="settings-panel" id="panel-ai">
          <div class="settings-group">
            <h3><i class="fas fa-key"></i> مفاتيح API للذكاء الاصطناعي</h3>
            <p style="color:#94a3b8;font-size:.85em;margin-bottom:16px;line-height:1.7;">لتفعيل الذكاء الاصطناعي الحقيقي، أدخل مفتاح API مجاني من Groq أو Google Gemini. سيتم حفظ المفتاح في جهازك فقط. إذا لم تُدخل مفتاحاً، سيعمل المساعد بقاعدة معرفة داخلية شاملة.</p>
            <div class="setting-item">
              <div class="setting-info"><h4>مزود الخدمة</h4><p>Groq (أسرع) أو Gemini (Google)</p></div>
              <select id="ai-provider"><option value="internal">داخلي (بدون API)</option><option value="groq">Groq</option><option value="gemini">Google Gemini</option></select>
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>Groq API Key</h4><p>من console.groq.com (مجاني)</p></div>
              <input type="password" id="ai-groq-key" placeholder="gsk_..." style="min-width:250px;">
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>Gemini API Key</h4><p>من aistudio.google.com (مجاني)</p></div>
              <input type="password" id="ai-gemini-key" placeholder="AIza..." style="min-width:250px;">
            </div>
            <div class="setting-item">
              <div class="setting-info"><h4>حفظ المفاتيح</h4><p>حفظ في المتصفح</p></div>
              <button class="btn" onclick="saveAIKeys()"><i class="fas fa-save"></i> حفظ المفاتيح</button>
            </div>
          </div>
        </div>

        <div class="settings-panel" id="panel-notifications">
          <div class="settings-group">
            <h3><i class="fas fa-bell"></i> تفضيلات الإشعارات</h3>
            <div class="setting-item"><div class="setting-info"><h4>تفعيل الإشعارات</h4><p>عرض الإشعارات</p></div><label class="toggle-switch"><input type="checkbox" id="notifications-enabled" checked><span class="slider"></span></label></div>
            <div class="setting-item"><div class="setting-info"><h4>تنبيهات الاختبارات</h4><p>عند إتمام الاختبارات</p></div><label class="toggle-switch"><input type="checkbox" id="quiz-notif" checked><span class="slider"></span></label></div>
            <div class="setting-item"><div class="setting-info"><h4>تنبيهات الإنجازات</h4><p>عند تحقيق إنجاز</p></div><label class="toggle-switch"><input type="checkbox" id="achieve-notif" checked><span class="slider"></span></label></div>
          </div>
        </div>

        <div class="settings-panel" id="panel-quiz">
          <div class="settings-group">
            <h3><i class="fas fa-sliders-h"></i> تفضيلات الاختبارات</h3>
            <div class="setting-item"><div class="setting-info"><h4>عدد الأسئلة الافتراضي</h4><p>في كل اختبار</p></div><input type="number" id="default-question-count" value="20" min="5" max="50"></div>
            <div class="setting-item"><div class="setting-info"><h4>مستوى الصعوبة</h4><p>افتراضي</p></div>
              <select id="default-difficulty"><option value="all">جميع المستويات</option><option value="easy">سهل</option><option value="medium" selected>متوسط</option><option value="hard">صعب</option></select>
            </div>
            <div class="setting-item"><div class="setting-info"><h4>الشرح الفوري</h4><p>عرض التفسير مباشرة</p></div><label class="toggle-switch"><input type="checkbox" id="instant-explain" checked><span class="slider"></span></label></div>
            <div class="setting-item"><div class="setting-info"><h4>مدة المؤقت</h4><p>في وضع الامتحان</p></div>
              <select id="timer-duration"><option value="15">15 دقيقة</option><option value="30" selected>30 دقيقة</option><option value="45">45 دقيقة</option><option value="60">60 دقيقة</option></select>
            </div>
          </div>
        </div>

        <div class="settings-panel" id="panel-data">
          <div class="settings-group">
            <h3><i class="fas fa-database"></i> إدارة البيانات</h3>
            <div class="setting-item"><div class="setting-info"><h4>حجم البيانات</h4><p id="data-size">-- KB</p></div><button class="btn btn-outline" onclick="calculateDataSize()"><i class="fas fa-calculator"></i> حساب</button></div>
            <div class="setting-item"><div class="setting-info"><h4>تصدير البيانات</h4><p>تنزيل نسخة احتياطية</p></div><button class="btn btn-outline" id="export-data"><i class="fas fa-download"></i> تصدير</button></div>
            <div class="setting-item"><div class="setting-info"><h4>استيراد</h4><p>استعادة نسخة</p></div><label class="btn btn-outline" style="cursor:pointer;"><i class="fas fa-upload"></i> استيراد<input type="file" id="import-file" accept=".json" hidden></label></div>
            <div class="setting-item"><div class="setting-info"><h4>حذف جميع البيانات</h4><p>مسح كل شيء</p></div><button class="btn btn-danger" onclick="clearAllData()"><i class="fas fa-trash"></i> حذف الكل</button></div>
          </div>
        </div>

        <div class="quiz-controls">
          <button class="btn" id="save-settings"><i class="fas fa-save"></i> حفظ الإعدادات</button>
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-times"></i> إلغاء</button>
        </div>
      </div>
    </div>

    <!-- Simulation -->
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
          <p>مقدمة من مشروع PhET التفاعلي بجامعة كولورادو.</p>
        </div>
      </div>
    </div>

    <!-- Notifications -->
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

    <!-- Profile -->
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
            <div class="profile-name"><span id="profile-username-text">-</span><span id="profile-rank" class="rank-badge rank-inert">غاز خامل</span></div>
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

<!-- 3D Element Modal -->
<div class="modal-overlay" id="modal-3d-overlay">
  <div class="modal modal-3d">
    <div class="modal-3d-header">
      <h3 id="modal-3d-title"><i class="fas fa-atom"></i> نموذج ذري</h3>
      <button class="close-btn" onclick="closeElement3D()"><i class="fas fa-times"></i></button>
    </div>
    <div class="modal-3d-body">
      <div class="atomic-3d-canvas" id="atomic-3d-canvas">
        <div class="atomic-3d-hint">🖱️ اسحب للتدوير | عجلة الفأرة للتكبير</div>
      </div>
      <div class="atomic-info" id="atomic-info"></div>
    </div>
  </div>
</div>

<!-- AI Settings Modal -->
<div class="modal-overlay" id="ai-modal-overlay">
  <div class="modal">
    <h3><i class="fas fa-key"></i> إعدادات الذكاء الاصطناعي</h3>
    <p>لتشغيل AI الحقيقي، اختر مزود وحفظ المفتاح:</p>
    <div class="setting-item"><div class="setting-info"><h4>المزود</h4></div>
      <select id="ai-modal-provider" style="min-width:200px;">
        <option value="internal">داخلي (بدون API)</option>
        <option value="groq">Groq</option>
        <option value="gemini">Google Gemini</option>
      </select>
    </div>
    <div class="setting-item"><div class="setting-info"><h4>Groq Key</h4></div><input type="password" id="ai-modal-groq" placeholder="gsk_..." style="min-width:250px;"></div>
    <div class="setting-item"><div class="setting-info"><h4>Gemini Key</h4></div><input type="password" id="ai-modal-gemini" placeholder="AIza..." style="min-width:250px;"></div>
    <div class="calc-help" style="margin:16px 0;">
      💡 <strong>Groq:</strong> من <a href="https://console.groq.com" target="_blank" style="color:var(--cyan);">console.groq.com</a> — مجاني<br>
      💡 <strong>Gemini:</strong> من <a href="https://aistudio.google.com/app/apikey" target="_blank" style="color:var(--cyan);">aistudio.google.com</a> — مجاني
    </div>
    <div class="modal-actions">
      <button class="btn btn-outline" onclick="closeAIModal()">إلغاء</button>
      <button class="btn" onclick="saveAIModal()"><i class="fas fa-save"></i> حفظ</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"><i class="fas fa-info-circle"></i><span id="toast-message"></span></div>
<div class="url-indicator" id="url-indicator" title="اضغط لنسخ الرابط"></div>

<audio id="correct-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/257/257-preview.mp3" type="audio/mp3"></audio>
<audio id="fail-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/250/250-preview.mp3" type="audio/mp3"></audio>
<audio id="complete-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/255/255-preview.mp3" type="audio/mp3"></audio>

<script>
/* ============================================================
   🌌 Background
   ============================================================ */
(function(){
  const canvas = document.getElementById('bg-canvas');
  const ctx = canvas.getContext('2d');
  let W,H,particles=[];
  const isMobile = window.innerWidth < 768;
  const COUNT = isMobile ? 40 : 80;
  const MAX_DIST = isMobile ? 100 : 140;
  function resize(){ W=canvas.width=window.innerWidth; H=canvas.height=window.innerHeight; }
  resize(); window.addEventListener('resize', resize);
  const colors=['#6366f1','#8b5cf6','#06d6a0','#22d3ee','#ec4899'];
  function P(){ this.x=Math.random()*W; this.y=Math.random()*H; this.vx=(Math.random()-.5)*.5; this.vy=(Math.random()-.5)*.5; this.r=Math.random()*2+1; this.color=colors[Math.floor(Math.random()*colors.length)]; this.pulse=Math.random()*Math.PI*2; }
  P.prototype.update=function(){ this.x+=this.vx; this.y+=this.vy; if(this.x<0||this.x>W) this.vx*=-1; if(this.y<0||this.y>H) this.vy*=-1; this.pulse+=.03; };
  P.prototype.draw=function(){ const r=this.r+Math.sin(this.pulse)*.5; ctx.beginPath(); ctx.arc(this.x,this.y,Math.max(.5,r),0,Math.PI*2); ctx.fillStyle=this.color; ctx.shadowBlur=12; ctx.shadowColor=this.color; ctx.fill(); ctx.shadowBlur=0; };
  for(let i=0;i<COUNT;i++) particles.push(new P());
  const mouse={x:-1000,y:-1000};
  window.addEventListener('mousemove',e=>{mouse.x=e.clientX;mouse.y=e.clientY;});
  window.addEventListener('mouseleave',()=>{mouse.x=-1000;mouse.y=-1000;});
  function animate(){
    ctx.clearRect(0,0,W,H);
    for(let i=0;i<particles.length;i++){
      const p1=particles[i];
      for(let j=i+1;j<particles.length;j++){
        const p2=particles[j];
        const dx=p1.x-p2.x, dy=p1.y-p2.y;
        const d=Math.sqrt(dx*dx+dy*dy);
        if(d<MAX_DIST){
          const a=(1-d/MAX_DIST)*.35;
          ctx.beginPath(); ctx.moveTo(p1.x,p1.y); ctx.lineTo(p2.x,p2.y);
          const g=ctx.createLinearGradient(p1.x,p1.y,p2.x,p2.y);
          g.addColorStop(0,p1.color); g.addColorStop(1,p2.color);
          ctx.strokeStyle=g; ctx.globalAlpha=a; ctx.lineWidth=.7; ctx.stroke(); ctx.globalAlpha=1;
        }
      }
      const mdx=p1.x-mouse.x, mdy=p1.y-mouse.y;
      const md=Math.sqrt(mdx*mdx+mdy*mdy);
      if(md<180){
        const a=(1-md/180)*.6;
        ctx.beginPath(); ctx.moveTo(p1.x,p1.y); ctx.lineTo(mouse.x,mouse.y);
        ctx.strokeStyle=p1.color; ctx.globalAlpha=a; ctx.lineWidth=1; ctx.stroke(); ctx.globalAlpha=1;
      }
    }
    particles.forEach(p=>{p.update(); p.draw();});
    requestAnimationFrame(animate);
  }
  animate();
})();

/* ============================================================
   🧭 Router
   ============================================================ */
const VALID_SCREENS = ['auth','welcome','chemistry-human','periodic-table','calculator','ai-assistant','quiz-setup','quiz','results','settings','simulation','notifications','profile'];
function getScreenFromHash(){
  let h = window.location.hash.replace(/^#\/?/,'');
  return VALID_SCREENS.includes(h) ? h : (h ? 'welcome' : 'auth');
}
function navigateTo(screen, replace){
  if(!VALID_SCREENS.includes(screen)) screen='welcome';
  const hash='#/'+screen;
  if(replace){ history.replaceState(null,'',hash); handleRoute(); }
  else { if(window.location.hash===hash) handleRoute(); else window.location.hash=hash; }
}
function handleRoute(){
  const screen=getScreenFromHash();
  const user=currentUser();
  if(!user && screen!=='auth'){ navigateTo('auth', true); return; }
  if(user && screen==='auth'){ navigateTo('welcome', true); return; }
  if(screen==='auth'){
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
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-link').forEach(l=>l.classList.remove('active'));
  const t=document.getElementById(id);
  if(t) t.classList.add('active');
  const l=document.querySelector(`.nav-link[data-screen="${id}"]`);
  if(l) l.classList.add('active');
  if(id==='notifications') renderNotifications();
  if(id==='profile') updateProfileUI();
  if(id==='settings') loadSettingsUI();
  if(id==='chemistry-human') setupResearchLinks();
  if(id==='welcome') updateDashboard();
  if(id==='periodic-table') renderPeriodicTable();
  if(id==='ai-assistant') initChat();
  if(id==='calculator') initCalculator();
  const mc=document.querySelector('.main-content');
  if(mc) mc.scrollTop=0;
}
function updateUrlIndicator(){
  const ind=document.getElementById('url-indicator');
  if(!ind) return;
  ind.textContent='🔗 '+window.location.href;
  ind.classList.add('visible');
}
document.getElementById('url-indicator').addEventListener('click',function(){
  navigator.clipboard.writeText(window.location.href).then(()=>{
    const o=this.textContent; this.textContent='✅ تم نسخ الرابط!';
    setTimeout(()=>this.textContent=o,1500);
  });
});

/* ============================================================
   📊 Periodic Table Data
   ============================================================ */
const PT = [
[1,'H','الهيدروجين','Hydrogen',1.008,'nonmetal',1,1,'1'],
[2,'He','الهيليوم','Helium',4.003,'noble',18,1,'2'],
[3,'Li','الليثيوم','Lithium',6.94,'alkali',1,2,'2,1'],
[4,'Be','البيريليوم','Beryllium',9.012,'alkaline',2,2,'2,2'],
[5,'B','البورون','Boron',10.81,'metalloid',13,2,'2,3'],
[6,'C','الكربون','Carbon',12.011,'nonmetal',14,2,'2,4'],
[7,'N','النيتروجين','Nitrogen',14.007,'nonmetal',15,2,'2,5'],
[8,'O','الأكسجين','Oxygen',15.999,'nonmetal',16,2,'2,6'],
[9,'F','الفلور','Fluorine',18.998,'halogen',17,2,'2,7'],
[10,'Ne','النيون','Neon',20.180,'noble',18,2,'2,8'],
[11,'Na','الصوديوم','Sodium',22.990,'alkali',1,3,'2,8,1'],
[12,'Mg','المغنيسيوم','Magnesium',24.305,'alkaline',2,3,'2,8,2'],
[13,'Al','الألومنيوم','Aluminum',26.982,'post-transition',13,3,'2,8,3'],
[14,'Si','السيليكون','Silicon',28.086,'metalloid',14,3,'2,8,4'],
[15,'P','الفوسفور','Phosphorus',30.974,'nonmetal',15,3,'2,8,5'],
[16,'S','الكبريت','Sulfur',32.06,'nonmetal',16,3,'2,8,6'],
[17,'Cl','الكلور','Chlorine',35.45,'halogen',17,3,'2,8,7'],
[18,'Ar','الأرجون','Argon',39.948,'noble',18,3,'2,8,8'],
[19,'K','البوتاسيوم','Potassium',39.098,'alkali',1,4,'2,8,8,1'],
[20,'Ca','الكالسيوم','Calcium',40.078,'alkaline',2,4,'2,8,8,2'],
[21,'Sc','السكانديوم','Scandium',44.956,'transition',3,4,'2,8,9,2'],
[22,'Ti','التيتانيوم','Titanium',47.867,'transition',4,4,'2,8,10,2'],
[23,'V','الفاناديوم','Vanadium',50.942,'transition',5,4,'2,8,11,2'],
[24,'Cr','الكروم','Chromium',51.996,'transition',6,4,'2,8,13,1'],
[25,'Mn','المنجنيز','Manganese',54.938,'transition',7,4,'2,8,13,2'],
[26,'Fe','الحديد','Iron',55.845,'transition',8,4,'2,8,14,2'],
[27,'Co','الكوبالت','Cobalt',58.933,'transition',9,4,'2,8,15,2'],
[28,'Ni','النيكل','Nickel',58.693,'transition',10,4,'2,8,16,2'],
[29,'Cu','النحاس','Copper',63.546,'transition',11,4,'2,8,18,1'],
[30,'Zn','الزنك','Zinc',65.38,'transition',12,4,'2,8,18,2'],
[31,'Ga','الجاليوم','Gallium',69.723,'post-transition',13,4,'2,8,18,3'],
[32,'Ge','الجرمانيوم','Germanium',72.630,'metalloid',14,4,'2,8,18,4'],
[33,'As','الزرنيخ','Arsenic',74.922,'metalloid',15,4,'2,8,18,5'],
[34,'Se','السيلينيوم','Selenium',78.971,'nonmetal',16,4,'2,8,18,6'],
[35,'Br','البروم','Bromine',79.904,'halogen',17,4,'2,8,18,7'],
[36,'Kr','الكريبتون','Krypton',83.798,'noble',18,4,'2,8,18,8'],
[37,'Rb','الروبيديوم','Rubidium',85.468,'alkali',1,5,'2,8,18,8,1'],
[38,'Sr','السترونشيوم','Strontium',87.62,'alkaline',2,5,'2,8,18,8,2'],
[39,'Y','الإيتريوم','Yttrium',88.906,'transition',3,5,'2,8,18,9,2'],
[40,'Zr','الزركونيوم','Zirconium',91.224,'transition',4,5,'2,8,18,10,2'],
[41,'Nb','النيوبيوم','Niobium',92.906,'transition',5,5,'2,8,18,12,1'],
[42,'Mo','الموليبدينوم','Molybdenum',95.95,'transition',6,5,'2,8,18,13,1'],
[43,'Tc','التكنيشيوم','Technetium',98,'transition',7,5,'2,8,18,13,2'],
[44,'Ru','الروثينيوم','Ruthenium',101.07,'transition',8,5,'2,8,18,15,1'],
[45,'Rh','الروديوم','Rhodium',102.906,'transition',9,5,'2,8,18,16,1'],
[46,'Pd','البلاديوم','Palladium',106.42,'transition',10,5,'2,8,18,18'],
[47,'Ag','الفضة','Silver',107.868,'transition',11,5,'2,8,18,18,1'],
[48,'Cd','الكادميوم','Cadmium',112.414,'transition',12,5,'2,8,18,18,2'],
[49,'In','الإنديوم','Indium',114.818,'post-transition',13,5,'2,8,18,18,3'],
[50,'Sn','القصدير','Tin',118.710,'post-transition',14,5,'2,8,18,18,4'],
[51,'Sb','الأنتيمون','Antimony',121.760,'metalloid',15,5,'2,8,18,18,5'],
[52,'Te','التيلوريوم','Tellurium',127.60,'metalloid',16,5,'2,8,18,18,6'],
[53,'I','اليود','Iodine',126.904,'halogen',17,5,'2,8,18,18,7'],
[54,'Xe','الزينون','Xenon',131.293,'noble',18,5,'2,8,18,18,8'],
[55,'Cs','السيزيوم','Cesium',132.905,'alkali',1,6,'2,8,18,18,8,1'],
[56,'Ba','الباريوم','Barium',137.327,'alkaline',2,6,'2,8,18,18,8,2'],
[57,'La','اللانثانوم','Lanthanum',138.905,'lanthanide',3,9,'2,8,18,18,9,2'],
[58,'Ce','السيريوم','Cerium',140.116,'lanthanide',4,9,'2,8,18,19,9,2'],
[59,'Pr','البراسيوديميوم','Praseodymium',140.908,'lanthanide',5,9,'2,8,18,21,8,2'],
[60,'Nd','النيوديميوم','Neodymium',144.242,'lanthanide',6,9,'2,8,18,22,8,2'],
[61,'Pm','البروميثيوم','Promethium',145,'lanthanide',7,9,'2,8,18,23,8,2'],
[62,'Sm','الساماريوم','Samarium',150.36,'lanthanide',8,9,'2,8,18,24,8,2'],
[63,'Eu','اليوروبيوم','Europium',151.964,'lanthanide',9,9,'2,8,18,25,8,2'],
[64,'Gd','الجادولينيوم','Gadolinium',157.25,'lanthanide',10,9,'2,8,18,25,9,2'],
[65,'Tb','التيربيوم','Terbium',158.925,'lanthanide',11,9,'2,8,18,27,8,2'],
[66,'Dy','الديسبروسيوم','Dysprosium',162.5,'lanthanide',12,9,'2,8,18,28,8,2'],
[67,'Ho','الهولميوم','Holmium',164.930,'lanthanide',13,9,'2,8,18,29,8,2'],
[68,'Er','الإربيوم','Erbium',167.259,'lanthanide',14,9,'2,8,18,30,8,2'],
[69,'Tm','الثوليوم','Thulium',168.934,'lanthanide',15,9,'2,8,18,31,8,2'],
[70,'Yb','الإيتربيوم','Ytterbium',173.045,'lanthanide',16,9,'2,8,18,32,8,2'],
[71,'Lu','اللوتيشيوم','Lutetium',174.967,'lanthanide',17,9,'2,8,18,32,9,2'],
[72,'Hf','الهافنيوم','Hafnium',178.49,'transition',4,6,'2,8,18,32,10,2'],
[73,'Ta','التانتالوم','Tantalum',180.948,'transition',5,6,'2,8,18,32,11,2'],
[74,'W','التنجستن','Tungsten',183.84,'transition',6,6,'2,8,18,32,12,2'],
[75,'Re','الرينيوم','Rhenium',186.207,'transition',7,6,'2,8,18,32,13,2'],
[76,'Os','الأوزميوم','Osmium',190.23,'transition',8,6,'2,8,18,32,14,2'],
[77,'Ir','الإيريديوم','Iridium',192.217,'transition',9,6,'2,8,18,32,15,2'],
[78,'Pt','البلاتين','Platinum',195.084,'transition',10,6,'2,8,18,32,17,1'],
[79,'Au','الذهب','Gold',196.967,'transition',11,6,'2,8,18,32,18,1'],
[80,'Hg','الزئبق','Mercury',200.592,'transition',12,6,'2,8,18,32,18,2'],
[81,'Tl','الثاليوم','Thallium',204.38,'post-transition',13,6,'2,8,18,32,18,3'],
[82,'Pb','الرصاص','Lead',207.2,'post-transition',14,6,'2,8,18,32,18,4'],
[83,'Bi','البزموت','Bismuth',208.980,'post-transition',15,6,'2,8,18,32,18,5'],
[84,'Po','البولونيوم','Polonium',209,'post-transition',16,6,'2,8,18,32,18,6'],
[85,'At','الأستاتين','Astatine',210,'halogen',17,6,'2,8,18,32,18,7'],
[86,'Rn','الرادون','Radon',222,'noble',18,6,'2,8,18,32,18,8'],
[87,'Fr','الفرانسيوم','Francium',223,'alkali',1,7,'2,8,18,32,18,8,1'],
[88,'Ra','الراديوم','Radium',226,'alkaline',2,7,'2,8,18,32,18,8,2'],
[89,'Ac','الأكتينيوم','Actinium',227,'actinide',3,10,'2,8,18,32,18,9,2'],
[90,'Th','الثوريوم','Thorium',232.038,'actinide',4,10,'2,8,18,32,18,10,2'],
[91,'Pa','البروتكتينيوم','Protactinium',231.036,'actinide',5,10,'2,8,18,32,20,9,2'],
[92,'U','اليورانيوم','Uranium',238.029,'actinide',6,10,'2,8,18,32,21,9,2'],
[93,'Np','النبتونيوم','Neptunium',237,'actinide',7,10,'2,8,18,32,22,9,2'],
[94,'Pu','البلوتونيوم','Plutonium',244,'actinide',8,10,'2,8,18,32,24,8,2'],
[95,'Am','الأمريسيوم','Americium',243,'actinide',9,10,'2,8,18,32,25,8,2'],
[96,'Cm','الكوريوم','Curium',247,'actinide',10,10,'2,8,18,32,25,9,2'],
[97,'Bk','البركيليوم','Berkelium',247,'actinide',11,10,'2,8,18,32,27,8,2'],
[98,'Cf','الكاليفورنيوم','Californium',251,'actinide',12,10,'2,8,18,32,28,8,2'],
[99,'Es','الأينشتاينيوم','Einsteinium',252,'actinide',13,10,'2,8,18,32,29,8,2'],
[100,'Fm','الفرميوم','Fermium',257,'actinide',14,10,'2,8,18,32,30,8,2'],
[101,'Md','المندليفيوم','Mendelevium',258,'actinide',15,10,'2,8,18,32,31,8,2'],
[102,'No','النوبليوم','Nobelium',259,'actinide',16,10,'2,8,18,32,32,8,2'],
[103,'Lr','اللورنسيوم','Lawrencium',266,'actinide',17,10,'2,8,18,32,32,8,3'],
[104,'Rf','الرذرفورديوم','Rutherfordium',267,'transition',4,7,'2,8,18,32,32,10,2'],
[105,'Db','الدوبنيوم','Dubnium',268,'transition',5,7,'2,8,18,32,32,11,2'],
[106,'Sg','السيبورجيوم','Seaborgium',269,'transition',6,7,'2,8,18,32,32,12,2'],
[107,'Bh','البوريوم','Bohrium',270,'transition',7,7,'2,8,18,32,32,13,2'],
[108,'Hs','الهاسيوم','Hassium',269,'transition',8,7,'2,8,18,32,32,14,2'],
[109,'Mt','الميتمريوم','Meitnerium',278,'unknown',9,7,'2,8,18,32,32,15,2'],
[110,'Ds','الدارمشتاتيوم','Darmstadtium',281,'unknown',10,7,'2,8,18,32,32,16,2'],
[111,'Rg','الرونتجينيوم','Roentgenium',282,'unknown',11,7,'2,8,18,32,32,17,2'],
[112,'Cn','الكوبرنيسيوم','Copernicium',285,'transition',12,7,'2,8,18,32,32,18,2'],
[113,'Nh','النيهونيوم','Nihonium',286,'unknown',13,7,'2,8,18,32,32,18,3'],
[114,'Fl','الفليفوريوم','Flerovium',289,'unknown',14,7,'2,8,18,32,32,18,4'],
[115,'Mc','الموسكوفيوم','Moscovium',290,'unknown',15,7,'2,8,18,32,32,18,5'],
[116,'Lv','الليفرموريوم','Livermorium',293,'unknown',16,7,'2,8,18,32,32,18,6'],
[117,'Ts','التينيسين','Tennessine',294,'unknown',17,7,'2,8,18,32,32,18,7'],
[118,'Og','الأوغانيسون','Oganesson',294,'unknown',18,7,'2,8,18,32,32,18,8']
];

const CATEGORIES = {
  alkali:{ar:'فلزات قلوية',cls:'pt-category-alkali',color:'#f43f5e'},
  alkaline:{ar:'قلوية ترابية',cls:'pt-category-alkaline',color:'#fb923c'},
  transition:{ar:'فلزات انتقالية',cls:'pt-category-transition',color:'#facc15'},
  'post-transition':{ar:'فلزات بعد انتقالية',cls:'pt-category-post-transition',color:'#84cc16'},
  metalloid:{ar:'أشباه فلزات',cls:'pt-category-metalloid',color:'#14b8a6'},
  nonmetal:{ar:'لا فلزات',cls:'pt-category-nonmetal',color:'#3b82f6'},
  halogen:{ar:'هالوجينات',cls:'pt-category-halogen',color:'#06b6d4'},
  noble:{ar:'غازات نبيلة',cls:'pt-category-noble',color:'#a855f7'},
  lanthanide:{ar:'لانثانيدات',cls:'pt-category-lanthanide',color:'#ec4899'},
  actinide:{ar:'أكتينيدات',cls:'pt-category-actinide',color:'#d946ef'},
  unknown:{ar:'غير مصنفة',cls:'pt-category-unknown',color:'#64748b'}
};

function getElementByZ(z){ return PT.find(e => e[0] === z); }

function renderPeriodicTable(){
  const grid = document.getElementById('pt-grid');
  if(grid.dataset.rendered) return;
  grid.dataset.rendered = '1';
  grid.innerHTML = '';
  const legend = document.getElementById('pt-legend');
  if(legend && !legend.dataset.rendered){
    legend.dataset.rendered = '1';
    legend.innerHTML = Object.entries(CATEGORIES).map(([k,v]) => 
      `<div class="legend-item"><div class="legend-dot" style="background:${v.color}"></div>${v.ar}</div>`
    ).join('');
  }
  const positions = {};
  PT.forEach(el => {
    const [z, sym, ar, en, mass, cat, group, period, shells] = el;
    let row = period, col = group;
    if(cat === 'lanthanide'){ row = 9; col = group - 2; }
    if(cat === 'actinide'){ row = 10; col = group - 2; }
    positions[z] = {row, col};
  });
  for(let r=1; r<=10; r++){
    for(let c=1; c<=18; c++){
      const found = PT.find(el => {
        const p = positions[el[0]];
        return p.row === r && p.col === c;
      });
      const div = document.createElement('div');
      if(found){
        const [z, sym, ar, en, mass, cat] = found;
        div.className = 'pt-element ' + CATEGORIES[cat].cls;
        div.dataset.z = z;
        div.title = `${ar} (${sym})`;
        div.innerHTML = `<span class="num">${z}</span><span class="symbol">${sym}</span><span class="name">${ar}</span>`;
        div.onclick = () => openElement3D(z);
      } else {
        div.className = 'pt-spacer';
      }
      grid.appendChild(div);
    }
  }
  const searchInp = document.getElementById('pt-search');
  if(searchInp && !searchInp.dataset.bound){
    searchInp.dataset.bound = '1';
    searchInp.addEventListener('input', function(){
      const q = this.value.trim().toLowerCase();
      document.querySelectorAll('.pt-element').forEach(el => {
        const z = parseInt(el.dataset.z);
        const e = getElementByZ(z);
        const match = !q || e[1].toLowerCase().includes(q) || e[2].includes(q) || e[3].toLowerCase().includes(q) || String(z) === q;
        el.style.opacity = match ? '1' : '0.15';
        el.style.transform = match && q ? 'scale(1.05)' : '';
      });
    });
  }
}

/* ============================================================
   🎨 3D Bohr Model
   ============================================================ */
let currentThreeAnim = null;
let currentThreeScene = null;

function openElement3D(z){
  const el = getElementByZ(z);
  if(!el) return;
  const [num, sym, nameAr, nameEn, mass, cat, group, period, shellsStr] = el;
  const shells = shellsStr.split(',').map(Number);
  const catInfo = CATEGORIES[cat];
  document.getElementById('modal-3d-title').innerHTML = `<i class="fas fa-atom"></i> ${nameAr} (${sym})`;
  document.getElementById('atomic-info').innerHTML = `
    <div style="display:flex;gap:14px;align-items:center;margin-bottom:12px;">
      <div class="symbol-box" style="background:linear-gradient(135deg,${catInfo.color},${catInfo.color}aa);">${sym}</div>
      <div><h2 style="margin-bottom:4px;">${nameAr}</h2><div style="color:#94a3b8;font-size:.85em;">${nameEn}</div></div>
    </div>
    <span class="cat-badge" style="background:${catInfo.color}">${catInfo.ar}</span>
    <div class="info-grid">
      <div class="info-item"><div class="label">العدد الذري</div><div class="value">${num}</div></div>
      <div class="info-item"><div class="label">الكتلة الذرية</div><div class="value">${mass}</div></div>
      <div class="info-item"><div class="label">المجموعة</div><div class="value">${group}</div></div>
      <div class="info-item"><div class="label">الدورة</div><div class="value">${period}</div></div>
      <div class="info-item"><div class="label">البروتونات</div><div class="value">${num}</div></div>
      <div class="info-item"><div class="label">الإلكترونات</div><div class="value">${num}</div></div>
      <div class="info-item" style="grid-column:1/-1;"><div class="label">التوزيع الإلكتروني</div>
        <div class="shells-display">${shells.map((s,i) => `<span class="shell-badge">K${i+1}: ${s}</span>`).join('')}</div>
      </div>
    </div>
  `;
  document.getElementById('modal-3d-overlay').classList.add('active');
  setTimeout(() => createBohrModel(shells, num, catInfo.color), 100);
}

function closeElement3D(){
  document.getElementById('modal-3d-overlay').classList.remove('active');
  if(currentThreeAnim) cancelAnimationFrame(currentThreeAnim);
  if(currentThreeScene){
    currentThreeScene.traverse(obj => {
      if(obj.geometry) obj.geometry.dispose();
      if(obj.material){
        if(Array.isArray(obj.material)) obj.material.forEach(m => m.dispose());
        else obj.material.dispose();
      }
    });
    currentThreeScene = null;
  }
  const c = document.getElementById('atomic-3d-canvas');
  c.querySelectorAll('canvas').forEach(x => x.remove());
}

function createBohrModel(shells, protons, color){
  const container = document.getElementById('atomic-3d-canvas');
  container.querySelectorAll('canvas').forEach(x => x.remove());
  const W = container.clientWidth;
  const H = container.clientHeight;
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(50, W/H, 0.1, 1000);
  camera.position.set(0, 0, 22);
  const renderer = new THREE.WebGLRenderer({antialias:true, alpha:true});
  renderer.setSize(W, H);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  container.insertBefore(renderer.domElement, container.firstChild);
  currentThreeScene = scene;
  const ambient = new THREE.AmbientLight(0xffffff, 0.6);
  scene.add(ambient);
  const point = new THREE.PointLight(0xffffff, 1, 100);
  point.position.set(10, 10, 10);
  scene.add(point);
  const point2 = new THREE.PointLight(0x6366f1, 0.8, 100);
  point2.position.set(-10, -10, -10);
  scene.add(point2);
  const nucleusGroup = new THREE.Group();
  const protonColor = 0xef4444;
  const neutronColor = 0x64748b;
  const neutronCount = Math.round(protons * 1.2);
  const totalNucleons = Math.min(protons + neutronCount, 60);
  const clusterRadius = Math.min(0.3 + Math.log(protons + 1) * 0.25, 1.8);
  for(let i=0; i<totalNucleons; i++){
    const isProton = i < protons;
    const geo = new THREE.SphereGeometry(0.28, 16, 16);
    const mat = new THREE.MeshPhongMaterial({ color: isProton ? protonColor : neutronColor, shininess: 100, emissive: isProton ? 0x5a0000 : 0x1e293b, emissiveIntensity: 0.3 });
    const sphere = new THREE.Mesh(geo, mat);
    const phi = Math.acos(1 - 2 * (i + 0.5) / totalNucleons);
    const theta = Math.PI * (1 + Math.sqrt(5)) * i;
    sphere.position.set(clusterRadius * Math.sin(phi) * Math.cos(theta), clusterRadius * Math.sin(phi) * Math.sin(theta), clusterRadius * Math.cos(phi));
    nucleusGroup.add(sphere);
  }
  const glowGeo = new THREE.SphereGeometry(clusterRadius + 0.5, 24, 24);
  const glowMat = new THREE.MeshBasicMaterial({ color: 0xef4444, transparent: true, opacity: 0.12 });
  nucleusGroup.add(new THREE.Mesh(glowGeo, glowMat));
  scene.add(nucleusGroup);
  const electronColor = 0x22d3ee;
  const shellColor = 0x6366f1;
  const allElectrons = [];
  shells.forEach((count, idx) => {
    const radius = 2.5 + idx * 1.5;
    const ringGeo = new THREE.TorusGeometry(radius, 0.03, 8, 80);
    const ringMat = new THREE.MeshBasicMaterial({ color: shellColor, transparent: true, opacity: 0.35 });
    const ring = new THREE.Mesh(ringGeo, ringMat);
    ring.rotation.x = Math.PI / 2;
    ring.rotation.y = (idx * 0.4) - 0.4;
    ring.rotation.z = (idx * 0.3) - 0.4;
    scene.add(ring);
    for(let e=0; e<count; e++){
      const eGeo = new THREE.SphereGeometry(0.16, 12, 12);
      const eMat = new THREE.MeshPhongMaterial({ color: electronColor, emissive: electronColor, emissiveIntensity: 0.7, shininess: 100 });
      const electron = new THREE.Mesh(eGeo, eMat);
      scene.add(electron);
      allElectrons.push({ mesh: electron, radius, angle: (e / count) * Math.PI * 2, speed: 0.015 / (1 + idx * 0.3), tiltX: ring.rotation.x, tiltY: ring.rotation.y, tiltZ: ring.rotation.z });
    }
  });
  const starsGeo = new THREE.BufferGeometry();
  const starsCount = 200;
  const starsPos = new Float32Array(starsCount * 3);
  for(let i=0; i<starsCount*3; i++) starsPos[i] = (Math.random() - 0.5) * 80;
  starsGeo.setAttribute('position', new THREE.BufferAttribute(starsPos, 3));
  const starsMat = new THREE.PointsMaterial({color:0xffffff, size:0.08, transparent:true, opacity:0.5});
  scene.add(new THREE.Points(starsGeo, starsMat));
  let isDragging = false, prevX = 0, prevY = 0;
  let rotX = 0, rotY = 0, autoRotate = true;
  let zoom = 0;
  const canvas = renderer.domElement;
  canvas.style.cursor = 'grab';
  canvas.addEventListener('mousedown', e => { isDragging = true; prevX = e.clientX; prevY = e.clientY; autoRotate = false; canvas.style.cursor = 'grabbing'; });
  window.addEventListener('mouseup', () => { isDragging = false; canvas.style.cursor = 'grab'; setTimeout(() => { autoRotate = true; }, 2000); });
  canvas.addEventListener('mousemove', e => {
    if(!isDragging) return;
    const dx = e.clientX - prevX;
    const dy = e.clientY - prevY;
    rotY += dx * 0.01;
    rotX += dy * 0.01;
    prevX = e.clientX; prevY = e.clientY;
  });
  canvas.addEventListener('wheel', e => { e.preventDefault(); zoom += e.deltaY * 0.01; zoom = Math.max(-8, Math.min(8, zoom)); }, {passive:false});
  canvas.addEventListener('touchstart', e => { if(e.touches.length === 1){ isDragging = true; prevX = e.touches[0].clientX; prevY = e.touches[0].clientY; autoRotate = false; } });
  canvas.addEventListener('touchmove', e => {
    if(isDragging && e.touches.length === 1){
      const dx = e.touches[0].clientX - prevX;
      const dy = e.touches[0].clientY - prevY;
      rotY += dx * 0.015;
      rotX += dy * 0.015;
      prevX = e.touches[0].clientX;
      prevY = e.touches[0].clientY;
    }
  });
  canvas.addEventListener('touchend', () => { isDragging = false; setTimeout(() => autoRotate = true, 2000); });
  function animate(){
    currentThreeAnim = requestAnimationFrame(animate);
    if(autoRotate) rotY += 0.003;
    rotX = Math.max(-Math.PI/2, Math.min(Math.PI/2, rotX));
    scene.rotation.y = rotY;
    scene.rotation.x = rotX;
    allElectrons.forEach(e => {
      e.angle += e.speed;
      const x = Math.cos(e.angle) * e.radius;
      const z = Math.sin(e.angle) * e.radius;
      e.mesh.position.set(x, 0, z);
      e.mesh.applyMatrix4(new THREE.Matrix4().makeRotationX(e.tiltX));
      e.mesh.applyMatrix4(new THREE.Matrix4().makeRotationY(e.tiltY));
      e.mesh.applyMatrix4(new THREE.Matrix4().makeRotationZ(e.tiltZ));
    });
    nucleusGroup.rotation.y += 0.008;
    nucleusGroup.rotation.x += 0.005;
    camera.position.z = 22 + zoom;
    renderer.render(scene, camera);
  }
  animate();
  const ro = new ResizeObserver(() => {
    if(!currentThreeScene) return;
    const nW = container.clientWidth;
    const nH = container.clientHeight;
    camera.aspect = nW/nH;
    camera.updateProjectionMatrix();
    renderer.setSize(nW, nH);
  });
  ro.observe(container);
}

/* ============================================================
   🔐 Auth
   ============================================================ */
const USERS_KEY='chemUsers', SESSION_KEY='chemSession', SETTINGS_KEY='chemSettings', RESULTS_KEY='chemResults', NOTIF_KEY='chemNotifs';
function getUsers(){ try{ return JSON.parse(localStorage.getItem(USERS_KEY)||'[]'); }catch{ return []; } }
function saveUsers(a){ localStorage.setItem(USERS_KEY, JSON.stringify(a)); }
function currentUser(){
  const id = localStorage.getItem(SESSION_KEY);
  if(!id) return null;
  return getUsers().find(u => u.id === id) || null;
}
function hashPass(p){
  let h = 0;
  for(let i=0;i<p.length;i++){ h = ((h<<5)-h) + p.charCodeAt(i); h |= 0; }
  return 'h' + Math.abs(h).toString(36) + p.length;
}

document.querySelectorAll('.auth-tab').forEach(tab => {
  tab.addEventListener('click', function(){
    document.querySelectorAll('.auth-tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.auth-panel').forEach(p => p.classList.remove('active'));
    this.classList.add('active');
    document.getElementById('panel-' + this.dataset.tab).classList.add('active');
    hideAuthMessages();
  });
});
function showAuthError(m){ const e=document.getElementById('auth-error'); e.textContent=m; e.classList.add('show'); document.getElementById('auth-success').classList.remove('show'); setTimeout(()=>e.classList.remove('show'),4000); }
function showAuthSuccess(m){ const e=document.getElementById('auth-success'); e.textContent=m; e.classList.add('show'); document.getElementById('auth-error').classList.remove('show'); setTimeout(()=>e.classList.remove('show'),4000); }
function hideAuthMessages(){ document.getElementById('auth-error').classList.remove('show'); document.getElementById('auth-success').classList.remove('show'); }
function togglePass(id, btn){
  const inp = document.getElementById(id);
  if(inp.type === 'password'){ inp.type = 'text'; btn.innerHTML = '<i class="fas fa-eye-slash"></i>'; }
  else { inp.type = 'password'; btn.innerHTML = '<i class="fas fa-eye"></i>'; }
}
function showForgot(e){ e.preventDefault(); showToast('يرجى التواصل مع المشرف','info'); }

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

document.getElementById('signup-form').addEventListener('submit', function(e){
  e.preventDefault();
  const name = document.getElementById('signup-name').value.trim();
  const username = document.getElementById('signup-username').value.trim();
  const email = document.getElementById('signup-email').value.trim();
  const section = document.getElementById('signup-section').value;
  const pass = document.getElementById('signup-password').value;
  const pass2 = document.getElementById('signup-password2').value;
  if(!name || !username || !email || !section || !pass){ showAuthError('يرجى ملء جميع الحقول'); return; }
  if(!/^[a-zA-Z0-9_]{3,20}$/.test(username)){ showAuthError('اسم المستخدم: 3-20 حرف (إنجليزي/أرقام/_)'); return; }
  if(!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)){ showAuthError('البريد الإلكتروني غير صحيح'); return; }
  if(pass.length < 6){ showAuthError('كلمة المرور: 6 أحرف على الأقل'); return; }
  if(pass !== pass2){ showAuthError('كلمتا المرور غير متطابقتين'); return; }
  const users = getUsers();
  if(users.find(u => u.username.toLowerCase() === username.toLowerCase())){ showAuthError('❌ اسم المستخدم مستخدم مسبقاً'); return; }
  if(users.find(u => u.email.toLowerCase() === email.toLowerCase())){ showAuthError('❌ البريد مستخدم مسبقاً'); return; }
  const newUser = {
    id: 'u_' + Date.now() + '_' + Math.random().toString(36).slice(2,7),
    name, username, email, section,
    password: hashPass(pass),
    avatar: 'fa-user',
    createdAt: Date.now()
  };
  users.push(newUser);
  saveUsers(users);
  localStorage.setItem(SESSION_KEY, newUser.id);
  loadSettings();
  showToast('تم إنشاء حسابك بنجاح! 🎉','success');
  addNotification('مرحباً بك!', `أهلاً ${name}`, 'success');
  navigateTo('welcome', true);
});

document.getElementById('logout-btn').addEventListener('click', function(){
  confirmModal('تسجيل الخروج','هل أنت متأكد من تسجيل الخروج؟', () => {
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
   🎨 Theme & Settings
   ============================================================ */
const THEMES = [
  {id:'indigo',primary:'#6366f1',secondary:'#8b5cf6',name:'بنفسجي'},
  {id:'blue',primary:'#3b82f6',secondary:'#06b6d4',name:'أزرق'},
  {id:'green',primary:'#10b981',secondary:'#06d6a0',name:'أخضر'},
  {id:'pink',primary:'#ec4899',secondary:'#8b5cf6',name:'وردي'},
  {id:'orange',primary:'#f59e0b',secondary:'#ef4444',name:'برتقالي'},
  {id:'cyan',primary:'#06b6d4',secondary:'#3b82f6',name:'سماوي'}
];

let soundEnabled=true, autoSave=true, instantExplain=true, defaultTimer=1800;

function applySettings(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  const theme = THEMES.find(t => t.id === s.theme) || THEMES[0];
  document.documentElement.style.setProperty('--primary', theme.primary);
  document.documentElement.style.setProperty('--secondary', theme.secondary);
  document.documentElement.style.setProperty('--primary-dark', theme.primary);
  if(s.darkMode === false) document.body.classList.add('light'); else document.body.classList.remove('light');
  if(s.compactMode) document.body.classList.add('compact'); else document.body.classList.remove('compact');
  if(s.animations === false) document.body.classList.add('no-anim'); else document.body.classList.remove('no-anim');
  document.documentElement.style.fontSize = (s.fontSize || 16) + 'px';
  soundEnabled = s.sound !== false;
  defaultTimer = (s.timerDuration || 30) * 60;
  autoSave = s.autoSave !== false;
  instantExplain = s.instantExplain !== false;
}
function loadSettings(){ applySettings(); }

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
  document.getElementById('default-question-count').value = s.defaultQuestionCount || 20;
  document.getElementById('default-difficulty').value = s.defaultDifficulty || 'medium';
  document.getElementById('instant-explain').checked = s.instantExplain !== false;
  document.getElementById('timer-duration').value = s.timerDuration || 30;
  document.getElementById('ai-provider').value = s.aiProvider || 'internal';
  document.getElementById('ai-groq-key').value = s.groqKey || '';
  document.getElementById('ai-gemini-key').value = s.geminiKey || '';
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
      showToast('تم تغيير السمة','success');
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
      showToast('تم تحديث الصورة الرمزية','success');
    });
  });
}

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

document.querySelectorAll('.settings-nav-btn').forEach(btn => {
  btn.addEventListener('click', function(){
    document.querySelectorAll('.settings-nav-btn').forEach(b => b.classList.remove('active'));
    document.querySelectorAll('.settings-panel').forEach(p => p.classList.remove('active'));
    this.classList.add('active');
    document.getElementById('panel-' + this.dataset.panel).classList.add('active');
  });
});

['dark-mode','compact-mode','animations-enabled'].forEach(id => {
  const el = document.getElementById(id);
  if(el) el.addEventListener('change', function(){
    const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
    if(id === 'dark-mode') s.darkMode = this.checked;
    if(id === 'compact-mode') s.compactMode = this.checked;
    if(id === 'animations-enabled') s.animations = this.checked;
    localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
    applySettings();
  });
});

document.getElementById('save-settings').addEventListener('click', function(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  const user = currentUser();
  const newName = document.getElementById('username').value.trim();
  const newEmail = document.getElementById('user-email').value.trim();
  if(user && newName && newEmail){
    user.name = newName; user.email = newEmail; user.level = document.getElementById('user-level-select').value;
    const users = getUsers();
    const idx = users.findIndex(u => u.id === user.id);
    users[idx] = user;
    saveUsers(users);
    applyUserToUI(user);
  }
  s.userLevel = document.getElementById('user-level-select').value;
  s.notifications = document.getElementById('notifications-enabled').checked;
  s.quizNotif = document.getElementById('quiz-notif').checked;
  s.achieveNotif = document.getElementById('achieve-notif').checked;
  s.defaultQuestionCount = parseInt(document.getElementById('default-question-count').value);
  s.defaultDifficulty = document.getElementById('default-difficulty').value;
  s.instantExplain = document.getElementById('instant-explain').checked;
  s.timerDuration = parseInt(document.getElementById('timer-duration').value);
  s.aiProvider = document.getElementById('ai-provider').value;
  s.groqKey = document.getElementById('ai-groq-key').value;
  s.geminiKey = document.getElementById('ai-gemini-key').value;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  applySettings();
  showToast('✅ تم حفظ الإعدادات','success');
});

function saveAIKeys(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  s.aiProvider = document.getElementById('ai-provider').value;
  s.groqKey = document.getElementById('ai-groq-key').value;
  s.geminiKey = document.getElementById('ai-gemini-key').value;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  updateAIStatus();
  showToast('✅ تم حفظ مفاتيح API','success');
}

function openChangePassword(){
  const oldPass = prompt('أدخل كلمة المرور الحالية:');
  if(oldPass === null) return;
  const user = currentUser();
  if(!user) return;
  if(user.password !== hashPass(oldPass)){ showToast('كلمة المرور الحالية غير صحيحة','error'); return; }
  const newPass = prompt('أدخل كلمة المرور الجديدة (6 أحرف على الأقل):');
  if(!newPass || newPass.length < 6){ showToast('كلمة المرور قصيرة جداً','error'); return; }
  const confirm = prompt('أعد كتابة كلمة المرور الجديدة:');
  if(newPass !== confirm){ showToast('كلمتا المرور غير متطابقتين','error'); return; }
  user.password = hashPass(newPass);
  const users = getUsers();
  const idx = users.findIndex(u => u.id === user.id);
  users[idx] = user;
  saveUsers(users);
  showToast('✅ تم تغيير كلمة المرور','success');
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

document.getElementById('export-data').addEventListener('click', function(){
  const data = {
    user: currentUser(),
    settings: JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}'),
    results: JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]'),
    notifications: JSON.parse(localStorage.getItem(NOTIF_KEY) || '[]'),
    exportedAt: new Date().toISOString()
  };
  const blob = new Blob([JSON.stringify(data, null, 2)], {type:'application/json'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'chemistry-backup-' + new Date().toISOString().slice(0,10) + '.json';
  a.click();
  showToast('تم تصدير البيانات','success');
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
      showToast('✅ تم استيراد البيانات','success');
    }catch(err){ showToast('❌ ملف غير صالح','error'); }
  };
  reader.readAsText(file);
});

function clearAllData(){
  confirmModal('حذف شامل','سيتم حذف جميع البيانات نهائياً!', () => {
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
  notifications.unshift({id:Date.now(),title,msg,type,time:new Date().toLocaleTimeString('ar-SA'),date:new Date().toLocaleDateString('ar-SA'),read:false});
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
      saveNotifications(); renderNotifications();
    });
  });
}
function iconFor(t){ return t === 'success' ? 'check-circle' : t === 'warning' ? 'exclamation-triangle' : t === 'error' ? 'times-circle' : 'info-circle'; }
document.getElementById('clear-notifications').addEventListener('click', function(){
  confirmModal('مسح الإشعارات','هل تريد مسح جميع الإشعارات؟', () => {
    notifications = []; saveNotifications(); renderNotifications();
    showToast('تم مسح الإشعارات','success');
  });
});

/* ============================================================
   📊 Dashboard
   ============================================================ */
function updateDashboard(){
  const arr = JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]');
  if(arr.length){
    const total = arr.reduce((s,r) => s + r.score, 0);
    const avg = Math.round(total / arr.length);
    document.getElementById('qs-quizzes').textContent = arr.length;
    document.getElementById('qs-avg').textContent = avg + '%';
  } else {
    document.getElementById('qs-quizzes').textContent = '0';
    document.getElementById('qs-avg').textContent = '0%';
  }
  const xp = arr.reduce((sum, r) => sum + (r.score * 10), 0);
  document.getElementById('qs-xp').textContent = xp;
  const days = new Set(arr.map(r => r.date));
  document.getElementById('qs-streak').textContent = Math.min(days.size, 30);
  const hour = new Date().getHours();
  let greet = 'صباح الخير';
  if(hour >= 12 && hour < 17) greet = 'مساء الخير';
  else if(hour >= 17) greet = 'مساء الخير';
  document.getElementById('welcome-subtitle').textContent = greet + '، لنبدأ رحلة الكيمياء التفاعلية معاً';
}

/* ============================================================
   ❓ Questions
   ============================================================ */
const allQuestions = [
{q:"العملية التي يعاد فيها ترتيب الذرات لتكوين مواد جديدة تسمى:",o:["التغير الفيزيائي","التفاعل النووي","التفاعل الكيميائي"],c:2,e:"تعريف التفاعل الكيميائي",l:"التفاعلات",d:"easy"},
{q:"المعادلة التي توضح الأيونات المشاركة في التفاعل:",o:["المعادلة الكيميائية","المعادلة الأيونية","المعادلة الموزونة"],c:1,e:"المعادلة الأيونية",l:"التفاعلات",d:"medium"},
{q:"أقصى عدد إلكترونات في المستوى الرابع:",o:["18","8","32"],c:2,e:"2n² = 32",l:"الترتيب الإلكتروني",d:"medium"},
{q:"الصيغة الأيونية الصحيحة لكلوريد الصوديوم:",o:["Na²⁺ Cl⁻","Na⁺ Cl⁻","Na²⁺ Cl²⁻"],c:1,e:"Na⁺ Cl⁻",l:"التفاعلات",d:"easy"},
{q:"الأيون OH⁻ يسمى:",o:["الكربوكسيل","الهيدروكسيد","الكربونات"],c:1,e:"هيدروكسيد",l:"التفاعلات",d:"easy"},
{q:"التفاعل الذي تكون نواتجه مادة واحدة:",o:["احتراق","إحلال بسيط","تكوين"],c:2,e:"تكوين",l:"التفاعلات",d:"easy"},
{q:"التوزيع المنتهي بـ 3p⁶ يعني العدد الذري:",o:["10","12","18"],c:2,e:"18",l:"الترتيب الإلكتروني",d:"medium"},
{q:"الرمز بين المتفاعلات والنواتج:",o:["+","→","(s)"],c:1,e:"→",l:"التفاعلات",d:"easy"},
{q:"تفاعل مع الأكسجين ينتج ضوء وحرارة:",o:["تكوين","احتراق","إحلال مزدوج"],c:1,e:"احتراق",l:"التفاعلات",d:"easy"},
{q:"الصيغة الصحيحة لهيدروكسيد الألومنيوم:",o:["AlOH","Al(OH)₂","Al(OH)₃"],c:2,e:"Al(OH)₃",l:"التفاعلات",d:"medium"},
{q:"المستويات الثانوية في n=3:",o:["2","3","4"],c:1,e:"s,p,d",l:"الترتيب الإلكتروني",d:"medium"},
{q:"التوزيع الصحيح للكلور (17):",o:["1s² 2s² 2p⁶ 3s² 3p⁵","1s² 2s² 2p⁶ 3s² 3p⁶","1s² 2s² 2p⁶ 3s¹ 3p⁶"],c:0,e:"3s² 3p⁵",l:"الترتيب الإلكتروني",d:"hard"},
{q:"المستويات الثانوية في n=5:",o:["s,p","s,p,d","s,p,d,f"],c:2,e:"s,p,d,f",l:"الترتيب الإلكتروني",d:"hard"},
{q:"ليس من أدلة حدوث التفاعل الكيميائي:",o:["تصاعد غاز","تغير اللون","تغير الشكل"],c:2,e:"تغير الشكل",l:"التفاعلات",d:"easy"},
{q:"معامل H₂ في N₂ + 3H₂ → 2NH₃:",o:["1","2","3"],c:2,e:"3",l:"التفاعلات",d:"medium"},
{q:"عدد أكسدة الأكسجين عادة:",o:["+1","-1","-2"],c:2,e:"-2",l:"التفاعلات",d:"medium"},
{q:"توزيع البورون (5):",o:["1s² 2s³","1s² 2s² 2p¹","1s² 2p³"],c:1,e:"2p¹",l:"الترتيب الإلكتروني",d:"medium"},
{q:"ناتج Mg²⁺ + NO₃⁻:",o:["MgNO₃","Mg(NO₃)₂","Mg₂NO₃"],c:1,e:"Mg(NO₃)₂",l:"التفاعلات",d:"medium"},
{q:"صيغة أكسيد الحديد الثنائي:",o:["FeO₂","Fe₂O","FeO"],c:2,e:"FeO",l:"التفاعلات",d:"medium"},
{q:"المعادلة الأيونية الكاملة AgNO₃ + KI:",o:["Ag⁺+NO₃⁻+K⁺+I⁻ → AgI+K⁺+NO₃⁻","Ag⁺+I⁻ → AgI","AgNO₃+KI → AgI+KNO₃"],c:0,e:"كاملة",l:"التفاعلات",d:"hard"},
{q:"المعادلة الأيونية النهائية:",o:["Ag⁺+NO₃⁻+K⁺+I⁻ → AgI+K⁺+NO₃⁻","Ag⁺+I⁻ → AgI","K⁺+NO₃⁻ → KNO₃"],c:1,e:"Ag⁺+I⁻",l:"التفاعلات",d:"hard"},
{q:"قانون حفظ الكتلة:",o:["كتلة المتفاعلة = كتلة الناتجة","فقدان كتلة","زيادة كتلة"],c:0,e:"حفظ الكتلة",l:"التفاعلات",d:"easy"},
{q:"الغرض من المعاملات:",o:["تساوي عدد الذرات","سهولة القراءة","السرعة"],c:0,e:"حفظ الكتلة",l:"التفاعلات",d:"easy"},
{q:"احتراق CH₄ مع O₂:",o:["تكوين","إحلال بسيط","احتراق"],c:2,e:"احتراق",l:"التفاعلات",d:"medium"},
{q:"لماذا يحل Mg محل H في HCl?",o:["أكثر لمعاناً","أكثر نشاطاً","أرخص"],c:1,e:"سلسلة النشاط",l:"التفاعلات",d:"medium"},
{q:"إحلال مزدوج:",o:["Na+Cl₂","Zn+2HCl","AgNO₃+NaCl"],c:2,e:"تبادل",l:"التفاعلات",d:"medium"},
{q:"الخطأ في H₂+O₂=H₂O:",o:["الصيغ","غير موزونة","السهم"],c:1,e:"غير موزونة",l:"التفاعلات",d:"easy"},
{q:"(s) تعني:",o:["محلول","غاز","صلب"],c:2,e:"Solid",l:"التفاعلات",d:"easy"},
{q:"تغير كيميائي:",o:["ذوبان السكر","صدأ الحديد","تجمّد الماء"],c:1,e:"صدأ",l:"التفاعلات",d:"easy"},
{q:"النسبة H₂:O₂:",o:["1:1","2:1","1:2"],c:1,e:"2:1",l:"التفاعلات",d:"medium"},
{q:"صيغة كبريتات الألومنيوم:",o:["AlSO₄","Al₂(SO₄)₃","Al₃(SO₄)₂"],c:1,e:"Al₂(SO₄)₃",l:"التفاعلات",d:"hard"},
{q:"4غ H + 32غ O = ؟ ماء:",o:["28غ","32غ","36غ"],c:2,e:"36غ",l:"التفاعلات",d:"medium"},
{q:"المعادلة الصحيحة لاحتراق Mg:",o:["Mg+O→MgO","Mg+O₂→2MgO","2Mg+O₂→2MgO"],c:2,e:"2Mg+O₂→2MgO",l:"التفاعلات",d:"medium"},
{q:"هل تغير حالة المادة تفاعل كيميائي؟",o:["صحيحة","خاطئة","غير دقيقة"],c:2,e:"غير دقيقة",l:"التفاعلات",d:"easy"},
{q:"(g) تعني:",o:["صلب","سائل","غاز"],c:2,e:"Gas",l:"التفاعلات",d:"easy"},
{q:"لماذا لا يحل Cu محل Fe?",o:["أقل لمعاناً","أقل نشاطاً","أرخص"],c:1,e:"أقل نشاطاً",l:"التفاعلات",d:"medium"},
{q:"عدد ذرات O في Al₂(SO₄)₃:",o:["4","7","12"],c:2,e:"12",l:"التفاعلات",d:"hard"},
{q:"إحلال مزدوج:",o:["CaCO₃→CaO+CO₂","Zn+2HCl","AgNO₃+NaCl"],c:2,e:"تبادل",l:"التفاعلات",d:"medium"},
{q:"المعادلة الموزونة HCl+NaOH:",o:["HCl+NaOH→NaCl+H₂O","2HCl+NaOH","HCl+2NaOH"],c:0,e:"موزونة",l:"التفاعلات",d:"medium"},
{q:"سبب السهم:",o:["اتجاه التفاعل","السرعة","التمييز"],c:0,e:"اتجاه",l:"التفاعلات",d:"easy"},
{q:"عدد e المفقودة من Mg (12):",o:["واحد","اثنان","ثلاثة"],c:1,e:"اثنان",l:"الترتيب الإلكتروني",d:"medium"},
{q:"أيون بتوزيع 1s²2s²2p⁶:",o:["Na⁺","F⁻","O²⁻","جميعها"],c:3,e:"جميعها",l:"الترتيب الإلكتروني",d:"hard"},
{q:"المعادلة اللفظية AgNO₃+NaCl:",o:["كلوريد الفضة راسب + نترات الصوديوم","نترات الصوديوم + كلوريد الفضة","كلاهما"],c:2,e:"كلاهما صحيح",l:"التفاعلات",d:"easy"},
{q:"دليل على تفاعل كيميائي:",o:["ذوبان السكر","تغير لون عباد الشمس","تكوين راسب"],c:2,e:"راسب",l:"التفاعلات",d:"medium"},
{q:"الغرض من وزن المعادلات:",o:["القراءة","حفظ الكتلة","السرعة"],c:1,e:"حفظ الكتلة",l:"التفاعلات",d:"easy"},
{q:"معامل N في 4NH₃+5O₂→4NO+6H₂O:",o:["4","5","1"],c:2,e:"معامل ضمني",l:"التفاعلات",d:"hard"},
{q:"لماذا نكتب الحالة؟",o:["اللون","للمعادلة الأيونية","الصيغة"],c:1,e:"للمعادلة الأيونية",l:"التفاعلات",d:"medium"},
{q:"Ca + H₂O:",o:["Ca+H₂O→Ca(OH)₂+H₂","Ca+2H₂O→Ca(OH)₂+H₂","2Ca+2H₂O"],c:1,e:"موزونة",l:"التفاعلات",d:"medium"},
{q:"H₂+Cl₂→2HCl موزونة؟",o:["نعم","لا","خطأ"],c:0,e:"نعم",l:"التفاعلات",d:"easy"},
{q:"Na+Cl₂ الموزونة:",o:["Na+Cl→NaCl","2Na+Cl₂→2NaCl","2Na+2Cl→2NaCl"],c:1,e:"2Na+Cl₂",l:"التفاعلات",d:"medium"}
];

/* ============================================================
   🎮 Quiz
   ============================================================ */
let currentQuestions=[], currentQuestion=0, score=0, userAnswers=[], weakLessons={};
let quizMode='practice', quizTime=1800, timerInterval;
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
  let pool = allQuestions;
  if(diff !== 'all') pool = allQuestions.filter(q => q.d === diff);
  if(pool.length === 0) pool = allQuestions;
  currentQuestions = shuffle([...pool]).slice(0, Math.min(count, pool.length));
  currentQuestion = 0; score = 0; userAnswers = []; weakLessons = {};
  if(quizMode === 'exam'){ quizTime = defaultTimer; startTimer(); }
  navigateTo('quiz');
  showQuestion();
});

function shuffle(a){ const arr=[...a]; for(let i=arr.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [arr[i],arr[j]]=[arr[j],arr[i]]; } return arr; }

function showQuestion(){
  const q = currentQuestions[currentQuestion];
  document.getElementById('question').textContent = q.q;
  document.getElementById('question-counter').textContent = `السؤال ${currentQuestion+1} من ${currentQuestions.length}`;
  document.getElementById('progress').style.width = ((currentQuestion+1)/currentQuestions.length*100) + '%';
  const box = document.getElementById('options');
  box.innerHTML = '';
  q.o.forEach((op, i) => {
    const el = document.createElement('div');
    el.className = 'option';
    el.innerHTML = `${op}<span class="option-number">${String.fromCharCode(65+i)}</span>`;
    el.onclick = () => pickOption(i);
    box.appendChild(el);
  });
  document.getElementById('explanation').style.display = 'none';
  document.getElementById('next-btn').disabled = true;
  document.getElementById('next-btn').innerHTML = currentQuestion === currentQuestions.length-1 ? '<i class="fas fa-flag-checkered"></i> إنهاء' : '<i class="fas fa-arrow-left"></i> التالي';
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
    weakLessons[q.l] = (weakLessons[q.l]||0) + 1;
  }
  userAnswers.push({selected:idx, correct:q.c, isCorrect:correct, q:q.q, explanation:q.e, lesson:q.l});
  if(quizMode === 'practice' && instantExplain){
    const ex = document.getElementById('explanation');
    ex.innerHTML = `
      <h3><i class="fas ${correct ? 'fa-check-circle' : 'fa-times-circle'}"></i> ${correct ? 'صحيح!' : 'خطأ!'}</h3>
      <p><strong>التعليل:</strong> ${q.e}</p>
      ${!correct ? `<div style="background:rgba(245,158,11,.15);padding:14px;border-radius:10px;margin-top:14px;border-right:4px solid #f59e0b;color:#fef3c7;"><strong>الإجابة:</strong> ${q.o[q.c]}</div>` : ''}
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
  confirmModal('إنهاء الاختبار','هل أنت متأكد؟', endQuiz);
});

function startTimer(){
  updateTimer();
  clearInterval(timerInterval);
  timerInterval = setInterval(() => { quizTime--; updateTimer(); if(quizTime<=0) endQuiz(); }, 1000);
}
function updateTimer(){
  const m = Math.floor(quizTime/60).toString().padStart(2,'0');
  const s = (quizTime%60).toString().padStart(2,'0');
  document.getElementById('quiz-timer').innerHTML = `<i class="fas fa-hourglass-half"></i> ${m}:${s}`;
}

function endQuiz(){
  clearInterval(timerInterval);
  if(soundEnabled) completeSound.play().catch(()=>{});
  navigateTo('results');
  showResults();
  if(autoSave) saveResults();
  const p = Math.round(score/currentQuestions.length*100);
  addNotification('انتهى الاختبار!', `حصلت على ${p}%`, 'success');
}

function showResults(){
  const total = currentQuestions.length;
  const p = Math.round(score/total*100);
  document.getElementById('final-score').textContent = p + '%';
  document.getElementById('result-score').textContent = score;
  document.getElementById('total-questions').textContent = total;
  const rank = getRank(p);
  document.getElementById('user-rank').textContent = rank.text;
  document.getElementById('user-rank').className = 'rank-badge ' + rank.cls;
  document.getElementById('time-taken').textContent = quizMode === 'exam' ? document.getElementById('quiz-timer').textContent.replace(/[^\d:]/g,'').trim() : '--:--';
  const canvas = document.getElementById('result-chart');
  const ctx = canvas.getContext('2d');
  ctx.clearRect(0,0,230,230);
  ctx.beginPath(); ctx.arc(115,115,92,0,2*Math.PI);
  ctx.strokeStyle = 'rgba(255,255,255,.1)'; ctx.lineWidth = 16; ctx.stroke();
  const color = p >= 70 ? '#10b981' : p >= 50 ? '#f59e0b' : '#ef4444';
  ctx.beginPath(); ctx.arc(115,115,92,-0.5*Math.PI,(2*p/100-0.5)*Math.PI);
  ctx.strokeStyle = color; ctx.lineWidth = 16; ctx.lineCap = 'round'; ctx.stroke();
  ctx.font = 'bold 30px Arial'; ctx.fillStyle = color; ctx.textAlign='center'; ctx.textBaseline='middle';
  ctx.fillText(p + '%', 115, 115);
  const randPct = Math.floor(Math.random()*30) + 60;
  document.getElementById('comparison').innerHTML = `<h3><i class="fas fa-chart-line"></i> مقارنة</h3><p>${randPct}% من الطلاب حصلوا على نفس نتيجتك</p>`;
  const wl = document.getElementById('weak-lessons');
  const keys = Object.keys(weakLessons);
  if(keys.length){
    wl.innerHTML = '<h3><i class="fas fa-book"></i> الدروس التي تحتاج مراجعة:</h3>' + keys.map(k => `<div class="lesson-item"><strong>${k}</strong><p>راجع هذا الدرس</p></div>`).join('');
  } else {
    wl.innerHTML = '<h3><i class="fas fa-star"></i> ممتاز! لا توجد دروس للمراجعة</h3>';
  }
  createPerfChart(p);
  showTips(p);
  if(quizMode === 'exam') showExamDetails();
}

function getRank(p){
  if(p < 35) return {text:'غاز خامل', cls:'rank-inert'};
  if(p <= 50) return {text:'متفاعل نشيط', cls:'rank-active'};
  if(p <= 89) return {text:'كيميائي جيد', cls:'rank-good'};
  return {text:'العالم العبقري', cls:'rank-genius'};
}

let perfChartInst;
function createPerfChart(cp){
  if(perfChartInst) perfChartInst.destroy();
  const ctx = document.getElementById('performance-chart').getContext('2d');
  perfChartInst = new Chart(ctx, {
    type:'line',
    data:{labels:['1','2','3','4','5','الآن'], datasets:[{label:'النتائج %', data:[55,62,58,70,65,cp], borderColor:'#22d3ee', backgroundColor:'rgba(34,211,238,.15)', borderWidth:3, fill:true, tension:.4, pointBackgroundColor:'#6366f1', pointBorderColor:'#fff', pointBorderWidth:2, pointRadius:6}]},
    options:{responsive:true, maintainAspectRatio:false, plugins:{legend:{labels:{color:'#e2e8f0'}}}, scales:{y:{beginAtZero:true,max:100,ticks:{color:'#94a3b8',callback:v=>v+'%'},grid:{color:'rgba(255,255,255,.05)'}},x:{ticks:{color:'#94a3b8'},grid:{color:'rgba(255,255,255,.05)'}}}}
  });
}

function showTips(p){
  const el = document.getElementById('improvement-tips');
  if(p < 50) el.innerHTML = '<li>ركز على الأساسيات قبل المتقدم</li><li>استخدم وضع الممارسة</li><li>جرب المحاكاة</li>';
  else if(p < 80) el.innerHTML = '<li>ركز على نقاط ضعفك</li><li>جرب وضع الامتحان</li><li>راجع قسم الكيمياء والانسان</li>';
  else el.innerHTML = '<li>تهانينا! ممتاز</li><li>جرب المستوى المتقدم</li><li>ساعد زملائك</li>';
}

function showExamDetails(){
  const wl = document.getElementById('weak-lessons');
  let html = '<h3 style="margin-top:20px;"><i class="fas fa-clipboard-list"></i> تفاصيل الأخطاء:</h3>';
  userAnswers.forEach((a,i) => {
    if(!a.isCorrect) html += `<div class="lesson-item"><strong>س${i+1}:</strong> ${a.q}<p><strong>الصحيحة:</strong> ${currentQuestions[i].o[a.correct]}</p></div>`;
  });
  wl.innerHTML += html;
}

function saveResults(){
  const p = Math.round(score/currentQuestions.length*100);
  const arr = JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]');
  arr.unshift({date:new Date().toLocaleDateString('ar-SA'), time:new Date().toLocaleTimeString('ar-SA'), score:p, total:currentQuestions.length, correct:score, mode:quizMode, weak:weakLessons});
  if(arr.length > 100) arr.splice(100);
  localStorage.setItem(RESULTS_KEY, JSON.stringify(arr));
}

function updateProfileUI(){
  const user = currentUser();
  if(!user) return;
  const arr = JSON.parse(localStorage.getItem(RESULTS_KEY) || '[]');
  if(arr.length){
    const total = arr.reduce((s,r) => s + r.score, 0);
    const avg = Math.round(total/arr.length);
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
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  s.bio = document.getElementById('profile-bio').value;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  showToast('✅ تم الحفظ','success');
});

function setupResearchLinks(){
  document.querySelectorAll('.research-toc .toc-link[href^="#"]').forEach(link => {
    link.onclick = function(e){
      e.preventDefault();
      const id = this.getAttribute('href').substring(1);
      const el = document.getElementById(id);
      const mc = document.querySelector('.main-content');
      if(el && mc) mc.scrollTo({top: el.offsetTop - 20, behavior:'smooth'});
    };
  });
  const bt = document.getElementById('back-to-top');
  if(bt) bt.onclick = () => document.querySelector('.main-content').scrollTo({top:0, behavior:'smooth'});
}

document.querySelector('.main-content').addEventListener('scroll', function(){
  const bt = document.getElementById('back-to-top');
  if(!bt) return;
  if(this.scrollTop > 400) bt.classList.add('visible');
  else bt.classList.remove('visible');
});

document.getElementById('clear-whiteboard').addEventListener('click', function(){
  showToast('استخدم أدوات Canva داخل السبورة','info');
});

/* ============================================================
   🧮 Calculator
   ============================================================ */
const ATOMIC_WEIGHTS = {H:1.008,He:4.003,Li:6.94,Be:9.012,B:10.81,C:12.011,N:14.007,O:15.999,F:18.998,Ne:20.180,Na:22.990,Mg:24.305,Al:26.982,Si:28.086,P:30.974,S:32.06,Cl:35.45,Ar:39.948,K:39.098,Ca:40.078,Sc:44.956,Ti:47.867,V:50.942,Cr:51.996,Mn:54.938,Fe:55.845,Co:58.933,Ni:58.693,Cu:63.546,Zn:65.38,Ga:69.723,Ge:72.630,As:74.922,Se:78.971,Br:79.904,Kr:83.798,Rb:85.468,Sr:87.62,Y:88.906,Zr:91.224,Nb:92.906,Mo:95.95,Tc:98,Ru:101.07,Rh:102.906,Pd:106.42,Ag:107.868,Cd:112.414,In:114.818,Sn:118.710,Sb:121.760,Te:127.60,I:126.904,Xe:131.293,Cs:132.905,Ba:137.327,La:138.905,Ce:140.116,Pr:140.908,Nd:144.242,Pm:145,Sm:150.36,Eu:151.964,Gd:157.25,Tb:158.925,Dy:162.5,Ho:164.930,Er:167.259,Tm:168.934,Yb:173.045,Lu:174.967,Hf:178.49,Ta:180.948,W:183.84,Re:186.207,Os:190.23,Ir:192.217,Pt:195.084,Au:196.967,Hg:200.592,Tl:204.38,Pb:207.2,Bi:208.980,Po:209,At:210,Rn:222,Fr:223,Ra:226,Ac:227,Th:232.038,Pa:231.036,U:238.029,Np:237,Pu:244,Am:243,Cm:247,Bk:247,Cf:251,Es:252,Fm:257,Md:258,No:259,Lr:266,Rf:267,Db:268,Sg:269,Bh:270,Hs:269,Mt:278,Ds:281,Rg:282,Cn:285,Nh:286,Fl:289,Mc:290,Lv:293,Ts:294,Og:294};

function initCalculator(){
  document.querySelectorAll('.calc-tab').forEach(tab => {
    tab.addEventListener('click', function(){
      document.querySelectorAll('.calc-tab').forEach(t => t.classList.remove('active'));
      document.querySelectorAll('.calc-panel').forEach(p => p.classList.remove('active'));
      this.classList.add('active');
      document.getElementById('calc-' + this.dataset.calc).classList.add('active');
    });
  });
  toggleEnergyFields();
}

function parseFormula(f){
  f = f.replace(/\s+/g,'');
  const result = {};
  let i = 0;
  while(i < f.length){
    if(f[i] === '('){
      let depth = 1, j = i + 1;
      while(j < f.length && depth > 0){ if(f[j] === '(') depth++; if(f[j] === ')') depth--; j++; }
      const inner = f.substring(i+1, j-1);
      let mult = '';
      let k = j;
      while(k < f.length && /\d/.test(f[k])){ mult += f[k]; k++; }
      const multN = parseInt(mult) || 1;
      const innerResult = parseFormula(inner);
      Object.keys(innerResult).forEach(el => { result[el] = (result[el]||0) + innerResult[el]*multN; });
      i = k;
    } else {
      let el = '';
      if(i+1 < f.length && /[a-z]/.test(f[i+1])){ el = f[i] + f[i+1]; i += 2; } 
      else { el = f[i]; i++; }
      let num = '';
      while(i < f.length && /\d/.test(f[i])){ num += f[i]; i++; }
      const n = parseInt(num) || 1;
      if(ATOMIC_WEIGHTS[el]) result[el] = (result[el]||0) + n;
    }
  }
  return result;
}

function calcMolar(){
  const formula = document.getElementById('molar-formula').value.trim();
  if(!formula){ showToast('أدخل الصيغة','error'); return; }
  const parsed = parseFormula(formula);
  if(!Object.keys(parsed).length){ showToast('صيغة غير صحيحة','error'); return; }
  let total = 0, detail = [];
  Object.entries(parsed).forEach(([el, n]) => {
    const w = (ATOMIC_WEIGHTS[el] || 0) * n;
    total += w;
    detail.push(`${el}: ${n} × ${ATOMIC_WEIGHTS[el]} = ${w.toFixed(3)}`);
  });
  document.getElementById('molar-value').textContent = total.toFixed(3) + ' g/mol';
  document.getElementById('molar-detail').innerHTML = detail.join('<br>');
  document.getElementById('molar-result').classList.add('show');
}

function calcPH(){
  const type = document.getElementById('ph-type').value;
  const v = parseFloat(document.getElementById('ph-value').value);
  if(isNaN(v)){ showToast('أدخل قيمة صحيحة','error'); return; }
  let pH, pOH, h, oh;
  if(type === 'h'){ h = v; pH = -Math.log10(h); pOH = 14 - pH; oh = Math.pow(10, -pOH); }
  else if(type === 'oh'){ oh = v; pOH = -Math.log10(oh); pH = 14 - pOH; h = Math.pow(10, -pH); }
  else if(type === 'ph'){ pH = v; h = Math.pow(10, -pH); pOH = 14 - pH; oh = Math.pow(10, -pOH); }
  else { pOH = v; oh = Math.pow(10, -pOH); pH = 14 - pOH; h = Math.pow(10, -pH); }
  const nature = pH < 7 ? 'حمضي 🔴' : pH > 7 ? 'قاعدي 🔵' : 'متعادل ⚪';
  document.getElementById('ph-value-out').textContent = 'pH = ' + pH.toFixed(3);
  document.getElementById('ph-detail').innerHTML = `pOH = ${pOH.toFixed(3)}<br>[H⁺] = ${h.toExponential(3)} M<br>[OH⁻] = ${oh.toExponential(3)} M<br>${nature}`;
  document.getElementById('ph-result').classList.add('show');
}

function calcDilution(){
  const c1 = parseFloat(document.getElementById('d-c1').value);
  const v1 = parseFloat(document.getElementById('d-v1').value);
  const c2 = parseFloat(document.getElementById('d-c2').value);
  const v2 = parseFloat(document.getElementById('d-v2').value);
  let missing = [], result, label;
  if(isNaN(c1)) missing.push('c1');
  if(isNaN(v1)) missing.push('v1');
  if(isNaN(c2)) missing.push('c2');
  if(isNaN(v2)) missing.push('v2');
  if(missing.length !== 1){ showToast('اترك حقل واحد فارغاً','error'); return; }
  if(missing[0] === 'c1'){ result = (c2*v2)/v1; label = 'C₁'; document.getElementById('d-c1').value = result; }
  else if(missing[0] === 'v1'){ result = (c2*v2)/c1; label = 'V₁'; document.getElementById('d-v1').value = result; }
  else if(missing[0] === 'c2'){ result = (c1*v1)/v2; label = 'C₂'; document.getElementById('d-c2').value = result; }
  else { result = (c1*v1)/c2; label = 'V₂'; document.getElementById('d-v2').value = result; }
  document.getElementById('dilution-value').textContent = label + ' = ' + result.toFixed(4);
  document.getElementById('dilution-detail').innerHTML = `C₁ × V₁ = C₂ × V₂`;
  document.getElementById('dilution-result').classList.add('show');
}

function calcGas(){
  const P = parseFloat(document.getElementById('g-p').value);
  const V = parseFloat(document.getElementById('g-v').value);
  const n = parseFloat(document.getElementById('g-n').value);
  const T = parseFloat(document.getElementById('g-t').value);
  const R = 0.0821;
  const empty = [P,V,n,T].filter(x => isNaN(x)).length;
  if(empty !== 1){ showToast('اترك حقل واحد فارغاً','error'); return; }
  let result, label, unit;
  if(isNaN(P)){ P = (n*R*T)/V; result = P; label='P'; unit='atm'; document.getElementById('g-p').value = P.toFixed(4); }
  else if(isNaN(V)){ V = (n*R*T)/P; result = V; label='V'; unit='L'; document.getElementById('g-v').value = V.toFixed(4); }
  else if(isNaN(n)){ n = (P*V)/(R*T); result = n; label='n'; unit='mol'; document.getElementById('g-n').value = n.toFixed(4); }
  else { T = (P*V)/(n*R); result = T; label='T'; unit='K'; document.getElementById('g-t').value = T.toFixed(4); }
  document.getElementById('gas-value').textContent = label + ' = ' + result.toFixed(4) + ' ' + unit;
  document.getElementById('gas-detail').innerHTML = `PV = nRT<br>R = 0.0821 L·atm/(mol·K)`;
  document.getElementById('gas-result').classList.add('show');
}

function calcBalance(){
  const eq = document.getElementById('balance-eq').value.trim();
  if(!eq){ showToast('أدخل معادلة','error'); return; }
  const parts = eq.split(/=|->|→/).map(s => s.trim());
  if(parts.length !== 2){ showToast('صيغة المعادلة غير صحيحة','error'); return; }
  const react = parts[0].split('+').map(s => s.trim()).filter(s => s);
  const prod = parts[1].split('+').map(s => s.trim()).filter(s => s);
  const reactAtoms = react.map(parseFormula);
  const prodAtoms = prod.map(parseFormula);
  const allEls = new Set();
  [...reactAtoms, ...prodAtoms].forEach(m => Object.keys(m).forEach(e => allEls.add(e)));
  const elList = [...allEls];
  const nR = react.length, nP = prod.length, nC = nR + nP;
  const matrix = elList.map(el => {
    const row = [];
    reactAtoms.forEach(m => row.push(m[el] || 0));
    prodAtoms.forEach(m => row.push(-(m[el] || 0)));
    return row;
  });
  let solution = null;
  const max = 12;
  function search(coeffs, idx){
    if(solution) return;
    if(idx === nC){
      for(let i = 0; i < matrix.length; i++){
        let sum = 0;
        for(let j = 0; j < nC; j++) sum += matrix[i][j] * coeffs[j];
        if(sum !== 0) return;
      }
      if(coeffs.every(c => c > 0)) solution = [...coeffs];
      return;
    }
    for(let v = 1; v <= max; v++){
      coeffs.push(v);
      search(coeffs, idx + 1);
      coeffs.pop();
      if(solution) return;
    }
  }
  search([], 0);
  if(solution){
    const gcd = (a,b) => b === 0 ? a : gcd(b, a % b);
    let g = solution.reduce((a,b) => gcd(a,b));
    if(g > 1) solution = solution.map(c => c / g);
    const reactStr = react.map((s, i) => (solution[i] > 1 ? solution[i] : '') + s).join(' + ');
    const prodStr = prod.map((s, i) => {
      const c = solution[nR + i];
      return (c > 1 ? c : '') + s;
    }).join(' + ');
    document.getElementById('balance-value').textContent = reactStr + ' → ' + prodStr;
    document.getElementById('balance-detail').innerHTML = '✅ المعادلة موزونة';
  } else {
    document.getElementById('balance-value').textContent = 'لم يتم العثور على حل';
    document.getElementById('balance-detail').innerHTML = 'تأكد من صحة الصيغ';
  }
  document.getElementById('balance-result').classList.add('show');
}

function calcConc(){
  const m = parseFloat(document.getElementById('c-mass').value);
  const mw = parseFloat(document.getElementById('c-mw').value);
  const v = parseFloat(document.getElementById('c-vol').value);
  if(isNaN(m) || isNaN(mw) || isNaN(v)){ showToast('أدخل جميع القيم','error'); return; }
  const moles = m / mw;
  const conc = moles / v;
  document.getElementById('conc-value').textContent = conc.toFixed(4) + ' M';
  document.getElementById('conc-detail').innerHTML = `n = ${moles.toFixed(4)} mol<br>M = n/V = ${conc.toFixed(4)} M`;
  document.getElementById('conc-result').classList.add('show');
}

function calcConvert(){
  const v = parseFloat(document.getElementById('cv-val').value);
  const mw = parseFloat(document.getElementById('cv-mw').value);
  const type = document.getElementById('cv-type').value;
  if(isNaN(v) || isNaN(mw)){ showToast('أدخل القيم','error'); return; }
  const NA = 6.022e23;
  let mol, g, at;
  if(type === 'g'){ g = v; mol = g / mw; at = mol * NA; }
  else if(type === 'mol'){ mol = v; g = mol * mw; at = mol * NA; }
  else { at = v; mol = at / NA; g = mol * mw; }
  document.getElementById('convert-value').textContent = `${g.toFixed(4)} g | ${mol.toFixed(6)} mol | ${at.toExponential(3)}`;
  document.getElementById('convert-detail').innerHTML = `الكتلة المولية: ${mw} g/mol`;
  document.getElementById('convert-result').classList.add('show');
}

function toggleEnergyFields(){
  const type = document.getElementById('e-type').value;
  const el = document.getElementById('energy-fields');
  if(type === 'heat'){
    el.innerHTML = `
      <div class="calc-form-row">
        <div class="calc-field"><label>الكتلة (g)</label><input type="number" id="e-m" step="any" value="100"></div>
        <div class="calc-field"><label>الحرارة النوعية (J/g·°C)</label><input type="number" id="e-c" step="any" value="4.18"></div>
      </div>
      <div class="calc-field"><label>فرق الحرارة ΔT (°C)</label><input type="number" id="e-dt" step="any" value="10"></div>
    `;
  } else if(type === 'mass'){
    el.innerHTML = `<div class="calc-field"><label>الكتلة (kg)</label><input type="number" id="e-mass" step="any" value="1"></div>`;
  } else {
    el.innerHTML = `
      <div class="calc-form-row">
        <div class="calc-field"><label>التردد f (Hz)</label><input type="number" id="e-f" step="any" value="5e14"></div>
        <div class="calc-field"><label>أو الطول الموجي λ (nm)</label><input type="number" id="e-lambda" step="any" placeholder="اختياري"></div>
      </div>
    `;
  }
}

function calcEnergy(){
  const type = document.getElementById('e-type').value;
  let result, detail;
  if(type === 'heat'){
    const m = parseFloat(document.getElementById('e-m').value);
    const c = parseFloat(document.getElementById('e-c').value);
    const dt = parseFloat(document.getElementById('e-dt').value);
    const q = m * c * dt;
    result = q.toFixed(2) + ' J';
    detail = `q = m × c × ΔT = ${q.toFixed(2)} J = ${(q/1000).toFixed(4)} kJ`;
  } else if(type === 'mass'){
    const m = parseFloat(document.getElementById('e-mass').value);
    const c = 3e8;
    const E = m * c * c;
    result = E.toExponential(4) + ' J';
    detail = `E = mc² = ${E.toExponential(4)} J`;
  } else {
    const h = 6.626e-34;
    let f = parseFloat(document.getElementById('e-f').value);
    const lambda = parseFloat(document.getElementById('e-lambda').value);
    if(!isNaN(lambda) && lambda > 0) f = 3e8 / (lambda * 1e-9);
    const E = h * f;
    result = E.toExponential(4) + ' J';
    detail = `E = hf = ${E.toExponential(4)} J`;
  }
  document.getElementById('energy-value').textContent = result;
  document.getElementById('energy-detail').innerHTML = detail;
  document.getElementById('energy-result').classList.add('show');
}

/* ============================================================
   🤖 AI Assistant (Updated for 2026 models)
   ============================================================ */
const KNOWLEDGE = [
  {k:['ذرة','الذرة','تركيب الذرة'],a:'🔬 الذرة أصغر وحدة بنائية للمادة. تتكون من:\n\n• النواة: بروتونات (+) ونيوترونات\n• الإلكترونات: تدور في مدارات (-)\n\nعدد البروتونات = العدد الذري.'},
  {k:['جدول دوري','الجدول الدوري'],a:'📊 الجدول الدوري تنظيم للعناصر:\n\n• 118 عنصراً\n• مرتبة حسب العدد الذري\n• 7 دورات و18 مجموعة\n\nتصفح الجدول التفاعلي مع نماذج 3D!'},
  {k:['رابطة','روابط','الرابطة'],a:'🔗 أنواع الروابط:\n\n1. أيونية: انتقال e (NaCl)\n2. تساهمية: مشاركة e (H₂O)\n3. فلزية: بين الفلزات\n4. هيدروجينية: ضعيفة'},
  {k:['تفاعل','تفاعلات','معادلة'],a:'⚗️ التفاعل: إعادة ترتيب الذرات.\n\nالأنواع:\n• تكوين (A+B→AB)\n• تفكك (AB→A+B)\n• إحلال (A+BC→AC+B)\n• احتراق'},
  {k:['حمض','أحماض','قاعدة','قواعد'],a:'🧪 الأحماض: تُعطي H⁺\nالقواعد: تستقبل H⁺ أو تُعطي OH⁻\n\nالتعادل: حمض + قاعدة → ملح + ماء'},
  {k:['ph','pH'],a:'📏 pH = -log[H⁺]\n\n• pH<7 حمضي\n• pH=7 متعادل\n• pH>7 قاعدي\n\npOH = 14 - pH'},
  {k:['مول','المول'],a:'⚖️ المول:\n\n• 1 مول = 6.022×10²³ جزيء\n• n = m / M\n• عدد الجزيئات = n × 6.022×10²³'},
  {k:['تركيز','مولارية'],a:'💧 M = n/V\n\nM = (m/MW)/V\n\nاستخدم الآلة الحاسبة!'},
  {k:['ماء','H2O','h2o'],a:'💧 H₂O: جزيء قطبي، زاوية 104.5°، كتلة 18.015 g/mol'},
  {k:['حديد','Fe'],a:'🔴 Fe (26): فلز انتقالي، يدخل في الهيموغلوبين. اقرأ التفاصيل في "الكيمياء والانسان"'},
  {k:['كيمياء حيوية','bio'],a:'🧬 الكيمياء الحيوية: تفاعلات الجسم.\n\nاقرأ البحث في قسم "الكيمياء والانسان"'},
  {k:['انزيم','إنزيم'],a:'🔬 الإنزيمات: محفزات حيوية تسرّع التفاعل دون استهلاك.'},
  {k:['dna','DNA'],a:'🧬 DNA: يحمل المعلومات الوراثية. سلسلة مزدوجة. A-T، G-C.'},
  {k:['حسابات','حساب'],a:'🧮 n = m/M، M = n/V، جزيئات = n × 6.022×10²³'},
  {k:['اتزان','توازن'],a:'⚖️ الاتزان: معدل أمامي = عكسي. Keq = [P]/[R]'},
  {k:['قانون حفظ الكتلة'],a:'⚖️ المادة لا تفنى ولا تستحدث.'},
  {k:['سلام','مرحبا','اهلا'],a:'👋 أهلاً! أنا كيميائي AI. اسأل عن: ذرة، تفاعل، حمض، pH، مول، أي موضوع كيميائي!'},
  {k:['شكرا','thanks'],a:'🌟 على الرحب والسعة!'},
  {k:['من انت','من أنت'],a:'🤖 أنا كيميائي AI، مساعدك الذكي في الكيمياء. يمكن تفعيلي بـ Groq أو Gemini من الإعدادات.'}
];

let chatMessages = [];

function initChat(){
  const saved = localStorage.getItem('chemChat');
  if(saved){ try { chatMessages = JSON.parse(saved); } catch { chatMessages = []; } }
  if(!chatMessages.length){
    chatMessages.push({role:'bot', text:'👋 أهلاً! أنا كيميائي AI. كيف أستطيع مساعدتك اليوم؟\n\nاسأل عن:\n• الذرة والجدول الدوري\n• التفاعلات والروابط\n• الأحماض والقواعد\n• أي موضوع كيميائي', html:true});
  }
  renderChat();
  updateAIStatus();
  const input = document.getElementById('chat-input');
  if(!input.dataset.bound){
    input.dataset.bound = '1';
    input.addEventListener('keydown', e => {
      if(e.key === 'Enter' && !e.shiftKey){ e.preventDefault(); sendChat(); }
    });
    input.addEventListener('input', function(){
      this.style.height = 'auto';
      this.style.height = Math.min(this.scrollHeight, 120) + 'px';
    });
  }
}

function updateAIStatus(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  const status = document.getElementById('ai-status');
  if(!status) return;
  if(s.aiProvider === 'groq' && s.groqKey) status.textContent = '🟢 Groq AI';
  else if(s.aiProvider === 'gemini' && s.geminiKey) status.textContent = '🟢 Google Gemini';
  else status.textContent = '🟢 جاهز — قاعدة معرفة داخلية';
}

function renderChat(){
  const box = document.getElementById('chat-msgs');
  box.innerHTML = chatMessages.map(m => {
    const cls = m.role === 'user' ? 'user' : 'bot';
    const content = m.html ? m.text : escapeHtml(m.text).replace(/`([^`]+)`/g, '<code>$1</code>').replace(/\n/g, '<br>');
    return `<div class="chat-msg ${cls}">${content}</div>`;
  }).join('');
  box.scrollTop = box.scrollHeight;
}

function escapeHtml(s){
  return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}

function addChatMessage(role, text, html){
  chatMessages.push({role, text, html});
  if(chatMessages.length > 100) chatMessages = chatMessages.slice(-100);
  localStorage.setItem('chemChat', JSON.stringify(chatMessages));
  renderChat();
}

function clearChat(){
  confirmModal('مسح المحادثة','هل تريد مسح جميع الرسائل؟', () => {
    chatMessages = [];
    localStorage.removeItem('chemChat');
    initChat();
    showToast('تم مسح المحادثة','success');
  });
}

function sendSuggestion(text){
  document.getElementById('chat-input').value = text;
  sendChat();
}

async function sendChat(){
  const input = document.getElementById('chat-input');
  const text = input.value.trim();
  if(!text) return;
  input.value = '';
  input.style.height = 'auto';
  addChatMessage('user', text);
  showTyping();
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  try {
    let response;
    if(s.aiProvider === 'groq' && s.groqKey) response = await callGroq(text, s.groqKey);
    else if(s.aiProvider === 'gemini' && s.geminiKey) response = await callGemini(text, s.geminiKey);
    else response = getInternalResponse(text);
    hideTyping();
    addChatMessage('bot', response, s.aiProvider && s.aiProvider !== 'internal');
  } catch(err){
    hideTyping();
    addChatMessage('bot', '⚠️ حدث خطأ: ' + err.message + '\n\nسأجيب من قاعدة المعرفة:\n\n' + getInternalResponse(text));
  }
}

function showTyping(){
  const box = document.getElementById('chat-msgs');
  const el = document.createElement('div');
  el.className = 'chat-msg bot typing';
  el.id = 'typing-msg';
  el.innerHTML = '<span></span><span></span><span></span>';
  box.appendChild(el);
  box.scrollTop = box.scrollHeight;
}
function hideTyping(){
  const el = document.getElementById('typing-msg');
  if(el) el.remove();
}

function getInternalResponse(q){
  const lower = q.toLowerCase().trim();
  let best = null, bestScore = 0;
  KNOWLEDGE.forEach(k => {
    let score = 0;
    k.k.forEach(kw => {
      const kwl = kw.toLowerCase();
      if(lower.includes(kwl)) score += kwl.length * 2;
    });
    if(score > bestScore){ bestScore = score; best = k; }
  });
  if(best && bestScore > 0) return best.a;
  return `🤔 لم أجد إجابة دقيقة.\n\nجرّب:\n• إعادة صياغة السؤال\n• كلمات مفتاحية: ذرة، تفاعل، حمض، pH، مول\n• أو شغّل الذكاء الاصطناعي الحقيقي من الإعدادات`;
}

/* ============================================================
   🚀 callGroq — يجرب عدة نماذج تلقائياً
   ============================================================ */
async function callGroq(prompt, key){
  const models = [
    'llama-3.3-70b-versatile',
    'llama-3.1-70b-versatile',
    'llama-3.1-8b-instant',
    'mixtral-8x7b-32768',
    'gemma2-9b-it'
  ];
  
  const savedModel = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}').groqModel;
  if(savedModel && !models.includes(savedModel)) models.unshift(savedModel);
  
  let lastError = null;
  
  for(const model of models){
    try {
      const res = await fetch('https://api.groq.com/openai/v1/chat/completions', {
        method: 'POST',
        headers: {'Content-Type':'application/json','Authorization':'Bearer ' + key},
        body: JSON.stringify({
          model: model,
          messages: [
            {role:'system', content:'أنت كيميائي AI متخصص في الكيمياء. أجب بالعربية بشكل واضح ومفيد. كن دقيقاً علمياً.'},
            {role:'user', content: prompt}
          ],
          temperature: 0.7,
          max_tokens: 1024
        })
      });
      
      if(res.ok){
        const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
        s.groqModel = model;
        localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
        const data = await res.json();
        return data.choices[0].message.content;
      }
      
      const errText = await res.text();
      lastError = `${model}: ${res.status}`;
      
      if(res.status === 401 || res.status === 403){
        throw new Error('المفتاح غير صحيح أو منتهي الصلاحية');
      }
    } catch(e){
      lastError = e.message;
      if(e.message.includes('المفتاح')) throw e;
    }
  }
  
  throw new Error('جميع النماذج فشلت. آخر خطأ: ' + lastError);
}

/* ============================================================
   🚀 callGemini — محدث لنماذج 2026
   ============================================================ */
async function callGemini(prompt, key){
  const models = [
    'gemini-2.5-flash',
    'gemini-2.5-flash-lite',
    'gemini-2.0-flash',
    'gemini-2.0-flash-lite',
    'gemini-flash-latest'
  ];
  
  const savedModel = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}').geminiModel;
  if(savedModel && !models.includes(savedModel)) models.unshift(savedModel);
  
  let lastError = null;
  
  for(const model of models){
    try {
      const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${key}`, {
        method: 'POST',
        headers: {'Content-Type':'application/json'},
        body: JSON.stringify({
          contents: [{parts:[{text: 'أنت كيميائي AI متخصص في الكيمياء. أجب بالعربية بشكل واضح ومفيد.\n\nالسؤال: ' + prompt}]}]
        })
      });
      
      if(res.ok){
        const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
        s.geminiModel = model;
        localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
        const data = await res.json();
        return data.candidates[0].content.parts[0].text;
      }
      
      const errText = await res.text();
      lastError = `${model}: ${res.status}`;
      
      if(res.status === 401 || res.status === 403){
        throw new Error('المفتاح غير صحيح أو منتهي الصلاحية');
      }
    } catch(e){
      lastError = e.message;
      if(e.message.includes('المفتاح')) throw e;
    }
  }
  
  throw new Error('جميع النماذج فشلت. آخر خطأ: ' + lastError);
}

function openAISettings(){ openAIModal(); }
function openAIModal(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  document.getElementById('ai-modal-provider').value = s.aiProvider || 'internal';
  document.getElementById('ai-modal-groq').value = s.groqKey || '';
  document.getElementById('ai-modal-gemini').value = s.geminiKey || '';
  document.getElementById('ai-modal-overlay').classList.add('active');
}
function closeAIModal(){ document.getElementById('ai-modal-overlay').classList.remove('active'); }
function saveAIModal(){
  const s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
  s.aiProvider = document.getElementById('ai-modal-provider').value;
  s.groqKey = document.getElementById('ai-modal-groq').value;
  s.geminiKey = document.getElementById('ai-modal-gemini').value;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  updateAIStatus();
  closeAIModal();
  showToast('✅ تم الحفظ','success');
}

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

document.addEventListener('keydown', function(e){
  if(document.getElementById('quiz').classList.contains('active')){
    const opts = document.querySelectorAll('.option');
    if(opts[0] && opts[0].classList.contains('disabled')){
      if(e.key === 'Enter' && !document.getElementById('next-btn').disabled){
        document.getElementById('next-btn').click();
      }
      return;
    }
    const map = {'1':0,'2':1,'3':2,'4':3};
    if(map[e.key] !== undefined && opts[map[e.key]]) opts[map[e.key]].click();
  }
  if(e.key === 'Escape'){ closeModal(); closeElement3D(); closeAIModal(); }
});

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
