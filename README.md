# PureScript Yoga GitHub Actions

Reusable GitHub Actions for PureScript Yoga packages.

## Actions

### `setup` - Complete PureScript Yoga Stack Setup

Sets up everything needed to build and test PureScript Yoga packages:
- Bun (JavaScript runtime + fast package manager)
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
| `spago-version` | Spago version (`latest`, `unstable`, or semver) | `unstable` |
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

## Why Bun?

Bun is a fast all-in-one JavaScript runtime that can run Spago 1.x (including its `node:sqlite` dependency) and provides incredibly fast package installation. No separate Node.js installation needed!

## License

MIT
