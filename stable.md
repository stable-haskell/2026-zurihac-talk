% Stable Haskell
% Julian Ospald
% June, 2026

# Introduction

## What is Stable Haskell?

![](turtle.jpg){#id .class height=500px}

## State of upstream GHC

* lack of stability
* lack of engineering leadership
* poor contribution experience
* communication issues
* too tight coupling between systems/projects
* driving change is hard
* 😭

## Lack of stability

**Upgrading GHC is costly.**

. . .

* GHC ships with changes to base and boot libraries
  - user can't opt out
  - cabal solver is peculiar [#9669](https://github.com/haskell/cabal/issues/9669)
* GHC ships with breaking changes to the surface language
  - simplified subsumption
  - changes in warnings/error flags
  - [GHC stability principles](https://github.com/ghc-proposals/ghc-proposals/blob/master/principles.rst#3GHC-stability-principles)
* Breaking changes to the bindist
* Breaking changes to the build system

## Lack of engineering leadership

* lots of experts are working on GHC
* GHC steering committee oversees the language development
* GHC HQ solves stalemates in engineering disputes
  - has no active role
  - decisions in GHC are usually "collective"
  - someone just has to drive something
  - someone has to fund something
* things develop "organically" instead of strategically
* a possible structure of documenting goals and progress
  - do vs want vs can

## Poor contribution experience

See [#26728](https://gitlab.haskell.org/ghc/ghc/-/issues/26728).

. . .

- self-hosted gitlab
  - poor performance
  - constant html errors and UI glitches
  - commenting on large MRs is impossible
  - ...
- CI and testsuite
  - too many levels of CI (validate, full-ci, nightly)
  - turnaround times of 6+ hours
  - inability to reproduce failures (no access to runners on obscure architectures)
- poor merge coordination
- hadrian
  - caching doesn't actually work (try changing cabal file or switching branches)
  - custom build system

## Poor contribution experience (pt. 2)

Case study [Support statically linking executables properly](https://gitlab.haskell.org/ghc/ghc/-/merge_requests/14935):

. . .

- maybe took half a week to implement
- took ~3 months to get merged
- still not backported to 9.14, because we missed the barely communicated deadline

## Communication issues

- staying in the loop is hard
- who are even the stakeholders?
- getting involved in releases is confusing
- why are there no calls for help
  - FreeBSD?

## Driving change is hard

- how to gather consensus
  - who is responsible?
  - who is the domain expert?
- contributing is unpleasant and below current open source standards
  - despite there being a helpful team of GHC developers
- general risk aversion
  - why can't we be more bold?
  - why is developing confidence about changes in GHC so hard?

  - GHC and cabal
  - GHC and hadrian
  - GHC and gitlab

## Impact

- alienating new contributors
  - whole development process and tools focus on core developers
- backporting is hard (dealing with flaky tests and spurious CI failures)
- everything becomes quietly coupled, because there's no vision of the systems design
- innovation stalls

# Stable haskell to the rescue

## Prior art and related efforts

* [GHC stability principles](https://github.com/ghc-proposals/ghc-proposals/blob/master/principles.rst#3GHC-stability-principles)
* base split proposal
* GHC LTS (since 9.14)
* GHC breakage inventory at [https://github.com/tomjaguarpaw/tilapia](https://github.com/tomjaguarpaw/tilapia)
* GHC release management [document](https://gitlab.haskell.org/ghc/ghc-hq/-/blob/main/release-management.mkd?ref_type=heads)

## Motivation

So why have stable Haskell at all?

. . .

* pave the way to maintain a stable Haskell compiler
  - this requires lots of novel and experimental approaches first
* develop a vision of the systems design
  - decoupling of the components
* use a modern approach to hosting, CI and contribution
  - github right now, but who knows
* allow easy and swift shipping of updates
  - ghcup

## Who is stable Haskell?

Our team includes the engineers who built:

* GHC's Native Bignum Backend — Arbitrary-precision integers
* AArch64 Native Code Generator — ARM support in GHC
* JavaScript Backend in GHC — Haskell in the browser
* GHCup — The standard Haskell toolchain installer
* haskell.nix — Reproducible Haskell builds with Nix
* Cross-compilation support — Windows, iOS, Android targets
* GHC's In-Memory Loader & Linker — Dynamic loading infrastructure
* ...

## What we are working on

* ✅ Migrating to github
* ✅ Getting rid of Hadrian
* ✅ RTS split
*  🚧 cabal-install and cross compilation
*  🚧 retargetable GHC
*  🚧 one binary distribution
* ⭕️ our own LTS releases

## Deep dive: RTS split

* terrible coupling between GHC, hadrian and RTS
  - `rts.cabal` only describes one library
* hadrian escape hatches do the rest...
  - rebuilding the rts for each different flavor (+-threaded, +-debug)
  - filename conventions and interaction with `Ways`
  - bundled libffi copy
  - knows about per-file flags
  - ...
* stable-haskell uses cabal to build GHC and the rts
  - we have 4 real sublibraries for each flavor
  - the rts is completely described by the cabal file
  - requires changes to cabal
