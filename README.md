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
- **Direct Select** - Edit individual path points

### Editing

- **Select & Move** - Click and drag elements
- **Multi-Select** - Shift+Click or drag to select multiple elements
- **Resize** - Use handles to resize shapes
- **Group/Ungroup** - Ctrl+G / Ctrl+Shift+G
- **Duplicate** - Ctrl+D to duplicate selected elements
- **Align & Distribute** - Align multiple elements (left, center, right, top, middle, bottom)
- **Arrange** - Bring to front, send to back, move forward/backward
- **Properties Panel** - Edit fill, stroke, opacity, and more
- **Layers Panel** - View and organize SVG hierarchy
- **Undo/Redo** - Full undo history with Ctrl+Z / Ctrl+Y

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
- **Optimized SVG** - Compressed and optimized output

---

## Installation

1. Run `SVGHMIStudio_Setup_3.1.0.exe`
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

| Syntax | Description | Example |
| --- | --- | --- |
| `ParamProps.Name` | Reference a widget parameter | `ParamProps.FillColor` |
| `LocalProps.Name` | Reference a local variable | `LocalProps.ScaledValue` |
| `'text'` | String literal | `'visible'` |
| `123` or `0.5` | Numeric literal | `100`, `0.75` |

### Arithmetic & Comparison Operators

| Operator | Description | Example |
| --- | --- | --- |
| `+` | Addition | `ParamProps.X + 10` |
| `-` | Subtraction | `ParamProps.Width - 20` |
| `*` | Multiplication | `LocalProps.NormalizedValue * 100` |
| `/` | Division | `ParamProps.Position / 100` |
| `%` | Modulo | `ParamProps.Index % 2` |
| `==` | Equal | `ParamProps.State == 1` |
| `!=` | Not equal | `ParamProps.Error != 0` |

### Ternary Operator

```
condition ? trueValue : falseValue
```

| Example | Description |
| --- | --- |
| `ParamProps.IsActive ? 'visible' : 'hidden'` | Toggle visibility |
| `gt(ParamProps.Temp, 100) ? 'red' : 'green'` | Conditional color |
| `eq(ParamProps.Mode, 1) ? ParamProps.ColorA : ParamProps.ColorB` | Switch between params |

### Converter Functions

Converters transform parameter values for use in SVG attributes.

| Function | Signature | Description |
| --- | --- | --- |
| `Converter.RGBA` | `RGBA(HmiColor)` | Convert ARGB hex color (0xAARRGGBB) to `rgba()` CSS value |
| `Converter.RGB` | `RGB(HmiColor)` | Extract RGB hex string, ignoring alpha |
| `Converter.Alpha` | `Alpha(HmiColor)` | Extract alpha channel as 0.0–1.0 |
| `Converter.Bounds` | `Bounds(value, min, max)` | Clamp a number between min and max |
| `Converter.Min` | `Min(num, num)` | Returns the minimum of two numbers |
| `Converter.Max` | `Max(num, num)` | Returns the maximum of two numbers |
| `Converter.Darker` | `Darker(HmiColor, deviation)` | Darken a color (deviation 0.0–1.0) |
| `Converter.Lighter` | `Lighter(HmiColor, deviation)` | Lighten a color (deviation 0.0–1.0) |
| `Converter.Illuminate` | `Illuminate(HmiColor, deviation, low?, high?)` | Illumination variant with optional range |
| `Converter.IsString` | `IsString(value)` | Returns true if value is a string |
| `Converter.IsNumber` | `IsNumber(value)` | Returns true if value is a number |
| `Converter.IsBoolean` | `IsBoolean(value)` | Returns true if value is a boolean |
| `Converter.CountItems` | `CountItems(array)` | Count items in an array |
| `Converter.FormatPattern` | `FormatPattern(value, pattern)` | Format a value according to a pattern string |
| `Converter.TextDecoration` | `TextDecoration(HmiFontPart)` | Returns text decoration for a font |
| `Converter.TextHeight` | `TextHeight(string, HmiFontPart)` | Returns text height for given text and font |
| `Converter.TextWidth` | `TextWidth(string, HmiFontPart)` | Returns text width for given text and font |
| `Converter.TextEllipsis` | `TextEllipsis(string, width, HmiFontPart)` | Truncates text with ellipsis if too wide |

