# Haskell

**TL;DR**: [Simple Haskell](https://www.simplehaskell.org/) and [`rio`](https://github.com/commercialhaskell/rio#readme). Write the dumbest Haskell code.

This section describes **how I write Haskell**.

It is not **the** way to write Haskell, it is the way **I** write it, and it works for me.

## Setting up Haskell on Windows

- Install [Chocolatey](https://chocolatey.org/install)
- Run `choco install haskell-dev` from an admin PowerShell, if that doesn't work, try:
  - Install `cabal v3` with `choco install cabal` from an admin PowerShell
  - Install `ghc 8.6.5` with `choco install ghc --version 8.6.5` from an admin PowerShell
  - Make sure that you can run the `cabal` and `ghc` commands. If not, you'll need to add their paths to your PATH environment variable. Generally, they will be:
    - `C:\ProgramData\chocolatey\lib\ghc\tools\ghc-8.6.5\bin` for `ghc`
    - `%appdata%\cabal\bin` for `cabal`
- After installing these tools, run the following commands to install additional tooling for linting and styling:
  - `cabal install hspec-discover`
  - `cabal install hlint`
  - `cabal install ormolu`

## Setting up an IDE

- Clone the [`haskell-ide-engine` repo](https://github.com/haskell/haskell-ide-engine) somewhere and install it for GHC 8.6.5:
  - `git clone https://github.com/haskell/haskell-ide-engine && cd haskell-ide-engine`
  - `./cabal-hie-install hie-8.6.5`
  - `./cabal-hie-install data`
- Install the [VSCode HIE server extension](https://marketplace.visualstudio.com/items?itemName=alanz.vscode-hie-server) for Visual Studio Code.
- Install the [VSCode ormolu extension](https://marketplace.visualstudio.com/items?itemName=sjurmillidahl.ormolu-vscode) and set it as the default formatter.

## Build tool

I use [`cabal`](https://www.haskell.org/cabal/), but only over version **3.0**. I learned to embrace it with its quirks, and to be honest, the development is evolving rapidly, adding features, and removing pain-points very fast.

I used to use Stack, because it installed GHC for you, and allowed to specify common dependencies for all of the parts of your project (library, executable, tests...)

Now, with [`ghcups` for Windows](https://github.com/kakkun61/ghcups), and [`ghcup` for other OSes](https://www.haskell.org/ghcup/), the GHC installation and version management is so easy.

Common dependencies and extensions are now natively integrated using a feature called [Common Stanzas](https://vrom911.github.io/blog/common-stanzas), no need to go back to Stack on my side.

Stack, Nix, and others are nice, but Cabal is very comfortable for me, and has proven to work in the 80% of the situations with the 20% of effort.

## Standard Library (Prelude)

As my standard library, I use [`rio`](https://github.com/commercialhaskell/rio#readme). It is a bit more heavier than other alternatives, but it has some nice stuff included with it, that I don't like to think about every single project:

* Logging
* A well-defined architecture framework
* Common libraries that are reexported

## Language Extensions

I always avoid complex Haskell code, but I always enable these extensions just in case. As they don't modify the behavior of my code:

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

## Testing

I use [`hspec`](http://hspec.github.io/) for writing my test suites.

I also use [`hedgehog`](https://hedgehog.qa/) for writing my property based tests, that I plug into my `hspec` test suites using [`hw-hspec-hedgehog`](https://github.com/haskell-works/hw-hspec-hedgehog).