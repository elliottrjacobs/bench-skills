# CSS Patterns

Complete stylesheet for the visual explainer. Paste this into the `<style>` tag. Supports light and dark themes via `[data-theme="dark"]`.

## Full Stylesheet

```css
:root {
  --sage: #789a60;
  --sage-light: #e8f0e0;
  --sage-dark: #5a7a44;
  --bg: #fafafa;
  --card: #ffffff;
  --text: #18181b;
  --text-secondary: #71717a;
  --border: #e4e4e7;
  --muted: #f4f4f5;
  --code-bg: #f8f8f8;
  --sidebar-w: 280px;
  --radius: 12px;
}

[data-theme="dark"] {
  --bg: #0f0f0f;
  --card: #1a1a1a;
  --text: #e4e4e7;
  --text-secondary: #a1a1aa;
  --border: #2a2a2a;
  --muted: #1f1f1f;
  --code-bg: #1e1e1e;
  --sage-light: #2a3a20;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

/* ── Sidebar ── */
.sidebar {
  position: fixed;
  top: 0; left: 0;
  width: var(--sidebar-w);
  height: 100vh;
  background: var(--card);
  border-right: 1px solid var(--border);
  overflow-y: auto;
  z-index: 100;
  display: flex;
  flex-direction: column;
  transition: transform 0.3s ease;
}

.sidebar-header {
  padding: 24px 20px 16px;
  border-bottom: 1px solid var(--border);
  position: sticky;
  top: 0;
  background: var(--card);
  z-index: 1;
}

.sidebar-header h1 {
  font-size: 18px;
  font-weight: 700;
  color: var(--sage);
  letter-spacing: -0.3px;
}

.sidebar-header p {
  font-size: 12px;
  color: var(--text-secondary);
  margin-top: 4px;
}

.sidebar-nav { padding: 12px 0; flex: 1; }

.nav-section {
  padding: 6px 20px 4px;
  font-size: 10px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.8px;
  color: var(--text-secondary);
  margin-top: 12px;
}

.nav-section:first-child { margin-top: 0; }

.nav-link {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 7px 20px;
  font-size: 13px;
  color: var(--text-secondary);
  text-decoration: none;
  cursor: pointer;
  transition: all 0.15s;
  border-left: 3px solid transparent;
}

.nav-link:hover {
  background: var(--muted);
  color: var(--text);
}

.nav-link.active {
  color: var(--sage);
  background: var(--sage-light);
  border-left-color: var(--sage);
  font-weight: 500;
}

.nav-link .dot {
  width: 6px; height: 6px;
  border-radius: 50%;
  flex-shrink: 0;
}

.dot-red    { background: #ef4444; }
.dot-amber  { background: #f59e0b; }
.dot-green  { background: #22c55e; }
.dot-purple { background: #8b5cf6; }
.dot-blue   { background: #3b82f6; }
.dot-pink   { background: #ec4899; }
.dot-teal   { background: #14b8a6; }

.sidebar-footer {
  padding: 12px 20px;
  border-top: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.theme-toggle {
  background: var(--muted);
  border: 1px solid var(--border);
  color: var(--text);
  border-radius: 8px;
  padding: 6px 12px;
  font-size: 12px;
  cursor: pointer;
  transition: all 0.15s;
}

.theme-toggle:hover { border-color: var(--sage); }

.stats-badge {
  font-size: 11px;
  color: var(--text-secondary);
}

/* ── Main Content ── */
.main {
  margin-left: var(--sidebar-w);
  padding: 40px 48px 80px;
  max-width: 1100px;
}

.hero {
  margin-bottom: 48px;
  padding-bottom: 32px;
  border-bottom: 1px solid var(--border);
}

.hero h1 {
  font-size: 32px;
  font-weight: 800;
  letter-spacing: -0.8px;
  margin-bottom: 8px;
}

.hero p {
  color: var(--text-secondary);
  font-size: 15px;
  max-width: 600px;
}

.meta-row {
  display: flex;
  gap: 16px;
  margin-top: 16px;
  flex-wrap: wrap;
}

.meta-pill {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 500;
  background: var(--muted);
  color: var(--text-secondary);
  border: 1px solid var(--border);
}

.meta-pill .val {
  color: var(--text);
  font-weight: 600;
}

/* ── Sections ── */
.section {
  margin-bottom: 56px;
  scroll-margin-top: 24px;
}

.section-label {
  font-size: 10px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: var(--sage);
  margin-bottom: 6px;
}

.section h2 {
  font-size: 24px;
  font-weight: 700;
  letter-spacing: -0.4px;
  margin-bottom: 6px;
}

.section > p {
  color: var(--text-secondary);
  font-size: 14px;
  margin-bottom: 20px;
  max-width: 660px;
}

/* ── Diagram Cards ── */
.diagram-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  margin-bottom: 24px;
}

.diagram-card .card-header {
  padding: 16px 20px;
  border-bottom: 1px solid var(--border);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.diagram-card .card-header h3 {
  font-size: 14px;
  font-weight: 600;
}

.card-badge {
  font-size: 11px;
  padding: 2px 8px;
  border-radius: 6px;
  font-weight: 500;
  background: var(--sage-light);
  color: var(--sage-dark);
}

.diagram-body {
  padding: 24px;
  overflow: hidden;
  position: relative;
  cursor: grab;
  min-height: 200px;
}

.diagram-body.is-panning { cursor: grabbing; }

.diagram-body .zoom-inner {
  transform-origin: center center;
  transition: transform 0.15s ease;
  display: flex;
  justify-content: center;
  width: 100%;
}

.diagram-body .zoom-inner.no-transition { transition: none; }

.diagram-body .mermaid {
  width: 100%;
  max-width: 100%;
}

/* ── Zoom Controls (injected by JS) ── */
.zoom-controls {
  display: flex;
  align-items: center;
  gap: 2px;
}

.zoom-btn {
  width: 28px; height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--muted);
  border: 1px solid var(--border);
  border-radius: 6px;
  color: var(--text-secondary);
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.12s;
  line-height: 1;
  font-family: system-ui, sans-serif;
  user-select: none;
}

.zoom-btn:hover {
  background: var(--sage-light);
  border-color: var(--sage);
  color: var(--sage-dark);
}

.zoom-btn:active { transform: scale(0.92); }

.zoom-level {
  font-size: 11px;
  font-weight: 600;
  color: var(--text-secondary);
  min-width: 38px;
  text-align: center;
  font-variant-numeric: tabular-nums;
  user-select: none;
}

/* ── Info Grids ── */
.info-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 12px;
  margin-bottom: 24px;
}

.info-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 16px;
}

.info-card .label {
  font-size: 11px;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  font-weight: 600;
  margin-bottom: 4px;
}

.info-card .value {
  font-size: 22px;
  font-weight: 700;
  color: var(--sage);
}

.info-card .desc {
  font-size: 12px;
  color: var(--text-secondary);
  margin-top: 2px;
}

/* ── Tables ── */
.table-wrap {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  margin-bottom: 24px;
}

.table-wrap table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
}

.table-wrap th {
  text-align: left;
  padding: 10px 16px;
  font-weight: 600;
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: var(--text-secondary);
  background: var(--muted);
  border-bottom: 1px solid var(--border);
}

.table-wrap td {
  padding: 10px 16px;
  border-bottom: 1px solid var(--border);
  vertical-align: top;
}

.table-wrap tr:last-child td { border-bottom: none; }

.table-wrap code {
  font-family: 'SF Mono', 'Fira Code', monospace;
  font-size: 12px;
  background: var(--code-bg);
  padding: 2px 6px;
  border-radius: 4px;
  color: var(--sage-dark);
}

[data-theme="dark"] .table-wrap code {
  color: #a3c48a;
}

.tag {
  display: inline-block;
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 11px;
  font-weight: 500;
  margin: 1px 2px;
}

.tag-red    { background: #fee2e2; color: #b91c1c; }
.tag-green  { background: #dcfce7; color: #166534; }
.tag-amber  { background: #fef3c7; color: #92400e; }
.tag-blue   { background: #dbeafe; color: #1e40af; }
.tag-purple { background: #ede9fe; color: #6d28d9; }

[data-theme="dark"] .tag-red    { background: #3b1111; color: #fca5a5; }
[data-theme="dark"] .tag-green  { background: #0f2a14; color: #86efac; }
[data-theme="dark"] .tag-amber  { background: #2a1f04; color: #fcd34d; }
[data-theme="dark"] .tag-blue   { background: #0f1a2e; color: #93c5fd; }
[data-theme="dark"] .tag-purple { background: #1a0f2e; color: #c4b5fd; }

/* ── Code Tree (for file structures) ── */
.route-tree {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 20px 24px;
  font-family: 'SF Mono', 'Fira Code', monospace;
  font-size: 13px;
  line-height: 1.7;
  overflow-x: auto;
  margin-bottom: 24px;
}

.route-tree .folder { color: var(--sage); font-weight: 600; }
.route-tree .file { color: var(--text-secondary); }
.route-tree .comment { color: var(--text-secondary); opacity: 0.6; font-style: italic; }

/* ── Mobile ── */
.menu-toggle {
  display: none;
  position: fixed;
  top: 12px; left: 12px;
  z-index: 200;
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 8px 12px;
  font-size: 14px;
  cursor: pointer;
  color: var(--text);
}

@media (max-width: 768px) {
  .sidebar { transform: translateX(-100%); }
  .sidebar.open { transform: translateX(0); }
  .main { margin-left: 0; padding: 24px 16px 60px; }
  .menu-toggle { display: block; }
  .hero h1 { font-size: 24px; }
  .info-grid { grid-template-columns: repeat(2, 1fr); }
}
```

## Customizing the Accent Color

The default accent is `--sage: #789a60`. To change it for different projects, replace these three variables:

```css
--sage: #3b82f6;       /* Primary accent */
--sage-light: #dbeafe; /* Light background for active states */
--sage-dark: #1e40af;  /* Darker shade for text on light backgrounds */
```
