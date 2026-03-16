### Outline of this course

![alt text](<Screenshot (80).png>)

---

# Model Context Protocol (MCP) Mastery

Welcome to the definitive guide for the **Model Context Protocol (MCP)**. Think of MCP as a **USB port for AI**—it provides a universal way to plug different data sources (like Google Drive, GitHub, or Slack) into any AI model without writing custom code for every single combination.

---

## 📖 Table of Contents
* [The Foundation of MCP](#the-foundation-of-mcp)
* [The MCP Ecosystem](#the-mcp-ecosystem)
* [Empowering the MCP Server](#empowering-the-mcp-server)
* [Advanced Client-Side Features](#advanced-client-side-features)

---

## 🏗️ The Foundation of MCP
The Model Context Protocol is an open standard that simplifies how AI interacts with the outside world.

* **The $N \times M$ Problem:** Without MCP, if you have 5 AI models and 5 data sources, you need 25 different integrations. With MCP, you only need 1.
* **JSON-RPC:** This is the "language" they speak. It’s like a walkie-talkie protocol where the AI says `{"method": "list_tools"}` and the server replies with what it can do.

`# A simple JSON-RPC message example`
`{"jsonrpc": "2.0", "method": "resources/list", "id": 1}`

---

## 🌐 The MCP Ecosystem
Understanding how data moves between the AI and your files.

### Core Components
* **Hosts:** The app you are typing in (e.g., Claude Desktop, VS Code).
* **Clients:** The internal bridge that handles the "handshake."
* **Servers:** The specialized workers (e.g., a "Google Search" server or a "Local Files" server).

### Transport Layer
* **STDIO:** Used for local apps. It’s like a direct pipe between two programs on your computer.
* **SSE (HTTP):** Used for remote servers over the internet.

---

## ⚡ Empowering the MCP Server
Servers give LLMs "superpowers" through three main features:

### 🛠️ Tools (Actions)
Tools allow the AI to **do** things, like deleting a file or sending an email.
`@server.list_tools()`
`def handle_list_tools():`
`    return [Tool(name="get_weather", description="Tells the weather")]`

### 📚 Resources (Data)
Resources allow the AI to **read** things, like a CSV file or a database row. 
`# Example: Accessing a log file as a resource`
`mcp://local/logs/error.log`

### 📝 Prompts (Templates)
Think of these as "saved recipes" for the AI to follow. 
`# Example: A prompt template for code review`
`"Review the following code for security vulnerabilities: {{code}}"`

---

## 🤖 Advanced Client-Side Features
These features turn a basic chatbot into a smart **AI Agent**.

* **Roots:** Tells the AI exactly which folders it is allowed to see. It's like a digital "fence" for security.
* **Elicitation:** The AI asking you for permission. 
    * *Real-world:* Before deleting a file, the AI pops up a box: `[Accept] [Decline]`.
* **Sampling:** This is a "Inception" style feature. It allows the **Server** to ask the **Client** to run an AI completion. 
    * *Example:* A "SQL Server" gets an error, so it asks the AI client, "Hey, can you fix this SQL syntax for me?"

`# Example of a sampling request`
`client.create_message(messages=[{"role": "user", "content": "Fix this code"}])`