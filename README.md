
  ![Upgraded form with regime gate](media/IMG_0824.png)
 <p
# Π*‑Φ‑Λ‑Ω SYSTEM — v1.0 (Math Revealed)

**A Unified Model of Performance Stability, Lock‑In, and Post‑Render Dynamics**

**Author:** DEADyaMURDAhead / Caleb Joe Bastian
**Version:** 1.0
**Date:** April 24, 2026

---

### 1. Overview

The Π*‑Φ‑Λ‑Ω System proposes a unified conceptual framework describing how CPU and GPU workloads interact across stability zones, lock‑in behavior, and post‑render dynamics. It proposes a dimensionless, hardware-agnostic method for analyzing performance ceilings, efficiency scaling, collapse zones, internal resolution behavior, CPU/GPU flow balance, and tuning stability.

This system is intended for application across games, engines, and hardware generations.

---

### 2. Core Components

- **Π*** — Efficiency Index: dimensionless ratio of performance gain to voltage cost
- **Φ** — Flow: throughput of the CPU→GPU→Render pipeline
- **Λ** — Lock‑In Zone: the operating mode where Π*·Φ stabilizes
- **Ω** — Stability Envelope: the permissible region defined by resolution scaling and headroom

---

### 3. System Intent

The system provides a unified language, a dimensionless model, a predictive structure for internal resolution tuning, and a framework for identifying stability zones. It is not tied to any specific game or vendor.

---

### 4. Π* — Efficiency Index

Π* is the central metric.

**GPU form (proposed):**
Π*_gpu = (f_vram / f_vram,ref) × (Δf_core / Δf_core,ref) × (R_out / R_out,ref) / (ΔV / ΔV_ref)

**CPU form (proposed):**
Π*_cpu = (f_t / f_r) × (V_r / V_t) × (P_t / P_r) × (W_r / W_t)

Π* is dimensionless, increases in the Goldilocks zone, and saturates at the GPU ceiling.

*Note on voltage: Π* uses ΔV as a practical tuning proxy, not as a physical energy metric. True dynamic power scales with V² (P ≈ C·V²·f), so performance-per-watt would require multiplying Π* by (V_ref/V). ΔV is retained here because it is directly controllable in overclocking and correlates with stability headroom.*

---

### 5. Φ — Flow Dynamics
Φ = Flow_t / Flow_ref  (proposed)

High Φ = consistent pacing, minimal stalls. Low Φ = CPU stalls, GPU starvation. Φ determines how well Π* can be maintained.

---

### 6. Λ — Lock‑In Behavior
Λ = Π* × Φ  (proposed)

- Λ_CPU: CPU-bound, Π* suppressed
- Λ_GPU: GPU-bound, Π* stable
- Λ_Hybrid: transitional, unstable scaling

---

### 7. Ω — Stability Envelope

Ω is satisfied when all three hold:
1. Aspect: W_out / H_out ≈ W_int / H_int
2. Scale: 1.5 ≤ S ≤ 2.0 where S = W_out / W_int
3. Headroom: β = (W_int × H_int) / (W_out × H_out) ≤ 0.45

---

### 8. Goldilocks Zone

The range where Π* is maximized, Φ is smooth, Λ is GPU-dominant, and Ω conditions are met. Initial testing suggests this occurs near β ≈ 0.25–0.45.

---

### 9. Collapse Zones

Collapse occurs when Ω is violated: CPU thread saturation, excessive β, VRAM pressure, or scene spikes cause transition to Λ_CPU.

---

### 10. Internal Resolution Scaling

Independent third-party reviewer data (n=6 titles, 2021–2024) report FPS gains of 1.66× to 2.53× (mean 1.99×) at β ≈ 0.25–0.40, consistent with the proposed Ω envelope. These figures correspond to common temporal/spatial upscaling presets (commonly labeled Balanced to Performance).
FPS_tuned / FPS_ref ≈ 1.5–1.9×  at β ≈ 0.35–0.45

This corresponds to observed scaling at reduced internal resolution, with improved Π* and stabilized Ω.

---

### 11. CPU/GPU Flow Model

CPU → Φ → GPU → Render → Output

CPU determines Λ mode, Φ determines throughput, GPU determines Π*, Ω determines stability.

---

### 12. CPU Efficiency Mirror

The CPU follows the same progression. Its Λ-ridge occurs where ∂Π*/∂L = 0 where L = applied load — the point where thermal, voltage, and scheduler limits intersect. Beyond this, the system enters Ω-state: post-load equilibrium.

Optimal operation occurs sub-critically, just below the Λ-ridge, where Π* is maximized before voltage and thermal costs dominate. This corresponds to the untuned CPU state observed in initial testing.

---

### Glossary

**Π*** — Dimensionless efficiency index (see Section 4)
**Φ** — Flow ratio (Section 5)
**Λ** — Lock-in product Π*·Φ (Section 6)
**Ω** — Stability envelope defined by aspect, scale, and β ≤0.45
**β** — Resolution ratio (W_int·H_int)/(W_out·H_out)
**f_vram** — VRAM effective frequency
**Δf_core** — core clock delta from baseline
**R_out** — output resolution pixel count
**ΔV** — voltage delta
**f_t/f_r** — test/reference CPU frequency
**P_t/P_r** — power draw
**W_t/W_r** — work units completed
**Goldilocks Zone** — β range where Π* peaks and Ω holds
**Collapse Zone** — region where Ω fails

*Symbols chosen for mnemonic value within this framework, not to imply physical constants.*

---

### Appendix A — Example Behavior

**Configuration:** 3200×900 → 1600×448 (β=0.249, S=2.0, aspect ≈ matched)
*Note: 1600×448 is the internal render target acting as the control arm; it is not a perfect 50% scale, which is why Ω uses "≈" for aspect matching rather than strict equality.*

- At 0.5×: GPU load ↓, Π* ↑, Φ smooth, Ω stable (Run A: 192 FPS, 49% GPU, 46°C)
- At 1.0×: GPU saturates, Λ→GPU, Ω narrows (Runs B/C: ~150 FPS, 89% GPU)

---

### Appendix B — Tuning Methodology

1. Start native
2. Lower until Π* rises
3. Identify Ω-satisfying β
4. Confirm Φ
5. Validate Ω under load
6. Document Λ transitions
---

### Appendix C — Universality

Engine-agnostic, hardware-agnostic, resolution-agnostic. Describes behavior, not brand features.

