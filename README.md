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
- **Align & Distribute** - Align multiple elements (left, center, right, top, middle, bottom). Choose the reference with the toolbar picker: **Sel** (selection bounds), **Key** (last-clicked element stays fixed, others align to it), or **Page** (document rect). Keyboard: `Alt+Shift+L / R / H / T / B / V`
- **Arrange** - Bring to front, send to back, move forward/backward
- **Keyboard-only navigation** - `Tab` / `Shift+Tab` cycle through top-level elements; `Enter` enters in-place edit on text / paths; arrow keys nudge (1 px, or 10 px with Shift)
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
- **All-bindings overview** - Expand the "All Bindings" section at the top of the Bindings panel to see every binding in the document at once. Filter by element / attribute / expression, spot broken references (missing element, missing parameter, missing local def), and bulk-rename `ParamProps.*` / `LocalProps.*` references across all bindings in one undo-able step

### Canvas, Grid & Snapping

- **Grid size presets** - Click the grid-size picker next to the Snap-to-Grid toggle for WinCC-compatible presets (5 / 10 / 20 / 25 / 50 px) or enter a custom value
- **Snap to grid** - Toggle with the toolbar button; combines with element snapping for precise alignment
- **Smart snapping** - Elements snap to edges, centers, and edge midpoints of other elements; cyan guides show equal spacing

### Autosave & Recovery

- **Automatic crash recovery** - Unsaved work is persisted to local storage a few seconds after you stop editing; if the app is closed or crashes unexpectedly, the Recovery dialog offers to restore on next launch
- **Schema-validated** - Recovery data is validated on load; corrupted or incompatible blobs surface a clear error rather than silently corrupting state

### Localization

- **English / German** - Switch languages from the `EN` / `DE` picker at the right edge of the menu bar. Preference persists across sessions

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

1. Run `SVGHMIStudio_Setup_3.5.6.exe`
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

### Selection & Alignment

| Action                       | Shortcut          |
| ---------------------------- | ----------------- |
| Cycle next top-level element | Tab               |
| Cycle previous element       | Shift+Tab         |
| Edit path / text in place    | Enter             |
| Nudge selection (1 px)       | Arrow keys        |
| Nudge selection (10 px)      | Shift+Arrow       |
| Align left                   | Alt+Shift+L       |
| Align right                  | Alt+Shift+R       |
| Align center (horizontal)    | Alt+Shift+H       |
| Align top                    | Alt+Shift+T       |
| Align bottom                 | Alt+Shift+B       |
| Align center (vertical)      | Alt+Shift+V       |

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

## Changelog

The full release history has moved to [CHANGELOG.md](./CHANGELOG.md).
