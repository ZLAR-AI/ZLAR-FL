# Contributing to ZLAR-FL

ZLAR-FL is a multi-agent fleet governance tool. It is maintained by [ZLAR Inc.](https://zlar.ai) and welcomes contributions.

---

## How to Contribute

### Reporting Issues

If you find a bug, a gap in documentation, or have a suggestion, open an issue. Be specific — include what you expected, what happened, and steps to reproduce.

### Security Disclosures

If you discover a security vulnerability, **do not open a public issue.** Use [GitHub's private vulnerability reporting](https://github.com/ZLAR-AI/ZLAR-FL/security/advisories) instead. See [SECURITY.md](SECURITY.md) for details.

### Pull Requests

1. Fork the repository
2. Create a branch from `main` for your change
3. Make focused changes — one concern per PR
4. Run `bash -n bin/zlar-fl` to syntax-check
5. Write a clear description of what the change does and why
6. Submit the PR

### Code Standards

- Follow existing patterns and naming conventions
- Fleet operations must be idempotent where possible
- Registry format changes must be backward-compatible

---

## What We're Looking For

- **Fleet policies** — templates for common multi-agent governance scenarios
- **Drift detection** — more sophisticated policy drift analysis across fleet members
- **Topology visualization** — rendering fleet relationships and dependency graphs
- **Integration testing** — testing FL with real multi-agent setups across Gate, LT, and OC

---

## Code of Conduct

Be respectful, be specific, be constructive.

---

## License

By contributing to ZLAR-FL, you agree that your contributions will be licensed under the [Apache License 2.0](LICENSE).
