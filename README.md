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
  --light:#f8fafc;
  --dark:#0f172a;
  --success:#10b981;
  --warning:#f59e0b;
  --danger:#ef4444;
  --card:rgba(255,255,255,0.08);
  --card-solid:rgba(255,255,255,0.95);
  --glass:rgba(255,255,255,0.1);
  --glass-border:rgba(255,255,255,0.18);
  --transition:all .35s cubic-bezier(.4,0,.2,1);
}

*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,sans-serif}

html,body{height:100%}

body{
  background:#050b1e;
  color:#e2e8f0;
  overflow-x:hidden;
  position:relative;
}

/* ===== الخلفية التفاعلية ===== */
#bg-canvas{
  position:fixed;
  top:0;left:0;
  width:100%;height:100%;
  z-index:-2;
  display:block;
}

.bg-gradient-overlay{
  position:fixed;
  top:0;left:0;
  width:100%;height:100%;
  z-index:-1;
  background:
    radial-gradient(ellipse at 20% 10%, rgba(99,102,241,.35) 0%, transparent 55%),
    radial-gradient(ellipse at 80% 90%, rgba(139,92,246,.3) 0%, transparent 55%),
    radial-gradient(ellipse at 50% 50%, rgba(6,214,160,.08) 0%, transparent 70%);
  pointer-events:none;
}

/* ===== التمرير ===== */
.main-content::-webkit-scrollbar{width:8px}
.main-content::-webkit-scrollbar-track{background:rgba(255,255,255,.03)}
.main-content::-webkit-scrollbar-thumb{background:linear-gradient(180deg,var(--primary),var(--secondary));border-radius:10px}

/* ===== شاشة تسجيل الدخول ===== */
.login-screen{
  display:none;
  min-height:100vh;
  align-items:center;
  justify-content:center;
  padding:20px;
  position:relative;
  z-index:10;
}
.login-screen.active{display:flex}

.login-container{
  background:rgba(15,23,42,.72);
  backdrop-filter:blur(24px);
  -webkit-backdrop-filter:blur(24px);
  border:1px solid var(--glass-border);
  border-radius:28px;
  padding:50px 40px;
  width:100%;
  max-width:520px;
  text-align:center;
  box-shadow:0 30px 80px rgba(0,0,0,.5), 0 0 0 1px rgba(255,255,255,.05) inset;
  animation:loginIn .8s cubic-bezier(.4,0,.2,1);
  position:relative;
  overflow:hidden;
}

.login-container::before{
  content:'';
  position:absolute;
  top:-50%;left:-50%;
  width:200%;height:200%;
  background:conic-gradient(from 0deg, transparent, rgba(99,102,241,.15), transparent 30%);
  animation:rotateConic 8s linear infinite;
  z-index:-1;
}

@keyframes rotateConic{to{transform:rotate(360deg)}}

@keyframes loginIn{
  from{opacity:0;transform:translateY(40px) scale(.95)}
  to{opacity:1;transform:translateY(0) scale(1)}
}

.login-icon{
  width:110px;height:110px;
  margin:0 auto 25px;
  border-radius:50%;
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  display:flex;align-items:center;justify-content:center;
  font-size:3em;color:#fff;
  box-shadow:0 0 40px rgba(99,102,241,.6), 0 0 80px rgba(139,92,246,.3);
  animation:pulseIcon 2.5s ease-in-out infinite;
  position:relative;
}

.login-icon::before{
  content:'';
  position:absolute;
  inset:-15px;
  border-radius:50%;
  border:2px solid rgba(99,102,241,.3);
  animation:ringPulse 2.5s ease-out infinite;
}

.login-icon::after{
  content:'';
  position:absolute;
  inset:-25px;
  border-radius:50%;
  border:1px solid rgba(139,92,246,.2);
  animation:ringPulse 2.5s ease-out infinite .5s;
}

@keyframes pulseIcon{
  0%,100%{transform:scale(1)}
  50%{transform:scale(1.05)}
}

@keyframes ringPulse{
  0%{transform:scale(.8);opacity:1}
  100%{transform:scale(1.3);opacity:0}
}

