# Haskell

**TL;DR**: [Simple Haskell](https://www.simplehaskell.org/) and [`rio`](https://github.com/commercialhaskell/rio#readme). Write the dumbest Haskell code.

This section describes **how I write Haskell**.

It is not **the** way to write Haskell, it is the way **I** write it, and it works for me.

## Build tool

I use [`stack`](http://haskellstack.org/). 

Cabal, Nix, and others are nice, but Stack is very comfortable for me, and has proven to work in the 80% of the situations with the 20% of effort.

## Standard Library (Prelude)

As my standard library, I use [`rio`](https://github.com/commercialhaskell/rio#readme). It is a bit more heavier than other alternatives, but it has some nice stuff included with it, that I don't like to think about:

* Logging
* A well-defined architecture framework
* Common libraries that are reexported

## Language Extensions

I always avoid complex Haskell code, but I always enable these extensions just in case. As they don't modify the behaviour of my code:

```yaml
default-extensions:
  - AutoDeriveTypeable
  - BangPatterns
  - BinaryLiterals
  - ConstraintKinds
  - DataKinds
  - DefaultSignatures
  - DeriveDataTypeable
  - DeriveFoldable
  - DeriveFunctor
  - DeriveGeneric
  - DeriveTraversable
  - DoAndIfThenElse
  - EmptyDataDecls
  - ExistentialQuantification
  - FlexibleContexts
  - FlexibleInstances
  - FunctionalDependencies
  - GADTs
  - GeneralizedNewtypeDeriving
  - InstanceSigs
  - KindSignatures
  - LambdaCase
  - MonadFailDesugaring
  - MultiParamTypeClasses
  - MultiWayIf
  - NamedFieldPuns
  - NoImplicitPrelude
  - OverloadedStrings
  - PartialTypeSignatures
  - PatternGuards
  - PolyKinds
  - RankNTypes
  - RecordWildCards
  - ScopedTypeVariables
  - StandaloneDeriving
  - TupleSections
  - TypeFamilies
  - TypeSynonymInstances
  - ViewPatterns
```

## Compiler Options

I always enable these options so I can be sure that my code has been verified a little bit by my trusty compiler:

```yaml
ghc-options:
  - -Wall
  - -fno-warn-orphans
  - -optP-Wno-nonportable-include-path
  - -Wincomplete-uni-patterns
  - -Wincomplete-record-updates
  - -Wcompat
  - -Widentities
  - -Wredundant-constraints
  - -Wmissing-export-lists
  - -Wpartial-fields
  - -fhide-source-paths
  - -freverse-errors
```

Before a release I compile using `-Werror` so I can catch those pesky warnings.