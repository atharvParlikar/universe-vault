
---

## **1️⃣ Server: Define the Router**


```ts
import { createZapEvents, createZapServer } from "zap-socket";
import { z } from "zod";

export const events = {
  message: zapEvent({
    input: z.object({ message: z.string() }),
    process: ({ input }) => `your message was ${input.message}`,
  }),
};

export type Events = typeof events;

const zapServer = createZapServer({ port: 8000, events });
```

---

## **2️⃣ Client: Fully Type-Safe API**

On the client, you **import the router type** for **autocompletion**:

```ts
import { createWSClient } from "zap-socket";
import type { Events } from "./backend";

const client = createZapClient<Events>({url: "ws://localhost:3001"});

async function run() {
  // Full autocomplete, no magic strings
  // req-res model in websockets
  const result = await client.add.send({ a: 2, b: 3 });
  console.log(result); // 5

  // fire and forget, good old websockets
  client.add.send({ a: 2, b: 3 })

  // native subscriptions, no hacky bs
  client.timeUpdates.subscribe((data) => {
    console.log("Time update:", data);
  });
  
  // regular intuitive message listener (but typesafe)
  client.playerPosition.listen(({x,y}) => {
     console.log("Player position: ", x, y);
  })
}

run();
```

---

## **3️⃣ Context**

For context that server process function might need to know
like the id of the node that sent the message we will have
an additional argument in the *process* function.

```ts
process = (input, ctx) => { ... }
```

natural use can be something like this,
```ts
process = ({ name }, ctx) => {
	const { id } = ctx;
	...
}
```

later we can use ctx for middleware as well like tRPC uses it.

---

## **4️⃣ CLI**

Have some cli like, `npm zap-socket init` to init a basic zapsocket project, altho it is not that hard to set it up manually.

---

## **5️⃣ Streams**

Lets take an example of AI chatbots, they a stream of tokens.
like you can't get a stream of tokens with req-res right,
so you need listen or some other shit, but then its the same as socket.io which is bad dx.
so we add a stream, like
```ts
zap.chatCompletation.stream({prompt: "" }) // gives an async iterator back.
```

more here: [[Flow]]

---
## **Final Thoughts**

- **No `"string"` calls**, just real function names.
- **Full TypeScript autocompletion** like tRPC.
- **Zod validation built-in** for safety.
- **Native WebSocket subscriptions** (no hacks).
- **New pattern stream (my invention), its like scoped and managed subscriptions**
- **Simple server setup** (just pass `router`).

This would make **ZapSocket** a true **tRPC for WebSockets**. 
