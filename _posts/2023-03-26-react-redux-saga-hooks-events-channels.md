---
layout: page
title: "Event Channels in React Redux Saga Hooks"
categories: React
---

In the tutorial available here: [https://youtu.be/cTcThsNqqcg](https://youtu.be/cTcThsNqqcg)
I explain how eventChannels work using sockets ( a socket server ) and a react with redux hooks saga implementation.

## The code

### Server

the server app is just starting a socket server and emiting some strings every 3 seconds with the current value of the seconds of the server clock.

```
const express = require('express');
const app = express();
var cors = require('cors');
app.options('*', cors())
const http = require('http');
const httpServer = http.createServer(app);
const io = require("socket.io")(httpServer, {
  cors: {
    origin: "http://192.168.1.135:3000",
    methods: ["GET", "POST"],
    allowedHeaders: ["Access-Control-Allow-Origin"]
  },
  origins: ["http://192.168.1.135:3000"]
});


app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});
var intervalID;

io.on('connection', (socket) => {
  console.log('a user connected');
  clearInterval(intervalID);
  intervalID = setInterval(function () {
    console.log(new Date().getSeconds());
    socket.emit('seconds', new Date().getSeconds());
  }, 3000);
});

httpServer.listen(3001, () => {
  console.log('listening on *:3001');
});

```

### Saga Code

This is just the saga file containing the generator, but it's the full file showing the eventChannel and socket implementation

For a full react Implementation with redux saga in hooks, as a previous project showing react redux saga with hooke, you can find

Meanwhile, here's the code of the final saga file:

```
import { put, take, takeLatest, all, call } from "redux-saga/effects";
import { eventChannel, END } from 'redux-saga'
import { io } from "socket.io-client";
const receiveMessage = (socket) => {
  socket.on("disconnect", () => {
    socket.connect();
    console.log('socket disconnected');
  });
  return eventChannel((emitter) => {
    socket.on('seconds', (msg) => {
      emitter(msg);
    });
    return () => {
      emitter(END);
    }
  });
};

const runOurAction = function* () {
  const socket = io('http://192.168.1.135:3001');
  const chan = yield call(receiveMessage, socket);
  while (true) {
    try {
      const value = yield take(chan);
      yield put({ type: "SET_DATA", payload: value });
    } catch (err) {
      console.error('socket error:', err)
      // socketChannel is still open in catch block
      // if we want end the socketChannel, we need close it explicitly
      // socketChannel.close()
    }
  }
};
function* getAsyncDataWatcher() {
  yield takeLatest("GET_ASYNC_DATA", runOurAction);
}

export default function* rootSaga() {
  yield all([getAsyncDataWatcher()]);
}
```

#### That's all Folks!