.login-container h1{
  font-size:2em;
  margin-bottom:12px;
  background:linear-gradient(135deg,#fff,var(--cyan));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  font-weight:800;
}

.login-container > p{
  color:#94a3b8;
  margin-bottom:32px;
  font-size:1.02em;
  line-height:1.7;
}

.form-group{margin-bottom:20px;text-align:right}

.form-group label{
  display:block;
  margin-bottom:10px;
  color:#cbd5e1;
  font-weight:600;
  font-size:.95em;
}

.form-control{
  width:100%;
  padding:16px 18px;
  border:2px solid rgba(255,255,255,.1);
  border-radius:14px;
  font-size:1em;
  background:rgba(255,255,255,.05);
  color:#fff;
  transition:var(--transition);
}

.form-control::placeholder{color:#64748b}

.form-control:focus{
  border-color:var(--primary);
  outline:none;
  background:rgba(255,255,255,.08);
  box-shadow:0 0 0 4px rgba(99,102,241,.2);
}

.form-control option{background:#1e293b;color:#fff}

/* ===== التطبيق الرئيسي ===== */
.app-container{display:none;min-height:100vh}
.app-container.active{display:flex}

/* ===== الشريط الجانبي ===== */
.sidebar{
  width:280px;
  background:rgba(15,23,42,.78);
  backdrop-filter:blur(24px);
  -webkit-backdrop-filter:blur(24px);
  border-left:1px solid var(--glass-border);
  padding:22px 0;
  position:sticky;
  top:0;
  height:100vh;
  overflow-y:auto;
  z-index:100;
  display:flex;
  flex-direction:column;
}

.sidebar::-webkit-scrollbar{width:5px}
.sidebar::-webkit-scrollbar-thumb{background:rgba(99,102,241,.4);border-radius:10px}

.logo{
  text-align:center;
  padding:14px 20px 24px;
  border-bottom:1px solid rgba(255,255,255,.08);
  margin-bottom:18px;
}

.logo h1{
  font-size:1.4em;
  display:flex;
  align-items:center;
  justify-content:center;
  gap:10px;
  background:linear-gradient(135deg,var(--primary),var(--cyan));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  font-weight:800;
}

.logo i{
  font-size:1.7em;
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  animation:spinLogo 8s linear infinite;
}

@keyframes spinLogo{
  0%,100%{transform:rotate(0deg)}
  50%{transform:rotate(180deg)}
}

.nav-menu{list-style:none;padding:0 14px;flex-shrink:0}

.nav-item{margin-bottom:6px}

.nav-link{
  display:flex;
  align-items:center;
  gap:14px;
  padding:13px 16px;
  color:#cbd5e1;
  text-decoration:none;
  border-radius:12px;
  transition:var(--transition);
  font-size:.95em;
  position:relative;
  overflow:hidden;
}

.nav-link::before{
  content:'';
  position:absolute;
  inset:0;
  background:linear-gradient(135deg,rgba(99,102,241,.25),rgba(139,92,246,.25));
  opacity:0;
  transition:var(--transition);
  z-index:-1;
}

.nav-link:hover{
  color:#fff;
  transform:translateX(-4px);
}

.nav-link:hover::before{opacity:.5}

.nav-link.active{
  color:#fff;
  background:linear-gradient(135deg,rgba(99,102,241,.9),rgba(139,92,246,.9));
  box-shadow:0 8px 24px rgba(99,102,241,.4);
}

.nav-link.active::before{opacity:1}

.nav-link i{
  font-size:1.15em;
  width:22px;
  text-align:center;
}

/* ===== فريق العمل ===== */
.team-section{
  padding:18px 14px;
  border-top:1px solid rgba(255,255,255,.08);
  margin-top:16px;
}

.team-section h3{
  margin-bottom:14px;
  font-size:.95em;
  color:#94a3b8;
  display:flex;
  align-items:center;
  gap:8px;
  padding:0 6px;
}

.team-section h3 i{color:var(--primary)}

.team-member{
  display:flex;
  align-items:center;
  gap:12px;
  padding:9px 12px;
  margin-bottom:6px;
  border-radius:10px;
  transition:var(--transition);
  cursor:pointer;
  background:rgba(255,255,255,.03);
  border:1px solid rgba(255,255,255,.05);
}

.team-member:hover{
  background:rgba(255,255,255,.08);
  transform:translateX(-3px);
}

.team-avatar{
  width:34px;height:34px;
  border-radius:50%;
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:.85em;
  color:#fff;
  flex-shrink:0;
  box-shadow:0 3px 8px rgba(0,0,0,.3);
}

.team-member-info{flex:1;min-width:0}
.team-member-name{font-size:.85em;font-weight:600;color:#e2e8f0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.team-member-role{font-size:.72em;color:#94a3b8;margin-top:2px}

/* ===== معلومات المستخدم ===== */
.user-section{
  padding:18px;
  border-top:1px solid rgba(255,255,255,.08);
  margin-top:auto;
}

.user-info{
  display:flex;
  align-items:center;
  gap:12px;
  padding:12px;
  background:rgba(255,255,255,.04);
  border-radius:12px;
  border:1px solid rgba(255,255,255,.06);
}

.user-avatar{
  width:46px;height:46px;
  border-radius:50%;
  background:linear-gradient(135deg,var(--accent),var(--cyan));
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:1.3em;
  color:#fff;
  flex-shrink:0;
  box-shadow:0 4px 12px rgba(6,214,160,.35);
}

.user-info h4{font-size:.92em;color:#f1f5f9;margin-bottom:3px}
.student-info p{font-size:.78em;color:#94a3b8;margin-bottom:2px}

.logout-btn{
  margin-top:12px;
  width:100%;
  padding:11px;
  background:rgba(239,68,68,.15);
  color:#fca5a5;
  border:1px solid rgba(239,68,68,.3);
  border-radius:10px;
  cursor:pointer;
  font-size:.88em;
  font-weight:600;
  transition:var(--transition);
  display:flex;
  align-items:center;
  justify-content:center;
  gap:8px;
}

.logout-btn:hover{background:rgba(239,68,68,.3);color:#fff;transform:translateY(-2px)}

/* ===== المحتوى ===== */
.main-content{
  flex:1;
  padding:22px;
  overflow-y:auto;
  height:100vh;
  position:relative;
}

/* ===== الهيدر ===== */
.header{
  background:rgba(15,23,42,.68);
  backdrop-filter:blur(20px);
  -webkit-backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:18px 26px;
  margin-bottom:22px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  flex-wrap:wrap;
  gap:14px;
  box-shadow:0 8px 32px rgba(0,0,0,.25);
}

.header h2{
  color:#f1f5f9;
  display:flex;
  align-items:center;
  gap:14px;
  font-size:1.25em;
}

.header h2 i{
  background:linear-gradient(135deg,var(--primary),var(--cyan));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  font-size:1.3em;
}

.user-actions{display:flex;gap:12px;flex-wrap:wrap}

/* ===== الأزرار ===== */
.btn{
  padding:11px 22px;
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  color:#fff;
  border:none;
  border-radius:12px;
  cursor:pointer;
  font-size:.93em;
  font-weight:700;
  transition:var(--transition);
  display:inline-flex;
  align-items:center;
  justify-content:center;
  gap:8px;
  box-shadow:0 6px 18px rgba(99,102,241,.35);
  position:relative;
  overflow:hidden;
  white-space:nowrap;
}

.btn::after{
  content:'';
  position:absolute;
  top:50%;left:50%;
  width:0;height:0;
  background:rgba(255,255,255,.25);
  border-radius:50%;
  transform:translate(-50%,-50%);
  transition:width .6s,height .6s;
}

.btn:hover::after{width:400px;height:400px}

.btn:hover{transform:translateY(-2px);box-shadow:0 10px 24px rgba(99,102,241,.5)}
.btn:active{transform:translateY(0)}

.btn-success{background:linear-gradient(135deg,var(--success),#059669);box-shadow:0 6px 18px rgba(16,185,129,.35)}
.btn-warning{background:linear-gradient(135deg,var(--warning),#d97706);box-shadow:0 6px 18px rgba(245,158,11,.35)}
.btn-danger{background:linear-gradient(135deg,var(--danger),#dc2626);box-shadow:0 6px 18px rgba(239,68,68,.35)}
.btn-cyan{background:linear-gradient(135deg,var(--cyan),#0891b2);box-shadow:0 6px 18px rgba(34,211,238,.35)}

.btn-outline{
  background:rgba(255,255,255,.05);
  border:1.5px solid rgba(255,255,255,.15);
  color:#e2e8f0;
  box-shadow:none;
}

.btn-outline:hover{
  background:rgba(255,255,255,.1);
  border-color:var(--primary);
  color:#fff;
}

/* ===== الشاشات ===== */
.screen{display:none}
.screen.active{display:block;animation:fadeIn .5s ease}

@keyframes fadeIn{
  from{opacity:0;transform:translateY(15px)}
  to{opacity:1;transform:translateY(0)}
}

/* ===== شاشة الترحيب ===== */
.welcome-screen{text-align:center;padding:20px 10px}

.welcome-icon{
  font-size:4.5em;
  margin-bottom:18px;
  background:linear-gradient(135deg,var(--primary),var(--cyan),var(--secondary));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  animation:bounceIcon 3s ease-in-out infinite;
}

@keyframes bounceIcon{
  0%,100%{transform:translateY(0)}
  50%{transform:translateY(-12px)}
}

.welcome-screen h1{
  color:#f1f5f9;
  margin-bottom:14px;
  font-size:2.1em;
  font-weight:800;
  line-height:1.4;
}

.welcome-screen h1 span{
  background:linear-gradient(135deg,var(--cyan),var(--primary));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
}

.welcome-screen > p{
  color:#94a3b8;
  font-size:1.05em;
  max-width:640px;
  margin:0 auto 32px;
  line-height:1.8;
}

/* ===== البطاقات ===== */
.cards-container{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(280px,1fr));
  gap:20px;
  margin-bottom:30px;
}

.card{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  -webkit-backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:26px 22px;
  transition:var(--transition);
  cursor:pointer;
  position:relative;
  overflow:hidden;
}

.card::before{
  content:'';
  position:absolute;
  top:0;left:0;right:0;
  height:4px;
  background:linear-gradient(90deg,var(--primary),var(--cyan),var(--secondary));
  transform:scaleX(0);
  transform-origin:right;
  transition:transform .4s;
}

.card:hover::before{transform:scaleX(1);transform-origin:left}

.card:hover{
  transform:translateY(-8px);
  border-color:rgba(99,102,241,.4);
  box-shadow:0 20px 50px rgba(99,102,241,.25);
}

.card-header{
  display:flex;
  align-items:center;
  gap:16px;
  margin-bottom:16px;
}

.card-icon{
  width:56px;height:56px;
  border-radius:16px;
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:1.6em;
  color:#fff;
  flex-shrink:0;
  box-shadow:0 8px 20px rgba(0,0,0,.3);
}

.card h3{color:#f1f5f9;font-size:1.15em;font-weight:700}
.card p{color:#94a3b8;line-height:1.75;margin-bottom:18px;font-size:.92em}
.card .btn{width:100%}

/* ألوان البطاقات */
.card-1 .card-icon{background:linear-gradient(135deg,#6366f1,#8b5cf6)}
.card-2 .card-icon{background:linear-gradient(135deg,#06d6a0,#0891b2)}
.card-3 .card-icon{background:linear-gradient(135deg,#f59e0b,#dc2626)}
.card-4 .card-icon{background:linear-gradient(135deg,#ec4899,#8b5cf6)}
.card-5 .card-icon{background:linear-gradient(135deg,#0ea5e9,#6366f1)}

/* ===== إعدادات ===== */
.settings-container{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:30px;
}

.settings-group{margin-bottom:30px}
.settings-group:last-child{margin-bottom:0}

.settings-group h3{
  color:#e2e8f0;
  margin-bottom:20px;
  padding-bottom:12px;
  border-bottom:1px solid rgba(255,255,255,.08);
  display:flex;
  align-items:center;
  gap:12px;
  font-size:1.05em;
}

.settings-group h3 i{color:var(--cyan)}

.setting-item{
  display:flex;
  justify-content:space-between;
  align-items:center;
  gap:16px;
  padding:16px 0;
  border-bottom:1px solid rgba(255,255,255,.05);
  flex-wrap:wrap;
}

.setting-item:last-child{border-bottom:none}
.setting-info h4{color:#e2e8f0;margin-bottom:5px;font-size:.95em}
.setting-info p{color:#94a3b8;font-size:.83em}

select,input[type="number"],input[type="text"],input[type="email"],textarea{
  padding:11px 15px;
  border:1.5px solid rgba(255,255,255,.12);
  border-radius:11px;
  font-size:.92em;
  background:rgba(255,255,255,.05);
  color:#e2e8f0;
  transition:var(--transition);
  font-family:inherit;
}

select:focus,input:focus,textarea:focus{
  border-color:var(--primary);
  outline:none;
  background:rgba(255,255,255,.08);
  box-shadow:0 0 0 3px rgba(99,102,241,.2);
}

select option{background:#1e293b;color:#e2e8f0}

.toggle-switch{position:relative;display:inline-block;width:54px;height:28px;flex-shrink:0}
.toggle-switch input{opacity:0;width:0;height:0}
.slider{
  position:absolute;cursor:pointer;
  inset:0;
  background:rgba(255,255,255,.15);
  transition:.3s;
  border-radius:34px;
}
.slider:before{
  content:'';
  position:absolute;
  height:20px;width:20px;
  left:4px;bottom:4px;
  background:#fff;
  transition:.3s;
  border-radius:50%;
}
input:checked + .slider{background:linear-gradient(135deg,var(--success),#059669)}
input:checked + .slider:before{transform:translateX(26px)}

/* ===== إعداد الاختبار ===== */
.quiz-setup{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:32px;
  max-width:640px;
  margin:0 auto;
}

.quiz-setup h3{
  text-align:center;
  margin-bottom:26px;
  color:#f1f5f9;
  font-size:1.15em;
}

.quiz-options{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:16px;
  margin-bottom:26px;
}

.quiz-option{
  padding:22px 16px;
  border:2px solid rgba(255,255,255,.1);
  border-radius:16px;
  cursor:pointer;
  transition:var(--transition);
  text-align:center;
  background:rgba(255,255,255,.03);
  position:relative;
  overflow:hidden;
}

.quiz-option i{font-size:1.9em;margin-bottom:10px;color:var(--primary);display:block;transition:var(--transition)}
.quiz-option h4{color:#e2e8f0;margin-bottom:6px;font-size:1em}
.quiz-option p{color:#94a3b8;font-size:.82em;line-height:1.6}

.quiz-option:hover{
  border-color:var(--primary);
  transform:translateY(-4px);
  background:rgba(99,102,241,.1);
}

.quiz-option:hover i{transform:scale(1.15);color:var(--cyan)}

.quiz-option.selected{
  border-color:var(--primary);
  background:linear-gradient(135deg,rgba(99,102,241,.2),rgba(139,92,246,.2));
  box-shadow:0 10px 30px rgba(99,102,241,.25);
}

.quiz-option.selected i{color:var(--cyan)}

.quiz-controls{
  display:flex;
  gap:12px;
  margin-top:22px;
  flex-wrap:wrap;
}

.quiz-controls .btn{flex:1;min-width:140px}

/* ===== شاشة الاختبار ===== */
.quiz-container{display:flex;gap:20px;flex-wrap:wrap}

.quiz-content,.whiteboard-container{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:26px;
}

.quiz-content{flex:1;min-width:320px}
.whiteboard-container{flex:1;min-width:320px;display:flex;flex-direction:column}

.whiteboard-header{
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-bottom:14px;
  padding-bottom:14px;
  border-bottom:1px solid rgba(255,255,255,.08);
}

.whiteboard-header h3{color:#e2e8f0;font-size:1em;display:flex;align-items:center;gap:8px}

.whiteboard-frame{
  width:100%;
  height:480px;
  border:none;
  border-radius:14px;
  background:#fff;
}

.whiteboard-info{
  margin-top:14px;
  text-align:center;
  font-size:.82em;
  color:#94a3b8;
}

.quiz-header{
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-bottom:22px;
  padding-bottom:16px;
  border-bottom:1px solid rgba(255,255,255,.08);
  gap:14px;
  flex-wrap:wrap;
}

.quiz-info{display:flex;gap:18px;font-size:.88em;color:#cbd5e1;flex-wrap:wrap}

.quiz-timer{
  background:linear-gradient(135deg,var(--warning),#d97706);
  color:#fff;
  padding:9px 16px;
  border-radius:12px;
  font-weight:700;
  font-size:.92em;
  box-shadow:0 4px 14px rgba(245,158,11,.4);
  display:flex;
  align-items:center;
  gap:8px;
}

.progress-bar{
  height:8px;
  background:rgba(255,255,255,.08);
  border-radius:6px;
  overflow:hidden;
}

.progress{
  height:100%;
  background:linear-gradient(90deg,var(--primary),var(--cyan));
  width:0%;
  transition:width .5s ease;
  border-radius:6px;
  box-shadow:0 0 12px rgba(99,102,241,.6);
}

.question{
  font-size:1.18em;
  margin-bottom:22px;
  color:#f1f5f9;
  line-height:1.75;
  padding:22px;
  background:rgba(99,102,241,.08);
  border-radius:14px;
  border-right:4px solid var(--primary);
}

.options{display:grid;gap:12px;margin-bottom:22px}

.option{
  padding:16px 20px;
  background:rgba(255,255,255,.04);
  border:1.5px solid rgba(255,255,255,.1);
  border-radius:13px;
  cursor:pointer;
  transition:var(--transition);
  display:flex;
  align-items:center;
  justify-content:space-between;
  color:#e2e8f0;
  font-size:.96em;
}

.option:hover{
  border-color:var(--primary);
  background:rgba(99,102,241,.1);
  transform:translateX(-3px);
}

.option.selected{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#fff;border-color:transparent}
.option.correct{background:linear-gradient(135deg,var(--success),#059669);color:#fff;border-color:transparent}
.option.wrong{background:linear-gradient(135deg,var(--danger),#dc2626);color:#fff;border-color:transparent}
.option.disabled{cursor:not-allowed}

.option-number{
  display:inline-flex;
  align-items:center;
  justify-content:center;
  width:30px;height:30px;
  background:rgba(255,255,255,.08);
  border-radius:50%;
  font-weight:700;
  font-size:.85em;
  flex-shrink:0;
}

.selected .option-number,.correct .option-number,.wrong .option-number{background:rgba(255,255,255,.25)}

.explanation{
  background:rgba(16,185,129,.1);
  padding:20px;
  border-radius:14px;
  margin-top:20px;
  border-right:4px solid var(--success);
  display:none;
  color:#d1fae5;
  line-height:1.75;
}

.explanation h3{color:#6ee7b7;margin-bottom:10px;display:flex;align-items:center;gap:10px;font-size:1em}

/* ===== النتائج ===== */
.result-container{text-align:center;padding:26px;max-width:1000px;margin:0 auto}

.result-container .user-info{
  background:rgba(99,102,241,.1);
  padding:22px;
  border-radius:16px;
  margin-bottom:22px;
  border:1px solid rgba(99,102,241,.25);
  text-align:right;
}

.result-container .user-info p{margin-bottom:8px;color:#cbd5e1;font-size:.95em}
.result-container .user-info strong{color:#f1f5f9}

.result-container h2{color:#f1f5f9;margin-bottom:14px;display:flex;align-items:center;justify-content:center;gap:12px;font-size:1.3em}

.result-score{
  font-size:4em;
  font-weight:800;
  background:linear-gradient(135deg,var(--cyan),var(--primary));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  margin:14px 0;
  text-shadow:0 0 60px rgba(34,211,238,.3);
}

.result-chart{width:250px;height:250px;margin:0 auto 20px}

.comparison{
  background:rgba(139,92,246,.1);
  padding:22px;
  border-radius:14px;
  margin:22px 0;
  border:1px solid rgba(139,92,246,.25);
  color:#e2e8f0;
}

.comparison h3{margin-bottom:10px;color:#c4b5fd;font-size:1.05em}

.weak-lessons{text-align:right;margin-top:26px}

.weak-lessons > h3{
  color:#f1f5f9;
  margin-bottom:16px;
  display:flex;
  align-items:center;
  gap:10px;
  font-size:1.05em;
}

.lesson-item{
  background:rgba(239,68,68,.1);
  padding:16px;
  margin:10px 0;
  border-radius:12px;
  border-right:4px solid var(--danger);
  color:#fecaca;
  line-height:1.7;
  font-size:.92em;
}

.lesson-item strong{color:#fca5a5}
.lesson-item p{margin-top:6px}

.advanced-report{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:26px;
  margin-top:26px;
}

.report-section h3{
  color:#f1f5f9;
  margin-bottom:16px;
  padding-bottom:12px;
  border-bottom:1px solid rgba(255,255,255,.08);
  display:flex;
  align-items:center;
  gap:10px;
  font-size:1.05em;
}

.report-chart{width:100%;height:280px;margin:16px 0}

.improvement-tips{
  background:rgba(16,185,129,.1);
  padding:20px;
  border-radius:14px;
  margin-top:16px;
  border-right:4px solid var(--success);
  color:#d1fae5;
}

.improvement-tips h4{
  color:#6ee7b7;
  margin-bottom:12px;
  display:flex;
  align-items:center;
  gap:10px;
  font-size:1em;
}

.improvement-tips ul{padding-right:22px}
.improvement-tips li{margin-bottom:8px;line-height:1.7}

/* ===== التقدم ===== */
.progress-screen{padding:10px}

.progress-stats{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(220px,1fr));
  gap:18px;
  margin-bottom:26px;
}

.stat-card{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:18px;
  padding:22px;
  text-align:center;
  transition:var(--transition);
}

.stat-card:hover{transform:translateY(-4px);border-color:rgba(99,102,241,.4);box-shadow:0 15px 40px rgba(99,102,241,.2)}

.stat-card i{
  font-size:2.2em;
  background:linear-gradient(135deg,var(--primary),var(--cyan));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  margin-bottom:12px;
}

.stat-card h3{color:#94a3b8;margin-bottom:10px;font-size:.88em;font-weight:600}
.stat-card .stat-value{
  font-size:1.85em;
  font-weight:800;
  background:linear-gradient(135deg,var(--cyan),var(--primary));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
}

.progress-history{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:26px;
  overflow-x:auto;
}

.progress-history h3{color:#f1f5f9;margin-bottom:18px;display:flex;align-items:center;gap:10px;font-size:1.05em}

.history-table{width:100%;border-collapse:collapse;min-width:600px}

.history-table th,.history-table td{
  padding:14px;
  text-align:right;
  border-bottom:1px solid rgba(255,255,255,.06);
  font-size:.9em;
  color:#cbd5e1;
}

.history-table th{background:rgba(255,255,255,.03);color:#f1f5f9;font-weight:700}
.history-table tr:hover{background:rgba(99,102,241,.05)}
.history-table tr:last-child td{border-bottom:none}

.score-badge{padding:5px 12px;border-radius:20px;font-weight:700;color:#fff;font-size:.85em;display:inline-block}
.score-high{background:linear-gradient(135deg,var(--success),#059669)}
.score-medium{background:linear-gradient(135deg,var(--warning),#d97706)}
.score-low{background:linear-gradient(135deg,var(--danger),#dc2626)}

/* ===== المتصدرين ===== */
.leaderboard-screen{padding:10px}

.leaderboard-container{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:26px;
}

.leaderboard-container h3{
  color:#f1f5f9;
  margin-bottom:20px;
  display:flex;
  align-items:center;
  gap:10px;
  font-size:1.1em;
}

.leaderboard-container h3 i{
  color:#fbbf24;
  filter:drop-shadow(0 0 8px rgba(251,191,36,.6));
}

.leaderboard-list{margin-top:16px}

.leaderboard-item{
  display:flex;
  align-items:center;
  gap:16px;
  padding:14px 16px;
  border-radius:14px;
  margin-bottom:10px;
  background:rgba(255,255,255,.03);
  border:1px solid rgba(255,255,255,.05);
  transition:var(--transition);
}

.leaderboard-item:hover{
  transform:translateX(-4px);
  background:rgba(255,255,255,.07);
  border-color:rgba(99,102,241,.3);
}

.leaderboard-item.current-user{
  background:linear-gradient(135deg,rgba(99,102,241,.2),rgba(139,92,246,.2));
  border-color:rgba(99,102,241,.5);
  box-shadow:0 8px 25px rgba(99,102,241,.25);
}

.leaderboard-rank{
  width:42px;height:42px;
  border-radius:50%;
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  color:#fff;
  display:flex;
  align-items:center;
  justify-content:center;
  font-weight:800;
  font-size:.95em;
  flex-shrink:0;
  box-shadow:0 4px 14px rgba(99,102,241,.4);
}

.leaderboard-item:nth-child(1) .leaderboard-rank{background:linear-gradient(135deg,#fbbf24,#f59e0b)}
.leaderboard-item:nth-child(2) .leaderboard-rank{background:linear-gradient(135deg,#e5e7eb,#9ca3af)}
.leaderboard-item:nth-child(3) .leaderboard-rank{background:linear-gradient(135deg,#fb923c,#ea580c)}

.leaderboard-user{flex:1;min-width:0}
.leaderboard-user h4{color:#f1f5f9;font-size:.96em;margin-bottom:4px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.leaderboard-user p{color:#94a3b8;font-size:.8em}
.leaderboard-score{
  font-weight:800;
  font-size:1.05em;
  background:linear-gradient(135deg,var(--cyan),var(--primary));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  flex-shrink:0;
}

/* ===== الإشعارات ===== */
.notifications-container{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:22px;
  max-height:600px;
  overflow-y:auto;
}

.notification-item{
  padding:16px;
  border-bottom:1px solid rgba(255,255,255,.06);
  display:flex;
  align-items:flex-start;
  gap:14px;
  transition:var(--transition);
  cursor:pointer;
  border-radius:10px;
}

.notification-item:hover{background:rgba(255,255,255,.04)}
.notification-item:last-child{border-bottom:none}

.notification-icon{
  font-size:1.15em;
  color:var(--cyan);
  flex-shrink:0;
  width:34px;height:34px;
  border-radius:50%;
  background:rgba(34,211,238,.15);
  display:flex;
  align-items:center;
  justify-content:center;
}

.notification-content{flex:1;min-width:0}
.notification-title{font-weight:700;margin-bottom:5px;color:#f1f5f9;font-size:.92em}
.notification-message{color:#cbd5e1;font-size:.86em;line-height:1.6;margin-bottom:6px}
.notification-time{font-size:.75em;color:#64748b}

/* ===== الملف الشخصي ===== */
.profile-container{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:30px;
}

.profile-header{
  display:flex;
  align-items:center;
  gap:22px;
  margin-bottom:26px;
  flex-wrap:wrap;
}

.profile-avatar{
  width:100px;height:100px;
  border-radius:50%;
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:2.5em;
  color:#fff;
  flex-shrink:0;
  box-shadow:0 10px 30px rgba(99,102,241,.4);
}

.profile-info{flex:1;min-width:250px}
.profile-name{
  font-size:1.3em;
  font-weight:700;
  margin-bottom:10px;
  display:flex;
  align-items:center;
  gap:14px;
  flex-wrap:wrap;
}

.profile-info p{color:#94a3b8;font-size:.9em;margin-bottom:4px}

.profile-stats{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(180px,1fr));
  gap:16px;
  margin-top:26px;
}

.profile-stat{
  background:rgba(99,102,241,.08);
  padding:20px;
  border-radius:14px;
  text-align:center;
  border:1px solid rgba(99,102,241,.15);
  transition:var(--transition);
}

.profile-stat:hover{transform:translateY(-3px);border-color:rgba(99,102,241,.4)}
.profile-stat-value{
  font-size:1.8em;
  font-weight:800;
  background:linear-gradient(135deg,var(--cyan),var(--primary));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  margin-bottom:8px;
}
.profile-stat-label{color:#94a3b8;font-size:.82em}

/* شارات الرتب */
.rank-badge{
  display:inline-flex;
  align-items:center;
  padding:5px 14px;
  border-radius:20px;
  font-weight:700;
  font-size:.82em;
  color:#fff;
  white-space:nowrap;
}
.rank-inert{background:linear-gradient(135deg,#64748b,#475569)}
.rank-active{background:linear-gradient(135deg,#f59e0b,#d97706)}
.rank-good{background:linear-gradient(135deg,#10b981,#059669)}
.rank-genius{background:linear-gradient(135deg,#8b5cf6,#6366f1);animation:glowBadge 2s infinite alternate}

@keyframes glowBadge{
  from{box-shadow:0 0 8px rgba(139,92,246,.6)}
  to{box-shadow:0 0 22px rgba(99,102,241,.9)}
}

/* ===== المحاكاة ===== */
.simulation-container{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:20px;
  padding:26px;
  text-align:center;
}

.simulation-container h2{color:#f1f5f9;margin-bottom:10px;font-size:1.25em}
.simulation-container > p{color:#94a3b8;margin-bottom:20px;font-size:.92em}

.simulation-frame{
  width:100%;
  height:600px;
  border:none;
  border-radius:14px;
  background:#fff;
}

.simulation-info{margin-top:20px;text-align:right}
.simulation-info h3{color:#f1f5f9;margin-bottom:10px;font-size:1.05em;display:flex;align-items:center;gap:10px}
.simulation-info p{color:#94a3b8;line-height:1.8;font-size:.9em}

/* ============================================
   صفحة الكيمياء والانسان
   ============================================ */
.research-container{max-width:1100px;margin:0 auto;padding:10px}

.research-hero{
  background:linear-gradient(135deg,rgba(99,102,241,.25),rgba(139,92,246,.25));
  border:1px solid rgba(99,102,241,.3);
  border-radius:22px;
  padding:40px 30px;
  text-align:center;
  margin-bottom:26px;
  position:relative;
  overflow:hidden;
  backdrop-filter:blur(20px);
}

.research-hero::before{
  content:'';
  position:absolute;
  top:-50%;right:-50%;
  width:200%;height:200%;
  background:radial-gradient(circle,rgba(34,211,238,.15) 0%,transparent 60%);
  animation:rotateSlow 30s linear infinite;
  pointer-events:none;
}

@keyframes rotateSlow{to{transform:rotate(360deg)}}

.research-hero h1{
  font-size:1.95em;
  margin-bottom:14px;
  position:relative;
  z-index:1;
  background:linear-gradient(135deg,#fff,var(--cyan));
  -webkit-background-clip:text;
  background-clip:text;
  -webkit-text-fill-color:transparent;
  font-weight:800;
  line-height:1.4;
}

.research-hero > p{
  color:#cbd5e1;
  font-size:1em;
  max-width:680px;
  margin:0 auto;
  position:relative;
  z-index:1;
  line-height:1.75;
}

.research-elements-showcase{
  display:flex;
  justify-content:center;
  gap:12px;
  flex-wrap:wrap;
  margin-top:24px;
  position:relative;
  z-index:1;
}

.element-chip{
  background:rgba(255,255,255,.08);
  backdrop-filter:blur(10px);
  padding:10px 18px;
  border-radius:30px;
  font-weight:700;
  border:1px solid rgba(255,255,255,.15);
  display:flex;
  align-items:center;
  gap:8px;
  transition:var(--transition);
  color:#e2e8f0;
  font-size:.9em;
}

.element-chip:hover{
  background:rgba(99,102,241,.25);
  transform:translateY(-3px);
  border-color:rgba(99,102,241,.5);
}

.element-chip .symbol{font-size:1.15em;font-weight:900}

.research-toc{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:18px;
  padding:24px;
  margin-bottom:24px;
}

.research-toc h3{
  color:#f1f5f9;
  margin-bottom:18px;
  padding-bottom:12px;
  border-bottom:1px solid rgba(255,255,255,.08);
  display:flex;
  align-items:center;
  gap:10px;
  font-size:1.05em;
}

.research-toc h3 i{color:var(--cyan)}

.toc-grid{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(200px,1fr));
  gap:10px;
}

.toc-link{
  display:flex;
  align-items:center;
  gap:10px;
  padding:12px 14px;
  background:rgba(255,255,255,.04);
  border-radius:11px;
  color:#cbd5e1;
  text-decoration:none;
  transition:var(--transition);
  border-right:3px solid var(--primary);
  font-size:.88em;
  cursor:pointer;
}

.toc-link:hover{
  background:linear-gradient(135deg,rgba(99,102,241,.2),rgba(139,92,246,.2));
  transform:translateX(-4px);
  color:#fff;
  border-right-color:var(--cyan);
}

.toc-link i{color:var(--primary);font-size:1em;width:18px;text-align:center}
.toc-link:hover i{color:var(--cyan)}

.research-section{
  background:rgba(15,23,42,.65);
  backdrop-filter:blur(20px);
  border:1px solid var(--glass-border);
  border-radius:18px;
  padding:28px;
  margin-bottom:22px;
  scroll-margin-top:20px;
  transition:var(--transition);
}

.research-section:hover{border-color:rgba(99,102,241,.3);box-shadow:0 15px 40px rgba(99,102,241,.15)}

.section-header-element{
  display:flex;
  align-items:center;
  gap:18px;
  padding:22px;
  border-radius:16px;
  margin-bottom:22px;
  color:#fff;
  position:relative;
  overflow:hidden;
}

.section-header-element::before{
  content:'';
  position:absolute;
  top:0;left:0;
  width:100%;height:100%;
  background:radial-gradient(circle at 20% 50%,rgba(255,255,255,.15) 0%,transparent 60%);
  pointer-events:none;
}

.element-badge{
  width:78px;height:78px;
  border-radius:50%;
  background:rgba(255,255,255,.22);
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:2em;
  font-weight:900;
  backdrop-filter:blur(10px);
  border:3px solid rgba(255,255,255,.4);
  flex-shrink:0;
  box-shadow:0 8px 20px rgba(0,0,0,.3);
  position:relative;
  z-index:1;
}

.element-info-title{position:relative;z-index:1}
.element-info-title h2{font-size:1.35em;margin-bottom:6px;line-height:1.4;font-weight:800}
.element-info-title p{opacity:.9;font-size:.85em}

.element-fe{background:linear-gradient(135deg,#b91c1c,#7f1d1d)}
.element-zn{background:linear-gradient(135deg,#0891b2,#155e75)}
.element-mg{background:linear-gradient(135deg,#16a34a,#15803d)}
.element-i{background:linear-gradient(135deg,#7c3aed,#5b21b6)}
.element-se{background:linear-gradient(135deg,#ea580c,#c2410c)}
.element-intro{background:linear-gradient(135deg,#4f46e5,#7c3aed)}

.subsection{margin-bottom:24px;padding-bottom:20px;border-bottom:1px solid rgba(255,255,255,.06)}
.subsection:last-child{border-bottom:none;margin-bottom:0;padding-bottom:0}

.subsection h4{
  color:#a5b4fc;
  font-size:1.02em;
  margin-bottom:14px;
  display:flex;
  align-items:center;
  gap:10px;
  padding-right:12px;
  border-right:3px solid var(--primary);
  font-weight:700;
}

.subsection p{
  line-height:1.9;
  color:#cbd5e1;
  margin-bottom:12px;
  text-align:justify;
  font-size:.93em;
}

.subsection ul{padding-right:22px;margin-bottom:14px}
.subsection li{line-height:1.85;color:#cbd5e1;margin-bottom:8px;font-size:.92em}
.subsection strong{color:#f1f5f9;font-weight:700}

.formula-box{
  background:linear-gradient(135deg,#0f172a,#020617);
  color:#6ee7b7;
  padding:22px;
  border-radius:14px;
  text-align:center;
  font-family:'Courier New',monospace;
  font-size:1.15em;
  font-weight:bold;
  margin:16px 0;
  box-shadow:0 8px 24px rgba(0,0,0,.4);
  border:1px solid rgba(6,214,160,.35);
  direction:ltr;
  letter-spacing:1.5px;
  position:relative;
  overflow:hidden;
}

.formula-box::before{
  content:'';
  position:absolute;
  top:0;left:-100%;
  width:100%;height:100%;
  background:linear-gradient(90deg,transparent,rgba(6,214,160,.15),transparent);
  animation:shine 3s infinite;
}

@keyframes shine{
  0%{left:-100%}
  100%{left:100%}
}

.info-box{
  padding:16px 18px;
  border-radius:12px;
  margin:14px 0;
  border-right:4px solid;
  line-height:1.8;
  font-size:.92em;
}

.info-box h5{margin-bottom:10px;display:flex;align-items:center;gap:8px;font-size:.98em;font-weight:700}
.info-box p{color:#cbd5e1;font-size:.92em}

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

.comparison-table-wrapper{
  overflow-x:auto;
  border-radius:14px;
  border:1px solid rgba(255,255,255,.08);
  margin:18px 0;
}

.comparison-table{width:100%;border-collapse:collapse;min-width:750px}

.comparison-table th{
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  color:#fff;
  padding:14px 12px;
  text-align:right;
  font-weight:700;
  font-size:.9em;
}

.comparison-table td{
  padding:14px 12px;
  border-bottom:1px solid rgba(255,255,255,.06);
  text-align:right;
  line-height:1.7;
  color:#cbd5e1;
  font-size:.87em;
  background:rgba(255,255,255,.02);
}

.comparison-table tr:hover td{background:rgba(99,102,241,.08)}
.comparison-table tr:last-child td{border-bottom:none}

.comparison-table .element-cell{
  font-weight:700;
  color:#f1f5f9;
  display:flex;
  align-items:center;
  gap:8px;
}

.comparison-table .element-mini-badge{
  display:inline-flex;
  align-items:center;
  justify-content:center;
  width:30px;height:30px;
  border-radius:50%;
  color:#fff;
  font-weight:800;
  font-size:.82em;
  flex-shrink:0;
}

.references-list{list-style:none;padding:0;counter-reset:ref}

.references-list li{
  counter-increment:ref;
  padding:14px 18px 14px 60px;
  margin-bottom:12px;
  background:rgba(255,255,255,.03);
  border-radius:12px;
  position:relative;
  line-height:1.75;
  border-right:3px solid var(--primary);
  transition:var(--transition);
  font-size:.88em;
  direction:ltr;
  text-align:left;
  color:#cbd5e1;
}

.references-list li:hover{
  background:rgba(99,102,241,.08);
  transform:translateX(-4px);
}

.references-list li::before{
  content:counter(ref);
  position:absolute;
  left:15px;top:50%;
  transform:translateY(-50%);
  width:28px;height:28px;
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  color:#fff;
  border-radius:50%;
  display:flex;
  align-items:center;
  justify-content:center;
  font-weight:700;
  font-size:.8em;
}

.references-list a{color:#93c5fd;text-decoration:none;word-break:break-all;transition:var(--transition)}
.references-list a:hover{color:#60a5fa;text-decoration:underline}

.conclusion-box{
  background:linear-gradient(135deg,rgba(16,185,129,.12),rgba(6,214,160,.12));
  border-radius:16px;
  padding:26px;
  border-right:5px solid var(--success);
  line-height:1.9;
  color:#d1fae5;
  font-size:.93em;
}

.conclusion-box h3{
  color:#6ee7b7;
  margin-bottom:14px;
  display:flex;
  align-items:center;
  gap:10px;
  font-size:1.05em;
}

.back-to-top{
  position:fixed;
  bottom:24px;left:24px;
  width:46px;height:46px;
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  color:#fff;
  border:none;
  border-radius:50%;
  cursor:pointer;
  box-shadow:0 8px 24px rgba(99,102,241,.5);
  display:none;
  align-items:center;
  justify-content:center;
  font-size:1.05em;
  z-index:999;
  transition:var(--transition);
}

.back-to-top.visible{display:flex}
.back-to-top:hover{transform:translateY(-4px);box-shadow:0 12px 30px rgba(99,102,241,.7)}

/* مؤشر الرابط */
.url-indicator{
  position:fixed;
  bottom:16px;left:50%;
  transform:translateX(-50%);
  background:rgba(15,23,42,.85);
  backdrop-filter:blur(12px);
  color:var(--cyan);
  padding:8px 18px;
  border-radius:20px;
  font-size:.75em;
  font-family:'Courier New',monospace;
  z-index:9999;
  border:1px solid rgba(34,211,238,.3);
  cursor:pointer;
  display:none;
  max-width:80%;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  transition:var(--transition);
}

.url-indicator:hover{background:rgba(15,23,42,.95);color:#fff}
.url-indicator.visible{display:block}

/* استجابة */
@media(max-width:900px){
  .sidebar{width:100%;height:auto;position:relative;border-left:none;border-bottom:1px solid var(--glass-border)}
  .app-container.active{flex-direction:column}
  .main-content{height:auto;min-height:calc(100vh - 200px)}
  .nav-menu{display:flex;overflow-x:auto;gap:6px;padding:0 10px 10px}
  .nav-item{flex-shrink:0;margin-bottom:0}
  .nav-link{white-space:nowrap;padding:10px 14px}
  .quiz-options,.quiz-option{grid-template-columns:1fr}
  .quiz-option{width:100%}
  .quiz-container{flex-direction:column}
  .whiteboard-frame{height:400px}
  .research-hero h1{font-size:1.4em}
  .section-header-element{flex-direction:column;text-align:center}
  .element-badge{width:68px;height:68px;font-size:1.6em}
}

@media(max-width:500px){
  .header{padding:14px 18px}
  .header h2{font-size:1em}
  .btn{padding:9px 16px;font-size:.85em}
  .result-score{font-size:2.8em}
  .profile-avatar{width:80px;height:80px;font-size:2em}
  .welcome-screen h1{font-size:1.5em}
}
</style>
</head>
<body>

<canvas id="bg-canvas"></canvas>
<div class="bg-gradient-overlay"></div>

<!-- ============ شاشة الدخول ============ -->
<div class="login-screen active" id="login">
  <div class="login-container">
    <div class="login-icon"><i class="fas fa-atom"></i></div>
    <h1>منصة الكيمياء التفاعلية</h1>
    <p>سجل بياناتك للبدء في رحلة التعلم التفاعلية<br>سيتم توجيهك تلقائياً إلى المنصة بعد التسجيل</p>
    <form id="login-form">
      <div class="form-group">
        <label for="student-name"><i class="fas fa-user"></i> اسم الطالب</label>
        <input type="text" id="student-name" class="form-control" placeholder="اكتب اسمك الكامل" required>
      </div>
      <div class="form-group">
        <label for="student-section"><i class="fas fa-users"></i> الشعبة</label>
        <select id="student-section" class="form-control" required>
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
      <button type="submit" class="btn" style="width:100%;padding:14px;">
        <i class="fas fa-rocket"></i> دخول إلى المنصة
      </button>
    </form>
  </div>
</div>

<!-- ============ التطبيق ============ -->
<div class="app-container" id="main-app">
  <!-- الشريط الجانبي -->
  <aside class="sidebar">
    <div class="logo">
      <h1><i class="fas fa-atom"></i> كيمياء</h1>
    </div>

    <ul class="nav-menu">
      <li class="nav-item">
        <a href="#/welcome" class="nav-link active" data-screen="welcome">
          <i class="fas fa-home"></i><span>الرئيسية</span>
        </a>
      </li>
      <li class="nav-item">
        <a href="#/chemistry-human" class="nav-link" data-screen="chemistry-human">
          <i class="fas fa-dna"></i><span>الكيمياء والانسان</span>
        </a>
      </li>
      <li class="nav-item">
        <a href="#/quiz-setup" class="nav-link" data-screen="quiz-setup">
          <i class="fas fa-file-alt"></i><span>الاختبارات</span>
        </a>
      </li>
      <li class="nav-item">
        <a href="#/simulation" class="nav-link" data-screen="simulation">
          <i class="fas fa-flask"></i><span>المحاكاة</span>
        </a>
      </li>
      <li class="nav-item">
        <a href="#/progress" class="nav-link" data-screen="progress">
          <i class="fas fa-chart-line"></i><span>التقدم</span>
        </a>
      </li>
      <li class="nav-item">
        <a href="#/leaderboard" class="nav-link" data-screen="leaderboard">
          <i class="fas fa-trophy"></i><span>المتصدرين</span>
        </a>
      </li>
      <li class="nav-item">
        <a href="#/settings" class="nav-link" data-screen="settings">
          <i class="fas fa-cog"></i><span>الإعدادات</span>
        </a>
      </li>
    </ul>

    <div class="team-section">
      <h3><i class="fas fa-users"></i> فريق العمل</h3>
      <div class="team-member">
        <div class="team-avatar" style="background:linear-gradient(135deg,#fbbf24,#f59e0b);"><i class="fas fa-crown"></i></div>
        <div class="team-member-info"><div class="team-member-name">تميم الزهراني</div><div class="team-member-role">القائد والمبرمج</div></div>
      </div>
      <div class="team-member">
        <div class="team-avatar" style="background:linear-gradient(135deg,#e5e7eb,#9ca3af);"><i class="fas fa-user-tie"></i></div>
        <div class="team-member-info"><div class="team-member-name">عمار الغامدي</div><div class="team-member-role">المشرف</div></div>
      </div>
      <div class="team-member">
        <div class="team-avatar" style="background:linear-gradient(135deg,#fb923c,#ea580c);"><i class="fas fa-lightbulb"></i></div>
        <div class="team-member-info"><div class="team-member-name">خالد العتيبي</div><div class="team-member-role">الأفكار والبحث</div></div>
      </div>
      <div class="team-member">
        <div class="team-avatar" style="background:linear-gradient(135deg,#3b82f6,#2563eb);"><i class="fas fa-pen"></i></div>
        <div class="team-member-info"><div class="team-member-name">أحمد الصالح</div><div class="team-member-role">الكاتب</div></div>
      </div>
      <div class="team-member">
        <div class="team-avatar" style="background:linear-gradient(135deg,#6b7280,#4b5563);"><i class="fas fa-users"></i></div>
        <div class="team-member-info"><div class="team-member-name">سلطان المطيري</div><div class="team-member-role">الكاتب</div></div>
      </div>
      <div class="team-member">
        <div class="team-avatar" style="background:linear-gradient(135deg,#6b7280,#4b5563);"><i class="fas fa-users"></i></div>
        <div class="team-member-info"><div class="team-member-name">عبدالاله القحطاني</div><div class="team-member-role">الكاتب</div></div>
      </div>
      <div class="team-member">
        <div class="team-avatar" style="background:linear-gradient(135deg,#6b7280,#4b5563);"><i class="fas fa-users"></i></div>
        <div class="team-member-info"><div class="team-member-name">ناصر العقيل</div><div class="team-member-role">الكاتب</div></div>
      </div>
    </div>

    <div class="user-section">
      <div class="user-info">
        <div class="user-avatar"><i class="fas fa-user"></i></div>
        <div style="flex:1;min-width:0;">
          <h4 id="username-display">طالب كيمياء</h4>
          <div class="student-info">
            <p id="section-display">الشعبة: -</p>
            <p id="user-level">المستوى: مبتدئ</p>
          </div>
        </div>
      </div>
      <button class="logout-btn" id="logout-btn">
        <i class="fas fa-sign-out-alt"></i> تسجيل الخروج
      </button>
    </div>
  </aside>

  <!-- المحتوى -->
  <main class="main-content">

    <!-- الرئيسية -->
    <div class="screen active" id="welcome">
      <div class="header">
        <h2><i class="fas fa-home"></i> الرئيسية</h2>
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('notifications')"><i class="fas fa-bell"></i> الإشعارات</button>
          <button class="btn btn-outline" onclick="navigateTo('profile')"><i class="fas fa-user"></i> الملف الشخصي</button>
        </div>
      </div>

      <div class="welcome-screen">
        <div class="welcome-icon"><i class="fas fa-atom"></i></div>
        <h1>مرحباً بك في <span>منصة الكيمياء التفاعلية</span></h1>
        <p>منصة شاملة لتعلم الكيمياء من خلال الاختبارات التفاعلية، المحاكاة، والتجارب الافتراضية مع محتوى علمي متقدم</p>

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
            <p>اختبر معرفتك في الكيمياء من خلال اختبارات تفاعلية مع تصحيح فوري وتحليل مفصل للأداء</p>
            <button class="btn">ابدأ الاختبار</button>
          </div>

          <div class="card card-5" onclick="navigateTo('simulation')">
            <div class="card-header">
              <div class="card-icon"><i class="fas fa-flask"></i></div>
              <h3>المحاكاة والتجارب</h3>
            </div>
            <p>قم بتجربة التفاعلات الكيميائية افتراضياً في مختبر آمن مع شرح مفصل للنتائج</p>
            <button class="btn btn-cyan"><i class="fas fa-play"></i> ابدأ المحاكاة</button>
          </div>

          <div class="card card-3" onclick="navigateTo('progress')">
            <div class="card-header">
              <div class="card-icon"><i class="fas fa-chart-line"></i></div>
              <h3>تتبع التقدم</h3>
            </div>
            <p>راجع إحصائيات أدائك، تتبع تقدمك التعليمي، واحصل على توصيات مخصصة للتحسين</p>
            <button class="btn btn-warning"><i class="fas fa-chart-bar"></i> عرض التقارير</button>
          </div>

          <div class="card card-4" onclick="navigateTo('leaderboard')">
            <div class="card-header">
              <div class="card-icon"><i class="fas fa-trophy"></i></div>
              <h3>لوحة المتصدرين</h3>
            </div>
            <p>تنافس مع زملائك، احصل على نقاط، وارتقِ في لوحة المتصدرين لتصير الأفضل</p>
            <button class="btn btn-danger"><i class="fas fa-crown"></i> عرض المتصدرين</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ============ الكيمياء والانسان ============ -->
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
            <div class="element-chip"><span class="symbol">Fe</span> الحديد</div>
            <div class="element-chip"><span class="symbol">Zn</span> الزنك</div>
            <div class="element-chip"><span class="symbol">Mg</span> المغنيسيوم</div>
            <div class="element-chip"><span class="symbol">I</span> اليود</div>
            <div class="element-chip"><span class="symbol">Se</span> السيلينيوم</div>
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

            <div class="toc-grid" style="margin-top:15px;">
              <div class="toc-link" style="cursor:default;">
                <i class="fas fa-fire" style="color:#ef4444;"></i>
                <div>
                  <strong>عناصر التخليق الهرموني</strong>
                  <div style="font-size:.8em;opacity:.75;margin-top:3px;">اليود (I)</div>
                </div>
              </div>
              <div class="toc-link" style="cursor:default;">
                <i class="fas fa-shield-alt" style="color:#10b981;"></i>
                <div>
                  <strong>عناصر التثبيت البنيوي</strong>
                  <div style="font-size:.8em;opacity:.75;margin-top:3px;">الزنك (Zn) والمغنيسيوم (Mg)</div>
                </div>
              </div>
              <div class="toc-link" style="cursor:default;">
                <i class="fas fa-exchange-alt" style="color:#3b82f6;"></i>
                <div>
                  <strong>عناصر نقل الطاقة والإلكترون</strong>
                  <div style="font-size:.8em;opacity:.75;margin-top:3px;">الحديد (Fe) والسيلينيوم (Se)</div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- الحديد -->
        <div class="research-section" id="res-fe">
          <div class="section-header-element element-fe">
            <div class="element-badge">Fe</div>
            <div class="element-info-title">
              <h2>أولاً: الحديد (Fe) — كيمياء الأكسجين وانتقال الإلكترونات</h2>
              <p>العدد الذري: 26 | الوزن الذري: 55.84</p>
            </div>
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
            <div class="element-info-title">
              <h2>ثانياً: الزنك (Zn) — عنصر التثبيت البنيوي والتحفيز الإنزيمي</h2>
              <p>العدد الذري: 30 | عنصر فلزي انتقالي</p>
            </div>
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
            <div class="element-info-title">
              <h2>ثالثاً: المغنيسيوم (Mg) — عنصر نقل الطاقة والاستقرار الجيني</h2>
              <p>العدد الذري: 12 | الوزن الذري: 24.30</p>
            </div>
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
            <div class="element-info-title">
              <h2>رابعاً: اليود (I) — عنصر يدخل في بناء الهرمونات ببراعة تساهمية</h2>
              <p>العدد الذري: 53 | الوزن الذري: 126.90</p>
            </div>
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
            <div class="element-info-title">
              <h2>خامساً: السيلينيوم (Se) — درع مضاد الأكسدة الحيوي</h2>
              <p>العدد الذري: 34 | الوزن الذري: 78.96</p>
            </div>
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
            <div class="element-info-title">
              <h2>جدول المقارنة الشامل للخصائص والوظائف</h2>
              <p>ملخص الخصائص الكيميائية والوظائف الحيوية للعناصر الخمسة</p>
            </div>
          </div>

          <div class="comparison-table-wrapper">
            <table class="comparison-table">
              <thead>
                <tr>
                  <th>العنصر</th>
                  <th>الصورة الأيونية الحيوية</th>
                  <th>الخاصية الكيميائية المعززة</th>
                  <th>الوظيفة الجزئية الرئيسية</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#b91c1c,#7f1d1d);">Fe</span> الحديد</div></td>
                  <td>Fe²⁺ / Fe³⁺</td>
                  <td>التناوب بين حالات الأكسدة وتكوين الهيم</td>
                  <td>نقل O₂ عبر الهيموغلوبين ونقل الإلكترونات في السيتوكرومات</td>
                </tr>
                <tr>
                  <td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#0891b2,#155e75);">Zn</span> الزنك</div></td>
                  <td>Zn²⁺ (ثابتة)</td>
                  <td>روابط تناسقية رباعية مع Cys/His (حمض لويس)</td>
                  <td>تثبيت أشكال أصابع الزنك للـ DNA وتحفيز الأنهيدراز الكربوني</td>
                </tr>
                <tr>
                  <td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#16a34a,#15803d);">Mg</span> المغنيسيوم</div></td>
                  <td>Mg²⁺</td>
                  <td>الألفة العالية لشحنات الفوسفات الكثيفة</td>
                  <td>تكوين معقد [Mg-ATP]²⁻ وترتيب الـ DNA والريبوسومات</td>
                </tr>
                <tr>
                  <td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#7c3aed,#5b21b6);">I</span> اليود</div></td>
                  <td>I⁻ / يود عضوي</td>
                  <td>تكوين روابط تساهمية كربونية-يودية دائمة</td>
                  <td>بناء الهرمونات الدرقية (T₃ & T₄) للتحكم بالمستقلب الخلوي</td>
                </tr>
                <tr>
                  <td><div class="element-cell"><span class="element-mini-badge" style="background:linear-gradient(135deg,#ea580c,#c2410c);">Se</span> السيلينيوم</div></td>
                  <td>سيلينوسيستين</td>
                  <td>حمض أميني نادر ذو قدرة اختزال فائقة</td>
                  <td>الموقع النشط لإنزيم GPx لمكافحة التأكسد وإنزيمات Deiodinases</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- الخاتمة -->
        <div class="research-section" id="res-conclusion">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-flag-checkered"></i></div>
            <div class="element-info-title">
              <h2>الخاتمة والتوصيات العلمية</h2>
              <p>ملخص النتائج والتوصيات البحثية</p>
            </div>
          </div>

          <div class="conclusion-box">
            <h3><i class="fas fa-lightbulb"></i> الخاتمة</h3>
            <p>تكشف هذه الدراسة الكيميائية الحيوية الموسعة للعناصر الخمسة (الحديد، الزنك، المغنيسيوم، اليود، والسيلينيوم) عن حقيقة جوهرية: إن المظاهر الفسيولوجية المعقدة لجسم الإنسان تستند كلياً إلى سلوكيات تفاعلية دقيقة تحدث على المستوى الذري والإلكتروني.</p>
            <p>فالتوازنات الدقيقة بين هذه العناصر، وتوافق هندستها التناسقية مع الجزيئات الحيوية الكبرى، هي ما يضمن كفاءة تحويل الطاقة، حماية الخلايا من التلف التأكسدي، والتعبير الجيني المضبوط.</p>

            <h3 style="margin-top:20px;"><i class="fas fa-bullseye"></i> التوصيات</h3>
            <p>نوصي بالتوسع في أبحاث علم الكيمياء التناسقية الحيوية (Bioinorganic Chemistry) لفهم التدخلات المباشرة بين النقص الغذائي لهذه العناصر والاضطرابات الجزئية الناتجة عنها.</p>
          </div>
        </div>

        <!-- المراجع -->
        <div class="research-section" id="res-refs">
          <div class="section-header-element element-intro">
            <div class="element-badge"><i class="fas fa-book"></i></div>
            <div class="element-info-title">
              <h2>المراجع العلمية الأصلية</h2>
              <p>المصادر والدراسات المعتمدة في البحث</p>
            </div>
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
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button>
        </div>
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
          <div class="setting-info">
            <h4>عدد الأسئلة</h4>
            <p>اختر عدد الأسئلة في الاختبار</p>
          </div>
          <select id="question-count">
            <option value="10">10 أسئلة</option>
            <option value="20">20 أسئلة</option>
            <option value="30">30 أسئلة</option>
            <option value="50" selected>50 سؤالاً (كاملة)</option>
          </select>
        </div>
        <div class="setting-item">
          <div class="setting-info">
            <h4>مستوى الصعوبة</h4>
            <p>اختر مستوى الصعوبة المناسب</p>
          </div>
          <select id="difficulty">
            <option value="all">جميع المستويات</option>
            <option value="easy">سهل</option>
            <option value="medium" selected>متوسط</option>
            <option value="hard">صعب</option>
          </select>
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
          <div class="question" id="question" style="margin-top:20px;"></div>
          <div class="options" id="options"></div>
          <button class="btn" id="next-btn" disabled><i class="fas fa-arrow-left"></i> التالي</button>
          <div class="explanation" id="explanation"></div>
        </div>
        <div class="whiteboard-container">
          <div class="whiteboard-header">
            <h3><i class="fas fa-chalkboard"></i> السبورة التفاعلية</h3>
            <button class="btn btn-outline" id="clear-whiteboard" style="padding:8px 14px;font-size:.82em;"><i class="fas fa-eraser"></i> مسح</button>
          </div>
          <iframe src="https://www.canva.com/design/DAG37-MyN6M/31ae22xdkRDIFafdn72jBw/view?embed" 
                  class="whiteboard-frame" id="whiteboard" allowfullscreen></iframe>
          <div class="whiteboard-info">
            <p>استخدم السبورة لحل المسائل وكتابة الملاحظات أثناء الاختبار</p>
          </div>
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
        <div class="user-info">
          <p><i class="fas fa-user-graduate"></i> <strong>الاسم:</strong> <span id="result-username">طالب كيمياء</span></p>
          <p><i class="fas fa-clock"></i> <strong>الوقت المستغرق:</strong> <span id="time-taken">--:--</span></p>
          <p><i class="fas fa-chart-bar"></i> <strong>النتيجة:</strong> <span id="result-score">0</span> من <span id="total-questions">50</span></p>
          <p><i class="fas fa-medal"></i> <strong>رتبتك:</strong> <span id="user-rank">غاز خامل</span></p>
        </div>
        <h2><i class="fas fa-trophy"></i> نتيجة الاختبار</h2>
        <div class="result-score" id="final-score">0%</div>
        <div class="result-chart"><canvas id="result-chart" width="250" height="250"></canvas></div>
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
        <div class="quiz-controls" style="margin-top:26px;">
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
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button>
        </div>
      </div>
      <div class="progress-screen">
        <div class="progress-stats">
          <div class="stat-card"><i class="fas fa-chart-bar"></i><h3>متوسط النتائج</h3><div class="stat-value" id="average-score">0%</div></div>
          <div class="stat-card"><i class="fas fa-check-circle"></i><h3>أفضل نتيجة</h3><div class="stat-value" id="best-score">0%</div></div>
          <div class="stat-card"><i class="fas fa-list-ol"></i><h3>عدد الاختبارات</h3><div class="stat-value" id="total-quizzes">0</div></div>
          <div class="stat-card"><i class="fas fa-clock"></i><h3>إجمالي الوقت</h3><div class="stat-value" id="total-time">0 د</div></div>
        </div>
        <div class="progress-history">
          <h3><i class="fas fa-history"></i> تاريخ الاختبارات</h3>
          <table class="history-table">
            <thead>
              <tr><th>التاريخ</th><th>النوع</th><th>النتيجة</th><th>الوقت</th><th>الأسئلة</th></tr>
            </thead>
            <tbody id="quiz-history"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- المتصدرين -->
    <div class="screen" id="leaderboard">
      <div class="header">
        <h2><i class="fas fa-trophy"></i> لوحة المتصدرين</h2>
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button>
        </div>
      </div>
      <div class="leaderboard-screen">
        <div class="leaderboard-container">
          <h3><i class="fas fa-crown"></i> أعلى 10 متصدرين هذا الأسبوع</h3>
          <div class="leaderboard-list" id="leaderboard-list"></div>
        </div>
      </div>
    </div>

    <!-- الإعدادات -->
    <div class="screen" id="settings">
      <div class="header">
        <h2><i class="fas fa-cog"></i> الإعدادات</h2>
      </div>
      <div class="settings-container">
        <div class="settings-group">
          <h3><i class="fas fa-user-cog"></i> إعدادات الحساب</h3>
          <div class="setting-item">
            <div class="setting-info"><h4>الاسم</h4><p>اسم المستخدم المعروض</p></div>
            <input type="text" id="username" value="طالب كيمياء">
          </div>
          <div class="setting-item">
            <div class="setting-info"><h4>البريد الإلكتروني</h4><p>للاستلام الإشعارات</p></div>
            <input type="email" id="user-email" value="student@chemistry.com">
          </div>
          <div class="setting-item">
            <div class="setting-info"><h4>المستوى</h4><p>مستواك الحالي في الكيمياء</p></div>
            <select id="user-level-select">
              <option value="beginner">مبتدئ</option>
              <option value="intermediate">متوسط</option>
              <option value="advanced">متقدم</option>
            </select>
          </div>
        </div>

        <div class="settings-group">
          <h3><i class="fas fa-palette"></i> الإعدادات العامة</h3>
          <div class="setting-item">
            <div class="setting-info"><h4>الأصوات</h4><p>تشغيل أصوات التصحيح والإشعارات</p></div>
            <label class="toggle-switch"><input type="checkbox" id="sound-enabled" checked><span class="slider"></span></label>
          </div>
          <div class="setting-item">
            <div class="setting-info"><h4>الإشعارات</h4><p>تلقي إشعارات التحديثات</p></div>
            <label class="toggle-switch"><input type="checkbox" id="notifications" checked><span class="slider"></span></label>
          </div>
          <div class="setting-item">
            <div class="setting-info"><h4>عدد الأسئلة الافتراضي</h4><p>في كل اختبار</p></div>
            <input type="number" id="default-question-count" value="20" min="5" max="50">
          </div>
        </div>

        <div class="settings-group">
          <h3><i class="fas fa-download"></i> البيانات</h3>
          <div class="setting-item">
            <div class="setting-info"><h4>تصدير البيانات</h4><p>تحميل جميع نتائجك</p></div>
            <button class="btn btn-outline" id="export-data">تصدير</button>
          </div>
          <div class="setting-item">
            <div class="setting-info"><h4>مسح البيانات</h4><p>حذف كل شيء</p></div>
            <button class="btn btn-danger" id="clear-data">مسح</button>
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
        <div class="user-actions">
          <button class="btn btn-outline" onclick="navigateTo('welcome')"><i class="fas fa-arrow-right"></i> العودة</button>
        </div>
      </div>
      <div class="simulation-container">
        <h2><i class="fas fa-balance-scale"></i> محاكاة موازنة المعادلات الكيميائية</h2>
        <p>استخدم هذه المحاكاة التفاعلية لموازنة المعادلات الكيميائية واكتشاف مبادئ حفظ الكتلة</p>
        <iframe src="https://phet.colorado.edu/sims/html/balancing-chemical-equations/latest/balancing-chemical-equations_ar.html" 
                class="simulation-frame" allowfullscreen></iframe>
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
          <div class="profile-avatar"><i class="fas fa-user"></i></div>
          <div class="profile-info">
            <div class="profile-name">
              <input type="text" id="profile-username" value="طالب كيمياء" style="background:transparent;border:none;color:#f1f5f9;font-weight:700;font-size:1em;padding:6px 10px;border-radius:8px;">
              <span id="profile-rank" class="rank-badge rank-inert">غاز خامل</span>
            </div>
            <p id="profile-section">الشعبة: -</p>
            <p id="profile-level">المستوى: مبتدئ</p>
          </div>
        </div>
        <div class="profile-stats">
          <div class="profile-stat"><div class="profile-stat-value" id="profile-total-quizzes">0</div><div class="profile-stat-label">إجمالي الاختبارات</div></div>
          <div class="profile-stat"><div class="profile-stat-value" id="profile-average-score">0%</div><div class="profile-stat-label">متوسط النتائج</div></div>
          <div class="profile-stat"><div class="profile-stat-value" id="profile-best-score">0%</div><div class="profile-stat-label">أفضل نتيجة</div></div>
          <div class="profile-stat"><div class="profile-stat-value" id="profile-total-time">0 د</div><div class="profile-stat-label">إجمالي الوقت</div></div>
        </div>
        <div class="settings-group" style="margin-top:26px;">
          <h3><i class="fas fa-pen"></i> السيرة الذاتية</h3>
          <textarea id="profile-bio" class="form-control" rows="4" placeholder="اكتب عن نفسك..."></textarea>
        </div>
      </div>
    </div>

  </main>
</div>

<div class="url-indicator" id="url-indicator" title="اضغط لنسخ الرابط"></div>

<audio id="correct-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/257/257-preview.mp3" type="audio/mp3"></audio>
<audio id="fail-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/250/250-preview.mp3" type="audio/mp3"></audio>
<audio id="complete-sound" preload="auto"><source src="https://assets.mixkit.co/active_storage/sfx/255/255-preview.mp3" type="audio/mp3"></audio>

<script>
/* ============================================================
   🌌 الخلفية التفاعلية - الجزيئات والذرات
   ============================================================ */
(function(){
  const canvas = document.getElementById('bg-canvas');
  const ctx = canvas.getContext('2d');
  let W, H, particles = [];
  const isMobile = window.innerWidth < 768;
  const PARTICLE_COUNT = isMobile ? 40 : 80;
  const MAX_DIST = isMobile ? 100 : 140;

  function resize(){
    W = canvas.width = window.innerWidth;
    H = canvas.height = window.innerHeight;
  }
  resize();
  window.addEventListener('resize', resize);

  const colors = ['#6366f1','#8b5cf6','#06d6a0','#22d3ee','#ec4899'];

  function Particle(){
    this.x = Math.random() * W;
    this.y = Math.random() * H;
    this.vx = (Math.random() - 0.5) * 0.5;
    this.vy = (Math.random() - 0.5) * 0.5;
    this.r = Math.random() * 2 + 1;
    this.color = colors[Math.floor(Math.random() * colors.length)];
    this.pulse = Math.random() * Math.PI * 2;
  }

  Particle.prototype.update = function(){
    this.x += this.vx;
    this.y += this.vy;
    if(this.x < 0 || this.x > W) this.vx *= -1;
    if(this.y < 0 || this.y > H) this.vy *= -1;
    this.pulse += 0.03;
  };

  Particle.prototype.draw = function(){
    const rad = this.r + Math.sin(this.pulse) * 0.5;
    ctx.beginPath();
    ctx.arc(this.x, this.y, Math.max(0.5, rad), 0, Math.PI * 2);
    ctx.fillStyle = this.color;
    ctx.shadowBlur = 12;
    ctx.shadowColor = this.color;
    ctx.fill();
    ctx.shadowBlur = 0;
  };

  for(let i = 0; i < PARTICLE_COUNT; i++) particles.push(new Particle());

  // تفاعل مع الماوس
  const mouse = { x: -1000, y: -1000 };
  window.addEventListener('mousemove', e => { mouse.x = e.clientX; mouse.y = e.clientY; });
  window.addEventListener('mouseleave', () => { mouse.x = -1000; mouse.y = -1000; });

  function animate(){
    ctx.clearRect(0, 0, W, H);

    // رسم الخطوط
    for(let i = 0; i < particles.length; i++){
      const p1 = particles[i];
      for(let j = i + 1; j < particles.length; j++){
        const p2 = particles[j];
        const dx = p1.x - p2.x;
        const dy = p1.y - p2.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if(dist < MAX_DIST){
          const alpha = (1 - dist / MAX_DIST) * 0.35;
          ctx.beginPath();
          ctx.moveTo(p1.x, p1.y);
          ctx.lineTo(p2.x, p2.y);
          const grad = ctx.createLinearGradient(p1.x, p1.y, p2.x, p2.y);
          grad.addColorStop(0, p1.color);
          grad.addColorStop(1, p2.color);
          ctx.strokeStyle = grad;
          ctx.globalAlpha = alpha;
          ctx.lineWidth = 0.7;
          ctx.stroke();
          ctx.globalAlpha = 1;
        }
      }

      // خطوط مع الماوس
      const mdx = p1.x - mouse.x;
      const mdy = p1.y - mouse.y;
      const mdist = Math.sqrt(mdx * mdx + mdy * mdy);
      if(mdist < 180){
        const alpha = (1 - mdist / 180) * 0.6;
        ctx.beginPath();
        ctx.moveTo(p1.x, p1.y);
        ctx.lineTo(mouse.x, mouse.y);
        ctx.strokeStyle = p1.color;
        ctx.globalAlpha = alpha;
        ctx.lineWidth = 1;
        ctx.stroke();
        ctx.globalAlpha = 1;
      }
    }

    particles.forEach(p => { p.update(); p.draw(); });
    requestAnimationFrame(animate);
  }
  animate();
})();

/* ============================================================
   🧭 نظام التوجيه - Routing
   ============================================================ */
const VALID_SCREENS = ['login','welcome','chemistry-human','quiz-setup','quiz','results','progress','leaderboard','settings','simulation','notifications','profile'];

function getScreenFromHash(){
  let h = window.location.hash.replace(/^#\/?/, '');
  return VALID_SCREENS.includes(h) ? h : (h ? 'welcome' : 'login');
}

function navigateTo(screen, replace){
  if(!VALID_SCREENS.includes(screen)) screen = 'welcome';
  const hash = '#/' + screen;
  if(replace){
    history.replaceState(null, '', hash);
    handleRoute();
  } else {
    if(window.location.hash === hash) handleRoute();
    else window.location.hash = hash;
  }
}

function handleRoute(){
  const screen = getScreenFromHash();
  const user = getLoggedInUser();

  if(!user && screen !== 'login'){ navigateTo('login', true); return; }
  if(user && screen === 'login'){ navigateTo('welcome', true); return; }

  if(screen === 'login'){
    document.getElementById('login').classList.add('active');
    document.getElementById('main-app').classList.remove('active');
    document.body.style.background = '#050b1e';
  } else {
    document.getElementById('login').classList.remove('active');
    document.getElementById('main-app').classList.add('active');
    applyUserData(user);
    showScreenInternal(screen);
  }
  updateUrlIndicator();
}

function showScreenInternal(id){
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));

  const target = document.getElementById(id);
  if(target) target.classList.add('active');

  const link = document.querySelector(`.nav-link[data-screen="${id}"]`);
  if(link) link.classList.add('active');

  if(id === 'progress') updateProgress();
  if(id === 'leaderboard') updateLeaderboard();
  if(id === 'notifications') renderNotifications();
  if(id === 'profile') updateProfileScreen();
  if(id === 'chemistry-human') setupResearchLinks();

  const mc = document.querySelector('.main-content');
  if(mc) mc.scrollTop = 0;
}

function updateUrlIndicator(){
  const ind = document.getElementById('url-indicator');
  ind.textContent = '🔗 ' + window.location.href;
  ind.classList.add('visible');
}

document.getElementById('url-indicator').addEventListener('click', function(){
  navigator.clipboard.writeText(window.location.href).then(() => {
    const orig = this.textContent;
    this.textContent = '✅ تم نسخ الرابط!';
    setTimeout(() => this.textContent = orig, 1500);
  });
});

/* ============================================================
   👤 تسجيل الدخول
   ============================================================ */
function saveUser(name, section){
  localStorage.setItem('chemUser', JSON.stringify({ name, section, at: Date.now() }));
}
function getLoggedInUser(){
  try { return JSON.parse(localStorage.getItem('chemUser') || 'null'); }
  catch { return null; }
}
function applyUserData(user){
  if(!user) return;
  studentName = user.name;
  studentSection = user.section;
  document.getElementById('username-display').textContent = user.name;
  document.getElementById('section-display').textContent = 'الشعبة: ' + user.section;
  document.getElementById('result-username').textContent = user.name;
  document.getElementById('profile-section').textContent = 'الشعبة: ' + user.section;
}

document.getElementById('login-form').addEventListener('submit', function(e){
  e.preventDefault();
  const name = document.getElementById('student-name').value.trim();
  const section = document.getElementById('student-section').value;
  if(!name || !section){ alert('يرجى إدخال الاسم واختيار الشعبة'); return; }

  saveUser(name, section);
  loadSettings();
  addNotification('مرحباً بك!', `أهلاً ${name} في منصة الكيمياء التفاعلية. نتمنى لك تجربة ممتعة!`, 'info');
  navigateTo('welcome', true);
});

document.getElementById('logout-btn').addEventListener('click', function(){
  if(confirm('هل أنت متأكد من تسجيل الخروج؟')){
    localStorage.removeItem('chemUser');
    document.getElementById('student-name').value = '';
    document.getElementById('student-section').value = '';
    navigateTo('login', true);
  }
});

/* ============================================================
   📚 فهرس البحث - التنقل
   ============================================================ */
function setupResearchLinks(){
  document.querySelectorAll('.research-toc .toc-link[href^="#"]').forEach(link => {
    link.onclick = function(e){
      e.preventDefault();
      const id = this.getAttribute('href').substring(1);
      const el = document.getElementById(id);
      const mc = document.querySelector('.main-content');
      if(el && mc){
        mc.scrollTo({ top: el.offsetTop - 20, behavior: 'smooth' });
      }
    };
  });

  const bt = document.getElementById('back-to-top');
  if(bt){
    bt.onclick = () => {
      const mc = document.querySelector('.main-content');
      if(mc) mc.scrollTo({ top: 0, behavior: 'smooth' });
    };
  }
}

// زر العودة للأعلى
const mainContent = document.querySelector('.main-content');
mainContent.addEventListener('scroll', function(){
  const bt = document.getElementById('back-to-top');
  if(!bt) return;
  if(this.scrollTop > 400) bt.classList.add('visible');
  else bt.classList.remove('visible');
});

/* ============================================================
   ❓ بيانات الأسئلة
   ============================================================ */
const allQuestions = [
{q:"العملية التي يعاد فيها ترتيب الذرات في مادة أو أكثر لتكوين مواد جديدة تسمى:",o:["التغير الفيزيائي","التفاعل النووي","التفاعل الكيميائي"],c:2,e:"التعريف المباشر للتفاعل الكيميائي هو عملية يعاد فيها ترتيب الذرات لتكوين مواد جديدة، بينما التغير الفيزيائي لا يتضمن تكوين مواد جديدة",l:"التفاعلات والمعادلات",d:"easy"},
{q:"المعادلة التي توضح الجسيمات (الذرات أو الأيونات) المشاركة في التفاعل بشكل مفصل تسمى:",o:["المعادلة الكيميائية","المعادلة الأيونية","المعادلة الموزونة"],c:1,e:"المعادلة الأيونية هي التي تمثل المواد المتفاعلة والناتجة في المحاليل المائية على شكل أيونات",l:"التفاعلات والمعادلات",d:"medium"},
{q:"ما هو العدد الأقصى من الإلكترونات الذي يستوعبه مستوى الطاقة الرئيسي الرابع (n=4)?",o:["18","8","32"],c:2,e:"حسب قاعدة 2n²: 2 × (4)² = 32 إلكترون",l:"الترتيب الإلكتروني",d:"medium"},
{q:"أي من الرموز التالية يمثل الصيغة الأيونية الصحيحة لكلوريد الصوديوم (NaCl)?",o:["Na²⁺ Cl⁻","Na⁺ Cl⁻","Na²⁺ Cl²⁻"],c:1,e:"الصوديوم يفقد إلكترونًا ليصبح Na⁺، والكلور يكتسب إلكترونًا ليصبح Cl⁻",l:"التفاعلات والمعادلات",d:"easy"},
{q:"الأيون OH⁻ يسمى أيون:",o:["الكربوكسيل","الهيدروكسيد","الكربونات"],c:1,e:"الاسم الشائع لأيون الهيدروكسيد",l:"التفاعلات والمعادلات",d:"easy"},
{q:"التفاعل الذي تكون نواتجه مادة واحدة فقط يُصنف على أنه تفاعل:",o:["احتراق","إحلال بسيط","تكوين"],c:2,e:"يتحد متفاعلان أو أكثر لتكوين مادة واحدة",l:"التفاعلات والمعادلات",d:"easy"},
{q:"إذا انتهى التوزيع الإلكتروني بـ 3p⁶، فإن العدد الذري هو:",o:["10","12","18"],c:2,e:"1s² 2s² 2p⁶ 3s² 3p⁶ = 18 إلكترون",l:"الترتيب الإلكتروني",d:"medium"},
{q:"الرمز المستخدم للفصل بين المواد المتفاعلة والنواتج:",o:["+","→","(s)"],c:1,e:"السهم يشير لاتجاه التفاعل",l:"التفاعلات والمعادلات",d:"easy"},
{q:"تفاعل مادة مع الأكسجين ينتج ضوء أو حرارة هو تفاعل:",o:["تكوين","احتراق","إحلال مزدوج"],c:1,e:"التعريف المباشر لتفاعل الاحتراق",l:"التفاعلات والمعادلات",d:"easy"},
{q:"الصيغة الصحيحة لهيدروكسيد الألومنيوم:",o:["AlOH","Al(OH)₂","Al(OH)₃"],c:2,e:"شحنة Al 3+ وشحنة OH 1-",l:"التفاعلات والمعادلات",d:"medium"},
{q:"كم عدد المستويات الثانوية في n=3?",o:["2","3","4"],c:1,e:"s, p, d",l:"الترتيب الإلكتروني",d:"medium"},
{q:"أي توزيع إلكتروني صحيح للكلور (17)?",o:["1s² 2s² 2p⁶ 3s² 3p⁵","1s² 2s² 2p⁶ 3s² 3p⁶","1s² 2s² 2p⁶ 3s¹ 3p⁶"],c:0,e:"وفقاً لمبدأ البناء التصاعدي وقاعدة هوند",l:"الترتيب الإلكتروني",d:"hard"},
{q:"ما المستويات الثانوية التي يحتويها n=5?",o:["s, p","s, p, d","s, p, d, f"],c:2,e:"المستوى الخامس يحتوي على s, p, d, f, g",l:"الترتيب الإلكتروني",d:"hard"},
{q:"أي مما يلي ليس من أدلة حدوث التفاعل الكيميائي?",o:["تصاعد غاز","تغير اللون","تغير الشكل"],c:2,e:"تغير الشكل تغير فيزيائي",l:"التفاعلات والمعادلات",d:"easy"},
{q:"في N₂ + 3H₂ → 2NH₃، ما معامل H₂?",o:["1","2","3"],c:2,e:"3 جزئيات من H₂",l:"التفاعلات والمعادلات",d:"medium"},
{q:"عدد الأكسدة للأكسجين في معظم مركباته:",o:["+1","-1","-2"],c:2,e:"-2 في معظم الأكاسيد",l:"التفاعلات والمعادلات",d:"medium"},
{q:"التوزيع الإلكتروني الصحيح للبورون (5):",o:["1s² 2s³","1s² 2s² 2p¹","1s² 2p³"],c:1,e:"وفقاً لمبدأ البناء التصاعدي",l:"الترتيب الإلكتروني",d:"medium"},
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
   🎮 متغيرات التطبيق
   ============================================================ */
let studentName = '', studentSection = '';
let currentQuestions = [], currentQuestion = 0, score = 0, userAnswers = [], weakLessons = {};
let quizMode = 'practice', quizTime = 1800, timerInterval;
let soundEnabled = true;
let notifications = [];

const correctSound = document.getElementById('correct-sound');
const failSound = document.getElementById('fail-sound');
const completeSound = document.getElementById('complete-sound');

const defaultLeaderboard = [
{name:"أحمد محمد",score:98,section:"الشعبة 1"},
{name:"خالد إبراهيم",score:95,section:"الشعبة 3"},
{name:"يوسف حسن",score:92,section:"الشعبة 2"},
{name:"محمد عبدالرحمن",score:90,section:"الشعبة 4"},
{name:"عمر ناصر",score:88,section:"الشعبة 5"},
{name:"سعيد علي",score:85,section:"الشعبة 6"},
{name:"محمود خالد",score:82,section:"الشعبة 7"},
{name:"حسن سليمان",score:80,section:"الشعبة 1"},
{name:"فيصل عبدالله",score:78,section:"الشعبة 3"},
{name:"ناصر راشد",score:75,section:"الشعبة 2"}
];

/* ============================================================
   ⚙️ الإعدادات
   ============================================================ */
function loadSettings(){
  const s = JSON.parse(localStorage.getItem('chemSettings') || '{}');
  if(s.username) document.getElementById('username').value = s.username;
  if(s.email) document.getElementById('user-email').value = s.email;
  if(s.userLevel) document.getElementById('user-level-select').value = s.userLevel;
  if(s.soundEnabled !== undefined) document.getElementById('sound-enabled').checked = s.soundEnabled;
  if(s.notifications !== undefined) document.getElementById('notifications').checked = s.notifications;
  if(s.defaultQuestionCount) document.getElementById('default-question-count').value = s.defaultQuestionCount;
  soundEnabled = document.getElementById('sound-enabled').checked;
  
  const n = JSON.parse(localStorage.getItem('chemNotifications') || '[]');
  notifications = n;
}

document.getElementById('save-settings').addEventListener('click', function(){
  const s = {
    username: document.getElementById('username').value,
    email: document.getElementById('user-email').value,
    userLevel: document.getElementById('user-level-select').value,
    soundEnabled: document.getElementById('sound-enabled').checked,
    notifications: document.getElementById('notifications').checked,
    defaultQuestionCount: parseInt(document.getElementById('default-question-count').value)
  };
  localStorage.setItem('chemSettings', JSON.stringify(s));
  soundEnabled = s.soundEnabled;
  alert('✅ تم حفظ الإعدادات بنجاح!');
});

document.getElementById('sound-enabled').addEventListener('change', function(){ soundEnabled = this.checked; });

document.getElementById('export-data').addEventListener('click', function(){
  const data = {
    user: getLoggedInUser(),
    settings: JSON.parse(localStorage.getItem('chemSettings') || '{}'),
    results: JSON.parse(localStorage.getItem('chemResults') || '[]'),
    notifications
  };
  const blob = new Blob([JSON.stringify(data, null, 2)], {type: 'application/json'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'chemistry-data.json';
  a.click();
});

document.getElementById('clear-data').addEventListener('click', function(){
  if(confirm('هل أنت متأكد من مسح جميع البيانات؟ لا يمكن التراجع!')){
    localStorage.removeItem('chemSettings');
    localStorage.removeItem('chemResults');
    localStorage.removeItem('chemNotifications');
    notifications = [];
    alert('تم مسح البيانات');
  }
});

/* ============================================================
   📝 الاختبار
   ============================================================ */
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

  currentQuestions = shuffle(pool).slice(0, Math.min(count, pool.length));
  currentQuestion = 0; score = 0; userAnswers = []; weakLessons = {};

  if(quizMode === 'exam'){
    quizTime = 1800;
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

  if(quizMode === 'practice'){
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
  if(currentQuestion < currentQuestions.length - 1){
    currentQuestion++;
    showQuestion();
  } else endQuiz();
});

document.getElementById('end-quiz').addEventListener('click', function(){
  if(confirm('هل أنت متأكد من إنهاء الاختبار؟')) endQuiz();
});

function startTimer(){
  updateTimer();
  clearInterval(timerInterval);
  timerInterval = setInterval(() => {
    quizTime--;
    updateTimer();
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
  saveResults();
  const p = Math.round(score / currentQuestions.length * 100);
  addNotification('انتهى الاختبار!', `حصلت على ${p}%`, 'success');
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

  // دائرة النتيجة
  const canvas = document.getElementById('result-chart');
  const ctx = canvas.getContext('2d');
  ctx.clearRect(0, 0, 250, 250);
  ctx.beginPath();
  ctx.arc(125, 125, 100, 0, 2 * Math.PI);
  ctx.strokeStyle = 'rgba(255,255,255,.1)';
  ctx.lineWidth = 18;
  ctx.stroke();

  const color = p >= 70 ? '#10b981' : p >= 50 ? '#f59e0b' : '#ef4444';
  ctx.beginPath();
  ctx.arc(125, 125, 100, -0.5 * Math.PI, (2 * p / 100 - 0.5) * Math.PI);
  ctx.strokeStyle = color;
  ctx.lineWidth = 18;
  ctx.lineCap = 'round';
  ctx.stroke();

  ctx.font = 'bold 32px Arial';
  ctx.fillStyle = color;
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  ctx.fillText(p + '%', 125, 125);

  const randPct = Math.floor(Math.random() * 30) + 60;
  document.getElementById('comparison').innerHTML = `
    <h3><i class="fas fa-chart-line"></i> مقارنة مع الآخرين</h3>
    <p>${randPct}% من الطلاب حصلوا على نفس نتيجتك تقريباً</p>
  `;

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
      plugins: {
        legend: { labels: { color: '#e2e8f0', font: { family: 'Segoe UI' } } }
      },
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
    html = '<li>ركز على تحسين نقاط ضعفك المحددة</li><li>جرب وضع الامتحان لتحسين إدارة الوقت</li><li>راجع قسم الكيمياء والانسان لفهم التطبيقات</li><li>تنافس مع زملائك لتحفيز نفسك</li>';
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
  const arr = JSON.parse(localStorage.getItem('chemResults') || '[]');
  arr.unshift({
    date: new Date().toLocaleDateString('ar-SA'),
    time: new Date().toLocaleTimeString('ar-SA'),
    score: p,
    total: currentQuestions.length,
    correct: score,
    mode: quizMode,
    weak: weakLessons,
    studentName,
    studentSection
  });
  localStorage.setItem('chemResults', JSON.stringify(arr));
}

/* ============================================================
   📊 شاشة التقدم
   ============================================================ */
function updateProgress(){
  const arr = JSON.parse(localStorage.getItem('chemResults') || '[]');
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
   🏆 المتصدرين
   ============================================================ */
function updateLeaderboard(){
  const list = document.getElementById('leaderboard-list');
  list.innerHTML = '';
  const arr = JSON.parse(localStorage.getItem('chemResults') || '[]');
  let myBest = 0;
  if(arr.length) myBest = Math.max(...arr.map(r => r.score));

  const data = [...defaultLeaderboard.map(u => ({...u, isMe: false}))];
  if(myBest > 0) data.push({ name: studentName, score: myBest, section: studentSection, isMe: true });
  data.sort((a,b) => b.score - a.score);

  data.slice(0, 10).forEach((u, i) => {
    const el = document.createElement('div');
    el.className = 'leaderboard-item' + (u.isMe ? ' current-user' : '');
    el.innerHTML = `
      <div class="leaderboard-rank">${i + 1}</div>
      <div class="leaderboard-user">
        <h4>${u.name}${u.isMe ? ' (أنت)' : ''}</h4>
        <p>${u.section}</p>
      </div>
      <div class="leaderboard-score">${u.score}%</div>
    `;
    list.appendChild(el);
  });
}

/* ============================================================
   🔔 الإشعارات
   ============================================================ */
function addNotification(title, msg, type = 'info'){
  notifications.unshift({
    id: Date.now(),
    title, msg, type,
    time: new Date().toLocaleTimeString('ar-SA'),
    date: new Date().toLocaleDateString('ar-SA'),
    read: false
  });
  localStorage.setItem('chemNotifications', JSON.stringify(notifications));
  if(document.getElementById('notifications').classList.contains('active')) renderNotifications();
}

function renderNotifications(){
  const list = document.getElementById('notifications-list');
  if(!notifications.length){
    list.innerHTML = '<p style="text-align:center;padding:30px;color:#94a3b8;">لا توجد إشعارات</p>';
    return;
  }
  list.innerHTML = notifications.map(n => `
    <div class="notification-item">
      <div class="notification-icon"><i class="fas fa-${iconFor(n.type)}"></i></div>
      <div class="notification-content">
        <div class="notification-title">${n.title}</div>
        <div class="notification-message">${n.msg}</div>
        <div class="notification-time">${n.date} - ${n.time}</div>
      </div>
    </div>
  `).join('');
}

function iconFor(t){
  return t === 'success' ? 'check-circle' : t === 'warning' ? 'exclamation-triangle' : t === 'error' ? 'times-circle' : 'info-circle';
}

document.getElementById('clear-notifications').addEventListener('click', function(){
  if(confirm('مسح جميع الإشعارات؟')){
    notifications = [];
    localStorage.setItem('chemNotifications', JSON.stringify(notifications));
    renderNotifications();
  }
});

/* ============================================================
   👤 الملف الشخصي
   ============================================================ */
function updateProfileScreen(){
  const arr = JSON.parse(localStorage.getItem('chemResults') || '[]');
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
  document.getElementById('profile-username').value = studentName;
  document.getElementById('profile-section').textContent = 'الشعبة: ' + studentSection;
  const s = JSON.parse(localStorage.getItem('chemSettings') || '{}');
  if(s.bio) document.getElementById('profile-bio').value = s.bio;
}

document.getElementById('save-profile').addEventListener('click', function(){
  const newName = document.getElementById('profile-username').value.trim();
  const bio = document.getElementById('profile-bio').value;
  if(newName){
    studentName = newName;
    const u = getLoggedInUser();
    if(u){ u.name = newName; localStorage.setItem('chemUser', JSON.stringify(u)); }
    document.getElementById('username-display').textContent = newName;
    document.getElementById('result-username').textContent = newName;
  }
  const s = JSON.parse(localStorage.getItem('chemSettings') || '{}');
  s.bio = bio;
  localStorage.setItem('chemSettings', JSON.stringify(s));
  alert('✅ تم حفظ الملف الشخصي');
});

/* ============================================================
   🧹 زر السبورة
   ============================================================ */
document.getElementById('clear-whiteboard').addEventListener('click', function(){
  alert('استخدم أدوات Canva داخل السبورة للمسح أو الرسم');
});

/* ============================================================
   🚀 التهيئة
   ============================================================ */
window.addEventListener('hashchange', handleRoute);
window.addEventListener('load', () => {
  loadSettings();
  handleRoute();
});
</script>
</body>
</html>
