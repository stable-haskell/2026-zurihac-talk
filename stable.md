% Stable Haskell
% Julian Ospald
% June, 2026

# Introduction

## What is Stable Haskell?

![](turtle.jpg){#id .class height=500px}

## State of GHC

* lack of stability
* lack of engineering leadership
* driving change is hard
* bloated processes
* poor contribution experience
  * dysfunctional CI
  * custom build system (hadrian)
  * obscure test suite that doesn't catch basic correctness issues
  * a Gitlab instance that is sometimes not down
* 😭

## Case studies

- getting proper FreeBSD support (tier 2 platforms)
  - sometimes worked, sometimes didn't
  - got silently dropped
  - no call for help
- gathering consensus on larger engineering issues
  - architecture, CI, release cadence, ...
- contributing patches
  - [Support statically linking executables properly](https://gitlab.haskell.org/ghc/ghc/-/merge_requests/14935)
  - maybe took half a week to implement
  - took ~3 months to get merged
  - still not backported to 9.14, because we missed the barely communicated deadline
- CI
  - ...

## Impact

- alienating new contributors
  - whole development process and tools focus on core developers
- backporting is hard (dealing with flaky tests and spurious CI failures)
- everything becomes quietly coupled
  - GHC and cabal
  - GHC and hadrian
  - GHC and gitlab
- innovation stalls

# Stable haskell to the rescue

## How it started

* Angerman got angry

