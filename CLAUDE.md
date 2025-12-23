# CLAUDE.md - AI Assistant Guide for Greasy-Fork Repository

## Project Overview

**Claude Token Saver** is a Tampermonkey userscript (v35) designed to optimize Claude.ai workflows by preventing token waste and timeout errors. It enforces file creation instead of chat pasting for large outputs.

- **Type**: Browser userscript (Tampermonkey/Greasemonkey)
- **Target Site**: `https://claude.ai/*`
- **Language**: Vanilla JavaScript (ES6+)
- **No Build System**: Direct execution without compilation
- **No Dependencies**: Pure JavaScript, no npm packages

## Repository Structure

```
/home/user/Greasy-Fork/
├── CLAUDE.md          # This file - AI assistant guide
├── README.md          # User-facing documentation (143 lines)
├── Saver.md           # Main userscript code (1073 lines)
└── image.png          # UI screenshot (679x286 px)
```

This is a minimal, flat repository with no subdirectories for code organization.

## Key Files

| File | Purpose |
|------|---------|
| `Saver.md` | The complete userscript - contains all JavaScript code with Tampermonkey metadata header |
| `README.md` | Installation guide, features, usage instructions for end users |
| `image.png` | Screenshot showing the draggable floating panel UI |

## Code Architecture (Saver.md)

The userscript follows a **modular manager pattern** within an IIFE:

```javascript
(function() {
    'use strict';

    // 1. CONFIG - Configuration constants
    // 2. state - Centralized state management
    // 3. utils - Utility functions (debounce, clipboard)
    // 4. statusManager - UI status indicator
    // 5. dragManager - Drag-and-drop functionality
    // 6. monitoringManager - Response length monitoring
    // 7. fileManager - File detection and commands
    // 8. uiManager - Panel creation and toggling
    // 9. promptInjector - Automatic prompt enhancement
    // 10. init() - Initialization function
})();
```

### Manager Modules

| Manager | Responsibility | Key Methods |
|---------|---------------|-------------|
| `statusManager` | Color-coded status indicator (green/yellow/red) | `create()`, `update()`, `close()` |
| `dragManager` | Panel and status indicator repositioning | `panelStart/Move/End()`, `statusStart/Move/End()` |
| `monitoringManager` | Detect long responses and paste patterns | `checkResponse()`, `start()`, `observeMessages()` |
| `fileManager` | Detect uploads and generate commands | `detect()`, `updateList()`, `copyViewCommand()` |
| `uiManager` | Create and manage floating panel UI | `createPanel()`, `toggleMaximize()` |
| `promptInjector` | Auto-append file creation reminders | `enhance()` |

### Configuration Constants

```javascript
CONFIG = {
    warningThreshold: 500,      // Yellow alert at 500 chars
    dangerThreshold: 1000,      // Red alert at 1000+ chars
    checkInterval: 500,         // Response check frequency (ms)
    fileCheckInterval: 2000,    // File detection frequency (ms)
    statusUpdateDebounce: 2000, // Debounce status updates (ms)
    pastePatterns: [...],       // Markdown/code patterns
    fileKeywords: [...]         // File creation indicators
}
```

## Coding Conventions

### JavaScript Style
- `'use strict';` directive at start
- Single quotes for strings
- Semicolons required
- camelCase for variables/functions
- UPPERCASE for configuration constants
- 4-space indentation

### Emoji Logging Convention
Console logs use emoji prefixes for visual categorization:
- `🎯` - Script initialization
- `✅` - Success operations
- `❌` - Errors
- `⚠️` - Warnings
- `📋` - Clipboard operations
- `📁` - File operations
- `🔍` - Search/scan operations
- `🖱️` - User interactions
- `🆕` - New detections
- `🔄` - Refresh/retry operations

### Section Comments
Code sections are marked with comment dividers:
```javascript
// ==================== SECTION NAME ====================
```

### CSS-in-JS Styling
- Inline styles using `element.style.cssText`
- Template literals for multi-line CSS
- Tailwind-inspired color palette:
  - Cyan: `#06b6d4`, `#0891b2`
  - Green: `#10b981`
  - Yellow: `#f59e0b`
  - Red: `#ef4444`
  - Slate grays: `#f8fafc`, `#e2e8f0`, `#cbd5e1`, `#94a3b8`, `#64748b`, `#334155`, `#1e293b`
- Font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`

## Development Workflow

### Making Changes
1. Edit `Saver.md` directly (it's the complete userscript)
2. Update version number in Tampermonkey metadata header (`@version`)
3. Update version references in console logs and UI
4. Test in browser by copying to Tampermonkey
5. Update `README.md` if features change

### Version Numbering
- Current version: 35
- Increment for any functional change
- Located in:
  - `// @version 35` (metadata header, line 4)
  - Console logs mentioning "v35"

### Testing
No automated tests. Manual testing approach:
1. Copy script to Tampermonkey
2. Navigate to claude.ai
3. Use built-in test button (`🧪 Test`) in maximized panel
4. Check console for emoji-prefixed logs
5. Verify drag functionality, status updates, file detection

## Common Tasks for AI Assistants

### Adding a New Feature
1. Identify appropriate manager module or create new one
2. Follow existing patterns (manager object with methods)
3. Add configuration options to `CONFIG` if needed
4. Update state management in `state` object if needed
5. Add emoji-prefixed console logging
6. Update README.md if user-facing

### Modifying Thresholds
Edit `CONFIG` object at top of script:
```javascript
warningThreshold: 500,  // Change warning level
dangerThreshold: 1000,  // Change danger level
```

### Adding New Pattern Detection
Add to `CONFIG.pastePatterns` or `CONFIG.fileKeywords` arrays.

### Modifying UI
- Panel structure: `uiManager.createPanel()` and related methods
- Status indicator: `statusManager.create()` and `update()`
- Styling: CSS-in-JS within respective create methods

### Debugging
Check browser console for emoji-prefixed logs:
```
🎯 Token Saver v35 - Script loaded!
🚀 Token Saver v35 initializing...
✅ Token Saver v35 initialized successfully!
```

## Important Notes

### File Format
`Saver.md` uses Markdown extension but contains JavaScript. The Tampermonkey metadata header (`// ==UserScript==`) must remain at the top for the script to be recognized.

### Browser APIs Used
- `navigator.clipboard` - Modern clipboard API
- `document.execCommand('copy')` - Fallback clipboard
- `MutationObserver` - DOM change detection
- `setTimeout`/`setInterval` - Timing
- No `@grant` permissions required

### DOM Selectors
The script targets Claude.ai specific elements:
- `[data-test-render-count]` - Response elements
- `[class*="attachment"]`, `[class*="file"]` - File indicators
- `textarea`, `[contenteditable="true"]` - Input areas

### Position Defaults (v35)
- Panel: `top: 20px; right: 100px;`
- Status indicator: `top: 100px; right: 20px;`

## Git Workflow

- Main development happens on feature branches
- Commit messages should be descriptive
- No CI/CD pipeline (simple userscript project)

## Quick Reference

| Action | Location |
|--------|----------|
| Change character thresholds | `CONFIG.warningThreshold`, `CONFIG.dangerThreshold` |
| Modify status colors | `statusManager.update()` → `statusConfig` object |
| Add paste patterns | `CONFIG.pastePatterns` array |
| Add file keywords | `CONFIG.fileKeywords` array |
| Modify UI layout | `uiManager.createPanel()` and column methods |
| Change default positions | `uiManager.createPanel()` (panel), `statusManager.create()` (indicator) |