*Limitations: Model validated on desktop DirectX 12/Vulkan titles only; does not account for frame generation latency or driver-level scheduling.*
<html lang="en"><head>
<!-- Playables SDK -->
<script>// Playables SDK v1.0.0
// Game lifecycle bridge: rAF-based game-ready detection + event communication
(function() {
  'use strict';

  // Idempotency: skip if already initialized (e.g., server-side injection
  // followed by client-side inject-javascript via the Bloks webview component).
  if (window.playablesSDK) return;

  var HANDLER_NAME = 'playablesGameEventHandler';
  var ANDROID_BRIDGE_NAME = '_MetaPlayablesBridge';
  var RAF_FRAME_THRESHOLD = 3;

  var gameReadySent = false;
  var firstInteractionSent = false;
  var errorSent = false;
  var frameCount = 0;
  var originalRAF = window.requestAnimationFrame;

  // --- Transport Layer ---

  function hasIOSBridge() {
    return !!(window.webkit &&
              window.webkit.messageHandlers &&
              window.webkit.messageHandlers[HANDLER_NAME]);
  }

  function hasAndroidBridge() {
    return !!(window[ANDROID_BRIDGE_NAME] &&
              typeof window[ANDROID_BRIDGE_NAME].postEvent === 'function');
  }

  function isInIframe() {
    return !!(window.parent && window.parent !== window);
  }

  function sendEvent(eventName, payload) {
    var message = {
      type: eventName,
      payload: payload || {},
      timestamp: Date.now()
    };

    if (hasIOSBridge()) {
      try {
        window.webkit.messageHandlers[HANDLER_NAME].postMessage(message);
      } catch (e) { /* ignore */ }
      return;
    }

    if (hasAndroidBridge()) {
    try {
      var p = payload || {};
      p.__secureToken = window.__fbAndroidBridgeAuthToken || '';
      window[ANDROID_BRIDGE_NAME].postEvent(
        eventName,
        JSON.stringify(p)
      );
    } catch (e) { /* ignore */ }
    return;
  }

    if (isInIframe()) {
      try {
        window.parent.postMessage(message, '*');
      } catch (e) { /* ignore */ }
      return;
    }
  }

  // --- rAF Game-Ready Detection ---

  function onFrame() {
    if (gameReadySent) return;

    frameCount++;
    if (frameCount >= RAF_FRAME_THRESHOLD) {
      gameReadySent = true;
      sendEvent('game_ready', {
        frame_count: frameCount,
        detected_at: Date.now()
      });
      return;
    }

    originalRAF.call(window, onFrame);
  }

  if (originalRAF) {
    window.requestAnimationFrame = function(callback) {
      if (!gameReadySent) {
        return originalRAF.call(window, function(timestamp) {
          frameCount++;
          if (frameCount >= RAF_FRAME_THRESHOLD && !gameReadySent) {
            gameReadySent = true;
            sendEvent('game_ready', {
              frame_count: frameCount,
              detected_at: Date.now()
            });
          }
          callback(timestamp);
        });
      }
      return originalRAF.call(window, callback);
    };
  }

  // --- First User Interaction Detection ---

  function setupFirstInteractionDetection() {
    var events = ['touchstart', 'mousedown', 'keydown'];

    function onFirstInteraction() {
      if (firstInteractionSent) return;
      firstInteractionSent = true;
      sendEvent('user_interaction_start', null);

      for (var i = 0; i < events.length; i++) {
        document.removeEventListener(events[i], onFirstInteraction, true);
      }
    }

    for (var i = 0; i < events.length; i++) {
      document.addEventListener(events[i], onFirstInteraction, true);
    }
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', setupFirstInteractionDetection);
  } else {
    setupFirstInteractionDetection();
  }

  // --- Auto Error Capture ---

  window.addEventListener('error', function(event) {
    if (errorSent) return;
    errorSent = true;
    sendEvent('error', {
      message: event.message || 'Unknown error',
      source: event.filename || '',
      lineno: event.lineno || 0,
      colno: event.colno || 0,
      auto_captured: true
    });
  });

  window.addEventListener('unhandledrejection', function(event) {
    if (errorSent) return;
    errorSent = true;
    var reason = event.reason;
    sendEvent('error', {
      message: (reason instanceof Error) ? reason.message : String(reason),
      type: 'unhandled_promise_rejection',
      auto_captured: true
    });
  });

  // --- Public API ---

  window.playablesSDK = {
    complete: function(score) {
      sendEvent('game_ended', {
        score: score,
        completed: true
      });
    },

    error: function(message) {
      if (errorSent) return;
      errorSent = true;
      sendEvent('error', {
        message: message || 'Unknown error',
        auto_captured: false
      });
    },

    sendEvent: function(eventName, payload) {
      if (!eventName || typeof eventName !== 'string') return;
      sendEvent(eventName, payload);
    }
  };

  // Kick off rAF detection in case no game code calls rAF immediately
  if (originalRAF) {
    originalRAF.call(window, onFrame);
  }
})();</script>
<script>window.Intl=window.Intl||{};Intl.t=function(s){return(Intl._locale&&Intl._locale[s])||s;};</script>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Π*‑Φ‑Λ‑Ω System — Formula Sheet</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&amp;family=JetBrains+Mono:wght@400;500&amp;display=swap" rel="stylesheet">
<script>
window.MathJax = {
  tex: { inlineMath: [['$', '$'], ['\\(','\\)']], displayMath: [['$$','$$'], ['\\[','\\]']] },
  svg: { fontCache: 'global' },
  chtml: { scale: 1 },
  options: { enableMenu: false }
};
</script>
<script id="MathJax-script" async="" src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { height: 100%; }
body {
  font-family: 'Inter', system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
  background: radial-gradient(1200px 800px at 80% -10%, #1e3a8a 0%, transparent 60%), 
              radial-gradient(1000px 600px at -10% 20%, #0f172a 0%, transparent 50%),
              #020617;
  color: #cbd5e1;
  padding: 24px;
  display: flex;
  justify-content: center;
  -webkit-font-smoothing: antialiased;
}
.page {
  width: 100%;
  max-width: 1050px;
  background: linear-gradient(180deg, rgba(255,255,255,0.04), rgba(255,255,255,0.01));
  border: 1px solid rgba(148,163,184,0.15);
  border-radius: 20px;
  box-shadow: 0 25px 80px rgba(0,0,0,0.6), inset 0 1px 0 rgba(255,255,255,0.05);
  padding: 28px 32px;
  backdrop-filter: blur(12px);
}
header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 20px;
  margin-bottom: 22px;
  border-bottom: 1px solid rgba(148,163,184,0.15);
  padding-bottom: 18px;
}
h1 {
  font-size: 26px;
  font-weight: 700;
  letter-spacing: -0.02em;
  color: #f1f5f9;
  line-height: 1.15;
}
h1 .pi-star {
  background: linear-gradient(90deg, #7dd3fc, #a78bfa);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
.subtitle {
  margin-top: 6px;
  color: #94a3b8;
  font-size: 14px;
  font-weight: 500;
}
.meta {
  text-align: right;
  font-size: 12px;
  color: #64748b;
  font-family: 'JetBrains Mono', monospace;
  line-height: 1.6;
  white-space: nowrap;
}
.meta strong { color: #7dd3fc; font-weight: 500; }

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 14px;
}
.section {
  background: rgba(2,6,23,0.7);
  border: 1px solid rgba(51,65,85,0.7);
  border-radius: 14px;
  padding: 14px 16px 12px;
  position: relative;
  overflow: hidden;
}
.section::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(56,189,248,0.08), transparent 40%);
  pointer-events: none;
}
.section h2 {
  display: flex;
  align-items: baseline;
  gap: 10px;
  font-size: 15px;
  font-weight: 600;
  color: #e2e8f0;
  margin-bottom: 8px;
}
.section h2 .num {
  font-size: 22px;
  font-weight: 700;
  color: #38bdf8;
  font-family: 'JetBrains Mono', monospace;
  line-height: 1;
  width: 24px;
}
.equation {
  background: rgba(0,0,0,0.35);
  border: 1px solid rgba(30,41,59,0.9);
  border-radius: 10px;
  padding: 10px 8px;
  margin-top: 4px;
  overflow-x: auto;
}
mjx-container { color: #e2e8f0 !important; font-size: 105% !important; }

.bottom {
  display: grid;
  grid-template-columns: 1.6fr 1fr;
  gap: 14px;
  margin-top: 14px;
}
.glossary, .example {
  background: rgba(2,6,23,0.7);
  border: 1px solid rgba(51,65,85,0.7);
  border-radius: 14px;
  padding: 14px 16px;
}
.glossary h3, .example h3 {
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: #7dd3fc;
  font-weight: 600;
  margin-bottom: 10px;
}
.glossary table {
  width: 100%;
  border-collapse: collapse;
  font-size: 12px;
}
.glossary td {
  padding: 5px 8px;
  border-bottom: 1px dashed rgba(51,65,85,0.6);
  vertical-align: top;
  line-height: 1.35;
}
.glossary tr:last-child td { border-bottom: none; }
.glossary td:first-child {
  font-family: 'JetBrains Mono', monospace;
  color: #f1f5f9;
  white-space: nowrap;
  width: 34%;
}
.glossary td:last-child { color: #94a3b8; }

.example {
  background: linear-gradient(135deg, rgba(14,165,233,0.12), rgba(139,92,246,0.12));
  border-color: rgba(56,189,248,0.35);
}
.example-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px 14px;
}
.example-item label {
  display: block;
  font-size: 11px;
  color: #7dd3fc;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: 2px;
  font-weight: 500;
}
.example-item strong {
  font-family: 'JetBrains Mono', monospace;
  font-size: 13px;
  color: #f8fafc;
  font-weight: 500;
}
.example .ratio {
  grid-column: span 2;
  background: rgba(0,0,0,0.25);
  border: 1px solid rgba(56,189,248,0.2);
  border-radius: 8px;
  padding: 8px 10px;
  margin-top: 2px;
  text-align: center;
}
.example .ratio strong { color: #a5f3fc; font-size: 14px; }

footer {
  margin-top: 16px;
  padding-top: 10px;
  border-top: 1px solid rgba(148,163,184,0.12);
  text-align: center;
  font-family: 'JetBrains Mono', monospace;
  font-size: 11px;
  color: #475569;
}

@media (max-width: 900px) {
  body { padding: 14px; }
  .page { padding: 20px; }
  header { flex-direction: column; }
  .meta { text-align: left; }
  .grid { grid-template-columns: 1fr 1fr; }
  .bottom { grid-template-columns: 1fr; }
}
@media (max-width: 640px) {
  .grid { grid-template-columns: 1fr; }
  h1 { font-size: 22px; }
}

@page { size: A4 portrait; margin: 10mm; }
@media print {
  body { background: white !important; padding: 0; }
  .page {
    max-width: none; width: auto;
    background: white !important;
    border: none; box-shadow: none; padding: 0;
    backdrop-filter: none;
  }
  body, h1, .section h2, .glossary td:first-child { color: #000 !important; }
  .subtitle, .meta, .glossary td:last-child, footer { color: #334155 !important; }
  .section, .glossary, .example { background: white !important; border-color: #cbd5e1 !important; }
  .section::before { display: none; }
  .equation { background: #f8fafc !important; border-color: #e2e8f0 !important; }
  mjx-container { color: #000 !important; }
  .example { background: #f1f5f9 !important; }
  .example-item label { color: #0369a1 !important; }
  .example-item strong, .example .ratio strong { color: #000 !important; }
  h1 .pi-star { color: #000 !important; background: none; -webkit-background-clip: unset; }
  header { border-color: #e2e8f0; }
}
</style>
<style type="text/css">.CtxtMenu_InfoClose {  top:.2em; right:.2em;}
.CtxtMenu_InfoContent {  overflow:auto; text-align:left; font-size:80%;  padding:.4em .6em; border:1px inset; margin:1em 0px;  max-height:20em; max-width:30em; background-color:#EEEEEE;  white-space:normal;}
.CtxtMenu_Info.CtxtMenu_MousePost {outline:none;}
.CtxtMenu_Info {  position:fixed; left:50%; width:auto; text-align:center;  border:3px outset; padding:1em 2em; background-color:#DDDDDD;  color:black;  cursor:default; font-family:message-box; font-size:120%;  font-style:normal; text-indent:0; text-transform:none;  line-height:normal; letter-spacing:normal; word-spacing:normal;  word-wrap:normal; white-space:nowrap; float:none; z-index:201;  border-radius: 15px;                     /* Opera 10.5 and IE9 */  -webkit-border-radius:15px;               /* Safari and Chrome */  -moz-border-radius:15px;                  /* Firefox */  -khtml-border-radius:15px;                /* Konqueror */  box-shadow:0px 10px 20px #808080;         /* Opera 10.5 and IE9 */  -webkit-box-shadow:0px 10px 20px #808080; /* Safari 3 & Chrome */  -moz-box-shadow:0px 10px 20px #808080;    /* Forefox 3.5 */  -khtml-box-shadow:0px 10px 20px #808080;  /* Konqueror */  filter:progid:DXImageTransform.Microsoft.dropshadow(OffX=2, OffY=2, Color="gray", Positive="true"); /* IE */}
</style><style type="text/css">.CtxtMenu_MenuClose {  position:absolute;  cursor:pointer;  display:inline-block;  border:2px solid #AAA;  border-radius:18px;  -webkit-border-radius: 18px;             /* Safari and Chrome */  -moz-border-radius: 18px;                /* Firefox */  -khtml-border-radius: 18px;              /* Konqueror */  font-family: "Courier New", Courier;  font-size:24px;  color:#F0F0F0}
.CtxtMenu_MenuClose span {  display:block; background-color:#AAA; border:1.5px solid;  border-radius:18px;  -webkit-border-radius: 18px;             /* Safari and Chrome */  -moz-border-radius: 18px;                /* Firefox */  -khtml-border-radius: 18px;              /* Konqueror */  line-height:0;  padding:8px 0 6px     /* may need to be browser-specific */}
.CtxtMenu_MenuClose:hover {  color:white!important;  border:2px solid #CCC!important}
.CtxtMenu_MenuClose:hover span {  background-color:#CCC!important}
.CtxtMenu_MenuClose:hover:focus {  outline:none}
</style><style type="text/css">.CtxtMenu_Menu {  position:absolute;  background-color:white;  color:black;  width:auto; padding:5px 0px;  border:1px solid #CCCCCC; margin:0; cursor:default;  font: menu; text-align:left; text-indent:0; text-transform:none;  line-height:normal; letter-spacing:normal; word-spacing:normal;  word-wrap:normal; white-space:nowrap; float:none; z-index:201;  border-radius: 5px;                     /* Opera 10.5 and IE9 */  -webkit-border-radius: 5px;             /* Safari and Chrome */  -moz-border-radius: 5px;                /* Firefox */  -khtml-border-radius: 5px;              /* Konqueror */  box-shadow:0px 10px 20px #808080;         /* Opera 10.5 and IE9 */  -webkit-box-shadow:0px 10px 20px #808080; /* Safari 3 & Chrome */  -moz-box-shadow:0px 10px 20px #808080;    /* Forefox 3.5 */  -khtml-box-shadow:0px 10px 20px #808080;  /* Konqueror */}
.CtxtMenu_MenuItem {  padding: 1px 2em;  background:transparent;}
.CtxtMenu_MenuArrow {  position:absolute; right:.5em; padding-top:.25em; color:#666666;  font-family: null; font-size: .75em}
.CtxtMenu_MenuActive .CtxtMenu_MenuArrow {color:white}
.CtxtMenu_MenuArrow.CtxtMenu_RTL {left:.5em; right:auto}
.CtxtMenu_MenuCheck {  position:absolute; left:.7em;  font-family: null}
.CtxtMenu_MenuCheck.CtxtMenu_RTL { right:.7em; left:auto }
.CtxtMenu_MenuRadioCheck {  position:absolute; left: .7em;}
.CtxtMenu_MenuRadioCheck.CtxtMenu_RTL {  right: .7em; left:auto}
.CtxtMenu_MenuInputBox {  padding-left: 1em; right:.5em; color:#666666;  font-family: null;}
.CtxtMenu_MenuInputBox.CtxtMenu_RTL {  left: .1em;}
.CtxtMenu_MenuComboBox {  left:.1em; padding-bottom:.5em;}
.CtxtMenu_MenuSlider {  left: .1em;}
.CtxtMenu_SliderValue {  position:absolute; right:.1em; padding-top:.25em; color:#333333;  font-size: .75em}
.CtxtMenu_SliderBar {  outline: none; background: #d3d3d3}
.CtxtMenu_MenuLabel {  padding: 1px 2em 3px 1.33em;  font-style:italic}
.CtxtMenu_MenuRule {  border-top: 1px solid #DDDDDD;  margin: 4px 3px;}
.CtxtMenu_MenuDisabled {  color:GrayText}
.CtxtMenu_MenuActive {  background-color: #606872;  color: white;}
.CtxtMenu_MenuDisabled:focus {  background-color: #E8E8E8}
.CtxtMenu_MenuLabel:focus {  background-color: #E8E8E8}
.CtxtMenu_ContextMenu:focus {  outline:none}
.CtxtMenu_ContextMenu .CtxtMenu_MenuItem:focus {  outline:none}
.CtxtMenu_SelectionMenu {  position:relative; float:left;  border-bottom: none; -webkit-box-shadow:none; -webkit-border-radius:0px; }
.CtxtMenu_SelectionItem {  padding-right: 1em;}
.CtxtMenu_Selection {  right: 40%; width:50%; }
.CtxtMenu_SelectionBox {  padding: 0em; max-height:20em; max-width: none;  background-color:#FFFFFF;}
.CtxtMenu_SelectionDivider {  clear: both; border-top: 2px solid #000000;}
.CtxtMenu_Menu .CtxtMenu_MenuClose {  top:-10px; left:-10px}
</style><style id="MJX-CHTML-styles">
mjx-container[jax="CHTML"] {
  line-height: 0;
}

mjx-container [space="1"] {
  margin-left: .111em;
}

mjx-container [space="2"] {
  margin-left: .167em;
}

mjx-container [space="3"] {
  margin-left: .222em;
}

mjx-container [space="4"] {
  margin-left: .278em;
}

mjx-container [space="5"] {
  margin-left: .333em;
}

mjx-container [rspace="1"] {
  margin-right: .111em;
}

mjx-container [rspace="2"] {
  margin-right: .167em;
}

mjx-container [rspace="3"] {
  margin-right: .222em;
}

mjx-container [rspace="4"] {
  margin-right: .278em;
}

mjx-container [rspace="5"] {
  margin-right: .333em;
}

mjx-container [size="s"] {
  font-size: 70.7%;
}

mjx-container [size="ss"] {
  font-size: 50%;
}

mjx-container [size="Tn"] {
  font-size: 60%;
}

mjx-container [size="sm"] {
  font-size: 85%;
}

mjx-container [size="lg"] {
  font-size: 120%;
}

mjx-container [size="Lg"] {
  font-size: 144%;
}

mjx-container [size="LG"] {
  font-size: 173%;
}

mjx-container [size="hg"] {
  font-size: 207%;
}

mjx-container [size="HG"] {
  font-size: 249%;
}

mjx-container [width="full"] {
  width: 100%;
}

mjx-box {
  display: inline-block;
}

mjx-block {
  display: block;
}

mjx-itable {
  display: inline-table;
}

mjx-row {
  display: table-row;
}

mjx-row > * {
  display: table-cell;
}

mjx-mtext {
  display: inline-block;
}

mjx-mstyle {
  display: inline-block;
}

mjx-merror {
  display: inline-block;
  color: red;
  background-color: yellow;
}

mjx-mphantom {
  visibility: hidden;
}

_::-webkit-full-page-media, _:future, :root mjx-container {
  will-change: opacity;
}

mjx-assistive-mml {
  position: absolute !important;
  top: 0px;
  left: 0px;
  clip: rect(1px, 1px, 1px, 1px);
  padding: 1px 0px 0px 0px !important;
  border: 0px !important;
  display: block !important;
  width: auto !important;
  overflow: hidden !important;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

mjx-assistive-mml[display="block"] {
  width: 100% !important;
}

mjx-math {
  display: inline-block;
  text-align: left;
  line-height: 0;
  text-indent: 0;
  font-style: normal;
  font-weight: normal;
  font-size: 100%;
  font-size-adjust: none;
  letter-spacing: normal;
  border-collapse: collapse;
  word-wrap: normal;
  word-spacing: normal;
  white-space: nowrap;
  direction: ltr;
  padding: 1px 0;
}

mjx-container[jax="CHTML"][display="true"] {
  display: block;
  text-align: center;
  margin: 1em 0;
}

mjx-container[jax="CHTML"][display="true"][width="full"] {
  display: flex;
}

mjx-container[jax="CHTML"][display="true"] mjx-math {
  padding: 0;
}

mjx-container[jax="CHTML"][justify="left"] {
  text-align: left;
}

mjx-container[jax="CHTML"][justify="right"] {
  text-align: right;
}

mjx-msub {
  display: inline-block;
  text-align: left;
}

mjx-mi {
  display: inline-block;
  text-align: left;
}

mjx-c {
  display: inline-block;
}

mjx-utext {
  display: inline-block;
  padding: .75em 0 .2em 0;
}

mjx-TeXAtom {
  display: inline-block;
  text-align: left;
}

mjx-mo {
  display: inline-block;
  text-align: left;
}

mjx-stretchy-h {
  display: inline-table;
  width: 100%;
}

mjx-stretchy-h > * {
  display: table-cell;
  width: 0;
}

mjx-stretchy-h > * > mjx-c {
  display: inline-block;
  transform: scalex(1.0000001);
}

mjx-stretchy-h > * > mjx-c::before {
  display: inline-block;
  width: initial;
}

mjx-stretchy-h > mjx-ext {
  /* IE */ overflow: hidden;
  /* others */ overflow: clip visible;
  width: 100%;
}

mjx-stretchy-h > mjx-ext > mjx-c::before {
  transform: scalex(500);
}

mjx-stretchy-h > mjx-ext > mjx-c {
  width: 0;
}

mjx-stretchy-h > mjx-beg > mjx-c {
  margin-right: -.1em;
}

mjx-stretchy-h > mjx-end > mjx-c {
  margin-left: -.1em;
}

mjx-stretchy-v {
  display: inline-block;
}

mjx-stretchy-v > * {
  display: block;
}

mjx-stretchy-v > mjx-beg {
  height: 0;
}

mjx-stretchy-v > mjx-end > mjx-c {
  display: block;
}

mjx-stretchy-v > * > mjx-c {
  transform: scaley(1.0000001);
  transform-origin: left center;
  overflow: hidden;
}

mjx-stretchy-v > mjx-ext {
  display: block;
  height: 100%;
  box-sizing: border-box;
  border: 0px solid transparent;
  /* IE */ overflow: hidden;
  /* others */ overflow: visible clip;
}

mjx-stretchy-v > mjx-ext > mjx-c::before {
  width: initial;
  box-sizing: border-box;
}

mjx-stretchy-v > mjx-ext > mjx-c {
  transform: scaleY(500) translateY(.075em);
  overflow: visible;
}

mjx-mark {
  display: inline-block;
  height: 0px;
}

mjx-mrow {
  display: inline-block;
  text-align: left;
}

mjx-mfrac {
  display: inline-block;
  text-align: left;
}

mjx-frac {
  display: inline-block;
  vertical-align: 0.17em;
  padding: 0 .22em;
}

mjx-frac[type="d"] {
  vertical-align: .04em;
}

mjx-frac[delims] {
  padding: 0 .1em;
}

mjx-frac[atop] {
  padding: 0 .12em;
}

mjx-frac[atop][delims] {
  padding: 0;
}

mjx-dtable {
  display: inline-table;
  width: 100%;
}

mjx-dtable > * {
  font-size: 2000%;
}

mjx-dbox {
  display: block;
  font-size: 5%;
}

mjx-num {
  display: block;
  text-align: center;
}

mjx-den {
  display: block;
  text-align: center;
}

mjx-mfrac[bevelled] > mjx-num {
  display: inline-block;
}

mjx-mfrac[bevelled] > mjx-den {
  display: inline-block;
}

mjx-den[align="right"], mjx-num[align="right"] {
  text-align: right;
}

mjx-den[align="left"], mjx-num[align="left"] {
  text-align: left;
}

mjx-nstrut {
  display: inline-block;
  height: .054em;
  width: 0;
  vertical-align: -.054em;
}

mjx-nstrut[type="d"] {
  height: .217em;
  vertical-align: -.217em;
}

mjx-dstrut {
  display: inline-block;
  height: .505em;
  width: 0;
}

mjx-dstrut[type="d"] {
  height: .726em;
}

mjx-line {
  display: block;
  box-sizing: border-box;
  min-height: 1px;
  height: .06em;
  border-top: .06em solid;
  margin: .06em -.1em;
  overflow: hidden;
}

mjx-line[type="d"] {
  margin: .18em -.1em;
}

mjx-mn {
  display: inline-block;
  text-align: left;
}

mjx-c::before {
  display: block;
  width: 0;
}

.MJX-TEX {
  font-family: MJXZERO, MJXTEX;
}

.TEX-B {
  font-family: MJXZERO, MJXTEX-B;
}

.TEX-I {
  font-family: MJXZERO, MJXTEX-I;
}

.TEX-MI {
  font-family: MJXZERO, MJXTEX-MI;
}

.TEX-BI {
  font-family: MJXZERO, MJXTEX-BI;
}

.TEX-S1 {
  font-family: MJXZERO, MJXTEX-S1;
}

.TEX-S2 {
  font-family: MJXZERO, MJXTEX-S2;
}

.TEX-S3 {
  font-family: MJXZERO, MJXTEX-S3;
}

.TEX-S4 {
  font-family: MJXZERO, MJXTEX-S4;
}

.TEX-A {
  font-family: MJXZERO, MJXTEX-A;
}

.TEX-C {
  font-family: MJXZERO, MJXTEX-C;
}

.TEX-CB {
  font-family: MJXZERO, MJXTEX-CB;
}

.TEX-FR {
  font-family: MJXZERO, MJXTEX-FR;
}

.TEX-FRB {
  font-family: MJXZERO, MJXTEX-FRB;
}

.TEX-SS {
  font-family: MJXZERO, MJXTEX-SS;
}

.TEX-SSB {
  font-family: MJXZERO, MJXTEX-SSB;
}

.TEX-SSI {
  font-family: MJXZERO, MJXTEX-SSI;
}

.TEX-SC {
  font-family: MJXZERO, MJXTEX-SC;
}

.TEX-T {
  font-family: MJXZERO, MJXTEX-T;
}

.TEX-V {
  font-family: MJXZERO, MJXTEX-V;
}

.TEX-VB {
  font-family: MJXZERO, MJXTEX-VB;
}

mjx-stretchy-v mjx-c, mjx-stretchy-h mjx-c {
  font-family: MJXZERO, MJXTEX-S1, MJXTEX-S4, MJXTEX, MJXTEX-A ! important;
}

@font-face /* 0 */ {
  font-family: MJXZERO;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Zero.woff") format("woff");
}

@font-face /* 1 */ {
  font-family: MJXTEX;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Main-Regular.woff") format("woff");
}

@font-face /* 2 */ {
  font-family: MJXTEX-B;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Main-Bold.woff") format("woff");
}

@font-face /* 3 */ {
  font-family: MJXTEX-I;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Math-Italic.woff") format("woff");
}

@font-face /* 4 */ {
  font-family: MJXTEX-MI;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Main-Italic.woff") format("woff");
}

@font-face /* 5 */ {
  font-family: MJXTEX-BI;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Math-BoldItalic.woff") format("woff");
}

@font-face /* 6 */ {
  font-family: MJXTEX-S1;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Size1-Regular.woff") format("woff");
}

@font-face /* 7 */ {
  font-family: MJXTEX-S2;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Size2-Regular.woff") format("woff");
}

@font-face /* 8 */ {
  font-family: MJXTEX-S3;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Size3-Regular.woff") format("woff");
}

@font-face /* 9 */ {
  font-family: MJXTEX-S4;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Size4-Regular.woff") format("woff");
}

@font-face /* 10 */ {
  font-family: MJXTEX-A;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_AMS-Regular.woff") format("woff");
}

@font-face /* 11 */ {
  font-family: MJXTEX-C;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Calligraphic-Regular.woff") format("woff");
}

@font-face /* 12 */ {
  font-family: MJXTEX-CB;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Calligraphic-Bold.woff") format("woff");
}

@font-face /* 13 */ {
  font-family: MJXTEX-FR;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Fraktur-Regular.woff") format("woff");
}

@font-face /* 14 */ {
  font-family: MJXTEX-FRB;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Fraktur-Bold.woff") format("woff");
}

@font-face /* 15 */ {
  font-family: MJXTEX-SS;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_SansSerif-Regular.woff") format("woff");
}

@font-face /* 16 */ {
  font-family: MJXTEX-SSB;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_SansSerif-Bold.woff") format("woff");
}

@font-face /* 17 */ {
  font-family: MJXTEX-SSI;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_SansSerif-Italic.woff") format("woff");
}

@font-face /* 18 */ {
  font-family: MJXTEX-SC;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Script-Regular.woff") format("woff");
}

@font-face /* 19 */ {
  font-family: MJXTEX-T;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Typewriter-Regular.woff") format("woff");
}

@font-face /* 20 */ {
  font-family: MJXTEX-V;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Vector-Regular.woff") format("woff");
}

@font-face /* 21 */ {
  font-family: MJXTEX-VB;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Vector-Bold.woff") format("woff");
}

mjx-c.mjx-c3A0::before {
  padding: 0.68em 0.75em 0 0;
  content: "\3A0";
}

mjx-c.mjx-c1D452.TEX-I::before {
  padding: 0.442em 0.466em 0.011em 0;
  content: "e";
}

mjx-c.mjx-c2C::before {
  padding: 0.121em 0.278em 0.194em 0;
  content: ",";
}

mjx-c.mjx-c1D454.TEX-I::before {
  padding: 0.442em 0.477em 0.205em 0;
  content: "g";
}

mjx-c.mjx-c1D45D.TEX-I::before {
  padding: 0.442em 0.503em 0.194em 0;
  content: "p";
}

mjx-c.mjx-c1D462.TEX-I::before {
  padding: 0.442em 0.572em 0.011em 0;
  content: "u";
}

mjx-c.mjx-c3D::before {
  padding: 0.583em 0.778em 0.082em 0;
  content: "=";
}

mjx-c.mjx-c28.TEX-S3::before {
  padding: 1.45em 0.736em 0.949em 0;
  content: "(";
}

mjx-c.mjx-c1D453.TEX-I::before {
  padding: 0.705em 0.55em 0.205em 0;
  content: "f";
}

mjx-c.mjx-c1D705.TEX-I::before {
  padding: 0.442em 0.576em 0.011em 0;
  content: "\3BA";
}

mjx-c.mjx-c1D461.TEX-I::before {
  padding: 0.626em 0.361em 0.011em 0;
  content: "t";
}

mjx-c.mjx-c1D45F.TEX-I::before {
  padding: 0.442em 0.451em 0.011em 0;
  content: "r";
}

mjx-c.mjx-c29.TEX-S3::before {
  padding: 1.45em 0.736em 0.949em 0;
  content: ")";
}

mjx-c.mjx-c22C5::before {
  padding: 0.31em 0.278em 0 0;
  content: "\22C5";
}

mjx-c.mjx-c1D708.TEX-I::before {
  padding: 0.442em 0.53em 0 0;
  content: "\3BD";
}

mjx-c.mjx-c1D449.TEX-I::before {
  padding: 0.683em 0.769em 0.022em 0;
  content: "V";
}

mjx-c.mjx-c1D439.TEX-I::before {
  padding: 0.68em 0.749em 0 0;
  content: "F";
}

mjx-c.mjx-c1D443.TEX-I::before {
  padding: 0.683em 0.751em 0 0;
  content: "P";
}

mjx-c.mjx-c1D446.TEX-I::before {
  padding: 0.705em 0.645em 0.022em 0;
  content: "S";
}

mjx-c.mjx-c1D43F.TEX-I::before {
  padding: 0.683em 0.681em 0 0;
  content: "L";
}

mjx-c.mjx-c1D450.TEX-I::before {
  padding: 0.442em 0.433em 0.011em 0;
  content: "c";
}

mjx-c.mjx-c1D712.TEX-I::before {
  padding: 0.442em 0.626em 0.204em 0;
  content: "\3C7";
}

mjx-c.mjx-c1D44A.TEX-I::before {
  padding: 0.683em 1.048em 0.022em 0;
  content: "W";
}

mjx-c.mjx-c3A6::before {
  padding: 0.683em 0.722em 0 0;
  content: "\3A6";
}

mjx-c.mjx-c1D460.TEX-I::before {
  padding: 0.442em 0.469em 0.01em 0;
  content: "s";
}

mjx-c.mjx-c3A9::before {
  padding: 0.704em 0.722em 0 0;
  content: "\3A9";
}

mjx-c.mjx-c39B::before {
  padding: 0.716em 0.694em 0 0;
  content: "\39B";
}

mjx-c.mjx-c2248::before {
  padding: 0.483em 0.778em 0 0;
  content: "\2248";
}

mjx-c.mjx-c31::before {
  padding: 0.666em 0.5em 0 0;
  content: "1";
}

mjx-c.mjx-c2E::before {
  padding: 0.12em 0.278em 0 0;
  content: ".";
}

mjx-c.mjx-c36::before {
  padding: 0.666em 0.5em 0.022em 0;
  content: "6";
}

mjx-c.mjx-c2F::before {
  padding: 0.75em 0.5em 0.25em 0;
  content: "/";
}
</style></head>
<body>
<div class="page">
  <header>
    <div>
      <h1><span class="pi-star">Π*‑Φ‑Λ‑Ω</span> System — Single-Page Formula Sheet</h1>
      <div class="subtitle">A Unified Model of Performance Stability, Lock-In, and Post-Render Dynamics</div>
    </div>
    <div class="meta">
      <div><strong>Version 1.0</strong> • April 2026</div>
      <div>Author: DEADyaMURDAhead</div>
    </div>
  </header>

  <main class="grid">
    <section class="section">
      <h2><span class="num">1</span>Efficiency Index — Πₑ (GPU)</h2>
      <div class="equation"><mjx-container class="MathJax" jax="CHTML" display="true" style="font-size: 123.5%; position: relative;"><mjx-math display="true" class="MJX-TEX" aria-hidden="true" style="margin-left: 0px; margin-right: 0px;"><mjx-msub><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A0"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D452 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D454 TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D45D TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D462 TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub><mjx-mo class="mjx-n" space="4"><mjx-c class="mjx-c3D"></mjx-c></mjx-mo><mjx-mrow space="4"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D705 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D705 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-mrow space="3"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D708 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D708 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-mrow space="3"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D449 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.186em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D449 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.186em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-mrow space="3"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-mrow><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D439 TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D446 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.032em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-mrow></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-mrow><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D439 TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D446 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.032em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-mrow></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-mrow space="3"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D43F TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D43F TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow></mjx-math><mjx-assistive-mml unselectable="on" display="block"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><msub><mi mathvariant="normal">Π</mi><mrow data-mjx-texclass="ORD"><mi>e</mi><mo>,</mo><mi>g</mi><mi>p</mi><mi>u</mi></mrow></msub><mo>=</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>κ</mi><mo>,</mo><mi>t</mi></mrow></msub><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>κ</mi><mo>,</mo><mi>r</mi></mrow></msub></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow><mo>⋅</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>ν</mi><mo>,</mo><mi>t</mi></mrow></msub><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>ν</mi><mo>,</mo><mi>r</mi></mrow></msub></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow><mo>⋅</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><msub><mi>V</mi><mi>r</mi></msub><msub><mi>V</mi><mi>t</mi></msub></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow><mo>⋅</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><mrow><mi>F</mi><mi>P</mi><msub><mi>S</mi><mi>t</mi></msub></mrow><mrow><mi>F</mi><mi>P</mi><msub><mi>S</mi><mi>r</mi></msub></mrow></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow><mo>⋅</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><msub><mi>L</mi><mi>r</mi></msub><msub><mi>L</mi><mi>t</mi></msub></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow></math></mjx-assistive-mml></mjx-container></div>
    </section>

    <section class="section">
      <h2><span class="num">2</span>Efficiency Index — Πₑ (CPU)</h2>
      <div class="equation"><mjx-container class="MathJax" jax="CHTML" display="true" style="font-size: 123.5%; position: relative;"><mjx-math display="true" class="MJX-TEX" aria-hidden="true" style="margin-left: 0px; margin-right: 0px;"><mjx-msub><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A0"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D452 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D450 TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D45D TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D462 TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub><mjx-mo class="mjx-n" space="4"><mjx-c class="mjx-c3D"></mjx-c></mjx-mo><mjx-mrow space="4"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D712 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D712 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-mrow space="3"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D449 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.186em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D449 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.186em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-mrow space="3"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.109em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.109em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-mrow space="3"><mjx-mo class="mjx-s3"><mjx-c class="mjx-c28 TEX-S3"></mjx-c></mjx-mo><mjx-mfrac><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D44A TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.104em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D44A TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.104em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac><mjx-mo class="mjx-s3"><mjx-c class="mjx-c29 TEX-S3"></mjx-c></mjx-mo></mjx-mrow></mjx-math><mjx-assistive-mml unselectable="on" display="block"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><msub><mi mathvariant="normal">Π</mi><mrow data-mjx-texclass="ORD"><mi>e</mi><mo>,</mo><mi>c</mi><mi>p</mi><mi>u</mi></mrow></msub><mo>=</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>χ</mi><mo>,</mo><mi>t</mi></mrow></msub><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>χ</mi><mo>,</mo><mi>r</mi></mrow></msub></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow><mo>⋅</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><msub><mi>V</mi><mi>r</mi></msub><msub><mi>V</mi><mi>t</mi></msub></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow><mo>⋅</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><msub><mi>P</mi><mi>t</mi></msub><msub><mi>P</mi><mi>r</mi></msub></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow><mo>⋅</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mfrac><msub><mi>W</mi><mi>r</mi></msub><msub><mi>W</mi><mi>t</mi></msub></mfrac><mo data-mjx-texclass="CLOSE">)</mo></mrow></math></mjx-assistive-mml></mjx-container></div>
    </section>

    <section class="section">
      <h2><span class="num">3</span>Stabilization Factor — Φₛ</h2>
      <div class="equation"><mjx-container class="MathJax" jax="CHTML" display="true" style="font-size: 123.5%; position: relative;"><mjx-math display="true" class="MJX-TEX" aria-hidden="true" style="margin-left: 0px; margin-right: 0px;"><mjx-msub><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A6"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D460 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-mo class="mjx-n" space="4"><mjx-c class="mjx-c3D"></mjx-c></mjx-mo><mjx-mfrac space="4"><mjx-frac type="d"><mjx-num><mjx-nstrut type="d"></mjx-nstrut><mjx-msub><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A9"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-num><mjx-dbox><mjx-dtable><mjx-line type="d"></mjx-line><mjx-row><mjx-den><mjx-dstrut type="d"></mjx-dstrut><mjx-msub><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A9"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-den></mjx-row></mjx-dtable></mjx-dbox></mjx-frac></mjx-mfrac></mjx-math><mjx-assistive-mml unselectable="on" display="block"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><msub><mi mathvariant="normal">Φ</mi><mi>s</mi></msub><mo>=</mo><mfrac><msub><mi mathvariant="normal">Ω</mi><mi>t</mi></msub><msub><mi mathvariant="normal">Ω</mi><mi>r</mi></msub></mfrac></math></mjx-assistive-mml></mjx-container></div>
    </section>

    <section class="section">
      <h2><span class="num">4</span>Lock-In Threshold — Λ</h2>
      <div class="equation"><mjx-container class="MathJax" jax="CHTML" display="true" style="font-size: 123.5%; position: relative;"><mjx-math display="true" class="MJX-TEX" aria-hidden="true" style="margin-left: 0px; margin-right: 0px;"><mjx-mi class="mjx-n"><mjx-c class="mjx-c39B"></mjx-c></mjx-mi><mjx-mo class="mjx-n" space="4"><mjx-c class="mjx-c3D"></mjx-c></mjx-mo><mjx-msub space="4"><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A0"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D452 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-msub space="3"><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A6"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D460 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="block"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><mi mathvariant="normal">Λ</mi><mo>=</mo><msub><mi mathvariant="normal">Π</mi><mi>e</mi></msub><mo>⋅</mo><msub><mi mathvariant="normal">Φ</mi><mi>s</mi></msub></math></mjx-assistive-mml></mjx-container></div>
    </section>

    <section class="section">
      <h2><span class="num">5</span>Sub-Critical Load Condition</h2>
      <div class="equation"><mjx-container class="MathJax" jax="CHTML" display="true" style="font-size: 123.5%; position: relative;"><mjx-math display="true" class="MJX-TEX" aria-hidden="true" style="margin-left: 0px; margin-right: 0px;"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D43F TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-mo class="mjx-n" space="4"><mjx-c class="mjx-c2248"></mjx-c></mjx-mo><mjx-msub space="4"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D43F TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D450 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="block"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><msub><mi>L</mi><mi>t</mi></msub><mo>≈</mo><msub><mi>L</mi><mi>c</mi></msub></math></mjx-assistive-mml></mjx-container></div>
    </section>

    <section class="section">
      <h2><span class="num">6</span>Goldilocks Regime Condition</h2>
      <div class="equation"><mjx-container class="MathJax" jax="CHTML" display="true" style="font-size: 123.5%; position: relative;"><mjx-math display="true" class="MJX-TEX" aria-hidden="true" style="margin-left: 0px; margin-right: 0px;"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D439 TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D446 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.032em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-mo class="mjx-n" space="4"><mjx-c class="mjx-c2248"></mjx-c></mjx-mo><mjx-mn class="mjx-n" space="4"><mjx-c class="mjx-c31"></mjx-c><mjx-c class="mjx-c2E"></mjx-c><mjx-c class="mjx-c36"></mjx-c></mjx-mn><mjx-mo class="mjx-n" space="3"><mjx-c class="mjx-c22C5"></mjx-c></mjx-mo><mjx-mi class="mjx-i" space="3"><mjx-c class="mjx-c1D439 TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D446 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.032em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="block"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><mi>F</mi><mi>P</mi><msub><mi>S</mi><mi>t</mi></msub><mo>≈</mo><mn>1.6</mn><mo>⋅</mo><mi>F</mi><mi>P</mi><msub><mi>S</mi><mi>r</mi></msub></math></mjx-assistive-mml></mjx-container></div>
    </section>
  </main>

  <div class="bottom">
    <section class="glossary">
      <h3>Variable Glossary</h3>
      <table>
        <tbody><tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D705 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>κ</mi><mo>,</mo><mi>t</mi></mrow></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned core frequency</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D705 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>κ</mi><mo>,</mo><mi>r</mi></mrow></msub></math></mjx-assistive-mml></mjx-container></td><td>reference core frequency</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D708 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>ν</mi><mo>,</mo><mi>t</mi></mrow></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned VRAM frequency</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D708 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>ν</mi><mo>,</mo><mi>r</mi></mrow></msub></math></mjx-assistive-mml></mjx-container></td><td>reference VRAM frequency</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D712 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>χ</mi><mo>,</mo><mi>t</mi></mrow></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned CPU frequency</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D453 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.06em;"><mjx-texatom size="s" texclass="ORD"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D712 TEX-I"></mjx-c></mjx-mi><mjx-mo class="mjx-n"><mjx-c class="mjx-c2C"></mjx-c></mjx-mo><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-texatom></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>f</mi><mrow data-mjx-texclass="ORD"><mi>χ</mi><mo>,</mo><mi>r</mi></mrow></msub></math></mjx-assistive-mml></mjx-container></td><td>reference CPU frequency</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D449 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.186em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-texatom texclass="ORD"><mjx-mo class="mjx-n"><mjx-c class="mjx-c2F"></mjx-c></mjx-mo></mjx-texatom><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D449 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.186em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>V</mi><mi>t</mi></msub><mrow data-mjx-texclass="ORD"><mo>/</mo></mrow><msub><mi>V</mi><mi>r</mi></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned / reference voltage</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D439 TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D446 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.032em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-texatom texclass="ORD"><mjx-mo class="mjx-n"><mjx-c class="mjx-c2F"></mjx-c></mjx-mo></mjx-texatom><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D439 TEX-I"></mjx-c></mjx-mi><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D446 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.032em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><mi>F</mi><mi>P</mi><msub><mi>S</mi><mi>t</mi></msub><mrow data-mjx-texclass="ORD"><mo>/</mo></mrow><mi>F</mi><mi>P</mi><msub><mi>S</mi><mi>r</mi></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned / reference FPS</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D43F TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-texatom texclass="ORD"><mjx-mo class="mjx-n"><mjx-c class="mjx-c2F"></mjx-c></mjx-mo></mjx-texatom><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D43F TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>L</mi><mi>t</mi></msub><mrow data-mjx-texclass="ORD"><mo>/</mo></mrow><msub><mi>L</mi><mi>r</mi></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned / reference load</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.109em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-texatom texclass="ORD"><mjx-mo class="mjx-n"><mjx-c class="mjx-c2F"></mjx-c></mjx-mo></mjx-texatom><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D443 TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.109em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>P</mi><mi>t</mi></msub><mrow data-mjx-texclass="ORD"><mo>/</mo></mrow><msub><mi>P</mi><mi>r</mi></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned / reference power</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D44A TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.104em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-texatom texclass="ORD"><mjx-mo class="mjx-n"><mjx-c class="mjx-c2F"></mjx-c></mjx-mo></mjx-texatom><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D44A TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em; margin-left: -0.104em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>W</mi><mi>t</mi></msub><mrow data-mjx-texclass="ORD"><mo>/</mo></mrow><msub><mi>W</mi><mi>r</mi></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned / reference work</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A9"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D461 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub><mjx-texatom texclass="ORD"><mjx-mo class="mjx-n"><mjx-c class="mjx-c2F"></mjx-c></mjx-mo></mjx-texatom><mjx-msub><mjx-mi class="mjx-n"><mjx-c class="mjx-c3A9"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D45F TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi mathvariant="normal">Ω</mi><mi>t</mi></msub><mrow data-mjx-texclass="ORD"><mo>/</mo></mrow><msub><mi mathvariant="normal">Ω</mi><mi>r</mi></msub></math></mjx-assistive-mml></mjx-container></td><td>tuned / reference stability envelope</td></tr>
        <tr><td><mjx-container class="MathJax" jax="CHTML" style="font-size: 124.4%; position: relative;"><mjx-math class="MJX-TEX" aria-hidden="true"><mjx-msub><mjx-mi class="mjx-i"><mjx-c class="mjx-c1D43F TEX-I"></mjx-c></mjx-mi><mjx-script style="vertical-align: -0.15em;"><mjx-mi class="mjx-i" size="s"><mjx-c class="mjx-c1D450 TEX-I"></mjx-c></mjx-mi></mjx-script></mjx-msub></mjx-math><mjx-assistive-mml unselectable="on" display="inline"><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>L</mi><mi>c</mi></msub></math></mjx-assistive-mml></mjx-container></td><td>critical load</td></tr>
      </tbody></table>
    </section>

    <aside class="example">
      <h3>Example — Black Ops 7 Benchmark</h3>
      <div class="example-grid">
        <div class="example-item"><label>Avg FPS</label><strong>180</strong></div>
        <div class="example-item"><label>Low-1% FPS</label><strong>118</strong></div>
        <div class="example-item ratio"><label>Ratio</label><strong>180 / 118 = 1.53 ≈ 1.6</strong></div>
        <div class="example-item"><label>CPU Bottleneck</label><strong>51%</strong></div>
        <div class="example-item"><label>GPU Bottleneck</label><strong>49%</strong></div>
        <div class="example-item" style="grid-column: span 2;"><label>GPU Temp</label><strong>47°C avg / 50°C peak</strong></div>
      </div>
    </aside>
  </div>

  <footer>github.com/DEADyaMURDAhead/pistar-stability-ceiling-model</footer>
</div>

</body></html>
