## 📡 Message Types

The Model Context Protocol (MCP) uses **JSON-RPC** as its "envelope." Every message must be **UTF-8 encoded**. Think of these four message types as the four ways parts of the system can talk to each other.

---

### 1. Requests (The Question)
A request is sent when one party wants the other to **do something** or **provide data**. It always expects a response.

`{`
`  "jsonrpc": "2.0",`
`  "method": "tools/call",`
`  "params": { "name": "get_weather", "arguments": {"city": "Delhi"} },`
`  "id": 1`
`}`

* **Real-world Example:** You ask a waiter, "Can I have the menu?" You are the requester; you expect the menu back.

---

### 2. Results (The Answer)
A result is the successful response to a Request. It uses the same `id` as the request so the sender knows which question is being answered.

`{`
`  "jsonrpc": "2.0",`
`  "result": { "content": [{"type": "text", "text": "It is 25°C and sunny."}] },`
`  "id": 1`
`}`

* **Real-world Example:** The waiter brings you the menu.

---

### 3. Errors (The "Something Went Wrong")
If a request cannot be fulfilled (e.g., the tool doesn't exist or the server crashed), an Error message is returned instead of a Result.

`{`
`  "jsonrpc": "2.0",`
`  "error": { "code": -32601, "message": "Method not found" },`
`  "id": 1`
`}`

* **Real-world Example:** The waiter says, "Sorry, we are out of menus."

---

### 4. Notifications (The Shout)
Notifications are one-way messages. They **do not expect a response**. These are used for status updates or logs.

`{`
`  "jsonrpc": "2.0",`
`  "method": "notifications/initialized",`
`  "params": {}`
`}`

* **Real-world Example:** A loudspeaker at a station says, "The train is arriving." It doesn't wait for you to say "Okay."

---

## 📝 Comparison Table

| Message Type | Expects Response? | Key Fields | Purpose |
| :--- | :--- | :--- | :--- |
| **Request** | ✅ Yes | `method`, `params`, `id` | Ask for data or action |
| **Result** | ❌ No (Is a response) | `result`, `id` | Return successful data |
| **Error** | ❌ No (Is a response) | `code`, `message`, `id` | Explain a failure |
| **Notification** | ❌ No | `method`, `params` | Send an update/alert |