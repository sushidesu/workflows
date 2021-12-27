# Workflows (for sushidesu)

My reusable workflows.

- compare-static-assets.yml (inspired by: https://zenn.dev/ryo_kawamata/articles/improve-dependabot-pr)

## Compare Static Assets

Compare static assets between main branch and current branch and comment the results on the PR.

- name: `sushidesu/workflows/.github/workflows/compare-static-assets.yml@main`
- source: [compare-static-assets.yml](.github/workflows/compare-static-assets.yml)

### Properties

| name | required | description |
| --- | --- | --- |
| build_command | `true` | command to build static assets. |

### Example Usage

Here is a sample directory structure.

```
.
├── .github
│  └── workflows
│     └── sample-workflow.yml
├── package.json
└── src
   └── main.ts // <- build target
```

#### .github/workflows/sample-workflow.yml


```yml
name: Sample Workflow (Compare Static Assets)

on:
  pull_request:
    types:
      - opened
      - synchronize
      - reopened

jobs:
  compare-static-assets:
    uses: sushidesu/workflows/.github/workflows/compare-static-assets.yml@main
    with:
      build_command: yarn build:base
```

#### package.json

Your build command needs to receive `$OUT_DIR`. The workflow specifies the build directory through it.

```json5
{
  // ...
  "scripts": {
    "build": "export OUT_DIR=dist; yarn build:base",
    "build:base": "esbuild src/main.ts --outfile=$OUT_DIR/main.js --bundle"
  }
}

```
