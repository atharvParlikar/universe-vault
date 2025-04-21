### Refactoring Checklist

1. **Extract `useGameEngine`**
   - Move canvas, game loop, and game object management
   - Keep hero, camera, and remote player logic
   - Expose only necessary state (e.g., `canvasRef`, `heroPosition`)

2. **Extract `useSocketHandler`**
   - Move all WebSocket event listeners
   - Handle player position syncing and ID assignment
   - Expose methods for sending messages (e.g., `sendPositionUpdate`)

3. **Extract `usePeerConnection`**
   - Move PeerJS initialization and call handling
   - Manage media streams and cleanup
   - Expose call state and methods (e.g., `startCall`, `endCall`)

4. **Extract `useCallManager`**
   - Handle call consent logic (toasts, UI state)
   - Coordinate between WebSocket and PeerJS
   - Expose call state (e.g., `isCalling`, `isOnCall`)

5. **Create `GameView` Component**
   - Just the canvas element
   - Receive `canvasRef` from `useGameEngine`

6. **Create `CallUI` Component**
   - Buttons, toasts, and logs
   - Receive call state and handlers from `useCallManager`

7. **Add Proper Cleanup**
   - Stop all media tracks
   - Close WebSocket and PeerJS connections
   - Clear intervals/timeouts

8. **Add Error Handling**
   - Wrap async operations in try/catch
   - Display user-friendly error messages

9. **Test Incrementally**
   - Test each hook in isolation
   - Verify game, WebSocket, and call functionality separately

---

### Quick Tips for Tomorrow

- Start with the easiest hook (`useGameEngine`) to build momentum.
- Use TypeScript interfaces to define clear boundaries between hooks.
- Keep the main component (`GameCanvas`) as lean as possible—it should just glue everything together.
- If you get stuck, focus on one small piece at a time (e.g., just the media stream cleanup).

---

### Example of a Lean `GameCanvas`

```tsx
export const GameCanvas = () => {
  const { canvasRef } = useGameEngine();
  const { callState, handleCall } = useCallManager();

  return (
    <div>
      <GameView canvasRef={canvasRef} />
      <CallUI
        callState={callState}
        onCall={handleCall}
      />
    </div>
  );
};