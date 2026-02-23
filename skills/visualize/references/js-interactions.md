# JS Interactions

All JavaScript for the visual explainer. Paste this into the `<script>` tag at the end of `<body>`.

Handles: Mermaid initialization, theme toggle with re-render, sidebar navigation with scroll-spy, and zoom/pan on every diagram card.

## Full Script

```javascript
// ── Mermaid Init ──
mermaid.initialize({
  startOnLoad: true,
  theme: 'neutral',
  flowchart: { useMaxWidth: true, htmlLabels: true, curve: 'basis' },
  themeVariables: {
    primaryColor: '#e8f0e0',
    primaryBorderColor: '#789a60',
    primaryTextColor: '#18181b',
    lineColor: '#a1a1aa',
    secondaryColor: '#f4f4f5',
    tertiaryColor: '#fafafa',
    fontSize: '13px'
  }
});

// ── Theme Toggle ──
function toggleTheme() {
  const html = document.documentElement;
  const current = html.getAttribute('data-theme');
  const next = current === 'dark' ? 'light' : 'dark';
  html.setAttribute('data-theme', next);
  localStorage.setItem('theme', next);

  // Re-render mermaid for dark mode
  document.querySelectorAll('.mermaid').forEach(el => {
    el.removeAttribute('data-processed');
  });
  mermaid.initialize({
    startOnLoad: false,
    theme: next === 'dark' ? 'dark' : 'neutral',
    flowchart: { useMaxWidth: true, htmlLabels: true, curve: 'basis' },
    themeVariables: next === 'dark' ? {
      primaryColor: '#2a3a20',
      primaryBorderColor: '#789a60',
      primaryTextColor: '#e4e4e7',
      lineColor: '#555',
      secondaryColor: '#1f1f1f',
      tertiaryColor: '#0f0f0f',
      fontSize: '13px'
    } : {
      primaryColor: '#e8f0e0',
      primaryBorderColor: '#789a60',
      primaryTextColor: '#18181b',
      lineColor: '#a1a1aa',
      secondaryColor: '#f4f4f5',
      tertiaryColor: '#fafafa',
      fontSize: '13px'
    }
  });
  mermaid.run();
}

// Persist theme
const saved = localStorage.getItem('theme');
if (saved === 'dark') {
  document.documentElement.setAttribute('data-theme', 'dark');
}

// ── Sidebar Navigation ──
document.querySelectorAll('.nav-link').forEach(link => {
  link.addEventListener('click', () => {
    const target = link.getAttribute('data-target');
    if (!target) return;

    const el = document.getElementById(target);
    if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' });

    document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
    link.classList.add('active');

    // Close mobile sidebar
    document.querySelector('.sidebar').classList.remove('open');
  });
});

// ── Scroll Spy ──
const sections = document.querySelectorAll('.section');
const navLinks = document.querySelectorAll('.nav-link[data-target]');

const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      navLinks.forEach(l => l.classList.remove('active'));
      const match = document.querySelector(`.nav-link[data-target="${entry.target.id}"]`);
      if (match) match.classList.add('active');
    }
  });
}, { rootMargin: '-20% 0px -70% 0px' });

sections.forEach(s => observer.observe(s));

// ── Zoom & Pan ──
function initZoom() {
  document.querySelectorAll('.diagram-card').forEach(card => {
    const header = card.querySelector('.card-header');
    const body = card.querySelector('.diagram-body');
    if (!header || !body) return;

    // Wrap diagram content in zoom-inner div
    const inner = document.createElement('div');
    inner.className = 'zoom-inner';
    while (body.firstChild) inner.appendChild(body.firstChild);
    body.appendChild(inner);

    // Inject controls into header
    const controls = document.createElement('div');
    controls.className = 'zoom-controls';
    controls.innerHTML = `
      <button class="zoom-btn" data-action="out" title="Zoom out">&#8722;</button>
      <span class="zoom-level">100%</span>
      <button class="zoom-btn" data-action="in" title="Zoom in">&#43;</button>
      <button class="zoom-btn" data-action="reset" title="Reset" style="margin-left:4px;font-size:11px;">&#8634;</button>
    `;
    header.appendChild(controls);

    const levelEl = controls.querySelector('.zoom-level');

    // State
    let scale = 1;
    let panX = 0, panY = 0;
    const MIN = 0.3, MAX = 3, STEP = 0.15;

    function applyTransform(smooth) {
      if (!smooth) inner.classList.add('no-transition');
      else inner.classList.remove('no-transition');
      inner.style.transform = `translate(${panX}px, ${panY}px) scale(${scale})`;
      levelEl.textContent = Math.round(scale * 100) + '%';
    }

    function zoom(delta, cx, cy) {
      const prev = scale;
      scale = Math.min(MAX, Math.max(MIN, scale + delta));
      if (scale === prev) return;

      // Zoom towards pointer position
      if (cx !== undefined && cy !== undefined) {
        const rect = body.getBoundingClientRect();
        const ox = cx - rect.left - rect.width / 2;
        const oy = cy - rect.top - rect.height / 2;
        const ratio = 1 - scale / prev;
        panX += (ox - panX) * ratio;
        panY += (oy - panY) * ratio;
      }

      applyTransform(true);
    }

    // Button controls
    controls.addEventListener('click', e => {
      const btn = e.target.closest('.zoom-btn');
      if (!btn) return;
      const action = btn.dataset.action;
      if (action === 'in') zoom(STEP);
      else if (action === 'out') zoom(-STEP);
      else if (action === 'reset') {
        scale = 1; panX = 0; panY = 0;
        applyTransform(true);
      }
    });

    // Ctrl/Cmd + scroll wheel zoom (or trackpad pinch)
    body.addEventListener('wheel', e => {
      if (!e.ctrlKey && !e.metaKey) return;
      e.preventDefault();
      const delta = e.deltaY > 0 ? -STEP : STEP;
      zoom(delta, e.clientX, e.clientY);
    }, { passive: false });

    // Plain scroll wheel zoom when already zoomed in
    body.addEventListener('wheel', e => {
      if (e.ctrlKey || e.metaKey) return;
      if (scale === 1 && !e.shiftKey) return;
      e.preventDefault();
      const delta = e.deltaY > 0 ? -STEP * 0.5 : STEP * 0.5;
      zoom(delta, e.clientX, e.clientY);
    }, { passive: false });

    // Drag to pan
    let dragging = false, startX, startY, startPanX, startPanY;

    body.addEventListener('mousedown', e => {
      if (e.button !== 0) return;
      dragging = true;
      startX = e.clientX;
      startY = e.clientY;
      startPanX = panX;
      startPanY = panY;
      body.classList.add('is-panning');
      e.preventDefault();
    });

    window.addEventListener('mousemove', e => {
      if (!dragging) return;
      panX = startPanX + (e.clientX - startX);
      panY = startPanY + (e.clientY - startY);
      applyTransform(false);
    });

    window.addEventListener('mouseup', () => {
      if (!dragging) return;
      dragging = false;
      body.classList.remove('is-panning');
    });

    // Double-click to reset
    body.addEventListener('dblclick', () => {
      scale = 1; panX = 0; panY = 0;
      applyTransform(true);
    });
  });
}

// Wait for mermaid to render, then init zoom
setTimeout(initZoom, 500);
```

## Zoom Interaction Summary

| Input | Action |
|-------|--------|
| `+` button | Zoom in 15% |
| `-` button | Zoom out 15% |
| Reset button | Return to 100%, reset pan |
| Ctrl/Cmd + scroll | Zoom toward cursor |
| Scroll wheel (when zoomed) | Zoom toward cursor (half-step) |
| Click + drag | Pan the diagram |
| Double-click | Reset to 100% |
