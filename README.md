# PG Atlas SBOM demo: cargo

Phase 1 baseline for [D1](https://github.com/SCF-Public-Goods-Maintenance/pg-atlas-sbom-action/issues/10).
This is an unpublished test repository and must stay unattached to an Atlas project.

## Expected dependencies

- obsrvr_radar: `pkg:cargo/stellar-history-archive-hasher@0.11.0` → `pkg:github/withObsrvr/rs-stellar-history-archive-hasher`
- rust_soroban_client_library: `pkg:cargo/soroban-client@0.7.0` → `pkg:github/rahul-soshte/rs-soroban-client`
- scaffold_stellar: `pkg:cargo/stellar-scaffold-reporter@0.1.4` → `pkg:github/stellar-scaffold/cli`
- solidity_contracts_on_soroban: `pkg:cargo/solang-parser@0.3.5` → `pkg:github/hyperledger-solang/solang`
- soroban_disassembler_(working_title): `pkg:cargo/soroban-ret@0.0.4` → `pkg:github/Inferara/soroban-ret`
- stellar_hardware_wallet_support: `pkg:cargo/trezor-client@0.1.6` → `pkg:github/trezor/trezor-firmware`
- stellar_registry: `pkg:cargo/stellar-registry@0.1.4` → `pkg:github/stellar-registry/cli`
- serde control: `pkg:cargo/serde@1.0.229` → control (SBOM assertion only)

## Baseline procedure

Enable GitHub Dependency graph; leave Automatic dependency submission disabled.
The workflow resolves/builds the fixture, then runs the pinned, unchanged PG Atlas Action with read access and OIDC. No precursor submission action is installed.
The checker verifies package identities, versions, and paths from the root.
The workflow captures exact SBOM/submission evidence and distinguishes missing coverage (exit 1), errors (exit 2) and pending processing (exit 3).

Some toolchains are only installed on CI. Initial fixture setup runs export generated lockfiles as `fixture-locks` artifacts. Commit those files and rerun before classifying the definitive baseline: GitHub exports the committed tree, not the runner filesystem. A pre-lock setup run cannot establish a package-manager coverage failure.

Atlas processing may take hours. Run `verify-atlas.yml` manually with the recorded submission ID at the same fixture revision after processing. It only reads production API data. Preserve artifact/run links in the Phase 1 report. Artifacts expire after 30 days.

## Local check commands

```sh
python3 -m unittest -v
python3 check.py sbom --sbom /path/to/export.spdx.json
python3 check.py atlas --submission-id SUBMISSION_ID
```

The resolver/build commands are in `.github/workflows/baseline.yml`. An export or build error is reported separately from an omitted expected package. Successful package presence does not establish full transitive graph fidelity; only explicitly configured relationship assertions test that.
