# Stable Haskell

## View the presentation

Visit https://stable-haskell.github.io/2026-zurihac-talk/

## Building

Build via:

```sh
pandoc -t slidy -s stable.md -o stable-haskell.html
```

For a standalone html:

```sh
pandoc -t slidy -s --embed-resources --standalone stable.md -o stable-haskell.html
```

For PDF:

```sh
pandoc stable.md -t beamer --pdf-engine=xelatex -o stable.pdf
```
