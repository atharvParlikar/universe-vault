
Peer 1
```js
const lc = new RTCPeerConnection();
const dc = lc.createDataChannel("channel");
dc.onmessage = (e) => console.log("Just got a message " + e.data);
dc.onopen = (e) => console.log("Connection opened!");

lc.onicecandidate = (e) => console.log("new Ice Candidate reprited SDP + ", JSON.stringify(lc.localDescription));
lc.createOffer().then(o => lc.setLocalDescription(o)).then(a => console.log("set successfully"));
```
