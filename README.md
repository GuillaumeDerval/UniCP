# UniCP

This repository is (at least, will be, or aims to be) a proposal for a standard C interface to CP solvers. 

## Goals

- Be developper-oriented. UniCP is a low-level API, to be used by CP solver (such as Gecode, Choco, OscaR or MiniCP) developpers and high-level APIs (such as CPMpy) or application developpers, allowing these tools to be used interchangeably.
- Be researcher-minded. CP solvers are used a lot by the CP research community itself.
- Be used-minded. The API should allow to build easy-to-use high-level APIs 
- Be simple. The API should be minimalistic, in order to minimize the maintenance burden.
- Be backward-compatible. Older, unmaintained solvers should be able to discuss with newer high-level APIs or application, and conversely.
- Be extensible (or even forward-compatible). Different CP solvers have different specificities; the most obvious being custom constraints, but it can also be custom types of variables, search heuristics.
- Be discoverable. Applications and high-level APIs should be able to know the capabilities of the underlying solver, aka the features available via UniCP.
- Be safe. Memory and thread safety is important, and allowing to have multiple instances of the solver running concurrently too.
- Be usable. A lot of control on the solver should be available. This includes:
  - starting and stopping the search,
  - modifying parameters on-the-fly (including adding new variables and constraints)
  - querying and modifying domains, 
  - querying solver-state, 
  - gathering simple statistics, 
  - implementing a custom search in the host language,
  - having basic control on saving and backtracking state, at least on solver that supports that.
- Be open. Its licence should allow anyone to use it freely and without constraints (pun intented)

## Non-goals

- Supporting complex, too implementation-specific behaviors such as LNS. However, it should be possible to implement them in the application using UniCP.
- Implementing constraints in the host language.
- Exposing complex structures (such as sparsesets, ...) to the host language. Structures that should not be exposed to the user of the CP solver should not be available 
