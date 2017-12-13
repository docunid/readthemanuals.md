# Agnostic
Refers to something that is generalized so that it is interoperable among various systems.

*Example:* Platform-agnostic software runs on any combination of operating system and underlying processor architecture.

# Bubblesort
Sometimes referred to as **sinking sort**, is a simple sorting algorithm that repeatedly steps through the list to be sorted, compares each pair of adjacent items and swaps them if they are in the wrong order. The pass through the list is repeated until no swaps are needed, which indicates that the list is sorted.
- Mergesort
- Heapsort
- Insertion sort
- Quicksort
- Selection sort

# Entropy
"Randomness”. Computers need entropy for a lot of things, particularly cryptographic operations.

*Example:* `/dev/random` and `/dev/urandom`. Random is the high-quality one. When it runs out, it blocks until there is more available. urandom will supply high-quality entropy until it runs out, then it will generate more programmatically using the kernel's PRNG. It doesn't block, but it might not be as secure as random.

# Isomorphic applications
Applications that are build with the same language on the back and front-ends

# TOML
TOML stands for Tom’s Own Minimal Language. It is a configuration language vaguely similar to YAML or property lists, but far, far better.

# YAML
YAML Ain't Markup Language