### Comparator Functions

Return `true` or `false`. Arguments are numeric.

| Function | Description | Example |
| --- | --- | --- |
| `gt(a, b)` | Greater than (a > b) | `gt(ParamProps.Value, 50)` |
| `ge(a, b)` | Greater than or equal (a >= b) | `ge(ParamProps.Level, 0)` |
| `lt(a, b)` | Less than (a < b) | `lt(ParamProps.Temp, 100)` |
| `le(a, b)` | Less than or equal (a <= b) | `le(ParamProps.Speed, 200)` |
| `eq(a, b)` | Equal (a === b) | `eq(ParamProps.State, 1)` |
| `ne(a, b)` | Not equal (a !== b) | `ne(ParamProps.Error, 0)` |
| `has(a, b)` | Bitwise check | `has(ParamProps.Flags, 4)` |

### Logical Functions

| Function | Description | Example |
| --- | --- | --- |
| `and(a, b)` | Logical AND | `and(ParamProps.RunA, ParamProps.RunB)` |
| `or(a, b)` | Logical OR | `or(ParamProps.Alarm1, ParamProps.Alarm2)` |
| `not(a)` | Logical NOT | `not(ParamProps.IsHidden)` |

### Math Functions

| Function | Description | Function | Description |
| --- | --- | --- | --- |
| `abs(n)` | Absolute value | `sqrt(n)` | Square root |
| `round(n)` | Round to integer | `pow(n, exp)` | Power |
| `floor(n)` | Round down | `log(n)` | Natural logarithm |
| `ceil(n)` | Round up | `log10(n)` | Base-10 logarithm |
| `sin(n)` | Sine (radians) | `asin(n)` | Inverse sine |
| `cos(n)` | Cosine (radians) | `acos(n)` | Inverse cosine |
| `tan(n)` | Tangent (radians) | `atan(n)` | Inverse tangent |
| `atan2(y, x)` | 2-arg arctangent | `rad2deg(n)` | Radians to degrees |
| `deg2rad(n)` | Degrees to radians | | |

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

| Action          | Shortcut     |
| --------------- | ------------ |
| New Document    | Ctrl+N       |
| Open            | Ctrl+O       |
| Save            | Ctrl+S       |
| Save As         | Ctrl+Shift+S |
| Export SVGHMI   | Ctrl+Shift+E |
| Validate WinCC  | Ctrl+Shift+V |
| Undo            | Ctrl+Z       |
| Redo            | Ctrl+Y       |
| Delete          | Delete       |
| Cut             | Ctrl+X       |
| Copy            | Ctrl+C       |
| Paste           | Ctrl+V       |
| Duplicate       | Ctrl+D       |
| Select All      | Ctrl+A       |
| Group           | Ctrl+G       |
| Ungroup         | Ctrl+Shift+G |
| Zoom In         | Ctrl++       |
| Zoom Out        | Ctrl+-       |
| Zoom 100%       | Ctrl+1       |
| Fit to Window   | Ctrl+0       |
| Refresh Preview | F5           |

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

---

## Building from Source

### Prerequisites

- **Node.js** 18+ ([nodejs.org](https://nodejs.org))
- **Rust** toolchain ([rustup.rs](https://rustup.rs))
- **Tauri CLI**: `cargo install tauri-cli`

### Development

```bash
npm install
npm run tauri dev
```

This starts a development server with hot-reload. The Tauri window opens automatically.

### Production Build

```bash
npm run tauri build
```

The installer (`.msi` / `.exe`) is generated in `src-tauri/target/release/bundle/`.

### Frontend Only (no Tauri)

```bash
npm run dev
```

Opens the web UI at `http://localhost:1420` without the Tauri backend. File I/O and export features require the full Tauri build.

---

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

### v3.1.0 (Current)

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

- Window title shows file path and unsaved indicator (*)
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
