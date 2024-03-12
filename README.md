# GitHub workflows

This repository contains a collection of [reusable GitHub workflows] for use in Rust projects.

- [rust-check](#rust-check)
- [rust-release-plz](#rust-release-plz)
- [rust-test](#rust-test)

[reusable GitHub workflows]: https://docs.github.com/en/actions/using-workflows/reusing-workflows

## Rust Check

[rust-check.yml](.github/workflows/rust-check.yml)

- formatting using rustfmt
- lints using clippy and [rs-clippy-check](https://github.com/marketplace/actions/rs-clippy-check)
- docs (using nightly)
- msrv

```yaml
# .github/workflows/check.yml
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  check:
    permissions:
      checks: write
    uses: joshka/github-workflows/.github/workflows/rust-check.yml@main
    with:
      msrv: 1.76.0 # this is optional defaults to 1.56.0
```

## Rust Test

[rust-test.yml](.github/workflows/rust-test.yml)

Runs tests:

- ubuntu with stable and beta
- mac / windows with stable
- minimal versions
- upload coverage to codecov.io (requires CODECOV_TOKEN to be added to the repository secrets)

```yaml
# .github/workflows/test.yml
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  test:
    uses: joshka/github-workflows/.github/workflows/rust-test.yml@main
    secrets:
      CODECOV_TOKEN: ${{ secrets.CODECOV_TOKEN }}
```

## Rust Release-plz

[rust-release-plz.yml](.github/workflows/rust-replease-plz.yml)

Publishes the crate using [Release-plz].

Required Settings:

- `Settings | Actions | General | Allow GitHub Actions to create and approve pull requests`:  \
  ticked
- `Settings | Secrets and Variables | Actions | Repository Secret | CARGO_REGISTRY_TOKEN`:  \
  set to value from <https://crates.io/settings/tokens>

[Release-plz]: https://release-plz.ieni.dev

```yaml
# .github/workflows/release-plz.yml
permissions:
  pull-requests: write
  contents: write

on:
  push:
    branches:
      - main
jobs:
  release-plz:
    uses: joshka/github-workflows/.github/workflows/rust-release-plz.yml@main
    permissions:
      pull-requests: write
      contents: write
    secrets:
      CARGO_REGISTRY_TOKEN: ${{ secrets.CARGO_REGISTRY_TOKEN }}
```
