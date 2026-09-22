# Trevor-
Trevor从初学到精通的库

个人主页

源代码

<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="Trevor 的个人主页 · 液态玻璃风格 · 支持实时编辑">
<meta name="theme-color" content="#667eea">
<title>Trevor · 液态玻璃个人主页</title>
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'%3E%3Crect width='32' height='32' rx='8' fill='%23667eea'/%3E%3Ctext x='16' y='22' font-size='18' text-anchor='middle' fill='white' font-family='sans-serif' font-weight='bold'%3ET%3C/text%3E%3C/svg%3E">
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
:root {
  --bg1: #667eea; --bg2: #764ba2; --bg3: #5073e0;
  --glass: rgba(255,255,255,0.15);
  --glass-border: rgba(255,255,255,0.25);
  --glass-strong: rgba(255,255,255,0.22);
  --text: #ffffff; --text-dim: rgba(255,255,255,0.78);
  --shadow: 0 8px 32px rgba(31,38,135,0.25);
}
body.dark {
  --bg1: #1a1033; --bg2: #271b44; --bg3: #18244b;
  --glass: rgba(0,0,0,0.28); --glass-border: rgba(255,255,255,0.14);
  --glass-strong: rgba(255,255,255,0.18); --shadow: 0 8px 32px rgba(0,0,0,0.5);
}
html { scroll-behavior: smooth; }
body {
  min-height: 100vh; font-family: "PingFang SC","Microsoft YaHei",system-ui,sans-serif;
  color: var(--text);
  background: linear-gradient(135deg, var(--bg1) 0%, var(--bg2) 50%, var(--bg3) 100%);
  background-size: 200% 200%; animation: bgFlow 14s ease infinite;
  padding: 22px 16px 90px; overflow-x: hidden; transition: color .4s;
}
@keyframes bgFlow { 0%{background-position:0% 50%} 50%{background-position:100% 50%} 100%{background-position:0% 50%} }
.glow { position:fixed; width:340px; height:340px; border-radius:50%;
  background: radial-gradient(circle, rgba(255,255,255,0.18) 0%, transparent 70%);
  pointer-events:none; transform:translate(-50%,-50%); z-index:1; }
.wrap { position:relative; z-index:2; max-width:820px; margin:0 auto; }
.topbar { display:flex; justify-content:flex-end; gap:10px; margin-bottom:14px; flex-wrap:wrap; }
.tool-btn { padding:8px 15px; border-radius:999px; border:1px solid var(--glass-border);
  background:var(--glass); color:var(--text); cursor:pointer; backdrop-filter:blur(8px);
  font-size:13px; transition:all .3s; display:inline-flex; align-items:center; gap:6px; }
