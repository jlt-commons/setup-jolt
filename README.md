# setup-jolt

A GitHub Action that installs [jolt](https://github.com/jolt-lang/jolt)
(native Clojure on Chez Scheme, no JVM) on a runner. There's no marketplace
action for this today, the way
[`DeLaGuardo/setup-clojure`](https://github.com/DeLaGuardo/setup-clojure)
covers babashka and the Clojure CLI, so this is that, for jolt.

## Status

Proof of concept. Downloads a prebuilt release from
`jolt-lang/jolt`'s own GitHub releases and verifies its published
`sha256`. Covers Linux x86_64 and macOS x86_64/arm64, the platforms
jolt-lang/jolt ships prebuilt binaries for today (x86_64 Windows exists
upstream too, not wired here yet).

## Usage

```yaml
- uses: jlt-commons/setup-jolt@main
  with:
    version: '0.8.6'   # or omit for "latest"

- run: jolt --version
```

## Inputs

| Input | Default | Description |
|---|---|---|
| `version` | `latest` | jolt version to install, e.g. `0.8.6`. The `v` prefix is optional. `latest` resolves the newest GitHub release via the API. |

## Outputs

| Output | Description |
|---|---|
| `version` | The jolt version actually installed (useful when `version: latest` was requested and you want to log or cache-key on the resolved value). |

## Why this exists

[`lambda-mvp-jlt`](https://github.com/b12n-oss/lambda-mvp-jlt) wanted its
docs-site CI step to run via `jolt` instead of installing babashka
separately, since jolt reads a project's `bb.edn` tasks directly. That
specific swap turned out to be blocked on the consuming side (the docs
engine's own bb.edn eagerly requires `org.httpkit.server`, which jolt
doesn't bundle, so its tasks don't load under jolt yet, independent of
this action), but the "how do you even get jolt onto a runner" half of
the problem was real and generally useful, so it's split out here rather
than living inline in one project's workflow.

## License

EPL 2.0, see `LICENSE`.
