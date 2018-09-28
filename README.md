# EOS WebSocket

[EOS Devs: An Endpoint with Stronger Guarantees of Inclusion in a Block](https://www.youtube.com/watch?v=c0FNiv0aVZU&feature=youtu.be)

[![EOS Devs](https://i.ytimg.com/vi/c0FNiv0aVZU/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLC9AqY7ePSHIlEPpmMJIQRTuqBAZg)](https://www.youtube.com/watch?v=c0FNiv0aVZU&feature=youtu.be)

## Install

**npm**

```bash
$ npm install --save eosws
```

## Browser (client)

```javascript
import { get_actions, parse_actions, EosioToken } from "eosws";

const ws = new WebSocket("ws://<SERVER>/v1/stream");

ws.onopen = () => {
    ws.send(get_actions("eosio.token", "transfer"));
};

ws.onmessage = (message) => {
    const actions = parse_actions<EosioToken.Transfer.Data>(message.data);

    if (actions) {
        const { from, to, quantity, memo } = actions.data.trace.act.data;
        console.log(from , to, quantity, memo);
    }
};
```

## NodeJS (server)

```ts
import WebSocket from "ws";
import { get_table_deltas, parse_table_deltas, Eosio } from "eosws";

const ws = new WebSocket("ws://<SERVER>/v1/stream");

ws.onopen = () => {
    ws.send(get_table_deltas("eosio", "eosio", "global"));
};

ws.onmessage = (message) => {
    const table_deltas = parse_table_deltas<Eosio.Table.Global>(message.data);

    if (table_deltas) {
        const { total_ram_stake, total_ram_bytes_reserved } = table_deltas.data.row;
        console.log(total_ram_stake, total_ram_bytes_reserved);
    }
}
```

## Related Javascript

-   [x] WebSockets (<https://github.com/websockets/ws>)
-   [ ] Socket.io (<https://github.com/socketio/socket.io>)

## Related Video

-   Push Irreversible Transaction (<https://youtu.be/dO-Le3TTim0?t=34m6s>)

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [get_actions](#get_actions)
    -   [Parameters](#parameters)
    -   [Examples](#examples)
-   [get_table_deltas](#get_table_deltas)
    -   [Parameters](#parameters-1)
    -   [Examples](#examples-1)
-   [unlisten](#unlisten)
    -   [Parameters](#parameters-2)
    -   [Examples](#examples-2)
-   [generateReqId](#generatereqid)
    -   [Examples](#examples-3)
-   [parse_actions](#parse_actions)
    -   [Parameters](#parameters-3)
    -   [Examples](#examples-4)
-   [parse_table_deltas](#parse_table_deltas)
    -   [Parameters](#parameters-4)
    -   [Examples](#examples-5)
-   [parse_ping](#parse_ping)
    -   [Parameters](#parameters-5)
    -   [Examples](#examples-6)

### get_actions

Get Actions

#### Parameters

-   `account` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Account
-   `action_name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Action Name
-   `receiver` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Receiver
-   `req_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Request ID

#### Examples

```javascript
ws.send(get_actions("eosio.token", "transfer"));
```

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Message for `ws.send`

### get_table_deltas

Get Table Deltas

#### Parameters

-   `code` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Code
-   `scope` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Scope
-   `table_name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Table Name
-   `req_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Request ID

#### Examples

```javascript
ws.send(get_table_deltas("eosio", "eosio", "global"));
```

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Message for `ws.send`

### unlisten

Unlisten to WebSocket based on request id

#### Parameters

-   `req_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Request ID

#### Examples

```javascript
ws.send(unlisten("req123"));
```

### generateReqId

Generate Req ID

#### Examples

```javascript
generateReqId() // => req123
```

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Request ID

### parse_actions

Parse Actions from `get_actions` from WebSocket `onmessage` listener

#### Parameters

-   `data` **WebSocketData** WebSocket Data from message event

#### Examples

```javascript
const actions = parse_actions<any>(message);
```

Returns **ActionTrace** Action Trace

### parse_table_deltas

Parse Table Deltas from `get_table_deltas` from WebSocket `onmessage` listener

#### Parameters

-   `data` **WebSocketData** WebSocket Data from message event

#### Examples

```javascript
const table_deltas = parse_table_deltas<any>(message);
```

Returns **ActionTrace** Action Trace

### parse_ping

Parse Ping from WebSocket `onmessage` listener

#### Parameters

-   `data` **WebSocketData** WebSocket Data from message event

#### Examples

```javascript
const ping = parse_ping(message);
```

Returns **Ping** Ping
