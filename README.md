# UniCP

This repository is (at least, will be, or aims to be) a proposal for a standard C interface to CP solvers. 

## Goals and non-goals

### Goals

1. Be developper-oriented. UniCP is a low-level API, to be used by CP solver (such as Gecode, Choco, OscaR or MiniCP) developpers and high-level APIs (such as CPMpy) or application developpers, allowing these tools to be used interchangeably.
2. Be researcher-minded. CP solvers are used a lot by the CP research community itself.
3. Be user-minded. The API should allow to build easy-to-use high-level APIs 
4. Be simple. The API should be minimalistic, in order to minimize the maintenance burden.
5. Be backward-compatible. Older, unmaintained solvers should be able to discuss with newer high-level APIs or application, and conversely.
6. Be extensible (or even forward-compatible). Different CP solvers have different specificities; the most obvious being custom constraints, but it can also be custom types of variables, search heuristics.
7. Be discoverable. Applications and high-level APIs should be able to know the capabilities of the underlying solver, aka the features available via UniCP.
8. Be safe. Memory and thread safety is important, and allowing to have multiple instances of the solver running concurrently is too (avoid globally mutable state).
9. Be platform-independent. No hypothesis on the destination platform should be taken, apart that it implements dynamic linking and that it has a C compiler.
10. Be usable. A lot of control on the solver should be available. This includes:
   1. starting and stopping the search,
   2. modifying parameters on-the-fly (including adding new variables and constraints)
   3. querying and modifying domains, 
   4. querying solver-state, 
   5. gathering simple statistics, 
   6. implementing a custom search in the host language,
   7. having basic control on saving and backtracking state, at least on solver that supports that.
11. Be performant. Using most of the API of UniCP must be nearly overhead-free. API calls that involve a large overhead (such as callbacks to the host language) must be optionnal, but available.
12. Be open. Its licence should allow anyone to use it freely and without constraints (pun intented)

### Non-goals

- Supporting complex, too implementation-specific behaviors such as LNS. However, it should be possible to implement them in the application using UniCP.
- Implementing constraints in the host language.
- Exposing complex structures (such as sparsesets, ...) to the host language. Structures that should not be exposed to the user of the CP solver should not be available.

## Design decisions and rationale

### Usage of C

It is obvious that we need a low-level language to which most higher-level language can interface. Rust, C and C++ are immediate contenders. Rust seems overkill for that purpose, and while the expressive power of C++ and its standard library would be nice features to have, the C++ ABI is not well defined on some platforms (Windows), and so only C remains.

This does not forbids implementers (CP solver developpers) to use C++ or Rust (or actually anything else sensible) as they all provide a way to link function "like in C" (`extern "C"` in C++ for example).

This means that the API will have to remain very simple and use basic types. No std::vector, std::map, .... Linked Lists and tables are the way to go.

### Usage of tables

Listing functions and parameters as table is both a performance-enhancement choice and a choice detrimental to performance. We could have chosen to use instead lists, but that would have added a lot of indirections; we could also use dictionnaries by default, but they are not available in standard C. The rationale is as follows
- CP solver developpers can create the tables statically at compile time, so it's easy to use;
- API users will want to read anyway the table in full, so they can create a dictionnary directly in the target language.
