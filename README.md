# lib-mri

`mri` — a minimal argv parser implementing the [npm `mri`](https://github.com/lukeed/mri)
semantics in pure handl.

`parse(args, opts)` returns `{ "_": [positionals...], <flag>: <value>, ... }`
with mri's exact coercion, alias, default, string/boolean, negation, strict
(`unknown`) and `--` end-of-flags behavior — including the quirks (see
`src/index.hnl` header for the full list and the documented divergences).

## Usage

```handl
from "mri" import * as mri

mri.parse(["run", "--port=8080", "-v", "--", "--tail"], { alias: { v: "verbose" } })
-- { "_": ["run", "--tail"], port: 8080, v: true, verbose: true }
```

## Layout

```text
src/index.hnl              the parser (pure handl; deps: str, obj, typeof — builtin)
src/index.test.hnl         121 tests covering the documented mri surface (canonical)
src/index.unknown.test.hnl   8 strict-mode tests (arrow dialect — `unknown` callbacks)
pkg.hnl              package manifest — modules, description, license
ci.sh                bupha entry → scripts/hnl-gate.sh
```

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

The parser (`src/index.hnl`) uses the arrow dialect (`~>`), which the
canonical typecheck/lint tools skip — the gate reports it as
dialect-skipped. `src/index.test.hnl` is canonical and stays inside the
gate; `src/index.unknown.test.hnl` needs `~>` lambdas for the `unknown`
callback and is dialect-skipped like the implementation. The real check
is `hnl test` over both test files (needs `HANDL_EXT_DIR` pointing at the
extension `.so`s, as CI sets it).

## Verification

Beyond the in-repo suite, the implementation was differentially fuzzed
against `mri@1.2.0` on Node: 600 randomized argv/opts cases (aliases,
defaults, string/boolean promotion, strict mode, grouped shorts, `=`
forms, unicode) — all identical.

## License

MIT.
