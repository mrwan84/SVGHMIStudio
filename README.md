# SVGHMIStudio

**SVG Editor with SVGHMI Export for Siemens WinCC Unified**

---

## Overview

SVGHMIStudio is a specialized SVG graphics editor designed for creating HMI (Human Machine Interface) graphics for Siemens WinCC Unified. It allows you to create, edit, and export SVG files with dynamic bindings that can be used directly in WinCC Unified projects.

---

## Features

### Drawing Tools

- **Rectangle** - Create rectangular shapes with rounded corners
- **Ellipse** - Create circles and ellipses
- **Line** - Draw straight lines
- **Text** - Add text elements with font customization
- **Path** - Create custom paths with the pen tool
- **Star/Polygon** - Create regular polygons and star shapes with configurable sides and inner ratio
- **Spiral** - Draw Archimedean spirals with configurable turns
- **Pencil** - Freehand drawing with automatic smoothing (Catmull-Rom to cubic bezier)
- **Eyedropper** - Pick colors from any element on the canvas
- **Measure** - Measure distances and angles between two points
- **Direct Select** - Edit individual path points

### Editing

- **Select & Move** - Click and drag elements
- **Multi-Select** - Shift+Click or drag to select multiple elements
- **Resize** - Use handles to resize shapes
- **Group/Ungroup** - Ctrl+G / Ctrl+Shift+G
- **Duplicate** - Ctrl+D to duplicate selected elements
- **Align & Distribute** - Align multiple elements (left, center, right, top, middle, bottom)
- **Arrange** - Bring to front, send to back, move forward/backward
- **Flip Horizontal/Vertical** - Mirror elements across axes
- **Transform Dialog** - Numeric Move, Scale, Rotate, and Skew with precise values (Ctrl+Shift+T)
- **Properties Panel** - Edit fill, stroke, opacity, and more
- **Layers Panel** - View and organize SVG hierarchy
- **Undo/Redo** - Full undo history with Ctrl+Z / Ctrl+Y

### Path Operations

- **Object to Path** - Convert any shape (rect, ellipse, line, polygon) to a path element (Ctrl+Shift+C)
- **Stroke to Path** - Convert an element's stroke into a filled path
- **Combine Paths** - Merge multiple paths into a single compound path (Ctrl+K)
- **Break Apart** - Split a compound path into separate path elements (Ctrl+Shift+K)
- **Reverse Path** - Reverse the direction of a path
- **Simplify Path** - Reduce path complexity using Ramer-Douglas-Peucker algorithm (Ctrl+L)
- **Boolean Operations** - Union, Difference, Intersection, and Exclusion with arc recovery and containment detection

### SVG Filters & Effects

- **Object Opacity** - Per-element opacity control
- **Gaussian Blur** - Apply configurable blur to any element
- **Drop Shadow** - Add drop shadows with adjustable offset, blur, and color
- **Blend Modes** - Apply CSS blend modes (multiply, screen, overlay, etc.)
- **Filters Panel** - Manage and stack filters on elements

### Markers & Clip Paths

- **Markers/Arrowheads** - Add arrow, circle, square, or diamond markers to line/path endpoints
- **Clip Path** - Use one shape to clip another; create and release clip paths via context menu

### Parameter Bindings

- Bind SVG attributes to PLC tags
- Supported parameter types:
  - **NUMBER** - Numeric values with min/max range
  - **BOOLEAN** - True/false values
  - **STRING** - Text values
  - **HMI_COLOR** - Color values (0xAARRGGBB format)
- Built-in converters:
  - **RGBA/RGB** - Color conversion
  - **Bounds** - Value clamping
  - **Darker/Lighter** - Color manipulation

### Live Preview

- Real-time preview with parameter simulation
- Adjust parameter values with sliders
- See bindings in action before export

### Export

- **SVGHMI Export** - Export for WinCC Unified with all bindings and proper DOCTYPE
- **Standard SVG** - Export as regular SVG file
- **Optimized SVG** - Plugin-based optimizer with 31 SVGO-inspired plugins
- **PNG Export** - Export as PNG image with configurable scale and background color

---

