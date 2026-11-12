# Practical P13: Choosing Between Flexbox and CSS Grid

This technical guide documents the evaluation criteria, architectural paradigms, and real-world trade-offs when selecting between **CSS Flexbox (1D)** and **CSS Grid (2D)** for modern responsive web layouts.

---

## 1. Core Architectural Differences

| Dimension | CSS Flexbox | CSS Grid |
| :--- | :--- | :--- |
| **Dimensionality** | **One-Dimensional (1D)**: Operates along a single axis (either row *or* column at a time). | **Two-Dimensional (2D)**: Coordinates rows *and* columns simultaneously in a cohesive matrix. |
| **Design Mindset** | **Content-First ("Content-out")**: Child element contents determine sizing; flex items shrink or stretch to fit. | **Layout-First ("Grid-in")**: The parent container defines tracks, rows, and columns; items are positioned into those slots. |
| **Row/Col Alignment** | Multi-row items wrap independently. Items on row 2 do *not* align with items on row 1 unless explicitly sized. | Items in column 2 strictly align with column 2 in every row across the entire grid. |
| **Cell Overlapping** | Difficult without absolute positioning hacks. | Native overlapping supported via coordinate lines or named grid areas (`grid-area` / `z-index`). |

---

## 2. When to Choose Flexbox (1-Dimensional)

Use Flexbox when your layout problem concerns **aligning items along a single axis** or **distributing variable space**:

### Ideal Scenarios
1. **Navigation Bars (`<nav>` / Header):**
   - Distributing brand logo on the left and links on the right (`justify-content: space-between`).
   - Vertically centering icons and buttons (`align-items: center`).
2. **Form Input Clusters:**
   - Text input paired with an inline submit button where the input stretches (`flex-grow: 1`) while the button retains its natural width.
3. **Pill Badges and Tag Clouds:**
   - Tags of differing character lengths that wrap fluidly (`flex-wrap: wrap; gap: 0.5rem;`).
4. **Media Objects & Comment Lists:**
   - Avatar image on the left, author info and comment text on the right.
5. **Sticky Footers:**
   - Pushing the footer to the bottom of the viewport using `display: flex; flex-direction: column; min-height: 100vh;` and `margin-top: auto` or `flex-grow: 1` on `<main>`.

### Canonical Flexbox Code Example
```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1.5rem;
}

.search-input {
  flex-grow: 1; /* stretches to take available room */
}
```

---

## 3. When to Choose CSS Grid (2-Dimensional)

Use CSS Grid when you need **structural control over both horizontal and vertical axes** and want uniform alignment across columns:

### Ideal Scenarios
1. **Responsive Card Grids (Portfolio Projects / E-commerce):**
   - Uniform cards that dynamically fit any viewport width without media queries using `repeat(auto-fit, minmax(280px, 1fr))`.
2. **Asymmetric Editorial Layouts (Newspapers & Magazines):**
   - Distinct columns where stories occupy different fractional units (`2.2fr 1.2fr 1fr`) or span across multiple columns and rows (`grid-column: span 2`).
3. **Dashboard Applications:**
   - Full application shells featuring sticky sidebars, top status bars, widgets, and dynamic viewport heights.
4. **Overlapping Media Elements:**
   - Placing text captions or gradient overlays directly over background images without `position: absolute`.

### Canonical Grid Code Example
```css
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
}

/* Featured card spanning two columns */
.project-card.featured {
  grid-column: span 2;
}
```

---

## 4. The Modern Standard: Hybrid Composition

Modern web design does **not** treat Flexbox and Grid as rivals. Instead, they are combined harmoniously:

```
┌─────────────────────────────────────────────────────────────┐
│ CSS GRID: Macro Layout (Header, Main, Sidebars, Footer)     │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ FLEXBOX: Micro Layout (Brand Logo <---> Nav Links)      │ │
│ └─────────────────────────────────────────────────────────┘ │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ CSS GRID: Projects Grid (Auto-fitting responsive cards)  │ │
│ │ ┌─────────────────────────────────────────────────────┐ │ │
│ │ │ FLEXBOX: Inside Card (Title, Excerpt, Tag Badges)   │ │ │
│ │ └─────────────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

1. **Macro Layout (Grid):** The page scaffold (`index.html` projects grid, `newspaper.html` 3-column layout) is engineered with **CSS Grid**.
2. **Micro Components (Flexbox):** The inner contents of cards, navigation bars, buttons, and badges use **Flexbox** for alignment and spacing.
