# LINEA calculator modules

The modules of the calculator example of [LINEA](https://github.com/lbdudc/linea): a calculator product line whose products choose which operations they have. Each one is a folder with its `.linea/module.json`, its feature model and its annotated code.

| Module | What it gives | Needs |
|---|---|---|
| [`basic-module`](modules/basic-module) | The calculator every product is: display, numeric keyboard, clear, and the REST service that the operations fill in | |
| [`basic-operations-module`](modules/basic-operations-module) | The basic operations: add, subtract and multiply | `basic-module` |
| [`capabilities-module`](modules/capabilities-module) | The capabilities of the calculator: decimal numbers | `basic-module` |
| [`advanced-operations-module`](modules/advanced-operations-module) | The advanced operations: divide, power and square root; divide and square root need decimal numbers | `basic-module`, `capabilities-module` |

The modules need no metamodel besides the feature model every line has (from [linea-metamodels](https://github.com/lbdudc/linea-metamodels)).

## Publishing

LINEA reads every module from a repository of its own, with the topic `linea-module` and a tag `vX.Y.Z` per version. This repository is where they are written; `publish-registry.mjs` of the LINEA server makes those repositories from it. With this repository next to `linea`:

```bash
cd linea/apps/server
GITHUB_TOKEN=<token with repo scope> node scripts/publish-registry.mjs --host github --owner <user or organisation> \
  --dir ../../../linea-calculator-modules --registry github:<owner>/linea-registry
```

On GitLab: `GITLAB_URL=… GITLAB_TOKEN=… node scripts/publish-registry.mjs --group <group> --dir ../../../linea-calculator-modules`.

To publish a new version, raise `version` in the module's `.linea/module.json` and run it again: versions already tagged are left alone.
