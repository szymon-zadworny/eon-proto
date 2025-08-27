# Description

## Kademlia
The peer discovery layer is defined by hierarchical groups with access control.
Every group is defined by its own Kademlia network with the following additional
abstractions:
- Route caching that allows for early termination of future Kademlia requests
- Generalized search target - users don't search for a single best-fit host
that's tasked with managing specific content - they search for any host from
the content's *management group* instead.

## Groups
By default the system is divided into global, continent and country-level
groups. Furthermore, the users can create their own private groups that belong
to some other public group. This implies that users can't create another
global-level group.

A public group is defined as a private group whose signing key is publicly
known. Both types of groups are managed through the same system. Every group
contains its own Kademlia network with extensions, as previously stated. Users
are assigned to correct groups based on their IP address.

## Route caching
Base Kademlia requests return the user closest to a given content identifier.
This user serves as the primary gateway to the content management group. If the
content management group is composed of multiple nodes the gateway returns the
ID and address of a random node from this group.

Users are required to cache their requests into routing shortcut tables. If a
future Kademlia request passes through them they not only return their closest
peers, but also all their known cached members of the management group.

## Management groups
Management groups serve content with a hash that's the same as the group's
identifier. They also store objects related to that hash (e.g. *Pins* as defined
by the upper layer). Users that belong to this group are responsible for
distributed sharding. Every group member knows something about the hash, and
they synchronize that knowledge through the Gossipsub protocol.

Everyone can join a management grup. A member can ask other nodes from the 
High Availability group for help if it detects an increased load on the content.

External nodes can help with content delivery by becoming *seeders*, i.e. by 
notifying other members about their readiness to provide content with the same
hash. It is not required for them to also store related content (e.g. Pins).
Seeders aren't part of the distributed sharding, so they also aren't stored in
any routing shortcut tables.

## High availability nodes
Inactive users automatically join the High Availability group. All users present
there signal their readiness to help with other requests.
