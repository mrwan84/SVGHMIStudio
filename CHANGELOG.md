# Changelog

All notable changes to SVGHMIStudio. The most recent release is at the top; older releases follow in descending order. For the current feature set and usage documentation see [README_User.md](./README_User.md).

## v3.5.6 (Current)

Large correctness, robustness, and UX sweep across eight themed areas: code-duplication cleanup, transform correctness, align / distribute UX, HMI-specific features, performance, robustness, developer ergonomics, and accessibility & i18n. Every top-recommendation item shipped. No breaking changes — all additions are opt-in or transparently back-compatible.

### Transform Correctness

- **Centralized element translation** — All paths that move an element by `(dx, dy)` now go through a single shared helper (`translateElementInStore` / `translateElementTree` in `src/lib/element-transform.ts`). Fixes a class of drift bugs where arrow-key nudge and paste-at-offset on rotated elements previously translated by a rotated vector instead of exactly `(dx, dy)`.
- **Full 2D transform matrix** — `getTransformedBounds` now parses the complete SVG transform list (`matrix` / `translate` / `scale` / `rotate` / `skewX` / `skewY`, in any order, comma- or whitespace-separated) and composes them with ancestor transforms. Align, distribute, flip, marquee selection, "Zoom to selection", and the Transform dialog now report visual bounds for scaled / skewed / matrix-transformed elements — not just rotated ones.
- **Rotated groups compose correctly** — A shape inside a rotated `<g>` now aligns / distributes by its true document-space AABB (ancestor rotations are composed into the corner transform).
- **Comma-separator tolerance** — `rotate(45, 10, 20)` is now parsed the same as `rotate(45 10 20)` throughout the codebase (SVG-spec compliant).

### Align & Distribute

- **Three target modes** — The Alignment toolbar now has a `Sel / Key / Page` picker: align to the selection's union bounds (default), to the last-clicked element (Figma-style "key object" — stays fixed while others move to match), or to the document page (works with a single-element selection).
- **Keyboard shortcuts** — `Alt+Shift+L / R / H / T / B / V` map to align left / right / center-horizontal / top / bottom / center-vertical.
- **Context-menu Align fixed** — (Carried forward from v3.5.5 original release.)
- **Rotated-bounds alignment** — (Carried forward from v3.5.5 original release.)

### HMI / WinCC

- **Bindings Overview** — The Bindings panel now has a collapsible "All Bindings" section at the top that lists every binding in the document with filter / search, broken-reference indicators (missing element, missing `ParamProps.*`, missing `LocalProps.*`), and a bulk **Rename parameter** action that rewrites every `ParamProps.<old>` / `LocalProps.<old>` reference across bindings in a single undo-able step.
- **WinCC-compatible grid presets** — New grid-size picker next to the Snap-to-Grid toggle: one-click WinCC presets (5 / 10 / 20 / 25 / 50 px) plus a custom input. Grid size is now user-adjustable (previously hard-coded to 10).

### Performance

- **Memoized selection handles** — `SelectionBox` is now wrapped in `React.memo`. With N selected elements, mutations that touch a single element now re-render just one handle group instead of all N.
- **rAF-coalesced drag updates** — Rotation drag, path-point drag, and snap-line updates route through a new `rafThrottle` helper (`src/lib/raf-throttle.ts`). Updates that fired 150–200× per second during drag now collapse to one per frame, with `.flush()` on pointerup so history snapshots the final value.

### Robustness

- **Dirty-driven autosave** — Replaced the previous 60 s unconditional poll with a dirty-flag-subscribed, debounced writer (3 s after last change; max 30 s between writes during continuous edits). Crashes now lose ≤ ~3 s of work instead of up to a full minute; idle sessions burn no cycles. Writes are flushed on `beforeunload` for clean shutdowns.
- **Schema-validated recovery** — Crash-recovery data is now validated through a zod schema before being applied. If the stored blob is corrupt or incompatible, the Recovery dialog shows a clear error (with the offending field path) and offers only "Discard" — restoring a corrupt blob could corrupt document state.
- **Migration chain** — Recovery blobs now carry a `schemaVersion` field with a versioned migration path. Pre-versioned blobs from earlier builds are transparently upgraded on load.