## Installation

1. Run `SVGHMIStudio_Setup_3.5.5.exe`
2. Follow the installation wizard
3. Choose installation location
4. Select "Create desktop shortcut" if desired
5. Click "Install"

---

## Getting Started

### Create New Document

1. **File → New** or `Ctrl+N`
2. Set canvas width and height
3. Click "Create"

### Open Existing SVG

1. **File → Open** or `Ctrl+O`
2. Select any `.svg` or `.svghmi` file
3. The file opens with all elements visible in the Layers panel

### Draw Shapes

1. Select a tool from the toolbar (Rectangle, Ellipse, Line, Text)
2. Click and drag on the canvas to create the shape
3. Use the Properties panel to adjust colors and styles

### Edit Elements

1. Use the **Select** tool (V) to click on elements
2. Drag to move, use handles to resize
3. Hold **Shift** to select multiple elements
4. Use **Ctrl+G** to group selected elements

### Add Parameters

1. Open the **Parameters** panel
2. Click **"Add Parameter"**
3. Configure:
   - **Name**: Parameter name (e.g., `FillColor`)
   - **Type**: NUMBER, BOOLEAN, STRING, HMI_COLOR, or HMI_FONT_PART
   - **Default**: Default value
   - **Min/Max**: For NUMBER type, set range limits
4. Click "Add"

### Add Bindings

1. Select an element on the canvas
2. Open the **Bindings** panel
3. Click **"Add Binding"**
4. Configure:
   - **Attribute**: Property to bind (e.g., `fill`, `width`, `opacity`)
   - **Expression**: Binding expression using converters
     - Example: `{{Converter.RGBA(ParamProps.FillColor)}}`
     - Example: `{{ParamProps.Value}}`
5. Click "Add"

### Test with Live Preview

1. Open the **Live Preview** panel
2. Adjust parameter sliders to see real-time changes
3. Click **Refresh** to update the preview

### Export for WinCC Unified

1. **File → Export SVGHMI** or `Ctrl+E`
2. Configure:
   - **Widget Name**: e.g., `extended.mywidget`
   - **Display Name**: Human-readable name
   - **Version**: Widget version
3. Click "Export"
4. Import the generated `.svghmi` file into your WinCC Unified project

---

## Expression Reference

Binding expressions are wrapped in double curly braces: `{{expression}}`. They link SVG attributes to HMI parameters so graphics update dynamically at runtime.

### Reference Syntax

| Syntax            | Description                  | Example                  |
| ----------------- | ---------------------------- | ------------------------ |
| `ParamProps.Name` | Reference a widget parameter | `ParamProps.FillColor`   |
| `LocalProps.Name` | Reference a local variable   | `LocalProps.ScaledValue` |
| `'text'`          | String literal               | `'visible'`              |
| `123` or `0.5`    | Numeric literal              | `100`, `0.75`            |

### Arithmetic & Comparison Operators

| Operator | Description    | Example                            |
| -------- | -------------- | ---------------------------------- |
| `+`      | Addition       | `ParamProps.X + 10`                |
| `-`      | Subtraction    | `ParamProps.Width - 20`            |
| `*`      | Multiplication | `LocalProps.NormalizedValue * 100` |
| `/`      | Division       | `ParamProps.Position / 100`        |
| `%`      | Modulo         | `ParamProps.Index % 2`             |
| `==`     | Equal          | `ParamProps.State == 1`            |
| `!=`     | Not equal      | `ParamProps.Error != 0`            |

### Ternary Operator

```
condition ? trueValue : falseValue
```

| Example                                                          | Description           |
| ---------------------------------------------------------------- | --------------------- |
| `ParamProps.IsActive ? 'visible' : 'hidden'`                     | Toggle visibility     |
| `gt(ParamProps.Temp, 100) ? 'red' : 'green'`                     | Conditional color     |
| `eq(ParamProps.Mode, 1) ? ParamProps.ColorA : ParamProps.ColorB` | Switch between params |

### Converter Functions

Converters transform parameter values for use in SVG attributes.

