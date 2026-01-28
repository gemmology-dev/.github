# Gemmology Project Branching Standards

This document defines the Git branching strategy for all repositories in the `gemmology-dev` organization.

## Branch Model

We use **GitHub Flow** - a simple, trunk-based development model:

- `main` is always deployable
- All work happens in short-lived feature branches
- Changes reach `main` through pull requests with passing CI

## Git Worktrees (Required)

**Every new feature or fix MUST be developed in its own worktree.**

Git worktrees allow multiple branches to be checked out simultaneously in separate directories, all sharing the same `.git` database. This enables parallel development without stashing or context switching.

### Why Worktrees?

- **Parallel work**: Multiple features/fixes can progress simultaneously
- **AI agent parallelism**: Run multiple Claude Code instances on different tasks
- **Clean isolation**: Each task has its own working directory
- **No stashing**: Switch between tasks by changing directories

### Directory Structure

```
repository/
├── .trees/                    # All worktrees (add to .gitignore)
│   ├── feature-new-api/       # Worktree for feature/new-api
│   ├── fix-edge-case/         # Worktree for fix/edge-case
│   └── docs-examples/         # Worktree for docs/examples
├── .gitignore                 # Contains: .trees/
├── src/
└── ...
```

### Worktree Workflow

```bash
# 1. STARTING NEW WORK - Always create branch + worktree
git worktree add -b feature/my-feature .trees/my-feature
cd .trees/my-feature

# 2. DEVELOP in the worktree
# ... make changes, commit, push ...
git push -u origin feature/my-feature

# 3. CREATE PR and wait for CI

# 4. AFTER MERGE - Clean up
cd /path/to/main/repo
git worktree remove .trees/my-feature
git worktree prune
```

### Essential Commands

```bash
# Create worktree with new branch
git worktree add -b <branch> .trees/<name>

# Create worktree from existing branch
git worktree add .trees/<name> <existing-branch>

# List all worktrees
git worktree list

# Remove worktree (clean)
git worktree remove .trees/<name>

# Force remove (uncommitted changes)
git worktree remove -f .trees/<name>

# Clean stale references
git worktree prune

# Lock worktree (prevent accidental removal)
git worktree lock .trees/<name> --reason "work in progress"

# Unlock
git worktree unlock .trees/<name>
```

### Parallel AI Development

Run multiple Claude Code (or other AI) instances simultaneously:

```bash
# Terminal 1: Main worktree (code review, coordination)
cd ~/project
claude

# Terminal 2: Feature development
cd ~/project/.trees/feature-x
claude

# Terminal 3: Bug fix
cd ~/project/.trees/fix-y
claude

# Terminal 4: Documentation
cd ~/project/.trees/docs-update
claude
```

Each instance works in complete isolation. Commits made in any worktree are immediately available to all others.

### Rules for AI Agents

1. **Before starting any feature/fix**: Create a new worktree
2. **Check your location**: Run `git worktree list` to see where you are
3. **Never develop features in the main worktree**: Always use `.trees/`
4. **One task per worktree**: Don't mix unrelated changes
5. **Clean up after merge**: Remove worktree and run `git worktree prune`

### Environment Files

Copy necessary config to new worktrees:

```bash
git worktree add -b feature/thing .trees/thing
cp .env .trees/thing/.env 2>/dev/null || true
cp .env.local .trees/thing/.env.local 2>/dev/null || true
```

### Limitations

- Cannot check out the same branch in multiple worktrees
- Submodule support is experimental
- Each worktree needs its own `node_modules` / `.venv` if applicable

## Default Branch

All repositories use `main` as the default and protected branch.

## Branch Naming

```
<type>/<short-description>
```

| Type | Purpose |
|------|---------|
| `feature/` | New functionality |
| `fix/` | Bug fixes |
| `docs/` | Documentation |
| `refactor/` | Code restructuring |
| `test/` | Test changes |
| `chore/` | Maintenance |

