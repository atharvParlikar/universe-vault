

```ts
const events = {
  llmStream: zapFlow({
    input: z.object({ prompt: z.string() }),
    process: async function* ({ input }) {
      for (const word of generateTokens(input.prompt)) {
        yield word; // Stream tokens over time
        await delay(50); // Simulating response delay
      }
    }
  }),
};
```