| Function                   | Signature                                      | Description                                               |
| -------------------------- | ---------------------------------------------- | --------------------------------------------------------- |
| `Converter.RGBA`           | `RGBA(HmiColor)`                               | Convert ARGB hex color (0xAARRGGBB) to `rgba()` CSS value |
| `Converter.RGB`            | `RGB(HmiColor)`                                | Extract RGB hex string, ignoring alpha                    |
| `Converter.Alpha`          | `Alpha(HmiColor)`                              | Extract alpha channel as 0.0–1.0                          |
| `Converter.Bounds`         | `Bounds(value, min, max)`                      | Clamp a number between min and max                        |
| `Converter.Min`            | `Min(num, num)`                                | Returns the minimum of two numbers                        |
| `Converter.Max`            | `Max(num, num)`                                | Returns the maximum of two numbers                        |
| `Converter.Darker`         | `Darker(HmiColor, deviation)`                  | Darken a color (deviation 0.0–1.0)                        |
| `Converter.Lighter`        | `Lighter(HmiColor, deviation)`                 | Lighten a color (deviation 0.0–1.0)                       |
| `Converter.Illuminate`     | `Illuminate(HmiColor, deviation, low?, high?)` | Illumination variant with optional range                  |
| `Converter.IsString`       | `IsString(value)`                              | Returns true if value is a string                         |
| `Converter.IsNumber`       | `IsNumber(value)`                              | Returns true if value is a number                         |
| `Converter.IsBoolean`      | `IsBoolean(value)`                             | Returns true if value is a boolean                        |
| `Converter.CountItems`     | `CountItems(array)`                            | Count items in an array                                   |
| `Converter.FormatPattern`  | `FormatPattern(value, pattern)`                | Format a value according to a pattern string              |
| `Converter.TextDecoration` | `TextDecoration(HmiFontPart)`                  | Returns text decoration for a font                        |
| `Converter.TextHeight`     | `TextHeight(string, HmiFontPart)`              | Returns text height for given text and font               |
| `Converter.TextWidth`      | `TextWidth(string, HmiFontPart)`               | Returns text width for given text and font                |
| `Converter.TextEllipsis`   | `TextEllipsis(string, width, HmiFontPart)`     | Truncates text with ellipsis if too wide                  |

### Comparator Functions

Return `true` or `false`. Arguments are numeric.

| Function    | Description                    | Example                     |
| ----------- | ------------------------------ | --------------------------- |
| `gt(a, b)`  | Greater than (a > b)           | `gt(ParamProps.Value, 50)`  |
| `ge(a, b)`  | Greater than or equal (a >= b) | `ge(ParamProps.Level, 0)`   |
| `lt(a, b)`  | Less than (a < b)              | `lt(ParamProps.Temp, 100)`  |
| `le(a, b)`  | Less than or equal (a <= b)    | `le(ParamProps.Speed, 200)` |
| `eq(a, b)`  | Equal (a === b)                | `eq(ParamProps.State, 1)`   |
| `ne(a, b)`  | Not equal (a !== b)            | `ne(ParamProps.Error, 0)`   |
| `has(a, b)` | Bitwise check                  | `has(ParamProps.Flags, 4)`  |

### Logical Functions

| Function    | Description | Example                                    |
| ----------- | ----------- | ------------------------------------------ |
| `and(a, b)` | Logical AND | `and(ParamProps.RunA, ParamProps.RunB)`    |
| `or(a, b)`  | Logical OR  | `or(ParamProps.Alarm1, ParamProps.Alarm2)` |
| `not(a)`    | Logical NOT | `not(ParamProps.IsHidden)`                 |

### Math Functions

| Function      | Description        | Function      | Description        |
| ------------- | ------------------ | ------------- | ------------------ |
| `abs(n)`      | Absolute value     | `sqrt(n)`     | Square root        |
| `round(n)`    | Round to integer   | `pow(n, exp)` | Power              |
| `floor(n)`    | Round down         | `log(n)`      | Natural logarithm  |
| `ceil(n)`     | Round up           | `log10(n)`    | Base-10 logarithm  |
| `sin(n)`      | Sine (radians)     | `asin(n)`     | Inverse sine       |
| `cos(n)`      | Cosine (radians)   | `acos(n)`     | Inverse cosine     |
| `tan(n)`      | Tangent (radians)  | `atan(n)`     | Inverse tangent    |
| `atan2(y, x)` | 2-arg arctangent   | `rad2deg(n)`  | Radians to degrees |
| `deg2rad(n)`  | Degrees to radians |               |                    |