.tool-btn:hover { background:var(--glass-strong); transform:translateY(-2px); }
.tool-btn.accent { background:rgba(255,255,255,0.85); color:#333; font-weight:600; }
.tool-btn.accent:hover { background:#fff; }

/* ===== 顶部导航标签栏 ===== */
.nav { position:sticky; top:12px; z-index:50; display:flex; gap:4px; padding:8px;
  background:rgba(24,22,58,0.55); backdrop-filter:blur(16px); border:1px solid var(--glass-border);
  border-radius:18px; margin-bottom:20px; box-shadow:var(--shadow);
  overflow-x:auto; justify-content:center; scrollbar-width:none; }
.nav::-webkit-scrollbar { display:none; }
.nav-btn { padding:9px 16px; border-radius:999px; border:none; background:transparent;
  color:var(--text-dim); cursor:pointer; font-size:14px; white-space:nowrap;
  transition:all .25s; font-family:inherit; }
.nav-btn:hover { color:var(--text); background:var(--glass); }
.nav-btn.active { background:rgba(255,255,255,0.88); color:#2a2a4a; font-weight:700; }

/* ===== 页面切换 ===== */
.page { display:none; }
.page.active { display:block; animation:pageIn .4s ease; }
@keyframes pageIn { from{opacity:0; transform:translateY(16px)} to{opacity:1; transform:none} }

.glass { background:var(--glass); backdrop-filter:blur(18px); border:1px solid var(--glass-border);
  border-radius:24px; box-shadow:var(--shadow); padding:36px 30px; margin-bottom:20px; }
.hero { text-align:center; padding:40px 30px; }
.avatar { width:124px; height:124px; border-radius:50%; border:3px solid var(--glass-border);
  box-shadow:0 0 30px rgba(255,255,255,0.25); margin:0 auto 16px; background:#fff; overflow:hidden; }
.avatar svg { width:100%; height:100%; display:block; }
.name { font-size:34px; font-weight:800; margin-bottom:6px; text-shadow:0 2px 10px rgba(0,0,0,.2); }
.subtitle { font-size:16px; color:var(--text-dim); margin-bottom:14px; }
.clock { font-size:14px; color:var(--text-dim); font-variant-numeric:tabular-nums; margin-bottom:18px; }
.clock b { color:var(--text); font-size:19px; margin:0 4px; }
.intro { line-height:1.9; font-size:15px; color:var(--text-dim); max-width:600px; margin:0 auto; }
.intro p { margin-bottom:10px; }
.intro p:last-child { margin-bottom:0; }

/* 数据统计 */
.stats { display:grid; grid-template-columns:repeat(auto-fit,minmax(110px,1fr)); gap:14px; margin-top:22px; }
.stat { padding:16px 10px; background:var(--glass-strong); border:1px solid var(--glass-border);
  border-radius:16px; text-align:center; }
.stat .num { font-size:26px; font-weight:800; margin-bottom:4px; }
.stat .lbl { font-size:12px; color:var(--text-dim); }

/* 页面入口卡片 */
.entry-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(150px,1fr)); gap:12px; margin-top:22px; }
.entry { padding:18px 14px; background:var(--glass-strong); border:1px solid var(--glass-border);
  border-radius:16px; cursor:pointer; transition:all .3s; text-align:center; }
.entry:hover { background:var(--glass); transform:translateY(-4px); box-shadow:0 8px 20px rgba(0,0,0,.18); }
.entry .ic { font-size:22px; margin-bottom:8px; }
.entry .t { font-size:15px; font-weight:600; }

/* 关于我 - 时间轴 */
.about-text { line-height:1.9; font-size:15px; color:var(--text-dim); margin-bottom:24px; }
.about-text p { margin-bottom:10px; }
.timeline-title { font-size:16px; font-weight:700; margin-bottom:16px; }
.timeline { display:grid; grid-template-columns:110px 26px 1fr; gap:0; }
.tl-node { display:contents; }
.tl-time { padding:2px 12px 20px 0; font-size:14px; font-weight:600; color:var(--text);
  text-align:right; line-height:1.4; }
.tl-mid { display:flex; justify-content:center; position:relative; }
.tl-dot { width:12px; height:12px; margin-top:4px; border-radius:50%;
  background:#fff; box-shadow:0 0 0 4px rgba(255,255,255,.2); z-index:1; }
.tl-line { position:absolute; top:0; bottom:0; left:50%; width:2px;
  background:rgba(255,255,255,.2); transform:translateX(-50%); }
.tl-mid:last-child .tl-line { display:none; }
.tl-content { padding:0 0 20px 14px; }
.tl-content .t { font-size:15px; font-weight:600; margin-bottom:4px; }
.tl-content .d { font-size:13px; color:var(--text-dim); line-height:1.6; }

/* 技能 */
.skill { margin-bottom:16px; }
.skill-head { display:flex; justify-content:space-between; font-size:14px; margin-bottom:6px; color:var(--text-dim); }
.skill-head b { color:var(--text); }
.skill-note { font-size:12px; color:var(--text-dim); opacity:.7; margin-top:4px; line-height:1.5; }
.bar { height:8px; background:rgba(255,255,255,.12); border-radius:999px; overflow:hidden; }
.bar-fill { height:100%; width:0; background:linear-gradient(90deg,#fff,rgba(255,255,255,.6)); border-radius:999px; transition:width 1.2s cubic-bezier(.2,.8,.2,1); }
.tag-wrap { display:flex; flex-wrap:wrap; gap:10px; justify-co
：
