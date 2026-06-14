# jarret-parley-vscode

An interactive Visual Studio Code editor for [Jarret](https://github.com/ulysses4ever/jarret-lang),
modeled on [jpolitz/pyret-parley-vscode](https://github.com/jpolitz/pyret-parley-vscode)
and powered by a Jarret-flavored
[code.pyret.org](https://github.com/ulysses4ever/code.jarret.org) build
embedded into a VSCode webview.

Opens `.jrt` files in a custom editor that runs the full Jarret
compiler + runtime inside a webview pane, with an interactive REPL on
the side.

## Architecture

`jarret-parley-vscode` is a fork of
[jpolitz/pyret-parley-vscode](https://github.com/jpolitz/pyret-parley-vscode).
The substantive differences:

- Source extension is `.jrt` (Jarret), not `.arr` (Pyret).
- The embedded compiler bundle comes from
  [`ulysses4ever/code.jarret.org`](https://github.com/ulysses4ever/code.jarret.org)
  — a fork of `code.pyret.org` whose `pyret-lang` dependency is pinned
  to our [`ulysses4ever/jarret-lang`](https://github.com/ulysses4ever/jarret-lang)
  fork. That bundle includes the Jarret translator, so loading a `.jrt`
  source in the webview routes through `parse-java` rather than
  `parse-pyret`.
- VSCode command IDs and configuration keys are namespaced
  `jarret-parley.*`.

## Building

You need a built copy of `code.jarret.org` on disk. The expected
sibling layout:

```
~/Dev/pyret/
    code.jarret.org/        # cloned + `make web-local`
    jarret-parley-vscode/   # this repo, with build/ symlinked to ../code.jarret.org/build
```

Set up the symlink and compile:

```
ln -s ../code.jarret.org/build build
npm install
npm run compile
```

Webpack copies `build/web` into `dist/web/build/web` and bundles the
TypeScript extension code. Output appears in `dist/web/extension.js`.

To package a `.vsix`:

```
npx @vscode/vsce package
```

To test in a browser-based VSCode:

```
npx vscode-test-web --browserType=chromium --extensionDevelopmentPath . ./sampleFiles/
```

## Settings

- `jarret-parley.defaultContext` — starter context (e.g. `starter2024`).
- `jarret-parley.urlFileMode` — how to resolve `url-file()` imports.

## Issues

Jarret-language issues belong in
[ulysses4ever/jarret-lang](https://github.com/ulysses4ever/jarret-lang/issues).
Extension-specific issues go to
[ulysses4ever/jarret-parley-vscode](https://github.com/ulysses4ever/jarret-parley-vscode/issues).

## License

Apache-2.0. Upstream attribution: pyret-parley-vscode is by Joe Politz,
with grammar/language-configuration originally by Seth Poulsen.
