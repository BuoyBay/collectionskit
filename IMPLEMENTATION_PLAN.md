# CollectionsKit design notes

Current-toolchain probe: the standard-library overview is stale. Buoy 0.72 ships
Map and Set for primitive Hashable keys; dedicated map.md/set.md describe them.
CollectionsKit therefore targets explicit key policies and entry budgets, not a
claim to supply the first map/set. It supports user class keys by policy, exact
string matching, and custom matching without changing built-in operators.

API: CollectionKeyPolicy Of K (Hash, Equal), IntegerKeyPolicy,
ExactStringKeyPolicy, HashMap Of K,V (Set returns inserted flag; Get Optional;
ContainsKey, GetOrDefault, Remove, Clear; detached Keys/Values; Count/Capacity/
BucketCount getters), HashSet Of T (Add, Contains, Remove, Clear, Items).

Implementation: dense key/value/hash/next arrays with separate chaining; bucket
array doubles at 75% load. Removal unlinks then moves/relinks last entry. Rehash
uses cached hashes and never invokes user code. Counts bounded to 1..1,000,000.
Hash signed minimum handled by sign-bit masking rather than negation.

Policy contract: deterministic, stable hashes; equivalent keys have equal hashes;
equality is an equivalence relation. Stored object keys must not mutate relevant
fields. Reentrant operations are rejected; callback exceptions restore guard and
leave contents unchanged. Single-threaded; no ordering or hardened hash claims.
Snapshot arrays are structurally detached but payload references remain shared.
