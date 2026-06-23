# Contributing

Thanks for thinking about contributing — this skill is better because of it.

## Quick links

- [Code of Conduct](CODE_OF_CONDUCT.md)
- [Security policy](SECURITY.md)
- [Issue tracker](../../issues)

## Ways to contribute

- **Try the skill and report what breaks.** The most useful contribution is a bug report
  with the exact prompt you used, the skill's output, and what you expected instead. File
  it as a bug-report issue.
- **Improve the SKILL.md instructions.** Small clarifications, fixed typos, better examples
  — open a PR.
- **Add or fix evaluations.** Realistic test prompts in `evals/` help catch regressions.
- **Suggest new behaviour.** Open a feature-request issue and propose the change before
  writing a PR — for behavioural changes I prefer to align on the approach first.

## Development setup

This is a Claude Code skill — `SKILL.md` plus bundled resources. To work on it locally:

```bash
git clone <repo-url>
cd distil-research
ln -s "$(pwd)" ~/.claude/skills/distil-research   # symlink into your skills dir
```

Then in Claude Code, invoke the skill and iterate on `SKILL.md`.

## Pull request process

1. Fork the repo and create a branch from `main`.
2. Make your change. Keep PRs focused — one logical change per PR.
3. If you changed skill behaviour, add or update an eval under `evals/`.
4. Update `CHANGELOG.md` under `[Unreleased]`.
5. Open the PR using the template. Link the issue it closes.

## Style

- `SKILL.md`: clear imperative voice, short paragraphs, explain *why* alongside *what*.
  Avoid heavy MUSTs unless the behaviour is genuinely non-negotiable.
- Commit messages: conventional commits (`feat:`, `fix:`, `docs:`, `chore:`) preferred but
  not required.

## Code of Conduct

By contributing, you agree to abide by the [Code of Conduct](CODE_OF_CONDUCT.md).
