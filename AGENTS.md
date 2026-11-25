# lib-mri

`mri` — a minimal argv parser implementing the
[npm `mri`](https://github.com/lukeed/mri) semantics in pure handl.

## Layout

| path | role |
| --- | --- |
| `src/index.hnl` | the parser (pure handl; deps: `str`, `obj`, `typeof` — builtin) |
| `src/index.test.hnl` | 121 tests covering the documented mri surface (canonical) |
| `src/index.unknown.test.hnl` | 8 strict-mode tests (arrow dialect — `unknown` callbacks) |
| `pkg.hnl`, `ci.sh`, `scripts/hnl-gate.sh` | manifest / bupha entry / gate |

## Contract

- `parse(args, opts)` returns `{ "_": [positionals...], <flag>: <value>, ... }`
  with mri's exact coercion, alias, default, string/boolean,
  negation, strict (`unknown`) and `--` end-of-flags behavior —
  including the quirks; `src/index.hnl`'s header lists them plus the
  documented divergences.
- Pure `.hnl`; no Rust crate.

## Verification

```bash
./ci.sh   # bupha entry → scripts/hnl-gate.sh (typecheck/lint/test)
```

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
