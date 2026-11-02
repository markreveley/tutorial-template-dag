# Tutorial web editions

A builder for the static web edition of a tutorial kept as one Markdown file, plus the parts every edition shares. The IdentiKey tutorial (`identikey-dev/tutorials/web/`) and the Lightning Mesh tutorial (`lightning-mesh-dev/tutorials/web/`) are both built with it. Until November 2026 this was the `tutorial/` folder of artifact-dag; it moved here with its history.

## Where a project finds it

A project runs `build.py` here with its own `config.json`, from a small wrapper (identikey-dev's `tutorials/web/build.py`) or by path. A wrapper looks for this repo at `$TUTORIAL_TEMPLATE_DAG`, then at `../tutorial-template-dag` beside the project's checkout, then at `~/dev/repos/tutorial-template-dag`. CI checks this repo out beside the project and sets `TUTORIAL_TEMPLATE_DAG`. The repo is public, so that checkout needs no token.

## What is here

- `build.py`: generates the pages. `python3 build.py --config <web dir>/config.json` (needs `pip install markdown`). The keys of the config are listed at the top of the file.
- `assets/style.css`, `assets/site.js`: the design tokens (light and dark), layout, and the runtime that mounts widgets, numbers figures and builds the "On this page" rail.
- `assets/ikcrypto.js`: SHA-256, SHA-512, BLAKE3, HMAC, HKDF and Ed25519 for the interactives, bundled from `@noble/hashes@1.5.0` and `@noble/ed25519@2.1.0` (both MIT). The pinned inputs and the rebuild command are in `assets/vendor-src/`.
- `widgets/*.js`: widgets that more than one tutorial uses (`hash`, `signverify`). A tutorial names the ones it uses in `shared_widgets`.
- `CONTRIBUTING-widgets.md`: the widget and diagram contract.

## What a tutorial keeps

```
<web dir>/
  config.json              pages, nav, brand, source path, shared widgets
  content/figures.json     where each figure goes: page, anchor phrase, kind, name, caption
  content/examples/*.md    code examples; first line is the title
  assets/diagrams/*.svg    its diagrams
  assets/widgets/*.js      its own widgets
```

The builder copies the shared assets and widgets into the tutorial's `assets/` before it writes the pages, so the edition opens from `file://` on its own. Those copies are generated: change them here.

The build fails loudly if a figure's anchor phrase does not appear exactly once on its page. In the contents list, a section or Part that is not yet in the edition stays plain text.
