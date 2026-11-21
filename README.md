# AptaRender

**Art-first, browser-based editor for 2D nucleic acid secondary-structure figures.**

You provide the sequence and its dot-bracket structure — AptaRender gives you a fully
editable 2D layout for making clean, publication-quality figures. It does **no structure
prediction**; it is purely a drawing/annotation tool for figures you already know.

### ▶ Live site: https://yjhe99.github.io/aptarender/

📄 **Preprint:** [AptaRender on bioRxiv](https://www.biorxiv.org/content/10.64898/2026.11.12.766329v2)
(doi 10.64898/2026.11.12.766329). If AptaRender helped with a figure in your work, please cite it:

> He, Y., Zhu, Y., Tran, H., & Yeh, H. C. (2026). AptaRender: an interactive drawing and styling tool for 2D nucleic acid structures. *bioRxiv*, 2026-11.

---

## Why

Prediction tools like mfold/UNAfold draw structures well, but they're rigid on the
aesthetic side — you can't easily resize fonts, rotate a structure while keeping the
letters upright, or color-code individual bases the way a figure needs. AptaRender
separates *drawing* from *prediction* so you have full control over how the figure looks.

## What it does

- **Input:** a sequence plus a nested dot-bracket structure — `( )` for Watson-Crick pairs,
  `[ ]` / `{ }` for non-WC pairs (drawn dashed), `.` unpaired, and `+` for the G's of a
  G-quadruplex
- **Automatic layout** for hairpins, bulges, internal loops, and multiloops — unpaired
  bases enclosed by a real base pair are arranged as a loop. With the default **Natural**
  tail style and two or more stems, the whole outer loop (tails, linkers and the stems'
  first pairs) is drawn as one round circle with the stems pointing outward
- **G-quadruplexes:** mark the G's with `+` (four equal runs, e.g. TBA
  `GGTTGGTGTGGTTGG` / `++..++...++..++`) and the quadruplex is drawn as the textbook
  cartoon — stacked G-quartets in perspective, each G a colored slab — see
  [G-quadruplexes](#g-quadruplexes) below
- **Two-strand hybridization ("Build a Complex"):** manually pair up one or more regions of one
  sequence with regions of another and see them drawn as joined duplexes — see
  [Using the Complex mode](#using-the-complex-mode-two-strand-hybridization) below
- **Drag-to-reposition:** click-drag any base (and its whole paired branch) to nudge the
  auto-layout into exactly the shape you want
- **Undo / Redo** for every edit
- **Rotate** the whole structure while every letter stays upright
- **Font size** control (independent of the layout spacing)
- **Per-base letter color:** by A/U/G/C identity palette, a single monochrome color, or by hand-selecting bases
- **Base circles:** independent frame and fill — none / custom color / by base identity — plus per-base fill by selection, adjustable frame thickness
- **Per-hydrogen-bond coloring:** click any base-pair rung and recolor it
- **Free 5′/3′ tail layouts:** natural (curves inward), flatten, L-shape, or perpendicular
- **Backbone / rung colors, position numbering, custom canvas background**
- **Resizable control panel** — drag the divider to widen or narrow it
- **Export:** SVG (fully editable — every base is its own text element) and PNG at a chosen scale, with an optional transparent background
- Your sequences, structures, and complexes are **saved locally** and restored when you return

## How to use

1. Paste your **sequence** and its **dot-bracket structure** (or click *Hairpin* / *Multiloop* for a demo).
2. Click **Render**.
3. Style it with the panel on the left — colors, fonts, rotation, tails, circles.
4. Drag any base to nudge it into place; **Ctrl+Z / Ctrl+Y** to undo/redo.
5. **Export** as SVG or PNG.

## Using the Complex mode (two-strand hybridization)

The **Complex** tab lets you show two of your saved sequences hybridizing with each
other over a chosen region — useful for aptamer–target binding, toehold/strand-
displacement designs, primer binding, or a molecular-beacon–style probe/target pair.

**To build one:**
1. Have both sequences already entered and rendered at least once, so they're in your
   sequence list (the dropdowns pull from there).
2. Open the **Complex** tab. Pick **Strand A** and **Strand B**.
3. Enter the **A range** and **B range** — the 1-indexed start/end positions on each
   strand that pair up with each other. The two ranges must be the same length. Use
   **+ Add duplex region** for more than one region (Replace mode draws each as its own ladder;
   a strand's own pairs in the stretch between two ladders are drawn as small helices there).
4. Choose a **mode**:
   - **Replace** — builds a brand-new straight duplex ladder for the hybridizing region.
     Use this when you want the hetero-duplex to visually dominate and don't mind either
     strand's own existing stem-loop being redrawn from scratch around it.
   - **Side-by-side** — keeps Strand A's own layout exactly as it already renders (as if
     you'd rendered it alone) and draws Strand B alongside it. Use this when Strand A is
     the "main" molecule (e.g. your aptamer) whose own shape you don't want disturbed,
     and Strand B is hybridizing onto part of it (e.g. a short complementary probe/target
     strand).
5. Click **Build complex**. It's added to the **Complexes** list on the same tab, where
   you can reopen, rename via the sequence list, or delete it.
6. Drag any base into place afterward, same as a single sequence — the auto-layout only
   needs to get you a reasonable starting point.

### Cautions for Complex mode
- **Both modes break conflicting pairs, but differently.** Replace mode can break
  existing pairs in *both* strands if they touch the hybridized bases. Side-by-side mode
  only ever breaks **Strand B's** own pairs — Strand A is always left completely intact.
  Pick the strand whose own structure matters more to you as **Strand A** in side-by-side
  mode.
- **Side-by-side mode expects Strand A to already have a real stem in the chosen range.**
  If A's range has no base pairs of its own, there's no real "arm" to offset Strand B
  from, and you'll see a warning suggesting you swap A and B.
- **The auto-layout is a starting point, not a final figure**, especially for Replace
  mode when the hybridized region only partially overlaps an existing stem — the leftover
  loop/tail can land at an unexpected angle. Drag it into place rather than expecting a
  perfect result on the first build.
- **Deleting a sequence that's used in a saved complex** removes that complex too (you'll
  see a note when this happens) — rebuild it if you didn't mean to lose it.
- **Several duplex regions** work in Replace mode; Side-by-side mode uses the first region only.
  Three or more strands and automatic complementarity detection are not implemented —
  everything here is manual, matching the rest of the app's no-prediction philosophy.
- A strand containing a **G-quadruplex** can't be used in a complex yet.

## G-quadruplexes

Mark every G that takes part in a quadruplex with `+` in the structure (the ViennaRNA
convention). Four runs of `+` of the same length make one quadruplex; the run length is the
number of stacked G-quartets (`++` = 2 quartets, `+++` = 3, …). Every further 4 runs make
another quadruplex. G's you don't mark are ordinary bases.

```
GGTTGGTGTGGTTGG      TBA: quartets G1·G6·G10·G15 and G2·G5·G11·G14,
++..++...++..++      TT loops below, TGT loop above
```

- It can sit next to hairpins, stems and free tails in the same strand. Drag any of its bases
  to move the whole quadruplex.
- **Lines tab → G-quartet colors** sets the two alternating slab colors; turning off
  "Base-pair rungs" hides the slabs.
- **Cautions:** only unpaired bases may sit between a quadruplex's first and last G; every
  quadruplex is drawn with the chair (lateral-loop) arrangement; inside a larger molecule it's
  drawn upside down so its first and last G face the rest of the molecule (use Flip ↕ to turn
  it). AptaRender doesn't decide whether a G4 forms — you mark it.

## Using drag-to-reposition

Click and drag any base: by default it moves that base's **whole paired branch**
(everything nested inside its own base pair) together, keeping the rest of the structure
anchored. Hold **Alt** while dragging to move just the single base you clicked, leaving
its branch behind. An unpaired base always drags alone.

**Caution:** dragging only stores a manual offset on top of the auto-layout — if you
change the underlying sequence/structure and re-render, or switch the tail mode, those
manual offsets are kept and applied to the *new* layout, which can look wrong if the
structure changed significantly near a base you'd moved. Re-drag it if that happens.

## Undo / Redo

**Ctrl+Z** (or **Cmd+Z**) to undo, **Ctrl+Y** or **Ctrl+Shift+Z** to redo — up to 100
steps. This covers rendering, styling changes, drags, and sequence/complex management.

**Caution:** undo/redo operates on the whole app's saved state, not per-sequence — if
you've switched to a different sequence since the change you want to undo, switching
back first will make the history line up the way you expect.

## Updates

See [CHANGELOG.md](CHANGELOG.md) for the full version history.

## Feedback

Still under development — built mainly for my own figures, but I'd love to know whether
it's useful to other nucleic acid researchers too. Suggestions are very welcome.

Built by **Janet He** with Claude · [yjhe@utexas.edu](mailto:yjhe@utexas.edu?subject=AptaRender%20feedback)
