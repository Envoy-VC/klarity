# Klarity

Klarity is a collection of [Biome](https://biomejs.dev) presets that provide a set of rules for Linting and Formatting.

It is incredibly type-safe, and inspired by `@vercel-style-guide`, enforcing consistent code quality across your codebase.

<div>
  <img src="https://img.shields.io/github/actions/workflow/status/Envoy-VC/klarity/release.yml" alt="" />
  <!-- <img src="https://img.shields.io/npm/dy/klarity" alt="" /> -->
  <img src="https://img.shields.io/npm/v/klarity" alt="" />
  <img src="https://img.shields.io/github/license/Envoy-VC/klarity" alt="" />
  <a href="https://biomejs.dev"><img alt="Static Badge" src="https://img.shields.io/badge/Formatted_with-Biome-60a5fa?style=flat&logo=biome"></a>
  <a href="https://biomejs.dev"><img alt="Linted with Biome" src="https://img.shields.io/badge/Linted_with-Biome-60a5fa?style=flat&logo=biome"></a>
</div>

## Installation

To use Klarity, you need to install `biomejs`

```bash
npm install --save-dev --save-exact @biomejs/biome
# or
yarn add --dev --exact @biomejs/biome
# or
pnpm add --dev --exact @biomejs/biome
# or
bun add --dev --exact @biomejs/biome
```

Then, you can extend your `biome.json` file with the following:

```json
{
  "$schema": "https://biomejs.dev/schemas/1.9.4/schema.json",
  "extends": ["klarity/biome"]
}
```

### Preset Support for Monorepos

Biome does not support monorepos out of the box. Issue [here](https://github.com/biomejs/biome/issues/2228)

However, you can use the overrides configuration to change the behaviour of Biome in certain packages.

For example:

```json
{
  "$schema": "https://biomejs.dev/schemas/1.9.4/schema.json",
  "extends": ["klarity/biome"],
  "overrides": [
    {
      "include": ["packages/ui/**"],
      "linter": {
        "rules": {
          "suspicious": {
            "noConsoleLog": "error"
          }
        }
      }
    }
  ]
}
```

---

### Setting up VSCode Settings

Create a `.vscode/settings.json` file with the following contents to enable full formatting and fixing on save:

```json
{
  "typescript.tsdk": "node_modules/typescript/lib",
  "editor.defaultFormatter": "biomejs.biome",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "quickfix.biome": "explicit",
    "source.organizeImports.biome": "explicit"
  },
  "[typescript]": {
    "editor.defaultFormatter": "biomejs.biome"
  },
  "[json]": {
    "editor.defaultFormatter": "biomejs.biome"
  },
  "[css]": {
    "editor.defaultFormatter": "biomejs.biome"
  },
  "[graphql]": {
    "editor.defaultFormatter": "biomejs.biome"
  },
  "[javascript]": {
    "editor.defaultFormatter": "biomejs.biome"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "biomejs.biome"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "biomejs.biome"
  }
}

```


## Typescript Presets

Klarity provides presets for TypeScript, which are designed to be used with any type of project.

<!-- The tricky thing about tsconfig.json is there is not a single config file that can work for everyone. But, with two or three questions, we can get there: -->

Choosing the right preset for your project is as simple as asking a simple questions:

### Are You Using tsc To Turn Your .ts Files Into .js Files?

If yes, use the following presets:

```json
{
  // My code runs in the DOM:
  "extends": "klarity/tsconfig/tsc/dom/app", // For an app
  "extends": "klarity/tsconfig/tsc/dom/library", // For a library
  "extends": "klarity/tsconfig/tsc/dom/library-monorepo", // For a library in a monorepo

  // My code doesn't run in the DOM (for instance, in Node.js):
  "extends": "klarity/tsconfig/tsc/no-dom/app", // For an app
  "extends": "klarity/tsconfig/tsc/no-dom/library", // For a library
  "extends": "klarity/tsconfig/tsc/no-dom/library-monorepo" // For a library in a monorepo
}
```

If no, then you're probably using an external bundler. Most frontend frameworks, like Vite, Remix, Astro, Next, and others, will fall into this category.


```json
{
  // My code runs in the DOM:
  "extends": "klarity/tsconfig/bundler/dom/app", // For an app
  "extends": "klarity/tsconfig/bundler/dom/library", // For a library
  "extends": "klarity/tsconfig/bundler/dom/library-monorepo", // For a library in a monorepo

  // My code _doesn't_ run in the DOM (for instance, in Node.js):
  "extends": "klarity/tsconfig/bundler/no-dom/app", // For an app
  "extends": "klarity/tsconfig/bundler/no-dom/library", // For a library
  "extends": "klarity/tsconfig/bundler/no-dom/library-monorepo" // For a library in a monorepo
}
```

### Other Options

If you app has `jsx`, you can add the following to your `tsconfig.json`:

```json
{
  "extends": "klarity/tsconfig/bundler/dom/app",
  "jsx": "react-jsx"
}
```