### Bindable Attributes

These SVG attributes can be bound to parameters:

`fill`, `stroke`, `stroke-width`, `stroke-opacity`, `fill-opacity`, `opacity`, `x`, `y`, `width`, `height`, `rx`, `ry`, `cx`, `cy`, `r`, `x1`, `y1`, `x2`, `y2`, `transform`, `d`, `points`, `textContent`, `font-size`, `font-family`, `font-weight`, `visibility`, `display`, `href`, `offset`, `stop-color`, `stop-opacity`, `dur`

### Expression Examples

**Color binding with RGBA converter:**

```
{{Converter.RGBA(ParamProps.FillColor)}}
```

**Clamped position (0–100%):**

```
{{Converter.Bounds(ParamProps.Position / 100, 0.0, 1.0)}}
```

**Conditional visibility from boolean:**

```
{{ParamProps.IsVisible ? 'inline' : 'none'}}
```

**Temperature-based color with comparator:**

```
{{gt(ParamProps.Temperature, 80) ? 'red' : 'green'}}
```

**Darken a color by 30%:**

```
{{Converter.Darker(ParamProps.BaseColor, 0.3)}}
```

**Using a local variable:**

```
LocalDef "Scaled" = Converter.Bounds(ParamProps.RawValue / 100, 0.0, 1.0)
Binding: {{LocalProps.Scaled}}
```

**Rotation from parameter:**

```
{{rotate(ParamProps.Angle, 50, 50)}}
```

### Note: HmiProps (Runtime Only)

The WinCC runtime also provides `HmiProps.*` references (e.g., `HmiProps.Width`, `HmiProps.ProcessValue`, `HmiProps.IsActive`). These are built-in screen object model properties available at WinCC runtime only — they cannot be previewed in the editor. You can still use them in binding expressions; they will be exported correctly but won't resolve in the Live Preview.

---

## Keyboard Shortcuts

| Action           | Shortcut     |
| ---------------- | ------------ |
| New Document     | Ctrl+N       |
| Open             | Ctrl+O       |
| Save             | Ctrl+S       |
| Save As          | Ctrl+Shift+S |
| Export SVGHMI    | Ctrl+Shift+E |
| Validate WinCC   | Ctrl+Shift+V |
| Undo             | Ctrl+Z       |
| Redo             | Ctrl+Y       |
| Delete           | Delete       |
| Cut              | Ctrl+X       |
| Copy             | Ctrl+C       |
| Paste            | Ctrl+V       |
| Duplicate        | Ctrl+D       |
| Select All       | Ctrl+A       |
| Group            | Ctrl+G       |
| Ungroup          | Ctrl+Shift+G |
| Zoom In          | Ctrl++       |
| Zoom Out         | Ctrl+-       |
| Zoom 100%        | Ctrl+1       |
| Fit to Window    | Ctrl+0       |
| Refresh Preview  | F5           |
| Object to Path   | Ctrl+Shift+C |
| Combine Paths    | Ctrl+K       |
| Break Apart      | Ctrl+Shift+K |
| Simplify Path    | Ctrl+L       |
| Transform Dialog | Ctrl+Shift+T |

### Tool Shortcuts

| Tool          | Key |
| ------------- | --- |
| Select        | V   |
| Direct Select | A   |
| Rectangle     | R   |
| Ellipse       | E   |
| Line          | L   |
| Path/Pen      | P   |
| Text          | T   |
| Star/Polygon  | S   |
| Pencil        | N   |
| Eyedropper    | I   |
| Measure       | M   |

---

## Panels Overview

### Layers Panel (Left)

- Shows SVG element hierarchy
- Click to select elements
- Expand/collapse groups
- View element IDs and types

### Properties Panel (Right)

