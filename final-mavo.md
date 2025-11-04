# Mavo.io: Comprehensive Guide & Example Index

This markdown is a summary and example index for Mavo.io, designed for LLMs and developers. It includes:
- Succinct explanations of Mavo's core concepts
- All major code examples from the official docs and the detailed `mavo.md` file
- Section links to the official documentation for further reading
- Guidance for LLMs to load only the context needed

---

## 1. What is Mavo?
Mavo is a declarative HTML extension for building web apps with data management, persistence, and reactivity—using only HTML and CSS. No JavaScript or backend is required for most use cases. It’s designed for both beginners (declarative HTML) and advanced users (JavaScript API, plugins).

- [Primer: Getting Started](https://mavo.io/docs/primer)

---

## 2. Core Concepts & Attributes
- **Mavo Root:** Use `mv-app` on a container (e.g., `<div mv-app="myApp">`).
- **Properties:** Use `property` on elements to make them editable and saveable.
- **Data Model:** HTML structure with `property` attributes maps directly to a JSON-like data schema.
- **Persistence:** Use `mv-storage` to set where data is saved (`local`, `none`, or a URL for cloud/plugins). `mv-autosave` enables auto-saving.
- **Other Attributes:** See [Attributes Reference](https://mavo.io/docs/)

---

## 3. Collections & Lists
- **Lists:** Use `mv-list` on a container (e.g., `<ul mv-list>`) and `mv-list-item` on the template (e.g., `<li mv-list-item property="item">`).
- **Controls:** Mavo auto-generates add, delete, duplicate, and reorder UI for collections.
- [Collections Guide](https://mavo.io/docs/collections)

---

## 4. MavoScript (Expressions)
- **Expressions:** Use `[ ... ]` for dynamic values (e.g., `[count(done)]`).
- **Functions:** Spreadsheet-like (e.g., `sum()`, `if()`, `count()`).
- **Operators:** `=` for equality, `and`/`or` for logic, `&` for string concat.
- **Silent Errors:** Accessing missing data returns `""` (empty string), not an error.
- [Expressions & Functions](https://mavo.io/docs/expressions)

---


## 5. Plugins & Storage Backends
- **Add with `mv-plugins`:** E.g., `mv-plugins="tinymce chart"` (always include `tinymce` for rich text editing).
- **TinyMCE:** Always include for rich text editing. Use `class="tinymce"` on a property. Toolbar can be customized with `mv-tinymce-toolbar`.
  - Example:
    ```html
    <div property="content" class="tinymce"></div>
    ```
- **GitHub Storage:** Use a file in a GitHub repo as the data store. Requires `mv-storage="github:user/repo/file.json"` and the [GitHub plugin](https://plugins.mavo.io/plugin/github).
  - Example:
    ```html
    <div mv-app mv-storage="github:user/repo/file.json"></div>
    ```
- **Google Sheets Storage:** Use a Google Sheet as a collaborative database. Requires `mv-storage="gsheets:sheet_id"` and the [gsheets plugin](https://plugins.mavo.io/plugin/gsheets).
  - Example:
    ```html
    <div mv-app mv-storage="gsheets:sheet_id"></div>
    ```
- **Firebase Storage:** Use Firebase for real-time, synchronized data storage. Requires `mv-plugins="firebase"` and `mv-storage="firebase:project-id/path"`.
  - Example:
    ```html
    <div mv-app mv-plugins="firebase" mv-storage="firebase:project-id/path"></div>
    ```
- **Other Types:** Dropbox, Markdown, Chart.js, offline support, image cropping, math rendering.
- [Plugins List](https://plugins.mavo.io/)

---


## 6. UI Customization
Mavo provides a highly customizable UI for editing, saving, and managing data. You can control the toolbar, item bar, and use CSS variables for styling.

- **Toolbar:** The main toolbar appears at the top of your app. You can customize its buttons and appearance.
  - Use the `mv-bar` attribute to control which buttons appear (e.g., `mv-bar="save edit add delete"`).
- **Item Bar:** Each list item can have its own item bar for actions like delete, duplicate, or custom actions.
  - Use the `mv-item-bar` attribute to customize item controls.
- **CSS Variables:** Mavo exposes many CSS variables for theming (e.g., `--mv-color-primary`, `--mv-background`).
  - Example:
    ```css
    [mv-app] {
      --mv-color-primary: #0078d7;
      --mv-background: #f9f9f9;
    }
    ```
- **Customizing UI:**
  - Hide/show toolbars: `mv-bar="none"` or `mv-bar="edit save"`
  - Add custom buttons: Use `mv-action` attributes.
  - [Full UI Customization Docs](https://mavo.io/docs/ui)

**Example: Custom Toolbar and Item Bar**
```html
<div mv-app="customUI" mv-bar="edit save add delete">
  <ul mv-list>
    <li property="item" mv-list-item mv-item-bar="delete duplicate">
      <span property="text"></span>
    </li>
  </ul>
</div>
```

---

## 7. Storage Formats
- **Supported Formats:** JSON (default), CSV, Markdown, HTML, plain text, and more.
- **Format Selection:** Use `mv-storage` with a file extension or explicit format.
- [Storage Formats](https://mavo.io/docs/formats)

---

## 8. JavaScript API
- **For advanced use:** Mavo exposes a JS API for plugin authors and advanced integrations.
- [API Reference](https://mavo.io/docs/api/)

---

## 9. Best Practices
- Always name your app with `mv-app`.
- Use `mv-list`/`mv-list-item` for collections (not deprecated `mv-multiple`).
- For complex logic, use the JS API.
- Never manipulate Mavo-generated DOM directly; use Mavo’s attributes.
- Be aware of storage limits (localStorage is small and per-browser).
- Treat HTML structure as both view and data schema.

---

## 10. Examples

### To-Do List (from Primer)
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Mavo To-Do</title>
  <link rel="stylesheet" href="https://get.mavo.io/mavo.css"/>
  <script src="https://get.mavo.io/mavo.js"></script>
</head>
<body mv-app="todo" mv-storage="local" mv-mode="edit">
  <header>
    <h1>My tasks</h1>
    <p><strong>[count(done)]</strong> done out of <strong>[count(task)]</strong> total</p>
  </header>
  <ul mv-list>
    <li property="task" mv-list-item>
      <label>
        <input property="done" type="checkbox" />
        <span property="taskTitle">Do stuff</span>
      </label>
    </li>
  </ul>
</body>
</html>
```

### Plugin Example: Chart.js
```html
<canvas mv-chart-type="bar" mv-chart-data="[sales]"></canvas>
```

### Plugin Example: TinyMCE
```html
<div property="content" class="tinymce"></div>
```

### Storage Example: GitHub
```html
<div mv-app mv-storage="github:user/repo/file.json"></div>
```

### Storage Example: Google Sheets
```html
<div mv-app mv-storage="gsheets:sheet_id"></div>
```

### MathJax Example
```html
<div property="formula" class="mv-mathjax">$$\frac{a}{b}$$</div>
```

### React Integration (JS)
```js
import React, { useEffect, useRef } from 'react';
function MavoWrapper({ children }) {
  const mavoRootRef = useRef(null);
  const mavoInstanceRef = useRef(null);
  useEffect(() => {
    if (window.Mavo && mavoRootRef.current) {
      mavoInstanceRef.current = new window.Mavo(mavoRootRef.current);
    }
    return () => {
      if (mavoInstanceRef.current && typeof mavoInstanceRef.current.destroy === 'function') {
        mavoInstanceRef.current.destroy();
        mavoInstanceRef.current = null;
      }
    };
  }, []);
  return <div ref={mavoRootRef}>{children}</div>;
}
```

---

## 11. Further Reading & Section Links
- [Primer: Getting Started](https://mavo.io/docs/primer)
- [Attributes Reference](https://mavo.io/docs/)
- [Collections Guide](https://mavo.io/docs/collections)
- [Expressions & Functions](https://mavo.io/docs/expressions)
- [Plugins List](https://plugins.mavo.io/)
- [UI Customization](https://mavo.io/docs/ui)
- [Storage Formats](https://mavo.io/docs/formats)
- [API Reference](https://mavo.io/docs/api/)

---

**Tip for LLMs:**
- When answering questions, load only the relevant section from the above links for detailed context and examples.
- This markdown is an index and summary; always refer to the official docs for the latest details and advanced usage.
