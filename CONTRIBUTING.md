# Contributing to gama

Thank you for your interest in contributing to gama.  
This document explains how the project works and what we expect from contributors.

---

## Table of contents

- [Code of conduct](#code-of-conduct)
- [How to contribute](#how-to-contribute)
- [Branch model](#branch-model)
- [Pull request process](#pull-request-process)
- [CI checks](#ci-checks)
- [Commit conventions](#commit-conventions)
- [Local development](#local-development)
- [Project structure](#project-structure)

---

## Code of conduct

Be respectful. Disagreement is fine; disrespect is not.  
We follow the [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).

---

## How to contribute

### Reporting a bug

Open an issue using the **Bug report** template.  
Include: Go version, OS, gama version, steps to reproduce, and expected vs actual behavior.

### Suggesting a feature

Open an issue using the **Feature request** template before writing any code.  
This avoids effort on proposals that don't align with the project direction.

### Submitting a fix or feature

1. Check existing issues and open PRs — someone may already be working on it.
2. For small fixes (typos, docs, obvious bugs) you can open a PR directly.
3. For anything significant, open an issue first and wait for feedback.

---

## Branch model

```
main                   ← stable, always deployable
└── feat/your-feature  ← your work lives here
└── fix/issue-123      ← bug fixes
└── docs/update-readme ← documentation only
└── chore/update-deps  ← maintenance tasks (dependencies, tooling, build)
```

**`main` is protected.** Direct pushes are blocked for everyone including maintainers.  
All changes enter through a pull request.

### Branch naming

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feat/<short-description>` | `feat/wasm-plugin-runtime` |
| Bug fix | `fix/<issue-or-description>` | `fix/opa-timeout-handling` |
| Docs | `docs/<what-changed>` | `docs/update-plugin-guide` |
| Chore | `chore/<what-changed>` | `chore/update-mcp-go-dep` |

---

## Pull request process

### Before opening a PR

- [ ] Fork the repo and create your branch from `main`
- [ ] All CI checks pass locally (`make test`, `make lint`, `make build`)
- [ ] New code has tests
- [ ] Documentation is updated if behavior changed
- [ ] Your branch is up to date with `main`

### Opening the PR

- Use a clear title: `feat: add rate limiting per client` not `updates`
- Fill in the PR template completely
- Link the related issue: `Closes #123`
- Keep PRs focused — one concern per PR

### Review

- At least **1 approval** is required before merging
- The PR author is responsible for addressing review comments
- If a review requests changes, push new commits — do not force-push over existing ones
- Approvals are dismissed automatically if new commits are pushed after approval

### Merging

- Only the maintainer merges approved PRs
- We use **squash merge** to keep a linear history on `main`
- Delete your branch after merge

---

## CI checks

Every pull request targeting `main` must pass three checks:

| Check | Command | What it validates |
|-------|---------|-------------------|
| `test` | `go test ./...` | All unit and integration tests pass |
| `lint` | `golangci-lint run` | Code style, common errors, best practices |
| `build` | `go build ./...` | The project compiles without errors |

**A PR cannot be merged if any check fails.** Fix the issue and push — checks re-run automatically.

If you believe a check is failing due to a flaky test or infrastructure issue (not your code), leave a comment on the PR explaining why.

---

## Commit conventions

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <short description>

[optional body]

[optional footer: Closes #123]
```

### Types

| Type | When to use | Affects end user |
|------|------------|-----------------|
| `feat` | New feature or capability | ✅ Yes |
| `fix` | Bug fix — corrects existing behavior | ✅ Yes |
| `perf` | Performance improvement | ✅ Yes |
| `refactor` | Code change with no feature or fix | ❌ No |
| `test` | Adding or fixing tests | ❌ No |
| `docs` | Documentation only | ❌ No |
| `chore` | Maintenance tasks that don't change functionality: updating dependencies, modifying the Makefile, bumping Go version in CI, cleaning up build config. If it doesn't ship a feature and doesn't fix a bug, it's a chore. | ❌ No |

### Examples

```
feat(plugin): add support for WASM plugin runtime
fix(opa): handle timeout when OPA sidecar is unreachable
docs(readme): add Kubernetes deployment example
chore(deps): update mark3labs/mcp-go to v0.28.0
```

---

## Local development

### Requirements

- Go 1.23+
- Docker (for OPA sidecar and OTel collector)
- make

### Setup

```bash
git clone https://github.com/alejandrohrdzmtz/gama
cd gama
go mod download
```

### Common commands

```bash
make dev      # Start gateway + OPA + OTel collector via docker compose
make test     # Run test suite
make lint     # Run golangci-lint
make build    # Build binary
make clean    # Remove build artifacts
```

### Running a single test

```bash
go test ./internal/opa/... -run TestAllow -v
```

### Writing a plugin locally

```bash
# Build your plugin
go build -buildmode=plugin -o plugins/myplugin.so ./examples/myplugin

# Run gateway with it
gama serve --plugin ./plugins/myplugin.so --opa http://localhost:8181
```

---

## Project structure

```
gama/
├── cmd/
│   └── gama/           # CLI entrypoint
├── internal/
│   ├── gateway/        # Core request handling
│   ├── opa/            # OPA authorization middleware
│   ├── telemetry/      # OpenTelemetry setup
│   └── server/         # HTTP and stdio server setup
├── plugin/
│   └── interface.go    # ToolPlugin interface — start here
├── examples/
│   └── db-plugin/      # Example plugin implementation
├── policies/
│   └── mcp_tools.rego  # Example OPA policy
├── .github/
│   ├── workflows/
│   │   └── ci.yml      # CI pipeline
│   └── CODEOWNERS      # Auto-review assignment
├── CONTRIBUTING.md     # This file
├── README.md
├── gama.yaml           # Example configuration
├── Makefile
├── go.mod
└── go.sum
```

---

## Questions

Open a [GitHub Discussion](https://github.com/alejandrohrdzmtz/gama/discussions) for anything that is not a bug or feature request.

We appreciate every contribution, large or small.