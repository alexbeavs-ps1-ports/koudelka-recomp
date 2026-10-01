# Koudelka knowledge report

- Date: 2026-08-31
- Project: Koudelka setup-host release pilot
- Repository/branch: local pilot, `master`
- Retail identity: Europe `SLES-02897`, `SLES-12897`, `SLES-22897`, and `SLES-32897`
- Architecture lane: source-only owned-input setup host
- License boundary: portfolio files use PolyForm Noncommercial 1.0.0; PSXRecomp keeps the license in `psxrecomp/LICENSE`; recomp-ui keeps MIT

## Executive state

The exact `0.3.0` Windows ZIP passes the payload audit and a clean extracted
build. The build used a path with spaces, an invalid inherited
`SSL_CERT_FILE`, and the frozen RetComM toolchain. All four owned discs contain
the same boot program, so one setup host is structurally valid. The operator
confirmed Disc 1 gameplay in the earlier private package. Mid-session disc
change is still untested in this pilot.

## Product graduation state

- Current state: `bootstrap_verified`
- Evidence: zero required Studio audit failures; exact ZIP clean build; four-disc static identity
- Next release gate: exact-package R2 canaries
- Missing hard gates: exact installed startup, disc-change canary, and remote redownload audit
- Human-completion coverage: Disc 1 gameplay was confirmed only on the earlier private package

## Verified milestones

| Boundary | Evidence | Repeated? | Human confirmed? |
|---|---|:---:|:---:|
| Disc-set identity | Four serials; one byte-identical boot program SHA-256 `661BD13917E21032DCFE98CA012011D1021CB79F45688AA4C50A75E6D7D95390` | Yes | No |
| Setup archive payload | 2,003 files; zero forbidden files, generated retail source, private paths, or CRLF shell scripts | Yes | No |
| Exact extracted build | 166 of 166 actions; executable SHA-256 `815259BD76F3ED35A664380BDC30642FF585FCE807CF1FCCE31247675BEE2394` | Yes | No |
| Studio source audit | Zero required failures; two accepted box-art warnings | Yes | No |

## Shared findings consumed

| Finding ID | Status | Evidence |
|---|---|---|
| `PSX-DISC-001` | independently verified | One program is byte-identical across four owned disc images. |
| `PSX-BUILD-013` | independently verified | Runtime `d60f5947e` requires recomp-ui `4eda65430a`. |
| `PSX-PUB-004` | independently verified | Rejected draft contained copied memory-card files; sealed ZIP contains none. |
| `PSX-PUB-006` | independently verified | Packaging passes with no tracked optional launcher assets. |
| `PSX-WIN-005` | independently verified | C, C++, and RC resolve to RetComM toolchain `1.0.14`; the spaced-path build passes. |
| `PSX-WIN-006` | independently verified | Ambient WSL Bash lacked `zip`; explicit Git-for-Windows Bash produced the final archive. |

## Corpus consulted for the current blocker

- Symptom: prove whether four disc images can use one setup host.
- Consulted: `PSX-DISC-001`, `PSX-BUILD-013`, `PSX-PUB-004`,
  `PSX-PUB-006`, `PSX-WIN-005`, and the release regression ledger.
- Disposition: static program identity is confirmed. Connected mid-session
  disc change remains open and is not inferred from identical boot code.

## Reusable artifacts

- four-disc identity receipt under the campaign `_evidence` directory
- `test_setup_package_payload_filter.py`
- `test_windows_rc_compiler_arg.py`
- exact-ZIP independent payload audit

## Performance evidence

Not measured. This pilot does not make a performance claim.

## Quality debt

| Debt | Owner | User impact | Evidence/containment | Removal or acceptance gate |
|---|---|---|---|---|
| Mid-session disc change untested | Title pilot | Later discs may not be usable | Four-disc static identity only | Complete a natural connected disc-change route. |
| No exact installed startup | Title pilot | Setup build does not prove boot | `headless_boot_frames = 0` | Run the exact installed product past the bootstrap gate. |

## Current blockers

1. Complete the exact installed startup and risk canaries.
2. Complete a connected disc-change route.
3. Redownload and audit the private draft before any public release.

## Next decisive experiment

Build the exact private CI artifact, install all four owned images, run the
installed executable past the bootstrap gate, then complete the first natural
disc-change route without changing executable or memory-card identity.

## Knowledge-base actions

- Updated `PSX-PUB-004`, `PSX-PUB-006`, and `PSX-WIN-005` evidence.
- Added the Windows packaging-shell identity candidate as `PSX-WIN-006`.
- Retained `PSX-DISC-001` as a static result with an open connected route.
- Next independent consumer: another data-only multi-disc title.
- Upstream candidate: setup packager payload and Windows RC guards.

## v0.3.3 setup correction

The source now uses `Koudelka_Recompiled` as the only setup executable name. The batch source
gate passes. The exact-ZIP automatic-relaunch canary and remote release audit
remain open. Public `v0.3.0` remains unchanged.

## v0.3.5 three-platform refresh

The source now binds the package-only privacy correction and targets Windows
x64, Linux x64, macOS ARM64, and macOS x64. The replacement build-only CI,
complete archive audit, and native package gates remain required. This source
change does not publish a release or claim platform support.

## 2026-09-04 v0.3.6 POSIX setup-copy candidate

This candidate pins PSXRecomp 08ec704a974b1f3a16335b4afeb340b9eff19926 and recomp-ui be8ac1d03ee19d55394b5a5f2d9d1506edd56659.
Linux and macOS packages use native CMake, Ninja, Python, C, and C++ tools.
Windows keeps the portable toolchain route. This change does not change game
code or the graduation state. Build-only CI and every exact-package release
gate must pass before publication.
