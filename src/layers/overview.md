# Layer overview
## Session layer
This layer describes low-level primitives for transferring data over streams.
Data transport is encrypted based on long-term public keys gathered both through
Kademlia search and out-of-band exchange.

## Peer discovery
This layer implements structured network topology that allows for efficient
search and sending requests to other peers. It also decides who stores what.

## Object transfer
This layer introduces the notion of an extensible object and describes the set
of mandatory base objects. It's responsible for data fragmentation and the
immediate transfer mode, along with other things. It also defines an extensible
RPC interface.

## Application modules
A set of objects necessary to build apps based on the previous layer. It allows
defining its own subprotocols.