### Developer Ergonomics

- **Stricter TypeScript** — `exactOptionalPropertyTypes` is now on in the base config; `noUncheckedIndexedAccess` is available as an advisory check via `npm run typecheck:strict` (non-blocking, gradual-adoption target).
- **Unit tests** — 65 new tests across four new suites:
  - `svg-dom.test.ts` (46): `parseRotation` / `setRotation` round-trips, `getTransformedBounds` for rotate / scale / translate / skew / matrix / nested ancestors, and a 2D-matrix composition suite.
  - `raf-throttle.test.ts` (5): collapse behavior, per-frame re-arm, flush / cancel semantics.
  - `autosave.test.ts` (14): schema accept / reject, error-path surfacing, legacy-blob migration, `clearRecovery` edge cases.
  - Full suite: **106 / 106 passing.**

### Accessibility & i18n

- **German localization** — Added `i18next` + `react-i18next`. Full `de` + `en` translation catalogs for the menu / toolbar / panels / align / recovery namespaces; MenuBar top-level labels are fully localized as a working demonstration. A compact `EN` / `DE` picker sits at the right edge of the menu bar and persists the choice across sessions. Remaining UI strings migrate incrementally via `useTranslation()`.
- **Keyboard-only canvas navigation** — `Tab` / `Shift+Tab` cycle selection through top-level elements (groups treated as atomic). `Enter` on a single-selected text / path / polygon / polyline activates in-place edit — mirrors double-click. Combined with the existing arrow-nudge, Escape-deselect, and the new Alt+Shift align shortcuts, the core selection / alignment / edit loop is now fully operable without a pointer.

---

## v3.5.5

### Align & Distribute Fixes

- **Context menu Align now works** — The Align submenu in the canvas context menu previously used a duplicated, out-of-date implementation that silently failed for many shape types. It now delegates to the same shared `alignElements` used by the Alignment toolbar, so both entry points behave identically
- **Align/Distribute respect rotated bounds** — Rotated elements are now aligned and distributed by their visual (axis-aligned) bounding box instead of their pre-rotation local bbox, matching what you see on screen
- **Rotation pivot follows translation** — When an align/distribute move translates a rotated element, the rotation center (`cx`, `cy`) in `transform="rotate(...)"` is shifted by the same offset, so the element translates by exactly the requested delta in document space (same pattern used by drag-to-move)

## v3.5.4

### Animation Rotation Center Fix

- **Auto Center now works for all shape types** — The "Auto Center (from element)" button in the rotate animation panel now correctly computes the center for polygons, stars, polylines, lines, paths, and text (previously only rect/circle/ellipse were supported; everything else silently returned 0,0)

## v3.5.3

### Toast Notification System

- **User-visible feedback** — File open/save errors, export success/failure, and validation results now shown as toast notifications instead of silent console errors
- **Auto-dismiss** — Success toasts dismiss after 4s, error toasts after 6s, with manual close button
- **Themed styling** — Color-coded left border (green/red/amber/blue) matching the notification type

### Dashed Stroke Support

- **Dash pattern dropdown** — Custom dropdown selector with inline SVG preview lines for each dash preset (Solid, Dashed, Dotted, Dash-Dot, Dash-Dot-Dot)
- **Custom patterns** — "Custom..." option allows entering arbitrary `stroke-dasharray` values
- **Stroke dash offset** — New numeric input for `stroke-dashoffset` control
- **Bug fix** — Fixed a bug where selecting "Custom" wrote the literal string `'custom'` as the SVG attribute

### Real-Time Expression Validation

- **Inline validation** — Binding expressions are now validated as you type in the Bindings panel
- **Reference checking** — Validates `ParamProps.*`, `LocalProps.*`, and `Converter.*()` references against defined parameters, local variables, and known converters
- **Syntax checks** — Detects unbalanced parentheses and unknown references
- **Disabled submit** — Add/Save buttons are disabled when validation errors are present

