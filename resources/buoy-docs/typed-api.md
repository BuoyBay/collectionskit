---
title: "collectionskit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["CollectionKeyPolicy", "Deque", "ExactStringKeyPolicy", "HashMap", "HashSet", "IntegerKeyPolicy", "OrderedEntry", "OrderedHashMap"]
dependencies: []
symbols: ["CollectionKeyPolicy", "Deque", "ExactStringKeyPolicy", "HashMap", "HashSet", "IntegerKeyPolicy", "OrderedEntry", "OrderedHashMap"]
related: ["api.md", "getting-started.md", "index.md"]
---

# collectionskit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## CollectionKeyPolicy

`Public Interface CollectionKeyPolicy Of K`

A stable policy: equal keys must have identical hashes for their whole stored lifetime.

### Hash

`Function Hash(key As K) As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Integer` |

### Equal

`Function Equal(leftKey As K, rightKey As K) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `leftKey` | `K` | `ByVal` | Required |
| `rightKey` | `K` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

## ExactStringKeyPolicy

`Public Class ExactStringKeyPolicy Implements CollectionKeyPolicy Of String`

Exact Unicode scalar/string-byte equality; no implicit case folding or normalization.

### Hash

`Public Function Hash(key As String) As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Integer` |

### Equal

`Public Function Equal(leftKey As String, rightKey As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `leftKey` | `String` | `ByVal` | Required |
| `rightKey` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

## IntegerKeyPolicy

`Public Class IntegerKeyPolicy Implements CollectionKeyPolicy Of Integer`

### Hash

`Public Function Hash(key As Integer) As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Integer` |

### Equal

`Public Function Equal(leftKey As Integer, rightKey As Integer) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `leftKey` | `Integer` | `ByVal` | Required |
| `rightKey` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

## HashMap

`Public Class HashMap Of K, V`

Bounded, single-threaded hash map with separate chaining and dense entry storage.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Count` | `Integer` | Yes | No | — | — |
| `Capacity` | `Integer` | Yes | No | — | — |
| `BucketCount` | `Integer` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(policy As CollectionKeyPolicy Of K, maxEntries As Integer = 100000)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `policy` | `CollectionKeyPolicy Of K` | `ByVal` | Required |
| `maxEntries` | `Integer` | `ByVal` | `100000` |

Returns: no value.

### Set

`Public Function Set(key As K, item As V) As Boolean`

Insert or replace; True means a new key was inserted. Replacing retains the original key.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |
| `item` | `V` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### ContainsKey

`Public Function ContainsKey(key As K) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Get

`Public Function Get(key As K) As Optional Of V`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Optional Of V` |

### GetOrDefault

`Public Function GetOrDefault(key As K, fallback As V) As V`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |
| `fallback` | `V` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `V` |

### Remove

`Public Function Remove(key As K) As Boolean`

Remove a key, moving the last dense entry into its slot. Snapshot order is unspecified.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Keys

`Public Function Keys() As Array Of K`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of K` |

### Values

`Public Function Values() As Array Of V`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of V` |

### Clear

`Public Sub Clear()`

No parameters.

Returns: no value.

## HashSet

`Public Class HashSet Of T`

HashMap-backed bounded set using the same explicit key policy.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Count` | `Integer` | Yes | No | — | — |
| `Capacity` | `Integer` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(policy As CollectionKeyPolicy Of T, maxEntries As Integer = 100000)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `policy` | `CollectionKeyPolicy Of T` | `ByVal` | Required |
| `maxEntries` | `Integer` | `ByVal` | `100000` |

Returns: no value.

### Add

`Public Function Add(item As T) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `item` | `T` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Contains

`Public Function Contains(item As T) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `item` | `T` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Remove

`Public Function Remove(item As T) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `item` | `T` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Items

`Public Function Items() As Array Of T`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of T` |

### Clear

`Public Sub Clear()`

No parameters.

Returns: no value.

## OrderedEntry

`Public Class OrderedEntry Of K, V`

Detached key/value pair. Payload references are shallow copies.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Key` | `K` | Yes | No | — | — |
| `Value` | `V` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(key As K, item As V)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |
| `item` | `V` | `ByVal` | Required |

Returns: no value.

## OrderedHashMap

`Public Class OrderedHashMap Of K, V`

Bounded insertion-ordered hash map. Policy callbacks must be stable and non-reentrant.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Count` | `Integer` | Yes | No | — | — |
| `Capacity` | `Integer` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(policy As CollectionKeyPolicy Of K, maxEntries As Integer = 100000)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `policy` | `CollectionKeyPolicy Of K` | `ByVal` | Required |
| `maxEntries` | `Integer` | `ByVal` | `100000` |

Returns: no value.

### Set

`Public Function Set(key As K, item As V) As Boolean`

True for a new key; False for replacement, which retains key and position.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |
| `item` | `V` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Get

`Public Function Get(key As K) As Optional Of V`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Optional Of V` |

### ContainsKey

`Public Function ContainsKey(key As K) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Remove

`Public Function Remove(key As K) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `K` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Entries

`Public Function Entries() As Array Of OrderedEntry Of K, V`

Paired snapshots preserve insertion order and do not expose internal slots.

No parameters.

#### Returns

| Type |
| --- |
| `Array Of OrderedEntry Of K, V` |

### Clear

`Public Sub Clear()`

No parameters.

Returns: no value.

## Deque

`Public Class Deque Of T`

Bounded growing ring buffer. Removed slots are immediately reset to None.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Count` | `Integer` | Yes | No | — | — |
| `Capacity` | `Integer` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(maxEntries As Integer = 100000)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `maxEntries` | `Integer` | `ByVal` | `100000` |

Returns: no value.

### PushBack

`Public Sub PushBack(item As T)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `item` | `T` | `ByVal` | Required |

Returns: no value.

### PushFront

`Public Sub PushFront(item As T)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `item` | `T` | `ByVal` | Required |

Returns: no value.

### PeekFront

`Public Function PeekFront() As Optional Of T`

No parameters.

#### Returns

| Type |
| --- |
| `Optional Of T` |

### PeekBack

`Public Function PeekBack() As Optional Of T`

No parameters.

#### Returns

| Type |
| --- |
| `Optional Of T` |

### TryPopFront

`Public Function TryPopFront() As Optional Of T`

No parameters.

#### Returns

| Type |
| --- |
| `Optional Of T` |

### TryPopBack

`Public Function TryPopBack() As Optional Of T`

No parameters.

#### Returns

| Type |
| --- |
| `Optional Of T` |

### Items

`Public Function Items() As Array Of T`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of T` |

### Clone

`Public Function Clone() As Deque Of T`

No parameters.

#### Returns

| Type |
| --- |
| `Deque Of T` |

### Clear

`Public Sub Clear()`

No parameters.

Returns: no value.
