# API - Kademlia groups

## Base types

- A BLAKE3 hash that's used as both a content identifier and its respective
content management group identifier.
```rust
type Hash = [u8; 32];
```

- User identifier
```rust
type UserId = Hash;
```

- User/Kademlia group identifier
```rust
type GroupId = Hash
```

- User downloaded content (a byte buffer)
```rust
type Content = Vec<u8>
```

## Group structures

- Basic node descriptor - its identifier and address (IP and port)
```rust
struct Node {
    id: NodeId,
    addr: SocketAddr
}
```

- Group owners can give out signed group access certificates to other users. If
the signing key becomes public the whole group will also turn public.
```rust
struct GroupAccessToken {
    binding: GroupAccessBinding,
    group_owner_signature: Signature
}
```

- User access is limited to a given UTC timestamp.

```rust
type UnixTimestamp = u64;

struct GroupAccessBinding {
    group: GroupId,
    recipient: UserId,
    revocation_date: UnixTimestamp
}
```

- User group validity certificate - the group definition signed by its owner.

```rust
struct UserGroup {
    definition: GroupDefinition,
    signature: Signature
}
```

- Hierarchical group definition - its description along with the owner's access
certificate to the group higher in the hierarchy. The hierarchy finishes with
the top-level global group. It's a root node for other sub-groups. Other global
groups can't be created.

```rust
struct GroupDefinition {
    id: GroupId,
    owner: UserId,
    parent: GroupId,
    parent_membership_proof: GroupAccessToken
}
```
