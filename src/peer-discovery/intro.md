# Peer Discovery
The Peer Discovery layer is responsible for finding peers storing requested
content. This is done by creating a new virtual network topology (i.e. an
overlay network). This layer is both performance and reliability critical
as it provides a basis for the rest of the distributed system.

One also needs to define what exactly is the result of a peer discovery search.
A naive solution based on a simple Kademlia search will return nodes whose ID
is the most similar to the requested one. This is not enough for the purposes
of this project, as described in the *Motivation* section.

Therefore, it is assumed that a search can return *any* peer if it knows the
path to the destination.