### Batch Export

- **Multi-format export** — Export dialog now includes "Also export as SVG" and "Also export as PNG" checkboxes
- **PNG scale selector** — Choose 1x, 2x, or 4x resolution for PNG exports
- **Automatic naming** — Additional formats saved to the same directory with matching base filename

### Smart Snap Enhancements

- **Snap to centers** — Elements snap to the center points of other elements
- **Snap to edge midpoints** — Elements snap to midpoints of other elements' edges
- **Smart spacing guides** — Detects and snaps to equal spacing between elements (horizontal and vertical)
- **Color-coded snap lines** — Edge snaps (magenta), center snaps (purple), spacing guides (cyan)
- **View menu controls** — New "Element Snapping" submenu with toggles for each snap type

## v3.5.2

### Modern UI Redesign

- **New design system** — Introduced a comprehensive design token system with semantic surface elevations (`--bg-primary`, `--bg-secondary`, `--bg-sidebar`, `--bg-input`, `--bg-elevated`, `--bg-hover-subtle`), a text hierarchy (primary/secondary/tertiary/inverse), and soft accent variants
- **Modern indigo accent** — Replaced the dated Microsoft blue with a clean indigo palette (`#4f46e5` light / `#7c7aed` dark) for a more contemporary feel
- **Refined dark mode** — Deeper, richer dark palette with better contrast (slate-based) and properly tuned accent luminance
- **Shadow & radius scale** — Added `--shadow-xs` through `--shadow-xl` and `--radius-xs` through `--radius-xl` design tokens used consistently across all components
- **Motion tokens** — Standardized transition timings (`--transition-fast`, `--transition-base`) for consistent micro-interactions
- **Inter font stack** — Upgraded to Inter as the primary typeface with proper antialiasing, tabular numerals for shortcuts/zoom percentages, and optimized text rendering
- **Modern scrollbars** — Thin (10px) rounded scrollbars with transparent tracks for a cleaner look
- **Focus rings** — Visible accent-colored focus rings on all interactive elements for better accessibility

### Layout & Navigation Improvements

- **Tool rail repositioned** — Drawing tools moved to the leftmost edge (Figma/Sketch convention) with a wider 44px rail for better ergonomics
- **Grouped tool palette** — Drawing tools are now organized into logical clusters (Select / Shapes / Drawing / Utilities) separated by subtle dividers
- **Active tool indicator** — Active drawing tool now shows an accent fill with shadow plus a left-edge indicator pill
- **Modern segmented tab bar** — Right sidebar tabs replaced with pill-shaped segmented buttons (accent fill on active tab) instead of the old underline style
- **Polished resize handles** — Subtle 1px idle line that reveals a grip indicator pill on hover, turning solid accent while dragging
- **Backdrop tint** — Subtle secondary background tint between panels for better visual separation

### Toolbar & Menu Polish

- **Taller toolbar** (44px) with modern rounded buttons, smooth hover states, and accent-soft highlights for active buttons
- **Zoom percentage chip** styled as a bordered input pill with tabular numerals
- **Filename badge** — Current filename displayed in a rounded pill with a colored dirty indicator dot
- **Menu bar refinements** — Taller menu bar (34px), rounded menu buttons with accent-soft active state
- **Elevated dropdowns** — Menu dropdowns now use larger radius, layered shadows, and full accent hover fill on items (instead of flat gray)
- **Shortcut chips** in menus use tabular numerals for better alignment

### Dialog Modernization

- **Animated entrances** — Dialogs now fade in with a subtle scale and translate animation
- **Backdrop blur** — Modal backdrop now uses a blur effect for better focus on the dialog content
- **Header & footer dividers** — Clearer visual structure with bordered header and footer action bars
- **Larger radius** (14px) and richer multi-layer shadows for a premium feel
- **Enhanced form controls** — Inputs, selects, and checkboxes have hover states, focus rings, and better padding
- **Hover-lift OK button** — Primary action button subtly lifts on hover for better affordance

## v3.5.1

### Group & Transform Fixes

