
**Note**: This note covers [gorilla/websocket](https://github.com/gorilla/websocket) library. Also read [[Comp sci/WebSockets|WebSockets]] for background.

## Key components

1. HTTP server: we need http server as foundation to our websocket connection.
2. Upgrader struct: This is the struct which is going to upgrade your http connection to websocket connection.

## Program flow for websocket setup

1. create a HTTP server
2. create an upgrader struct with appropriate config
3. create a websocket endpoint on that server
4. link a function to that endpoint to initialize and handle websocket connection.
5. upgrade the connection in the function mentioned above using upgrader.Upgrade
6. manage the connection returned by upgrader.Upgrade

## Code example

```go package main

import (
	"fmt"
	"log"
	"net/http"

	"github.com/gorilla/websocket"
)

var upgrader = websocket.Upgrader{
	CheckOrigin: func(r *http.Request) bool {
		return true
	},
}

func handleWebSocket(w http.ResponseWriter, r *http.Request) {
	conn, err := upgrader.Upgrade(w, r, nil)

	if err != nil {
		log.Println("error creating Upgrader")
		log.Println(err)
		return
	}
	defer conn.Close()

	fmt.Println("WebSocket connection established")

	for {
		messageType, message, err := conn.ReadMessage()
		if err != nil {
			log.Println("error reading message")
			log.Println(err)
		}

		fmt.Println("Message recieved:\n", message)

		err = conn.WriteMessage(messageType, message)
		if err != nil {
			log.Println("Error writing message:")
			log.Println(err)
		}
	}
}

func main() {
	http.HandleFunc("/ws", handleWebSocket)

	port := "8080"
	fmt.Printf("Starting server on port %s\n", port)
	err := http.ListenAndServe(":"+port, nil)
	if err != nil {
		log.Println("Error starting http server:")
		log.Println(err)
	}
}
```