- Edit selected element properties
- Change fill and stroke colors
- Adjust opacity and stroke width
- Modify position and size

### Parameters Panel (Right)

- Define HMI parameters
- Set parameter types and defaults
- Configure min/max ranges

### Bindings Panel (Right)

- Create attribute bindings
- Use WinCC converters
- Link parameters to element properties

### Live Preview Panel (Right)

- Real-time visualization
- Parameter value sliders
- Animation preview

### Filters Panel (Right)

- Apply and manage SVG filters (blur, shadow)
- Configure blend modes per element
- Add/remove filter effects visually

### XML Editor (Bottom)

- View raw SVG/XML code
- Manual editing with syntax highlighting
- Apply changes with "Apply" button

---

## System Requirements

- **OS**: Windows 10 / 11 (64-bit)
- **RAM**: 4 GB minimum, 8 GB recommended
- **Disk Space**: 500 MB
- **Display**: 1920x1080 or higher recommended

---

## File Types

| Extension | Description                                     |
| --------- | ----------------------------------------------- |
| `.svg`    | Standard SVG file (can be opened and edited)    |
| `.svghmi` | WinCC Unified HMI file with bindings and params |

## Troubleshooting

### SVG not displaying correctly

- Complex filters and gradients render in Live Preview but may show simplified in canvas
- Check if all elements have valid IDs in the Layers panel

### Text position issues

- Text with `text-anchor="middle"` is centered correctly
- Font availability may affect rendering

### Export issues

- Ensure all parameters have valid names (no spaces or special characters)
- Check the validation results before export

---

## Support

- **GitHub**: https://github.com/mrwan84/SVGHMIStudio
- **Issues**: https://github.com/mrwan84/SVGHMIStudio/issues
- **Documentation**: https://github.com/mrwan84/SVGHMIStudio/wiki

---

## License

MIT License - © M.Alsouki

---

## Version History

### v3.5.5 (Current)

#### Align & Distribute Fixes

- **Context menu Align now works** — The Align submenu in the canvas context menu previously used a duplicated, out-of-date implementation that silently failed for many shape types. It now delegates to the same shared `alignElements` used by the Alignment toolbar, so both entry points behave identically
- **Align/Distribute respect rotated bounds** — Rotated elements are now aligned and distributed by their visual (axis-aligned) bounding box instead of their pre-rotation local bbox, matching what you see on screen
- **Rotation pivot follows translation** — When an align/distribute move translates a rotated element, the rotation center (`cx`, `cy`) in `transform="rotate(...)"` is shifted by the same offset, so the element translates by exactly the requested delta in document space (same pattern used by drag-to-move)

### v3.5.4

#### Animation Rotation Center Fix

- **Auto Center now works for all shape types** — The "Auto Center (from element)" button in the rotate animation panel now correctly computes the center for polygons, stars, polylines, lines, paths, and text (previously only rect/circle/ellipse were supported; everything else silently returned 0,0)

### v3.5.3

#### Toast Notification System

- **User-visible feedback** — File open/save errors, export success/failure, and validation results now shown as toast notifications instead of silent console errors
- **Auto-dismiss** — Success toasts dismiss after 4s, error toasts after 6s, with manual close button
- **Themed styling** — Color-coded left border (green/red/amber/blue) matching the notification type

#### Dashed Stroke Support

- **Dash pattern dropdown** — Custom dropdown selector with inline SVG preview lines for each dash preset (Solid, Dashed, Dotted, Dash-Dot, Dash-Dot-Dot)
- **Custom patterns** — "Custom..." option allows entering arbitrary `stroke-dasharray` values
- **Stroke dash offset** — New numeric input for `stroke-dashoffset` control
- **Bug fix** — Fixed a bug where selecting "Custom" wrote the literal string `'custom'` as the SVG attribute

#### Real-Time Expression Validation

- **Inline validation** — Binding expressions are now validated as you type in the Bindings panel
- **Reference checking** — Validates `ParamProps.*`, `LocalProps.*`, and `Converter.*()` references against defined parameters, local variables, and known converters
- **Syntax checks** — Detects unbalanced parentheses and unknown references
- **Disabled submit** — Add/Save buttons are disabled when validation errors are present

