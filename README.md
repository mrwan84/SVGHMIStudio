# SVGHMIStudio

**SVG Editor with SVGHMI Export for Siemens WinCC Unified**

---

## Overview

SVGHMIStudio is a specialized SVG graphics editor designed for creating HMI (Human Machine Interface) graphics for Siemens WinCC Unified. It allows you to create, edit, and export SVG files with dynamic bindings that can be used directly in WinCC Unified projects.

---

## Features

### Drawing Tools

- **Rectangle** - Create rectangular shapes
- **Ellipse** - Create circles and ellipses
- **Line** - Draw straight lines
- **Text** - Add text elements
- **Path** - Create custom paths

### Editing

- **Select & Move** - Click and drag elements
- **Resize** - Use handles to resize shapes
- **Properties Panel** - Edit fill, stroke, opacity, and more
- **Layers Panel** - Organize elements in layers
- **Undo/Redo** - Ctrl+Z / Ctrl+Y

### Parameter Bindings

- Bind SVG attributes to tags
- Supported binding types:
  - **Direct** - Direct value assignment
  - **Range** - Map value ranges to visual properties
  - **Color** - Dynamic color changes
  - **Visibility** - Show/hide elements based on values
  - **Rotation** - Rotate elements based on values
  - **Animation** - Animate properties

### Export

- **SVGHMI Export** - Export for WinCC Unified with all bindings
- **Standard SVG** - Export as regular SVG file
- **Optimized SVG** - Compressed and optimized output

---

## Installation

1. Run `SVGHMIStudio_Setup_0.1.0.exe`
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

### Draw Shapes

1. Select a tool from the toolbar (Rectangle, Ellipse, Line, Text)
2. Click and drag on the canvas to create the shape
3. Use the Properties panel to adjust colors and styles

### Add Parameter Bindings

1. Select an element on the canvas
2. Open the **Bindings** panel
3. Click **"Add Binding"**
4. Configure:
   - **Parameter**: tag name (e.g., `Tank1_Level`)
   - **Attribute**: Property to bind (e.g., `height`, `fill`, `opacity`)
   - **Binding Type**: How the value affects the attribute
5. Click "OK"

### Export for WinCC Unified

1. **File → Export SVGHMI** or `Ctrl+E`
2. Choose export location
3. Configure export options
4. Click "Export"
5. Import the generated `.svghmi` file into your WinCC Unified project

---

## Keyboard Shortcuts

| Action        | Shortcut     |
| ------------- | ------------ |
| New Document  | Ctrl+N       |
| Open          | Ctrl+O       |
| Save          | Ctrl+S       |
| Save As       | Ctrl+Shift+S |
| Export SVGHMI | Ctrl+E       |
| Undo          | Ctrl+Z       |
| Redo          | Ctrl+Y       |
| Delete        | Delete       |
| Cut           | Ctrl+X       |
| Copy          | Ctrl+C       |
| Paste         | Ctrl+V       |
| Select All    | Ctrl+A       |
| Zoom In       | Ctrl++       |
| Zoom Out      | Ctrl+-       |
| Fit to Window | Ctrl+0       |

---

## System Requirements

- **OS**: Windows 10 / 11 (64-bit)
- **RAM**: 4 GB minimum, 8 GB recommended
- **Disk Space**: 500 MB
- **Display**: 1920x1080 or higher recommended

---

## File Types

| Extension | Description                                   |
| --------- | --------------------------------------------- |
| `.svg`    | Standard SVG file                             |
| `.svghmi` | SVGHMIStudio project file (includes bindings) |

---

## Support

- **GitHub**: https://github.com/mrwan84/SVGHMIStudio
- **Issues**: https://github.com/mrwan84/SVGHMIStudio/issues

---

## License

MIT License - © M.Alsouki

---

## Version History

### v0.1.0 (Initial Release)

- Basic drawing tools (Rectangle, Ellipse, Line, Text, Path)
- Properties panel for element editing
- Parameter binding system
- SVGHMI export for WinCC Unified
- Layers panel
- Live preview with animations
- Undo/Redo support