Examples:
- `feature/add-cubic-presets`
- `fix/svg-viewbox-calculation`
- `docs/improve-api-reference`

## Commit Convention

All repositories use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`, `perf`

Breaking changes: Add `!` after type or include `BREAKING CHANGE:` in footer.

## Protected Branch Rules

The `main` branch on all repositories has these protections:

| Rule | Setting | Rationale |
|------|---------|-----------|
| Require pull request | Recommended | Ensures CI runs before merge |
| Require status checks | **Required** | Tests, lint, type check must pass |
| Require up-to-date | **Required** | Prevents broken merges |
| Require review | No | Solo project |
| Require signed commits | Optional | Extra security layer |
| Block force pushes | **Required** | Protect history integrity |
| Block deletions | **Required** | Prevent accidental loss |

## Security Enforcement

### Dependency Review (All PRs)

```yaml
- uses: actions/dependency-review-action@v4
  with:
    fail-on-severity: high
    deny-licenses: GPL-3.0, AGPL-3.0
```

Blocks PRs that:
- Introduce dependencies with high/critical CVEs
- Add GPL/AGPL licensed dependencies (incompatible with MIT)

### Additional Security

- **Secret scanning**: Enabled org-wide, blocks commits with detected secrets
- **Dependabot**: Automated security updates for dependencies
- **CodeQL**: Static analysis for Python security issues (optional per-repo)

## Merge Strategy

**Squash and merge** is the default for all PRs:
- Keeps `main` history clean
- Each PR = one commit on main
- Use conventional commit message for the squashed commit

## Release Process

Releases are triggered by Git tags:

```bash
git tag v1.2.3
git push origin v1.2.3
```

GitHub Actions will:
1. Run full test suite
2. Build distribution packages
3. Publish to PyPI (Python packages)
4. Create GitHub Release with auto-generated notes

### Version Bumping

Semantic versioning based on conventional commits:

| Commit Type | Version Bump | Example |
|-------------|--------------|---------|
| `fix:` | Patch (1.0.0 → 1.0.1) | Bug fixes |
| `feat:` | Minor (1.0.0 → 1.1.0) | New features |
| `feat!:` or `BREAKING CHANGE:` | Major (1.0.0 → 2.0.0) | Breaking changes |

## Cross-Repository Changes

When a change spans multiple repositories, coordinate in dependency order:

```
cdl-parser (1st - no dependencies)
    ↓
crystal-geometry + mineral-database (2nd - parallel)
    ↓
crystal-renderer (3rd)
    ↓
cdl-lsp + gemmology-plugin (4th - parallel)
```

**Workflow for cross-repo changes:**

1. Use matching branch names across repositories (e.g., `feature/new-miller-notation`)
2. Merge and release lowest-level package first
3. Update `pyproject.toml` in dependent packages to require new version
4. Merge dependent packages in order

## Repository-Specific Notes

### Python Packages (cdl-parser, crystal-geometry, etc.)

- PyPI publish on tag push
- Requires passing tests on Python 3.10, 3.11, 3.12
- Coverage gate enforced (minimum 60%, target 80%)

### gemmology.dev (Website)

- Cloudflare Pages auto-deploys on push to `main`
- Preview deployments generated for all PRs
- No version tags (continuously deployed)

### gemmology-knowledge (Documentation)

- GitHub Pages deployment on push to `main`
- CC BY-SA 4.0 license (not MIT)
- No version tags (content continuously updated)

### .github (This Repository)

- Organization-wide templates and workflows
- Changes here affect all repositories
- Extra caution required for workflow modifications

## Emergency Procedures

### Hotfix Process

1. Create `fix/` branch from `main`
2. Apply minimal fix with test
3. Push and verify CI passes
4. Merge immediately (no waiting)
5. Tag patch release

### Reverting a Bad Release

```bash
# Revert the commit on main
git revert <commit-sha>
git push origin main

# Tag a new patch version
git tag v1.2.4
git push origin v1.2.4
```

Never delete or move existing tags - always move forward.
