# CollectionsKit 0.1.0

`Import collectionskit` adds bounded hash maps and sets with explicit key policies.
Buoy 0.72 already ships `Map Of K,V` and `Set Of T` for primitive Hashable keys;
the standard-library overview's statement that they are missing is stale. Prefer
the built-in collections for their default behavior. This package adds custom
hash/equality policies, class keys, exact string comparison, and entry budgets.
It does not change built-in `Map`, `Set`, or string operators.

Copy `packages/collectionskit-0.1.0.buipkg` into your project's `packages/`
directory. The [example](../examples/collectionskit/main.bui) counts exact,
case-sensitive words. No native dependencies, imports, or online data are needed.

## Policy and API

Implement `CollectionKeyPolicy Of K` with `Hash(key As K) As Integer` and
`Equal(leftKey As K, rightKey As K) As Boolean`. The package includes
`IntegerKeyPolicy` and `ExactStringKeyPolicy`. The latter compares exactly,
including case and canonical spelling; it does not normalize or case-fold text.
Custom equality can apply a domain-specific identity rule to class or primitive
keys. Nil class keys are accepted when the supplied policy handles them.

Construct `New HashMap Of K,V(policy, maxEntries = 100000)` or
`New HashSet Of T(policy, maxEntries = 100000)`. A Nil policy and limits outside
1..1,000,000 throw `InvalidArgumentException`.

| HashMap API | Contract |
| --- | --- |
| `Set(key, item)` | Insert or replace; returns True for a new key, False for a replacement. A replacement retains the original stored key. |
| `Get(key)` | `Optional Of V`; missing gives None, present gives Some(value), including a present Nil class value. |
| `GetOrDefault(key, fallback)` | Stored value or the supplied fallback. |
| `ContainsKey(key)` | Test presence without changing the map. |
| `Remove(key)` | True if removed, False if absent. |
| `Keys()` / `Values()` | Structurally detached snapshots in matching order if no intervening mutation occurs. |
| `Clear()` | Release entries and restore the initial 16-bucket table. |
| `Count` / `Capacity` / `BucketCount` | Read-only properties: entry count, configured maximum entries, allocated bucket count. |

`HashSet` supplies `Add(item)` (True if new), `Contains(item)`, `Remove(item)`
(True if present), `Items()` (detached array), `Clear()`, `Count`, and `Capacity`.
Duplicates at capacity remain accepted; map replacement at capacity also works.
Only insertion of a new entry beyond the limit throws.

## Correctness and ownership

A policy must be stable and deterministic while a key is stored. Equality must
be an equivalence relation, and equal keys must produce identical hash integers.
Do not mutate fields that affect a stored key's hash/equality. Violating these
requirements can make entries unreachable or create apparent duplicates; the
package cannot detect arbitrary violations. Hashes may be negative, including
Integer.MinValue. Collisions are resolved by equality, never treated as matches.

Policies, class keys, and class values are retained strongly. Snapshot arrays
copy their structure; reference payloads still refer to the same objects. Keep
policies from strongly referencing their map, or break that ownership cycle
explicitly. Removing entries moves other entries: iteration order is unspecified.
The package offers snapshots rather than live iterators.

All operations use one thread. Policy callbacks must not call operations on the
same map or set; reentry throws `InvalidArgumentException`. Count/Capacity/
BucketCount getters are simple observations. Hash/equality exceptions propagate,
restore the operation guard, and leave map contents unchanged. A policy may not
mutate key state or release outside synchronization as a substitute for this
contract. There is no concurrent map, automatic UI dispatch, or lock management.
Removed/replaced payloads are retained until the map is consistent and the
operation guard has cleared. Their destructors may then read or mutate the map.
Such destructor callbacks observe the completed mutation; their own exceptions
are not policy-callback failures and cannot roll that mutation back.

## Complexity and limits

Separate chaining uses dense key/value/hash/link arrays. Buckets double before
75% occupancy; rebuilding uses stored hashes, without policy callbacks. Average
lookup/update/removal is O(1) with well-distributed, constant-cost policies;
worst-case collisions take O(n). Snapshot and Clear costs are O(n); rebuilding is
O(n + bucket count). Removal unlinks a chain entry, then moves/relinks the last
dense entry. There is no shrinking except Clear.

The entry limit bounds count, not payload bytes or callback time. Large keys,
large payloads, arbitrary policy code and snapshots can allocate additional
memory. The deterministic included hashes are not resistant to adversarial hash
flooding. Treat untrusted bulk input with appropriate caller limits or supply a
suitable keyed policy. No allocation-failure atomicity guarantee is made.

## Build and verification

```sh
/usr/bin/buoy pkg seal . -o packages/collectionskit-0.1.0.buipkg
```

The verifier seals fresh source into a temporary isolated project, checks source,
tests and example against six desktop targets, and compiles/runs consumers on
the current host. Linux x86_64 execution is verified; macOS/Windows are compiler
checks only. `BUOY` and `BPM_PACKAGER` override compiler/sealer paths.

Focused tests exercise signed-minimum hashes, forced collisions, resizing, dense
swap removal, replacement/capacity, detached arrays, empty and Unicode strings,
custom equality, class identity and Nil values, exceptions and callback reentry.
A Python dictionary oracle generates 12,000 deterministic operations, each run
through both ordinary and constant-hash policies. A baseline probe confirms that
core Map/Set already exist. The implementation is independently written for this
workspace and contains no copied third-party collection code.

## Ordered maps and double-ended queues

Ordered-map Clear retains replaced storage until its operation guard has cleared,
so released payload destructors can observe the completed empty map through its
public methods. Destructors run after the mutation; their own changes or failures
cannot roll it back.

Maintainers run the private validation suite before release. See the repository README for public build instructions.