#### Batch Export

- **Multi-format export** — Export dialog now includes "Also export as SVG" and "Also export as PNG" checkboxes
- **PNG scale selector** — Choose 1x, 2x, or 4x resolution for PNG exports
- **Automatic naming** — Additional formats saved to the same directory with matching base filename

#### Smart Snap Enhancements

- **Snap to centers** — Elements snap to the center points of other elements
- **Snap to edge midpoints** — Elements snap to midpoints of other elements' edges
- **Smart spacing guides** — Detects and snaps to equal spacing between elements (horizontal and vertical)
- **Color-coded snap lines** — Edge snaps (magenta), center snaps (purple), spacing guides (cyan)
- **View menu controls** — New "Element Snapping" submenu with toggles for each snap type

### v3.5.2

#### Modern UI Redesign

- **New design system** — Introduced a comprehensive design token system with semantic surface elevations (`--bg-primary`, `--bg-secondary`, `--bg-sidebar`, `--bg-input`, `--bg-elevated`, `--bg-hover-subtle`), a text hierarchy (primary/secondary/tertiary/inverse), and soft accent variants
- **Modern indigo accent** — Replaced the dated Microsoft blue with a clean indigo palette (`#4f46e5` light / `#7c7aed` dark) for a more contemporary feel
- **Refined dark mode** — Deeper, richer dark palette with better contrast (slate-based) and properly tuned accent luminance
- **Shadow & radius scale** — Added `--shadow-xs` through `--shadow-xl` and `--radius-xs` through `--radius-xl` design tokens used consistently across all components
- **Motion tokens** — Standardized transition timings (`--transition-fast`, `--transition-base`) for consistent micro-interactions
- **Inter font stack** — Upgraded to Inter as the primary typeface with proper antialiasing, tabular numerals for shortcuts/zoom percentages, and optimized text rendering
- **Modern scrollbars** — Thin (10px) rounded scrollbars with transparent tracks for a cleaner look
- **Focus rings** — Visible accent-colored focus rings on all interactive elements for better accessibility

#### Layout & Navigation Improvements

- **Tool rail repositioned** — Drawing tools moved to the leftmost edge (Figma/Sketch convention) with a wider 44px rail for better ergonomics
- **Grouped tool palette** — Drawing tools are now organized into logical clusters (Select / Shapes / Drawing / Utilities) separated by subtle dividers
- **Active tool indicator** — Active drawing tool now shows an accent fill with shadow plus a left-edge indicator pill
- **Modern segmented tab bar** — Right sidebar tabs replaced with pill-shaped segmented buttons (accent fill on active tab) instead of the old underline style
- **Polished resize handles** — Subtle 1px idle line that reveals a grip indicator pill on hover, turning solid accent while dragging
- **Backdrop tint** — Subtle secondary background tint between panels for better visual separation

#### Toolbar & Menu Polish

- **Taller toolbar** (44px) with modern rounded buttons, smooth hover states, and accent-soft highlights for active buttons
- **Zoom percentage chip** styled as a bordered input pill with tabular numerals
- **Filename badge** — Current filename displayed in a rounded pill with a colored dirty indicator dot
- **Menu bar refinements** — Taller menu bar (34px), rounded menu buttons with accent-soft active state
- **Elevated dropdowns** — Menu dropdowns now use larger radius, layered shadows, and full accent hover fill on items (instead of flat gray)
- **Shortcut chips** in menus use tabular numerals for better alignment

#### Dialog Modernization

- **Animated entrances** — Dialogs now fade in with a subtle scale and translate animation
- **Backdrop blur** — Modal backdrop now uses a blur effect for better focus on the dialog content
- **Header & footer dividers** — Clearer visual structure with bordered header and footer action bars
- **Larger radius** (14px) and richer multi-layer shadows for a premium feel
- **Enhanced form controls** — Inputs, selects, and checkboxes have hover states, focus rings, and better padding
- **Hover-lift OK button** — Primary action button subtly lifts on hover for better affordance

### v3.5.1

#### Group & Transform Fixes

