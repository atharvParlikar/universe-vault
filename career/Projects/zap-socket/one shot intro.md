
```ts
import { z } from "zod";
import { createZapServer, zapEvent, zapStream } from "@zap-socket/server";

// in the frontend you do,
//
// const zap = createZapClient<Events>({url: 'ws://localhost:8000'});
//
// zap.events.message.send("Hi friend");
// const sum = await zap.events.message.send({num1: 10, num2: 20});
//
// const tokens = zap.stream.llm.send("Hi");
// for await (const token of tokens) {
//   console.log(token);
// }

const events = {
  message: zapEvent({ // returns a Promise<null> that gets resolved when server sends ACK
    input: z.string(),
    process: (data, { server, id }) => {
      server.selectiveBroascast("message", data, server.clients.filter(x => x !== id));
    },
    middleware: [logger, rateLimiter], // middlewares go from index-0 to index-n and each function should return true if it wants to let message pass
    emitType: z.string()
  }),
  add: zapEvent({ // returns a Promise<number>
    input: z.object({
      a: z.number(),
      b: z.number()
    }),
    process: ({ a, b }) => a + b
  }),
  llm: zapStream({ // returns an async iterator
    input: z.string(),
    process: async function* (data) {
      for (const token of "Hello there, how can I assist you!".split(" ")) {
        yield await new Promise((resolve) => setTimeout(() => resolve(token), 100));
      }
    }
  }),
}

export type Events = typeof events;

const server = createZapServer<Events>({ port: 8000, events }, () => {
  console.log("Server is live");
});
```

