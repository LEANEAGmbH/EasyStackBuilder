

# Leanea Browser JS

A Node-RED node that runs arbitrary JavaScript in the Node-RED editor's browser context. Trigger custom scripts on incoming messages to extend the editor UI, integrate with browser APIs, or automate tasks.

---

## Table of Contents

- [🚀 Overview](#-overview)
- [📦 Installation](#-installation)
- [🛠️ How to Use](#️-how-to-use)
- [🔧 Node Configuration](#-node-configuration)
- [📁 Directory Contents](#-directory-contents)
- [📦 Dependencies](#-dependencies)
- [🔗 Links & References](#-links--references)
- [License](#license)

---

## 🚀 Overview

The **browser-js** node bridges Node-RED runtime messages with the Node-RED editor (browser) context. On each incoming message, it publishes the message payload to the editor via `RED.comms`, then executes your custom JavaScript snippet in the browser page. Use this to automate editor tasks, manipulate UI elements, or integrate with web APIs directly from Node-RED flows.

---

## 📦 Installation

1. Download `node-red-leanea-browser-js-0.0.2.tgz` from this repository.
2. In the [ORCE](https://github.com/LEANEAGmbH/EasyStackBuilder/tree/main/ORCE) editor, click "New Node" → **Install** tab.
3. Click **Upload** and select the `node-red-leanea-browser-js-0.0.2.tgz` file.
4. Alternatively, from your Node-RED user directory:

```bash
npm install /path/to/node-red-leanea-browser-js-0.0.2.tgz
```

---

## 🛠️ How to Use

1. **Deploy** your flow.
2. In your flow, **drag** the **browser-js** node from the **function** category.
3. **Double-click** the node to open its edit dialog.
4. Enter your JavaScript code in the **"JavaScript to run"** field. Example snippet:

```js
// Log payload to browser console
console.log("Received msg:", msg.payload);
```

5. **Connect** an Inject node (or any source) to **browser-js**.
6. Click **Done** and **Deploy**.
7. Trigger the inject node. The code will execute in the Node-RED editor's browser context.

---

## 🔧 Node Configuration

| Property              | Description                              | Required |
| --------------------- | ---------------------------------------- | -------- |
| **Name**              | Custom name for the node instance        | No       |
| **JavaScript to run** | Your code snippet; receives `msg` object | Yes      |

---

## 📁 Directory Contents

```
.
├── node-red-leanea-browser-js-0.0.2.tgz
├── browser-js.html
├── browser-js.js
└── package.json
```

---

## 📦 Dependencies

- `node-red` >= 1.0.0

---

## 🔗 Links & References

- [Node-RED Documentation](https://nodered.org/docs)

---

## License

This project is licensed under the Apache License 2.0.\
See the [LICENSE](../LICENSE) file for details.