- **Fixed group bounding box ignoring child transforms** — Groups containing rotated, skewed, or otherwise transformed children now show an accurate selection box using DOM `getBBox()` instead of computing the union of untransformed attribute bounds
- **Fixed moving groups with rotated children** — Moving a group now correctly updates the rotation center (`cx`, `cy`) of each child's `transform="rotate(...)"`, preventing elements from jumping to incorrect positions
- **Fixed spurious history entries on click/double-click** — Clicking or double-clicking an element without actually moving it no longer pushes empty "Move element" or "Resize element" entries to the undo history

### CSS Style Inlining Improvements

- **CSS inlining now uses the SVGO optimizer pipeline** — Both the file-open dialog and the Edit menu action run the `inlineCssStyles` + `convertStyleToAttrs` optimizer plugins via `runOptimizer()`, ensuring consistent behavior across all inlining paths
- **Fixed inline `style=""` attributes not being processed** — SVGs with `style=""` attributes (e.g., `style="stop-color:#1d4ed8"` on gradient stops) but no `<style>` tags are now correctly inlined
- **Fixed CSS parser breaking on nested @-rules** — Replaced naive `split('}')` parsing with brace-depth-aware block extraction, correctly handling `@keyframes`, `@media`, and other nested CSS rules

### Additional Bug Fixes

- Fixed Properties panel committing multiple undo entries per edit (now batches keystrokes into a single undo entry on blur/Enter)
- Fixed Properties panel element ID field being editable (now read-only to prevent breaking references)
- Fixed Select All (Ctrl+A) selecting non-visual elements (defs, clipPath, filter, etc.)
- Fixed arrow key nudging not working on grouped elements
- Fixed marquee selection not finding nested elements inside groups
- Fixed text edit overlay position not accounting for canvas pan offset
- Fixed Parameters panel and Bindings panel local state not syncing on undo/redo
- Fixed Transform dialog allowing negative dimensions from scale operations
- Fixed Fit to Window not resetting pan position (canvas could appear off-center)
- Fixed `releaseClipPath` redo causing infinite history recursion
- Fixed `ungroupElements` not working for nested groups (now uses recursive tree replacement)
- Fixed `saveFile` / `saveFileAs` silently swallowing errors (now shows alert on failure)
- Fixed rotation handle disappearing for path/text elements (separated rotate handle from resize handle visibility)
- Fixed odd-length polygon/polyline points array causing NaN in offset calculations

## v3.5.0

### CSS Style Inlining

- **Automatic CSS `<style>` inlining on file open** — SVGs using `<style>` tags with class selectors (e.g., `.box { fill: #e6f2ff; }`) are automatically resolved into individual SVG attributes on each element, making them editable in the Properties panel and compatible with WinCC
- **Edit > Inline CSS Styles** menu action — Manually inline CSS styles on already-open documents
- **Inline CSS Styles dialog** — Shows a summary of inlined rules with before/after preview
- **Optimizer plugin** — `inlineCssStyles` plugin added to the SVG optimizer pipeline for export-time CSS inlining
- Supports `.class`, `tag`, `#id`, `tag.class`, and comma-separated selector groups
- Respects specificity: inline `style=""` > element attributes > class rules > tag rules

### Text Element Handling Improvements

- Fixed browser native text selection interfering with canvas element selection (added `user-select: none` to SVG canvas)
- Fixed `<tspan>` elements being individually selectable — clicking any part of a `<text>` element now selects the whole text, not individual tspans
- Fixed text elements with `text-anchor` attribute (middle/end) shifting left when dragged — now uses delta-based positioning instead of absolute
- Fixed text bounding box calculation using DOM `getBBox()` for accurate bounds (handles tspan children, fonts, and text-anchor correctly)
- Fixed tspan child coordinates not updating when moving or resizing text elements

### Bug Fixes

- Fixed `font-size` with `px` unit (e.g., "14px") not displaying in the Properties panel number input

## v3.4.0

### SVG Optimizer — SVGO-Inspired Plugin Engine

