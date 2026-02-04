# PureScript Yoga GitHub Actions

Reusable GitHub Actions for PureScript Yoga packages.

## Actions

### `setup` - Complete PureScript Yoga Stack Setup

Sets up everything needed to build and test PureScript Yoga packages:
- Node.js 22+ (required for Spago 1.x)
- PureScript compiler
- Spago package manager
- Bun (for fast npm package management)

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
| `node-version` | Node.js version (required for Spago 1.x) | `22` |
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

## Why Node.js 22?

Spago 1.x requires Node.js 22+ because it uses the `node:sqlite` built-in module. Even though we use Bun for package management, Spago itself runs on the Node.js runtime.

## License

MIT
