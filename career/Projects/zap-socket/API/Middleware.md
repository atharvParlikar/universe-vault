
---

**✅ Basic Usage**

Add middleware per event by **passing an array of functions**:

```ts
const events = createZapEvents({
  securedAction: zapEvent({
    input: z.object({ message: z.string() }),
    middleware: [
      async (ctx) => {
        console.log("Middleware 1");
        return true; // return true if you want to let message through middleware
      },
      async (ctx) => {
        console.log("Middleware 2");
        return true;
      }
    ],
    process: ({ input }) => `Message: ${input.message}`
  })
});
```

• **ctx** → Context object → contains client ID, socket, input, etc.

• Middleware stack executes **sequentially** before reaching the process function.

---

**🔥 Reusable Middleware**

You can create **reusable middleware factories** for cleaner code:

```ts
const logger = () => async (ctx) => {
  console.log(`[${new Date().toISOString()}] ${ctx.event} from ${ctx.id}`);
  return true;
};

const auth = (secret: string) => async (ctx) => {
  if (!ctx.user) throw new Error("Unauthorized");

};

const rateLimiter = ({ limit = 5, windowMs = 10000 }) => {
  const requests = new Map<string, { count: number; lastReset: number }>();

  return async (ctx) => {
    const now = Date.now();
    const record = requests.get(ctx.id) || { count: 0, lastReset: now };

    if (now - record.lastReset > windowMs) {
      record.count = 0;
      record.lastReset = now;
    }

    record.count += 1;
    requests.set(ctx.id, record);

    if (record.count > limit) {
      throw new Error("Rate limit exceeded");
    }

    await next();
  };
};
```

✅ **Usage:**

```ts
const events = {
  privateAction: zapEvent({
    input: z.object({ data: z.string() }),
    middleware: [
      logger(),
      auth("my-secret"),
      rateLimiter({ limit: 10, windowMs: 30000 })
    ],
    process: ({ input }) => `Secure data: ${input.data}`
  })
};
```

  

---

**⚙️ Middleware Execution Flow**

The middleware stack executes in order:

1️⃣ middleware[0] → true

2️⃣ middleware[1] → true

3️⃣ … → continues through all middleware

4️⃣ **Finally** → Calls the process() function.

---
✅ **Usage:**

```ts
const events = createZapEvents({
  riskyAction: zapEvent({
    input: z.object({ foo: z.string() }),
    middleware: [errorHandler()],
    process: () => {
      throw new Error("Oops!");
    }
  })
});
```

---

**💡 Tips**

🔥 **Composability:**

• Combine multiple middleware factories → middleware: [logger(), auth(), rateLimiter()].

• Create **dynamic middleware stacks** based on roles or environments.

  

🔥 **Async & performant:**

• Middleware supports **async operations** → DB checks, token validation, etc.

• Non-blocking execution flow → await next() ensures smooth chaining.
