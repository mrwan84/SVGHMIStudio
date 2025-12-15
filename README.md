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

1. Run `SVGHMIStudio_Setup_1.0.4.exe`
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

## Keyboard Shortcuts

| Action          | Shortcut     |
| --------------- | ------------ |
| New Document    | Ctrl+N       |
| Open            | Ctrl+O       |
| Save            | Ctrl+S       |
| Save As         | Ctrl+Shift+S |
| Export SVGHMI   | Ctrl+E       |
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
- **Python**: 3.10+ (if using Python package installation)

---

## File Types

| Extension | Description                                     |
| --------- | ----------------------------------------------- |
| `.svg`    | Standard SVG file (can be opened and edited)    |
| `.svghmi` | WinCC Unified HMI file with bindings and params |

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

### v1.0.4 (Current)

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
