# Generic hash maps and sets with explicit key policies

Use custom hash and equality policies for primitive or class keys. The package's
maps and sets enforce entry budgets and return detached snapshots. Built-in
collections remain suitable when their default key semantics meet your needs.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add collectionskit --git https://github.com/BuoyBay/collectionskit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## API at a glance

Implement `CollectionKeyPolicy Of K` with `Hash(key As K) As Integer` and
`Equal(leftKey As K, rightKey As K) As Boolean`. The package includes
`IntegerKeyPolicy` and `ExactStringKeyPolicy`. The latter compares exactly,
including case and canonical spelling; it does not normalize or case-fold text.
Custom equality can apply a domain-specific identity rule to class or primitive
keys. Nil class keys are accepted when the supplied policy handles them.

Construct `New HashMap Of K,V(policy, maxEntries = 100000)` or
`New HashSet Of T(policy, maxEntries = 100000)`. A Nil policy and limits outside
1..1,000,000 throw `InvalidArgumentException`.

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer collectionskit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
