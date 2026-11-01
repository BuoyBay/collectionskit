---
title: "Ordered collections typed API"
summary: "Insertion-ordered hash maps and bounded double-ended queues."
category: "Reference"
order: 22
types: ["OrderedHashMap", "OrderedEntry", "Deque"]
symbols: ["OrderedHashMap", "OrderedEntry", "Deque"]
dependencies: []
related: ["api.md", "typed-api.md"]
---

# Ordered collections

Import `collectionskit` for key policies and `collectionskit.OrderedCollections`
for these additions. The package version remains 0.1.0. See
`examples/collectionskit_ordered/main.bui` in the workspace for a runnable example.

## OrderedHashMap

`OrderedHashMap Of K, V`

`New OrderedHashMap Of K, V(policy As CollectionKeyPolicy Of K, maxEntries As Integer = 100000)`

The policy must be non-Nil, stable and deterministic throughout an operation and
while keys are stored. Equality must be an equivalence relation, with identical
hashes for equal keys. Keys may be Nil if the policy supports them. Limits must
be in 1..1,000,000; invalid arguments throw `InvalidArgumentException`.

| Public signature | Contract |
| --- | --- |
| `Property Count As Integer` (Get) | Current entry count. |
| `Property Capacity As Integer` (Get) | Configured entry bound, not allocated storage. |
| `Function Set(key As K, item As V) As Boolean` | True inserts at the end. False replaces the value while retaining the original key object/spelling and its position. Replacement at capacity succeeds. A new key at capacity throws `InvalidArgumentException`. |
| `Function Get(key As K) As Optional Of V` | Missing is None; present is Some(value), including Some(Nil). |
| `Function ContainsKey(key As K) As Boolean` | Membership under the policy. |
| `Function Remove(key As K) As Boolean` | True removes a key; False leaves an absent key alone. Reinsertion appends, even if its storage slot is reused. |
| `Function Entries() As Array Of OrderedEntry Of K, V` | Detached paired snapshot in insertion order. Later replacement/removal does not alter the saved pair. |
| `Sub Clear()` | Empty entries, release payloads, reset backing storage; retain policy and entry bound. |

### OrderedEntry

`OrderedEntry Of K, V` has `Constructor(key As K, item As V)` and read-only
`Property Key As K` and `Property Value As V`. Arrays and entry objects in snapshots
are separate from map bookkeeping. Keys and values that are references remain
shared with their originals; this is a shallow snapshot.

The implementation reuses CollectionsKit HashMap for key-to-slot lookup and uses
integer previous/next links in reusable order slots. Links create no ownership
cycles. Removal releases the slot's key/value references. Storage grows with the
maximum simultaneous entry count since Clear, bounded by Capacity; repeated
remove/reinsert does not grow a tombstone history. None-valued slots deliberately
avoid the installed toolchain's bare class-array Nil overwrite release candidate
(`upstream-issues/ordered-array-nil-release` in the workspace).

Hash/equality callbacks run before logical mutation. A callback exception leaves
contents, count and order unchanged and releases operation guards; reserved empty
storage may remain reusable. Callbacks must not reenter Set/Get/ContainsKey/Remove/
Entries/Clear on the same ordered map: these throw `InvalidArgumentException`.
Count and Capacity remain readable observations. Catching a rejected nested call
inside a callback permits the outer operation to finish. These guarantees depend
on a valid policy; mutation of key or policy identity state is unsupported.
User destructors should not throw or call guarded map operations: payload release
can occur after a mutation commits. Allocation failure has no atomicity guarantee.

With constant-cost, well-distributed policies, lookup and replacement are average
O(1), insertion amortized average O(1), and removal average O(1). Forced collisions
are O(n); growth is O(n). Entries and Clear cost O(n). Space is O(high-water entry
count), plus HashMap buckets, policy/payload storage and caller-owned snapshots.
HashMap's deterministic default hashes offer no adversarial flood resistance.
This is a single-threaded collection with no lock or live iterator.

## Deque

`Deque Of T`

`New Deque Of T(maxEntries As Integer = 100000)`

Limits must be in 1..1,000,000; invalid arguments throw
`InvalidArgumentException`. Capacity is an entry limit and does not evict items.

| Public signature | Contract |
| --- | --- |
| `Property Count As Integer` (Get) | Current item count. |
| `Property Capacity As Integer` (Get) | Configured maximum count. |
| `Sub PushFront(item As T)` | Insert at front; full deque throws without changing contents. |
| `Sub PushBack(item As T)` | Insert at back; full deque throws without changing contents. |
| `Function TryPopFront() As Optional Of T` | Remove front, or None if empty. |
| `Function TryPopBack() As Optional Of T` | Remove back, or None if empty. |
| `Function PeekFront() As Optional Of T` | Read front, or None if empty. |
| `Function PeekBack() As Optional Of T` | Read back, or None if empty. |
| `Function Items() As Array Of T` | Detached front-to-back shallow snapshot. |
| `Function Clone() As Deque Of T` | Independent queue storage, same bound and ordering; shallow payload copies. |
| `Sub Clear()` | Release all slots and reset empty ring; preserve configured bound. |

A present Nil item is Some(Nil), distinct from empty. Pops reset the vacated
Optional-valued ring slot to None immediately; the deque retains no removed
payload. A returned Optional or existing snapshot can retain it normally. Ring
storage grows geometrically up to Capacity and is retained after pops for reuse;
Clear releases it. Pushes are amortized O(1), pops and peeks O(1), and growth,
Items, Clone and Clear O(n). Space is O(high-water item count) until Clear.
There are no callbacks, synchronization, eviction or live iteration features.
Destructors should not throw. As with the map, allocator failures are outside
the mutation atomicity contract.

## Verification


Maintainers run the private validation suite before release. See the repository README for public build instructions.

