### 🧠 Core Concepts of MCP

The Model Context Protocol (MCP) acts like a **negotiation table**. When an AI (Client) connects to a data source (Server), they first "handshake" to see what each other can do. This ensures that different AI models can work with different servers without custom, "fragile" code.

* **Resources:** (The Library) Raw data like a `README.md` file or a SQL table.
* **Prompts:** (The Recipe) Pre-set instructions, like "Summarize this bug report."
* **Tools:** (The Power Tool) Actions the AI can take, like `send_slack_message` or `run_script`.
* **Sampling:** (Phone-a-Friend) The server asking the AI for help with a task.
* **Roots:** (The Fence) Telling the AI, "You can look at the `/Desktop` folder, but stay out of `/System`."
* **Elicitation:** (The Question) When the AI stops and asks you, "Which file did you mean?"
* **Transports:** (The Wire) The physical way data moves, usually **STDIO** (local) or **SSE** (web).

---

### 🔄 Communication Sequence & Message Flow

Think of the flow as a conversation:
1.  **Host** (Claude) says: "I need to talk to the Google Drive server."
2.  **Client** starts a session.
3.  **Server** says: "I'm version 1.0 and I have 2 tools: `read_file` and `list_files`."

**High-Level Architecture:**

![alt text](<Screenshot (81).png>)

**Detailed Message Flow:**

![alt text](<Screenshot (82).png>)

---

### 🖥️ Hosts vs. 🔌 Clients

* **The Host:** This is the "Boss" (e.g., Claude Desktop). It decides which servers are allowed to run and handles your security permissions.
* **The Client:** This is the "Translator." It lives inside the Host and manages the actual 1:1 connection to a specific server.

> **Simple Analogy:** The **Host** is your Smartphone. The **Clients** are the individual apps (Spotify, WhatsApp) that connect to their own **Servers**.

---

### 🧠 Context (`ctx`)
In Python, the `Context` object is like a **Swiss Army Knife** injected into your functions. It lets your tool "see" who is calling it and ask for more information.

`@mcp.tool()`
`def search_database(query: str, ctx: Context):`
`    # ctx.session_id tells you which user is asking`
`    # ctx.info() can log messages back to the host`
`    return db.execute(query)`

---

### 🤖 MCP Agent
An **MCP Agent** is an AI that has been given "hands." Instead of just talking, it can use the protocol to solve problems across different apps.

* **Dynamic Chaining:** The agent can use a **Resource** (read a log file), realize there's an error, and then use a **Tool** (restart the server) automatically.
* **Constraint:** The AI model used must support "Function Calling" (like Gemini or Claude).

---

### ⚙️ Servers: The Power Providers
Servers are lightweight scripts that do one thing well. They provide three main "Primitives":

| Primitive | Control | Description | Example |
| :--- | :--- | :--- | :--- |
| **Prompts** | **User-controlled** | You choose to use them. | Type `/review` in a chat |
| **Resources** | **App-controlled** | Data given to the AI. | Attaching a `.csv` file |
| **Tools** | **Model-controlled** | The AI decides when to click the "button." | AI calls `Google Search()` |