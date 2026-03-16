# Model Context Protocol (MCP) Mastery

Welcome to the definitive guide for the **Model Context Protocol (MCP)**. Think of MCP as a **USB-C port for AI**. Just as a universal port allows your laptop to connect to any monitor or hard drive, MCP allows any AI (like Claude or Gemini) to connect to your databases, local files, or APIs using a single, secure standard.

---

## 📅 Project Agenda & Roadmap

### 🎯 Why MCP?
* [x] **Benefits:** Standardizing how AI "sees" and "acts" on your data.
* [x] **General Architecture:** A deep dive into Hosts, Clients, and Servers.
* [x] **Security:** How the protocol ensures the AI doesn't go "rogue" with your files.

---

### 📡 Brief Introduction to JSON-RPC
* [x] **RPC:** Triggering code on a different program as if it were local.
* [x] **Why JSON?:** It’s the "universal language"—lightweight for machines and readable for you.
* [ ] **Core Concepts:** Mastering requests, notifications, and error handling.

---

## 🏗️ Architecture & Ecosystem

MCP operates on a simple **Client-Server model**. 

| Component | Role | Real-World Example |
| :--- | :--- | :--- |
| **MCP Host** | The app you use. | Claude Desktop, VS Code. |
| **MCP Client** | The internal bridge. | The "translator" inside the app. |
| **MCP Server** | The data provider. | A script that reads your Google Calendar. |

---

## 🚚 Transport Layer: How Data Moves

The transport layer is the "physical pipe" that carries messages. MCP is "transport-agnostic," meaning it doesn't care if the data travels through a local cable or the internet.

### 1. STDIO Transport (Local)
Ideal for running local MCP servers with full control over their lifecycle. The client starts the server as a background subprocess.
* **How it works:** They talk via `stdin` and `stdout`.
* **Logging:** Errors and logs use `stderr` to keep the data stream clean.

### 2. Streamable HTTP (Remote)
The standard for connecting to production services or shared servers running independently.
* **Mechanism:** Uses HTTP POST for messages and **Server-Sent Events (SSE)** for bidirectional streaming.

### 3. In-Memory Transport
Best for **testing** FastMCP servers without the overhead of network or subprocess management.

---

## 🔄 Reliability: Resumability and Redelivery

To ensure the AI doesn't "lose its train of thought" during a spotty connection, Streamable HTTP provides:

* **Event IDs:** Servers tag every message (SSE event) with a unique ID.
* **Resume Header:** If the connection drops, the client says, "I last saw Event #42," and the server picks up from there.
* **Message Replay:** The server replies with any missed messages from the point of disconnection.

---

## 🛠️ Protocol Capabilities

### What Servers give to AI:
* **Resources:** (Read-only) "Here is the content of `database_schema.sql`."
* **Prompts:** (Instructions) "Help the user write a SQL query for this DB."
* **Tools:** (Actions) "Delete the old log files."

`# A simple Tool snippet in Python`
`@mcp.tool()`
`def create_folder(name: str):`
`    os.makedirs(name)`
`    return f"Folder {name} created!"`

### What Clients give to Servers:
* **Sampling:** The server can ask the AI, "I found a bug in this code, can you suggest a fix?"
* **Roots:** The client sets a "digital fence," e.g., "You can only see the `/User/Projects` folder."

---

## 🛡️ Security Best Practices

When implementing Streamable HTTP, follow these "Golden Rules" to keep data safe:

1. **Validate Origins:** Check the `Origin` header to block DNS rebinding attacks.
2. **Localhost Only:** If running locally, bind to `127.0.0.1` (your computer only) instead of `0.0.0.0` (the whole network).
3. **Use HTTPS:** In production, always use TLS/HTTPS to encrypt the connection.
4. **Secure Sessions:** Use cryptographically secure session IDs (e.g., `MCP-Session-ID: "sess_12345abcde"`).

---

## 📡 JSON-RPC: The Heartbeat

JSON-RPC is the simple text format used for every request.

`{`
`  "jsonrpc": "2.0",`
`  "method": "resources/read",`
`  "params": { "uri": "file:///logs/app.log" },`
`  "id": "req_001"`
`}`

> **Pro Tip:** Use **MCP JSON configuration** files when you need to manage and communicate with multiple servers simultaneously.