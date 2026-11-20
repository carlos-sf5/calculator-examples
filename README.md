# LINEA calculator example

Everything the calculator example of [LINEA](https://github.com/lbdudc/linea) is made of: a calculator product line whose products choose which operations they have. Every package is a folder: a metamodel has its `metamodel.json`, a module its `.linea/module.json`, its feature model and its annotated code.

## Metamodels

| Metamodel | What it describes |
|---|---|
| [`common`](metamodels/common) | Shared terminals and data types (a library: it has no entry rule) |
| [`featuremodel`](metamodels/featuremodel) | Feature model of an SPL and the feature selection of its products: every line has it |

They are the same as in [linea-metamodels](https://github.com/lbdudc/linea-metamodels), with the same versions: publishing this folder where those are already published leaves them alone.

## Modules

| Module | What it gives | Needs |
|---|---|---|
| [`basic-module`](modules/basic-module) | The calculator every product is: display, numeric keyboard, clear, and the REST service that the operations fill in | |
| [`basic-operations-module`](modules/basic-operations-module) | The basic operations: add, subtract and multiply | `basic-module` |
| [`capabilities-module`](modules/capabilities-module) | The capabilities of the calculator: decimal numbers | `basic-module` |
| [`advanced-operations-module`](modules/advanced-operations-module) | The advanced operations: divide, power and square root; divide and square root need decimal numbers | `basic-module`, `capabilities-module` |

## Publishing

LINEA reads every package from a repository of its own, with the topic `linea-metamodel` or `linea-module` and a tag `vX.Y.Z` per version. This repository is where they are written; `publish-registry.mjs` of the LINEA server makes those repositories from it. With this repository next to `linea`:

```bash
cd linea/apps/server
GITHUB_TOKEN=<token with repo scope> node scripts/publish-registry.mjs --host github --owner <user or organisation> \
  --dir ../../../calculator-examples --registry github:<owner>/linea-registry
```

On GitHub every package is a repository of the owner (`<owner>/basic-module`, `<owner>/featuremodel`...). Start the LINEA server with `LINEA_REGISTRY=github:<owner>/linea-registry` and `LINEA_FEATURE_METAMODEL=github:<owner>/featuremodel`.

On GitLab: `GITLAB_URL=… GITLAB_TOKEN=… node scripts/publish-registry.mjs --group <group> --dir ../../../calculator-examples --registry gitlab:<group>/index`, which makes `<group>/metamodels/<name>` and `<group>/modules/<name>`.

To publish a new version, raise `version` in the package's manifest and run it again: versions already tagged are left alone.
