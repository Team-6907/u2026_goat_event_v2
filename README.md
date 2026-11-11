# u2026_goat_event_v2

> FRC Team 6907 The G.O.A.T. · GOATEvent 第二代：FRC 赛事数据领域模型与晋级积分规则的 Python 库，已冻结归档，作为 GOATLab GOATEvent 的迁移基准与测试数据来源。

`goat-event-v2` is a library-only Python rewrite of the team's FRC event
calculator. It models seasons, events, teams, alliances and matches on top of
the official FRC Events API, and implements tournament and Championship
qualification point rules against that model. There is no CLI or UI; the
package is exercised through its test suite.

## Status

Frozen; kept as the migration oracle and fixture source for GOATEvent; archived.
No new features land here.

## Lineage

1. [`u2025_frc_event_calculator`](https://github.com/Team-6907/u2025_frc_event_calculator)
   - v1, a Streamlit regional-points calculator.
2. `u2026_goat_event_v2` (this repository) - typed Python domain model
   refactored from the v1 data structures.
3. GOATEvent inside the team's GOATLab platform - the TypeScript port, which
   uses this repository's behaviour and cached fixtures as its reference.

## Package layout

| Path | Contents |
| --- | --- |
| `real/` | Domain models: `Season`, `Event`, `Team` / `SeasonTeam`, `Alliance`, `Match`, `Award` |
| `ruleset/tournament/` | `TournamentRule` protocol with `SimpleTournament` and `DoubleElimTournament` |
| `ruleset/cmpqual/` | `CMPQualRule` protocol and the `SimpleEventPoints` / `SimpleCMPQualification` point rules |
| `data/` | FRC Events API client (`frc-api.firstinspires.org/v3.0`), JSON cache, cache refresh and event-bypass handling |
| `utils/` | Helpers: rule selection and award-name mapping, event time-zone normalisation to UTC, math, JSON type guards |
| `test/` | `unittest` suite plus `test/test_cache/` - cached API payloads (2019, 2022, 2024, 2026) used as fixtures |

Modules are imported from the repository root (`from real.event import Event`);
`[tool.pdm] distribution = false`, so nothing is built or published.

## Setup

Requires Python 3.13 and [PDM](https://pdm-project.org/).

```bash
pdm install -G typecheck   # runtime + black + basedpyright
pdm run test               # python -m unittest discover -s test -p "*.py" -v
pdm run typecheck          # basedpyright, strict mode
pdm run format-check       # black --check .
pdm run verify             # typecheck + test
```

CI (`.github/workflows/ci.yml`) runs `format-check` and `verify` on every push
and pull request. Pre-commit hooks (`.pre-commit-config.yaml`) insert the
license header from `LICENSE_HEADER.txt`, run black and run `pdm run verify`.

## Configuration and cache

- Live requests need FRC Events API credentials in a `.env` file at the
  repository root: `AUTH_USERNAME` and `AUTH_TOKEN`. Never commit `.env`.
- Responses are cached under `cache/<season>/` (git-ignored). Set
  `GOAT_EVENT_CACHE_ROOT` to point the cache somewhere else; the tests use this
  to read from `test/test_cache/`.
- `AGENTS.md` notes that some tests can exercise live API requests when a
  payload is not already cached, so a full run may need credentials and
  network access.

See `AGENTS.md` for coding conventions.

## License

MIT. See [LICENSE](LICENSE).
