## Random Notes

- A WebSocket connection starts as a regular HTTP request. This is because most browsers and networking layers only support opening WebSocket connections over HTTP or HTTPS.

- The the http request has a header indicating that it intends to upgrade to WebSocket connection, the server then reads that header and replies with an upgrade-acknowledgement header then the upgrade handshake(or whatever) starts.
