---
slug: unlisted-agents
name: unlisted agent guide
repo: .
status: active
last_verified: 2026-11-22
---

# unlisted: guide for coding agents

The doc index is docs/README.md. Read docs/ARCHITECTURE.md before changing structure.

## Commands

- `make test` runs `go vet ./...`, then `go test -race ./...` with cgo switched back on for the race detector. <!-- src: Makefile:11-16 -->
- `make build` writes a static `./unlisted` (`CGO_ENABLED=0`, version from `git describe`). <!-- src: Makefile:1-9 -->
- `make demo` builds, then runs `unlisted demo`. <!-- src: Makefile:25-26 -->
- `make dist` cross-compiles linux/amd64, linux/arm64 and darwin/arm64 into `dist/` with a `checksums.txt`. <!-- src: Makefile:18-23 -->
- `make screenshot` regenerates `docs/demo.png` and `docs/demo-hero.png` from `unlisted demo`; it needs `python3` with Pillow. <!-- src: Makefile:28-33, scripts/render-demo.py:1-10 -->
- After a deliberate change to the demo report, run `UPDATE_GOLDEN=1 go test ./internal/cli -run Demo` to rewrite `docs/demo.txt`. <!-- src: CONTRIBUTING.md:34-40, internal/cli/cli_test.go:89-107 -->
- To change the fake `/proc` tree or Docker listing under `demo/`, edit `scripts/gen-demo-fixture.py` and run `python3 scripts/gen-demo-fixture.py demo`. <!-- src: scripts/gen-demo-fixture.py:1-10 -->
- Go 1.22 is the floor; there is no lint or format step beyond `go vet`. <!-- src: go.mod, Makefile:11-12 -->
- CI runs vet, race tests, a static build, the demo, `shellcheck install.sh` and `make dist`. <!-- src: .github/workflows/ci.yml:25-43 -->

## Conventions

- Layout: `main.go` calls `internal/cli`; the other packages are `registry`, `discover`, `check`, `probe`, `report`, `adopt`, plus `internal/kit` for maintenance-file tests. <!-- src: main.go, internal/kit/kit_test.go:1-4 -->
- Standard library first. The only dependency is `gopkg.in/yaml.v3`, and a new one needs a reason. <!-- src: CONTRIBUTING.md:17, go.mod -->
- Tests never read the real machine: they use fixtures under `testdata/` and `demo/` through `--config-root`. Do not add a test that reads `/etc` or `/proc`. <!-- src: CONTRIBUTING.md:19 -->
- A new discovery source is one file in `internal/discover` implementing `Source`, a fixture under `testdata/`, a parser test, and an entry in `AllSources`. <!-- src: CONTRIBUTING.md:21-33, internal/discover/types.go:64-67, internal/discover/types.go:97-99 -->
- Source paths go through `env.resolve` so `--config-root` works; explicit path flags are literal and are not prefixed with the root. <!-- src: internal/discover/env.go:15, internal/discover/env.go:44-51 -->
- `internal/check` does no I/O: it takes a registry and a snapshot and returns findings. <!-- src: internal/check/check.go:1-2, internal/check/check.go:14-19 -->
- Exit codes are part of the interface: 0 clean, 1 findings matching `--fail-on`, 2 error. <!-- src: internal/cli/cli.go:28-33 -->
- The JSON schema is `unlisted/v1`: it changes only when a field is removed or changes meaning, and new optional fields are allowed. <!-- src: internal/report/json.go:12-14, docs/json.md:3 -->
- Comments say why the code does something. No emoji and no em dashes in code, comments or docs. Keep commits small. <!-- src: CONTRIBUTING.md:51 -->
- Dependabot patch and minor Go updates, and any `actions/` update, merge on their own after CI; other majors wait for a person. <!-- src: CONTRIBUTING.md:45, .github/scripts/automerge-eligible.mjs -->

## Traps

- `internal/kit/kit_test.go` checks the maintenance files: workflows must pin third-party actions by SHA, declare `permissions`, and never expand untrusted text. <!-- src: internal/kit/kit_test.go:93-125 -->
- That test also scans a fixed list of public files, including `dossiers/.dossier-lock.json`, for em dashes, absolute home-directory paths, a personal mail domain and co-author trailers. The lock stores the text of every pinned claim, so the same words in a pinned doc fail it, and a new public doc is not covered unless added to the list. <!-- src: internal/kit/kit_test.go:371-398 -->
- The ruleset on `main` requires the CI job name `go test (ubuntu-latest, go 1.22)`; renaming the job without editing `.github/rulesets/main.json` fails a test. <!-- src: .github/workflows/ci.yml:18, .github/rulesets/main.json, internal/kit/kit_test.go:213-280 -->
- `dossiers/unlisted.md` must keep between 5 and 15 cited claims and its lock must pin exactly that many. <!-- src: internal/kit/kit_test.go:410-428 -->
- `docs/demo.txt` is compared byte for byte with the demo report, so any change to report wording, order or counts fails `TestDemoReportMatchesDocs` until the golden file is regenerated. <!-- src: internal/cli/cli_test.go:89-107 -->
- `demo/` is embedded into the binary with `all:` patterns, so a new top-level folder under `demo/` needs the embed line extended or `unlisted demo` will not see it. <!-- src: demo/demo.go:9-10 -->
- `unlisted demo` exits 0 on purpose, while `unlisted check -f demo/registry.yml --config-root demo` exits 1; CI asserts the second. <!-- src: internal/cli/cli.go:132-134, .github/workflows/ci.yml:31-35 -->
- Under `--config-root` the systemd source reads `systemd/units.txt` instead of running `systemctl`, and the home directory is `/root`. <!-- src: internal/discover/systemd.go:20-28, internal/discover/env.go:32-42 -->
- Only `--probe` touches the network, and it contacts registry hostnames only; keep it that way, since the README and SECURITY.md promise it. <!-- src: internal/probe/probe.go:1-3, internal/cli/cli.go:236-248, SECURITY.md:11 -->
- Only `unlisted init` writes a file, and it refuses to overwrite one without `--force`. <!-- src: internal/cli/cli.go:267-269, internal/cli/cli_test.go:269-290 -->
- The Go module path `github.com/kd-011/unlisted` appears in `go.mod`, imports, `Makefile`, `action.yml`, `.goreleaser.yml` and `install.sh`; a rename touches all of them. <!-- src: go.mod, Makefile:2, .goreleaser.yml:22, action.yml:44, install.sh:7 -->
- Releases carry no version file: goreleaser stamps `internal/cli.Version` from the tag at build time, and `go install` builds read it from build info. <!-- src: .goreleaser.yml:21-22, internal/cli/cli.go:25-26, internal/cli/cli.go:91-100 -->

## Unverified

- Whether the composite Action works end to end on a real runner, and whether the floating `v0` tag that `docs/action.md` tells users to pin exists and is current. Neither can be seen from the repo.
- What tags and publishes releases: the repo shows only the tag-triggered `release` workflow, not who pushes the tag.