- **Fixed group bounding box ignoring child transforms** — Groups containing rotated, skewed, or otherwise transformed children now show an accurate selection box using DOM `getBBox()` instead of computing the union of untransformed attribute bounds
- **Fixed moving groups with rotated children** — Moving a group now correctly updates the rotation center (`cx`, `cy`) of each child's `transform="rotate(...)"`, preventing elements from jumping to incorrect positions
- **Fixed spurious history entries on click/double-click** — Clicking or double-clicking an element without actually moving it no longer pushes empty "Move element" or "Resize element" entries to the undo history

#### CSS Style Inlining Improvements

- **CSS inlining now uses the SVGO optimizer pipeline** — Both the file-open dialog and the Edit menu action run the `inlineCssStyles` + `convertStyleToAttrs` optimizer plugins via `runOptimizer()`, ensuring consistent behavior across all inlining paths
- **Fixed inline `style=""` attributes not being processed** — SVGs with `style=""` attributes (e.g., `style="stop-color:#1d4ed8"` on gradient stops) but no `<style>` tags are now correctly inlined
- **Fixed CSS parser breaking on nested @-rules** — Replaced naive `split('}')` parsing with brace-depth-aware block extraction, correctly handling `@keyframes`, `@media`, and other nested CSS rules

#### Additional Bug Fixes

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

### v3.5.0

#### CSS Style Inlining

- **Automatic CSS `<style>` inlining on file open** — SVGs using `<style>` tags with class selectors (e.g., `.box { fill: #e6f2ff; }`) are automatically resolved into individual SVG attributes on each element, making them editable in the Properties panel and compatible with WinCC
- **Edit > Inline CSS Styles** menu action — Manually inline CSS styles on already-open documents
- **Inline CSS Styles dialog** — Shows a summary of inlined rules with before/after preview
- **Optimizer plugin** — `inlineCssStyles` plugin added to the SVG optimizer pipeline for export-time CSS inlining
- Supports `.class`, `tag`, `#id`, `tag.class`, and comma-separated selector groups
- Respects specificity: inline `style=""` > element attributes > class rules > tag rules

#### Text Element Handling Improvements

- Fixed browser native text selection interfering with canvas element selection (added `user-select: none` to SVG canvas)
- Fixed `<tspan>` elements being individually selectable — clicking any part of a `<text>` element now selects the whole text, not individual tspans
- Fixed text elements with `text-anchor` attribute (middle/end) shifting left when dragged — now uses delta-based positioning instead of absolute
- Fixed text bounding box calculation using DOM `getBBox()` for accurate bounds (handles tspan children, fonts, and text-anchor correctly)
- Fixed tspan child coordinates not updating when moving or resizing text elements

#### Bug Fixes

- Fixed `font-size` with `px` unit (e.g., "14px") not displaying in the Properties panel number input

### v3.4.0

#### SVG Optimizer — SVGO-Inspired Plugin Engine

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

#### Bug Fixes

- Fixed exported SVGHMI files missing DOCTYPE declaration and attribution comment
- Fixed ExportDialog missing `extended.` prefix validation on widget name
- Fixed ExportDialog missing opacity range validation (0.0–1.0)
- Fixed ExportDialog missing orphan binding check for deleted elements
- Added SVG interactivity event attributes (onclick, onmouseover, etc.) and animation event attributes (onbegin, onend, onrepeat) to WinCC validation warnings

### v3.3.0

#### New Drawing Tools

- **Star/Polygon tool** (S) - Draw regular polygons and stars with configurable sides (3-32) and inner radius ratio
- **Spiral tool** - Draw Archimedean spirals with configurable turns (1-20)
- **Pencil/Freehand tool** (N) - Freehand drawing with automatic Catmull-Rom to cubic bezier smoothing
- **Eyedropper tool** (I) - Pick fill color from any element on the canvas
- **Measure tool** (M) - Measure distance (px), angle (degrees), and dx/dy between two points on the canvas
- **Contextual Tool Options bar** - Adjustable parameters per tool (star sides/mode/inner ratio, spiral turns, pencil smoothing)

#### Path Operations (new Path menu)

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

