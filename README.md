# Lambda network proxy event (Go)

This is a (very) small repository containing the event payload for the [Lambda network proxy](https://github.com/dacut/lambda-network-proxy).

## Event format

The current event format is (JSON):

```json
{
    "ClientProtocol": "tcp|tcp4|tcp6|udp|udp4|udp6",
    "ClientAddress": "client-ipv4-or-ipv6-address",
    "ClientPort": int,
    "ProxyProtocol": "tcp|tcp4|tcp6|udp|udp4|udp6",
    "ProxyAddress": "proxy-ipv4-or-ipv6-address",
    "ProxyPort": int,
    "Nonce": "random-string"
}
```

For TCP-based protocols, the Lambda function must connect to the proxy address/port and send the nonce as the first bytes of the stream, without any additional padding (e.g. CR/NL terminator).

For UDP-based protocols, the Lambda function must send a packet containing only the nonce, again without any padding.

Upon receiving and validating the nonce, the proxy will start relaying traffic between the client and the Lambda function, starting with any client data received and buffered while nonce handshaking was taking place.

## See also

* [lambda-network-proxy](https://github.com/dacut/lambda-network-proxy)
* [lambda-echo-server](https://github.com/dacut/lambda-echo-server)
