# Contributing to the Gemmology Project

Thank you for your interest in contributing! This document provides guidelines for contributing to any repository in the Gemmology Project organization.

## Code of Conduct

Please read and follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## How to Contribute

### Reporting Bugs

1. Check existing issues to avoid duplicates
2. Use the bug report template
3. Include:
   - Clear description
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (Python version, OS, package version)

### Suggesting Features

1. Check existing issues and discussions
2. Use the feature request template
3. Explain the motivation and use case

### Submitting Code

1. **Fork the repository**

2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Follow the coding style
   - Add tests for new functionality
   - Update documentation as needed

4. **Run tests locally**
   ```bash
   pip install -e ".[dev]"
   ruff check .
   mypy src/
   pytest
   ```

5. **Commit with clear messages**
   ```bash
   git commit -m "Add feature: brief description"
   ```

6. **Push and create a Pull Request**
   ```bash
   git push origin feature/your-feature-name
   ```

## Development Setup

### Prerequisites

- Python 3.10+
- Git
- GitHub CLI (`gh`) recommended

### Local Development

```bash
# Clone the repository
git clone https://github.com/gemmology-dev/<repo>.git
cd <repo>

# Create virtual environment
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows

# Install in development mode
pip install -e ".[dev]"

# Run tests
pytest

# Run linting
ruff check .

# Run type checking
mypy src/
```

## Coding Standards

### Python Style

- Follow PEP 8
- Use type hints
- Maximum line length: 100 characters
- Use `ruff` for formatting and linting

### Commit Messages

- Use present tense ("Add feature" not "Added feature")
- Use imperative mood ("Move cursor" not "Moves cursor")
- Keep the first line under 72 characters
- Reference issues when applicable

### Documentation

- Use docstrings for public functions/classes
- Update README if adding new features
- Update CHANGELOG.md

## Project-Specific Guidelines

### CDL Parser (`cdl-parser`)

- Maintain backwards compatibility with CDL syntax
- Add tests for new syntax features
- Update the specification document

### Crystal Geometry (`crystal-geometry`)

- Validate geometry with Euler's formula (V - E + F = 2)
- Test at multiple viewing angles
- Include 3D model validation (STL export)

### Mineral Database (`mineral-database`)

- YAML files are the source of truth
- Run `scripts/build_db.py` after modifying YAML
- Verify FGA accuracy for gemstone properties

### Crystal Renderer (`crystal-renderer`)

- Test SVG output for visual correctness
- Verify 3D exports (STL, glTF) in external viewers
- Maintain support for all output formats

## Release Process

Releases are handled by maintainers:

1. Update version in `pyproject.toml`
2. Update `CHANGELOG.md`
3. Create a git tag: `git tag v1.2.3`
4. Push tag: `git push origin v1.2.3`
5. GitHub Actions will publish to PyPI

## Getting Help

- [Documentation](https://gemmology.dev/docs)
- [Discussions](https://github.com/orgs/gemmology-dev/discussions)
- [Issue Tracker](https://github.com/gemmology-dev/<repo>/issues)

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