#### SVG Filters & Effects

- **Object Opacity** slider in Properties panel (0-1 range)
- **Gaussian Blur** slider - Apply and adjust feGaussianBlur filter dynamically
- **Drop Shadow** - Configurable feDropShadow with dx, dy, blur radius, and color
- **Blend Modes** dropdown - Apply mix-blend-mode (multiply, screen, overlay, darken, lighten, etc.)
- **Filters Panel** - Dedicated panel for managing filter effects on selected elements

#### Transform & Flip

- **Flip Horizontal / Flip Vertical** - Mirror elements across bounding box center (in Edit menu and context menu)
- **Transform Dialog** (Ctrl+Shift+T) - Numeric input for Move (dx/dy), Scale (sx/sy%), Rotate (angle + center), and Skew (horizontal/vertical)

#### Markers & Arrowheads

- Add start/end markers to line, path, polyline, and polygon elements
- Predefined marker types: Arrow, Circle, Square, Diamond
- Marker defs management with automatic cleanup of unused markers

#### Clip Path Support

- **Create Clip Path** - Use one shape to clip another via context menu
- **Release Clip Path** - Restore clipped elements to normal rendering

#### PNG Export

- **File > Export as PNG** - Export current document as PNG with configurable scale (1x, 2x) and background color

#### Bug Fixes

- Fixed S (smooth cubic) and T (smooth quadratic) SVG path commands not tracking reflected control points correctly
- Fixed Sutherland-Hodgman clipping requiring CW-normalized input polygons for correct intersection results
- Fixed `replaceElementInTree` always creating new array references even when no replacement occurred
- Fixed polygon/polyline empty `points` attribute causing NaN corruption in offset and flip operations
- Fixed division by zero in `arcToCubicBeziers` when arc half-segment is zero
- Fixed export-utils attribute filter using wrong logical operator (attributes were not stripped correctly)

### v3.2.0

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

### v3.1.0

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

### v3.0.1

- Window title shows file path and unsaved indicator (\*)
- Theme preference persists across app restarts
- Unsaved changes guard on window close
- Parameter/binding/local def changes now mark document as dirty
- Full expression reference added to documentation

### v3.0.0

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

### v2.0.0

- Modern VS Code/Figma-inspired UI redesign
- Fluent UI-style SVG icons replacing all system icons
- System theme detection (auto dark/light mode from Windows settings)
- Comprehensive theme-aware QSS stylesheet for all widgets
- Thin modern scrollbars, flat menus, clean dock panels
- Theme menu with System/Light/Dark options
- Fixed transform rendering (SVG column-vector convention)
- Fixed movement and resize of transformed elements
- Text font controls (font family dropdown, size, bold, italic, alignment)

### v1.0.5

- Added light/dark theme support (View → Theme)
- Added logging infrastructure replacing all print() statements
- Added OS clipboard support for copy/paste between instances
- Added rotation handle and flip horizontal/vertical transforms
- Added stroke properties UI (line cap, line join, dash pattern)
- Added opacity slider with visual canvas feedback
- Added element snapping (snap to edges and centers)
- Fixed shortcut conflict (Ctrl+Shift+G)
- Fixed opacity not rendering on canvas

### v1.0.4

- Fixed text positioning with text-anchor support
- Fixed SVG rendering for complex nested groups
- Added style inheritance from parent elements
- Improved transform support (translate, scale, rotate, matrix)
- Fixed selection not changing element position on click
- Added letter-spacing support for text

### v1.0.3

- Canvas refresh after duplicate/paste operations
- Group alignment and distribution support
- Duplicate ID generation fixes

### v1.0.2

- Path editing with Direct Select tool
- Quadratic and cubic bezier curve support
- Improved path rendering

### v1.0.1

- Multi-element selection and movement
- Alignment and distribution tools
- Context menu improvements

### v1.0.0

- Initial stable release
- Drawing tools (Rectangle, Ellipse, Line, Text, Path)
- Properties panel for element editing
- Parameter binding system
- SVGHMI export for WinCC Unified
- Layers panel with hierarchy view
- Live preview with parameter simulation
- Full undo/redo support
