# PureScript Yoga GitHub Actions

Reusable GitHub Actions for PureScript Yoga packages.

## Actions

### `setup` - Complete PureScript Yoga Stack Setup

Sets up everything needed to build and test PureScript Yoga packages:
- Node.js 22+ (required for Spago's `node:sqlite` dependency)
- Bun (for fast package installation)
- PureScript compiler
- Spago package manager

#### Usage

```yaml
- uses: rowtype-yoga/purescript-yoga-actions/setup@main
```

#### With Custom Versions

```yaml
- uses: rowtype-yoga/purescript-yoga-actions/setup@main
  with:
    purescript-version: '0.15.15'
    spago-version: 'unstable'
    node-version: '22'
    bun-version: 'latest'
    install-deps: 'true'
```

#### Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `purescript-version` | PureScript compiler version | `0.15.15` |
| `spago-version` | Spago version (`latest` or semver) | `latest` |
| `bun-version` | Bun version | `latest` |
| `install-deps` | Auto-run `bun install` | `true` |

## Example Workflows

### Basic Build

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: rowtype-yoga/purescript-yoga-actions/setup@main
      - run: spago build
```

### With Tests

```yaml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: rowtype-yoga/purescript-yoga-actions/setup@main
      - run: spago build
      - run: spago test
```

### With Redis Service

```yaml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      redis:
        image: redis:7-alpine
        ports:
          - 6380:6379
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4
      - uses: rowtype-yoga/purescript-yoga-actions/setup@main
      - run: spago test
```

## Why Node 22 + Bun?

**Node.js 22** is required because Spago 1.x uses the `node:sqlite` built-in module (only available in Node 22+). When you run `spago`, it executes with the system Node.js.

**Bun** is used for incredibly fast npm package installation (`bun install` is much faster than `npm install`).

## License

MIT
