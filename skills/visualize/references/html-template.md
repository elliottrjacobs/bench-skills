# HTML Template

The complete HTML skeleton for every visual explainer page. Copy this structure exactly, then customize the content sections.

## Full Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{Title} - Visual Diagrams</title>
<script src="https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js"></script>
<style>
  /* Paste full CSS from css-patterns.md */
</style>
</head>
<body>
<button class="menu-toggle" onclick="document.querySelector('.sidebar').classList.toggle('open')">Menu</button>

<!-- ════════ SIDEBAR ════════ -->
<aside class="sidebar">
  <div class="sidebar-header">
    <h1>{Project Name}</h1>
    <p>{Subtitle} &middot; {Date}</p>
  </div>
  <nav class="sidebar-nav">
    <!-- Repeat nav-section + nav-link groups -->
    <div class="nav-section">{Group Label}</div>
    <a class="nav-link active" data-target="{section-id}">
      <span class="dot dot-green"></span>{Section Name}
    </a>
    <!-- More links... -->
  </nav>
  <div class="sidebar-footer">
    <span class="stats-badge">{summary stats}</span>
    <button class="theme-toggle" onclick="toggleTheme()">Toggle Theme</button>
  </div>
</aside>

<!-- ════════ MAIN CONTENT ════════ -->
<main class="main">

<!-- Hero -->
<div class="hero">
  <h1>{Page Title}</h1>
  <p>{One-line description of what this page covers.}</p>
  <div class="meta-row">
    <span class="meta-pill">{Label} <span class="val">{Value}</span></span>
    <!-- More pills... -->
  </div>
</div>

<!-- Stat Cards (optional) -->
<section class="section" id="{section-id}">
  <div class="section-label">{Group}</div>
  <h2>{Section Title}</h2>
  <p>{Section description.}</p>

  <div class="info-grid">
    <div class="info-card">
      <div class="label">{Metric Name}</div>
      <div class="value">{Number}</div>
      <div class="desc">{Short description}</div>
    </div>
    <!-- More cards... -->
  </div>
</section>

<!-- Diagram Section -->
<section class="section" id="{section-id}">
  <div class="section-label">{Group}</div>
  <h2>{Section Title}</h2>
  <p>{What this diagram shows.}</p>

  <div class="diagram-card">
    <div class="card-header">
      <h3>{Diagram Name}</h3>
      <span class="card-badge">{Type}</span>
    </div>
    <div class="diagram-body">
      <pre class="mermaid">
{mermaid diagram code here}
      </pre>
    </div>
  </div>
</section>

<!-- Data Table Section -->
<section class="section" id="{section-id}">
  <div class="section-label">{Group}</div>
  <h2>{Section Title}</h2>

  <div class="table-wrap">
    <table>
      <thead><tr><th>{Col 1}</th><th>{Col 2}</th><th>{Col 3}</th></tr></thead>
      <tbody>
        <tr><td>{data}</td><td>{data}</td><td>{data}</td></tr>
      </tbody>
    </table>
  </div>
</section>

</main>

<script>
  /* Paste full JS from js-interactions.md */
</script>
</body>
</html>
```

## Component Reference

### Sidebar Nav Link

```html
<a class="nav-link" data-target="{section-id}">
  <span class="dot dot-{color}"></span>{Label}
</a>
```

Dot colors: `dot-red`, `dot-amber`, `dot-green`, `dot-purple`, `dot-blue`, `dot-pink`, `dot-teal`.

### Diagram Card

Every Mermaid diagram must be wrapped in this structure:

```html
<div class="diagram-card">
  <div class="card-header">
    <h3>{Diagram Title}</h3>
    <span class="card-badge">{Badge: Flowchart|Architecture|Data Flow|Sequence|State}</span>
  </div>
  <div class="diagram-body">
    <pre class="mermaid">
flowchart TD
    A --> B
    </pre>
  </div>
</div>
```

The zoom controls are injected automatically by JS — do NOT add them in the HTML.

### Info Card

```html
<div class="info-card">
  <div class="label">{METRIC NAME}</div>
  <div class="value">{42}</div>
  <div class="desc">{Short context}</div>
</div>
```

Wrap multiple info-cards in `<div class="info-grid">`.

### Data Table

```html
<div class="table-wrap">
  <table>
    <thead><tr><th>Column</th></tr></thead>
    <tbody>
      <tr><td>Data</td></tr>
    </tbody>
  </table>
</div>
```

Use `<code>` for code references, `<span class="tag tag-{color}">` for labels inside cells.

Tag colors: `tag-red`, `tag-green`, `tag-amber`, `tag-blue`, `tag-purple`.

### Meta Pills (Hero)

```html
<div class="meta-row">
  <span class="meta-pill">{Label} <span class="val">{Value}</span></span>
</div>
```

### Section Pattern

Every section follows this structure:

```html
<section class="section" id="{kebab-case-id}">
  <div class="section-label">{Group Name}</div>
  <h2>{Section Title}</h2>
  <p>{Description — one or two sentences.}</p>

  <!-- diagram-card, table-wrap, info-grid, or any combination -->
</section>
```

The `id` must match the sidebar `data-target` for scroll-spy to work.
