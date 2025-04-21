## **⏳ Timeline Breakdown**

#### **🟢 Phase 1: Core API & Surface-Level Features**

✅ `createZapServer()` and `attachEvents()`  
✅ `zapEvent.input().process()` API  
✅ Middleware support (with context)  
✅ Event broadcasting (rooms, namespaces later)  
✅ Client-side library for easy event sending
✅ Stream
✅ syncedState
❌ native subscriptions
❌ Auto-reconnection & exponential backoff  
❌ Heartbeat pings (prevent dead connections)  
❌ Backpressure handling for slow clients  
#### **🟡 Phase 2: Stability & Performance (3-4 weeks)**

❌ Per-message compression (PMD)
❌ Binary support (`ArrayBuffer`)  
❌ Transport fallback (SSE)

#### **🟠 Phase 3: Scaling (2-3 weeks)**

❌ Redis-based multi-server sync
❌ multiple horizontal scaling plug and play stretigies.

#### **🟠 Phase 4: language agnostic server (maybe)**

❌ go types to ts types transpiler so that client can still get trpc level autocomplete magic.
❌ same for a bunch of langauges.