Complete rewrite of the SVG optimization system with a plugin-based architecture inspired by [SVGO](https://github.com/svg/svgo). 31 optimization plugins organized into 8 categories, with 5 built-in profiles and multipass support.

**New Plugin Categories & Plugins:**

- **Cleanup** (7 plugins) — removeEmptyAttrs, removeEmptyText, removeDeprecatedAttrs, cleanupEnableBackground, sortAttrs, sortDefsChildren, cleanupListOfValues
- **Color** (1 plugin) — convertColors: normalize `rgb()` to hex, shorten `#RRGGBB` to `#RGB`, resolve named colors
- **Shape Conversion** (2 plugins) — convertShapeToPath (rect/circle/ellipse/line/polygon/polyline to `<path>`), convertEllipseToCircle (equal-radius ellipse to circle)
- **Style / Attributes** (5 plugins) — convertStyleToAttrs (critical for WinCC — converts CSS `style` attribute to individual SVG attributes), removeUselessStrokeAndFill, removeNonInheritableGroupAttrs, moveElemsAttrsToGroup, moveGroupAttrsToElems
- **Structure** (2 plugins) — removeUselessDefs, convertOneStopGradients (single-stop gradient to plain color)
- **Path Optimization** (2 plugins) — convertPathData (H/V shorthand, relative coords, redundant command removal), mergePaths (merge adjacent paths with identical attributes)
- **Transform** (1 plugin) — convertTransform (remove identity, merge consecutive, simplify `translate(x,0)` to `translate(x)`)
- **Miscellaneous** (2 plugins) — removeOffCanvasPaths, removeRasterImages

**Optimization Profiles:**

| Profile      | Description                                               |
| ------------ | --------------------------------------------------------- |
| Safe         | Minimal, non-destructive (7 plugins)                      |
| Moderate     | Safe + structural cleanup (13 plugins)                    |
| Aggressive   | Maximum optimization (26 plugins)                         |
| HMI Safe     | Safe with HMI preservation forced on                      |
| WinCC Export | Optimized for WinCC — always includes convertStyleToAttrs |

**UI:**

- Redesigned Optimize SVG dialog with category-based plugin list and per-plugin toggles
- Profile dropdown auto-sets plugin toggles; manual changes switch to "Custom"
- WinCC badge highlights the `convertStyleToAttrs` plugin

### Bug Fixes

- Fixed exported SVGHMI files missing DOCTYPE declaration and attribution comment
- Fixed ExportDialog missing `extended.` prefix validation on widget name
- Fixed ExportDialog missing opacity range validation (0.0–1.0)
- Fixed ExportDialog missing orphan binding check for deleted elements
- Added SVG interactivity event attributes (onclick, onmouseover, etc.) and animation event attributes (onbegin, onend, onrepeat) to WinCC validation warnings

## v3.3.0

### New Drawing Tools

- **Star/Polygon tool** (S) - Draw regular polygons and stars with configurable sides (3-32) and inner radius ratio
- **Spiral tool** - Draw Archimedean spirals with configurable turns (1-20)
- **Pencil/Freehand tool** (N) - Freehand drawing with automatic Catmull-Rom to cubic bezier smoothing
- **Eyedropper tool** (I) - Pick fill color from any element on the canvas
- **Measure tool** (M) - Measure distance (px), angle (degrees), and dx/dy between two points on the canvas
- **Contextual Tool Options bar** - Adjustable parameters per tool (star sides/mode/inner ratio, spiral turns, pencil smoothing)

### Path Operations (new Path menu)

- **Object to Path** (Ctrl+Shift+C) - Convert rect, ellipse, circle, line, polygon, polyline to path elements
- **Stroke to Path** - Convert stroke outlines into filled path geometry
- **Combine Paths** (Ctrl+K) - Merge multiple paths into a single compound path
- **Break Apart** (Ctrl+Shift+K) - Split compound paths at M commands into separate path elements
- **Reverse Path Direction** - Reverse path winding order (handles arc paths with sweep flag flipping)
- **Simplify Path** (Ctrl+L) - Reduce path complexity with Ramer-Douglas-Peucker algorithm
- **Boolean Union** - Merge overlapping shapes into one outline
- **Boolean Difference** - Subtract one shape from another, with proper hole creation for containment cases
- **Boolean Intersection** - Keep only the overlapping region (Sutherland-Hodgman polygon clipping)
- **Boolean Exclusion** - Keep non-overlapping areas with evenodd fill-rule
- Arc recovery in boolean results: detects points lying on original ellipses and restores SVG Arc commands with correct per-arc sweep flags

### SVG Filters & Effects

- **Object Opacity** slider in Properties panel (0-1 range)
- **Gaussian Blur** slider - Apply and adjust feGaussianBlur filter dynamically
- **Drop Shadow** - Configurable feDropShadow with dx, dy, blur radius, and color
- **Blend Modes** dropdown - Apply mix-blend-mode (multiply, screen, overlay, darken, lighten, etc.)
- **Filters Panel** - Dedicated panel for managing filter effects on selected elements

### Transform & Flip

- **Flip Horizontal / Flip Vertical** - Mirror elements across bounding box center (in Edit menu and context menu)
- **Transform Dialog** (Ctrl+Shift+T) - Numeric input for Move (dx/dy), Scale (sx/sy%), Rotate (angle + center), and Skew (horizontal/vertical)

### Markers & Arrowheads

- Add start/end markers to line, path, polyline, and polygon elements
- Predefined marker types: Arrow, Circle, Square, Diamond
- Marker defs management with automatic cleanup of unused markers

### Clip Path Support

- **Create Clip Path** - Use one shape to clip another via context menu
- **Release Clip Path** - Restore clipped elements to normal rendering

### PNG Export

- **File > Export as PNG** - Export current document as PNG with configurable scale (1x, 2x) and background color

### Bug Fixes

- Fixed S (smooth cubic) and T (smooth quadratic) SVG path commands not tracking reflected control points correctly
- Fixed Sutherland-Hodgman clipping requiring CW-normalized input polygons for correct intersection results
- Fixed `replaceElementInTree` always creating new array references even when no replacement occurred
- Fixed polygon/polyline empty `points` attribute causing NaN corruption in offset and flip operations
- Fixed division by zero in `arcToCubicBeziers` when arc half-segment is zero
- Fixed export-utils attribute filter using wrong logical operator (attributes were not stripped correctly)

## v3.2.0

- Fixed critical ID collision bug when loading documents (element IDs could overlap with generated IDs)
- Fixed infinite loop when redoing alignment operations
- Fixed stale state in undo/redo for cut, delete, align, and reorder operations
- Fixed parameter `unit` attribute being dropped on save (was only preserved on export)
- Fixed auto-ID counter not resetting between file opens (caused binding loss on round-trip)
- Fixed WinCC validation failing on files with DOCTYPE declarations
- Fixed reorder (z-order) operations not working for elements inside groups
- Fixed deleting a group leaving orphaned child IDs in selection state
- Fixed hmiColor picker in Preview panel showing wrong colors (0xAARRGGBB now converted correctly)
- Fixed Color Picker committing wrong color when clicking outside (stale closure)
- Fixed undo/redo stack corruption when a command throws an error
- Fixed active panel tab fallback pointing to a disabled panel
- Fixed gradient editor not finding gradients in secondary `<defs>` elements
- Fixed Preview panel gradient bindings not updating (DOM ID collision with main canvas)
- Fixed `Ctrl+A` only selecting top-level elements (now includes nested/grouped elements)
- Fixed path elements not being offset when pasting or aligning
- Fixed SVG number parsing dropping shorthand values like `.5` (valid SVG for `0.5`)
- Fixed zoom-to-selection producing extreme zoom on zero-dimension selections
- Fixed custom dash pattern input being unreachable in Properties panel
- Fixed XML Editor `parseIdCounter` producing non-deterministic IDs across applies
- Fixed `pointerEvents: 'none'` on guide overlay blocking double-click to remove guides
- Fixed SVG optimize "Remove non-essential IDs" being a no-op
- Fixed select dropdown arrows not showing (Tailwind CSS reset override)
- Improved `removeElementFromTree` to preserve object references for better React memoization

## v3.1.0

- Enhanced Color Picker with recent colors and popover UI
- Alignment & Distribution toolbar (align L/C/R/T/M/B, distribute H/V)
- Arrow key nudging (1px, Shift+Arrow 10px)
- Fit to Window (Ctrl+Shift+F) and Zoom to Selection (Ctrl+1)
- Inline text editing (double-click text to edit on canvas)
- Rulers & Guides (drag from ruler to create guides)
- Gradient Editor (linear/radial with stop management)
- Layer visibility (eye icon) and lock (lock icon)
- Arrange/Z-Order (Ctrl+]/[, Ctrl+Shift+]/[)
- Stroke panel (dash patterns, line cap, line join)
- Recent files list in File menu
- Snap visual feedback (magenta alignment lines)
- Auto-save & recovery (60s interval, recovery dialog on startup)
- Polygon tool with point editing (double-click to edit vertices)

## v3.0.1

- Window title shows file path and unsaved indicator (\*)
- Theme preference persists across app restarts
- Unsaved changes guard on window close
- Parameter/binding/local def changes now mark document as dirty
- Full expression reference added to documentation

## v3.0.0

- Complete rewrite: React 18 + TypeScript + Zustand + Vite + TailwindCSS 4 frontend
- Tauri 2 (Rust) backend replacing Python/Qt, with roxmltree/quick-xml for SVG parsing
- Native desktop app via Tauri webview with smaller footprint and faster startup
- CodeMirror 6 XML editor with syntax highlighting, bracket matching, and fold gutter
- Improved undo/redo reliability (stack consistency on errors)
- Safer coordinate handling in canvas (null-safe SVG coordinate conversion)
- Eliminated memory leaks in drawing overlay timer cleanup
- Type-safe dynamic element preview with SVG element allowlist
- Robust programmatic editor updates using CodeMirror annotations (no race conditions)
- Smoother canvas initial layout (no zero-size flash on mount)

## v2.0.0

- Modern VS Code/Figma-inspired UI redesign
- Fluent UI-style SVG icons replacing all system icons
- System theme detection (auto dark/light mode from Windows settings)
- Comprehensive theme-aware QSS stylesheet for all widgets
- Thin modern scrollbars, flat menus, clean dock panels
- Theme menu with System/Light/Dark options
- Fixed transform rendering (SVG column-vector convention)
- Fixed movement and resize of transformed elements
- Text font controls (font family dropdown, size, bold, italic, alignment)

## v1.0.5

- Added light/dark theme support (View → Theme)
- Added logging infrastructure replacing all print() statements
- Added OS clipboard support for copy/paste between instances
- Added rotation handle and flip horizontal/vertical transforms
- Added stroke properties UI (line cap, line join, dash pattern)
- Added opacity slider with visual canvas feedback
- Added element snapping (snap to edges and centers)
- Fixed shortcut conflict (Ctrl+Shift+G)
- Fixed opacity not rendering on canvas

## v1.0.4

- Fixed text positioning with text-anchor support
- Fixed SVG rendering for complex nested groups
- Added style inheritance from parent elements
- Improved transform support (translate, scale, rotate, matrix)
- Fixed selection not changing element position on click
- Added letter-spacing support for text

## v1.0.3

- Canvas refresh after duplicate/paste operations
- Group alignment and distribution support
- Duplicate ID generation fixes

## v1.0.2

- Path editing with Direct Select tool
- Quadratic and cubic bezier curve support
- Improved path rendering

## v1.0.1

- Multi-element selection and movement
- Alignment and distribution tools
- Context menu improvements

## v1.0.0

- Initial stable release
- Drawing tools (Rectangle, Ellipse, Line, Text, Path)
- Properties panel for element editing
- Parameter binding system
- SVGHMI export for WinCC Unified
- Layers panel with hierarchy view
- Live preview with parameter simulation
- Full undo/redo support
