# Gemmology Project

Crystal visualization and gemmological reference tools aligned with the FGA (Gem-A) curriculum.

## Packages

| Package | Description | Docs | PyPI |
|---------|-------------|------|------|
| [cdl-parser](https://github.com/gemmology-dev/cdl-parser) | Crystal Description Language parser | [![Docs](https://img.shields.io/badge/docs-live-green)](https://cdl-parser.gemmology.dev) | [![PyPI](https://img.shields.io/pypi/v/gemmology-cdl-parser)](https://pypi.org/project/gemmology-cdl-parser/) |
| [crystal-geometry](https://github.com/gemmology-dev/crystal-geometry) | 3D geometry engine for crystals & twins | [![Docs](https://img.shields.io/badge/docs-live-green)](https://crystal-geometry.gemmology.dev) | [![PyPI](https://img.shields.io/pypi/v/gemmology-crystal-geometry)](https://pypi.org/project/gemmology-crystal-geometry/) |
| [mineral-database](https://github.com/gemmology-dev/mineral-database) | SQLite database with 94+ mineral presets | [![Docs](https://img.shields.io/badge/docs-live-green)](https://mineral-database.gemmology.dev) | [![PyPI](https://img.shields.io/pypi/v/gemmology-mineral-database)](https://pypi.org/project/gemmology-mineral-database/) |
| [crystal-renderer](https://github.com/gemmology-dev/crystal-renderer) | SVG/STL/glTF visualization export | [![Docs](https://img.shields.io/badge/docs-live-green)](https://crystal-renderer.gemmology.dev) | [![PyPI](https://img.shields.io/pypi/v/gemmology-crystal-renderer)](https://pypi.org/project/gemmology-crystal-renderer/) |
| [cdl-lsp](https://github.com/gemmology-dev/cdl-lsp) | Language Server for IDE support | [![Docs](https://img.shields.io/badge/docs-live-green)](https://cdl-lsp.gemmology.dev) | [![PyPI](https://img.shields.io/pypi/v/gemmology-cdl-lsp)](https://pypi.org/project/gemmology-cdl-lsp/) |
| [gemmology-knowledge](https://github.com/gemmology-dev/gemmology-knowledge) | FGA curriculum documentation | [![Docs](https://img.shields.io/badge/docs-live-green)](https://knowledge.gemmology.dev) | - |

## Quick Links

- [Main Website](https://gemmology.dev) - Documentation hub
- [CDL Playground](https://gemmology.dev/playground) - Interactive crystal editor
- [Crystal Gallery](https://gemmology.dev/gallery) - Browse mineral presets
- [Learn](https://gemmology.dev/learn) - FGA curriculum-aligned content
- [Quiz & Exams](https://gemmology.dev/quiz) - Test your gemmology knowledge
- [Calculator Tools](https://gemmology.dev/tools) - SG, RI, birefringence calculators

## What is CDL?

Crystal Description Language (CDL) is a notation for describing crystal morphology:

```
cubic[m3m]:{111}@1.0 + {100}@1.3    # Truncated octahedron
trigonal[-3m]:{10-10}@1.0 + {10-11}@0.8  # Quartz prism with pyramids
```

## Installation

```bash
# Full suite
pip install gemmology-plugin

# Individual packages
pip install gemmology-cdl-parser gemmology-crystal-geometry gemmology-mineral-database gemmology-crystal-renderer
```

## Contributing

We welcome contributions! See [CONTRIBUTING.md](https://github.com/gemmology-dev/.github/blob/main/CONTRIBUTING.md) for guidelines.

## License

All packages are released under the MIT License.
