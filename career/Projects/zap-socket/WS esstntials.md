
---

### **1️⃣ WebSocketServer (WSS)**

This is the WebSocket **server** that listens for incoming connections.

```ts
import { WebSocketServer } from "ws";

const wss = new WebSocketServer({ port: 3000 });

wss.on("connection", (ws) => {
  console.log("Client connected");
});
```

#### 🔹 **Key Methods & Events**

| Method/Event                        | Description                                                   |
| ----------------------------------- | ------------------------------------------------------------- |
| `wss.on("connection", ws => {...})` | Fires when a client connects. `ws` is the WebSocket instance. |
| `wss.clients`                       | A `Set<WebSocket>` of all connected clients.                  |

---

### **2️⃣ WebSocket (WS)**

This is the actual **WebSocket connection** between the server and client.

```ts
wss.on("connection", (ws) => {
  ws.send("Hello Client!");
  ws.on("message", (data) => console.log("Received:", data.toString()));
});
```

#### 🔹 **Key Methods**

| Method                     | Description                                                             |
| -------------------------- | ----------------------------------------------------------------------- |
| `ws.send(data)`            | Sends data to the client. Accepts `string`, `Buffer`, or `ArrayBuffer`. |
| `ws.close([code, reason])` | Closes the connection with an optional code & reason.                   |
| `ws.terminate()`           | Forcefully closes the connection (without waiting for a close frame).   |
| `ws.ping([data])`          | Sends a **ping** to check if the client is still alive.                 |

#### 🔹 **Key Events**

| Event                                     | Description                                                       |
| ----------------------------------------- | ----------------------------------------------------------------- |
| `ws.on("message", data => {...})`         | Fired when a client sends a message.                              |
| `ws.on("close", (code, reason) => {...})` | Fired when the connection closes.                                 |
| `ws.on("error", err => {...})`            | Fires on a connection error.                                      |
| `ws.on("pong", data => {...})`            | Fired when a **pong** is received (for connection health checks). |

---

### **3️⃣ Sending JSON Messages**

Since `ws.send()` only supports **raw data**, you’ll need to handle JSON manually:

```ts
ws.send(JSON.stringify({ event: "message", data: "Hello" }));
ws.on("message", (msg) => {
  const { event, data } = JSON.parse(msg.toString());
});
```

You'll probably want to **wrap this in a helper function** for ZapSocket.

---

### **4️⃣ Broadcasting to All Clients**

Unlike `socket.io`, `ws` doesn’t have a built-in **broadcast feature**, but you can do:

```ts
wss.clients.forEach((client) => {
  if (client.readyState === WebSocket.OPEN) {
    client.send("Broadcast message");
  }
});
```

---

### **5️⃣ Handling Connection Keep-Alive (Pings)**

WebSockets **don’t automatically detect dead connections**. You should send **pings** and close dead sockets:

```ts
wss.on("connection", (ws) => {
  ws.isAlive = true;

  ws.on("pong", () => (ws.isAlive = true)); // Mark alive on pong

  setInterval(() => {
    if (!ws.isAlive) return ws.terminate(); // Kill dead connections
    ws.isAlive = false;
    ws.ping(); // Send ping
  }, 30000); // Every 30 sec
});
```

---

### **6️⃣ Reconnection Handling?**

Unlike `socket.io`, raw `ws` has **no built-in reconnection**. That’s up to **the client**. You could write a **ZapSocket client helper** like:

```ts
function connect() {
  const ws = new WebSocket("ws://localhost:3000");

  ws.onclose = () => setTimeout(connect, 1000); // Reconnect after 1 sec
}

connect();
```

But ideally, you let **users** handle reconnects their own way.

---

### **🚀 TL;DR – What You Get with `ws`**

1. **WebSocketServer** → Accepts connections.
2. **WebSocket** → Handles messages, sending, and closing.
3. **No event-based abstraction** like `socket.io`, so you need to **handle JSON, broadcasting, and reconnecting manually**.
4. **More control**, which is perfect for building a **clean, typed API** like `tRPC`.

This should be enough to **build ZapSocket in a clean, powerful way** without overengineering. 🚀