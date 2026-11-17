# Atlas publish

Publishes template versions to an Atlas panel when a release is tagged.

```yaml
on:
  push:
    tags: ["v*"]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - run: ./gradlew build
      - uses: getatlasgg/publish@v1
        with:
          panel: https://panel.example.net
          token: ${{ secrets.ATLAS_TOKEN }}
          files: |
            games/dusty-dunes:server.jar = build/libs/game.jar
            proxy/velocity:plugins/dusty.jar = proxy/build/libs/proxy.jar
```

Create the token in the panel under Settings, People, Tokens, and pick the
templates it may publish. The panel shows it once; save it as the
`ATLAS_TOKEN` repository secret.

A stable tag such as `v1.4.2` publishes the next version of every template
the files name, all of them or none. A tag with a dash before any `+`, such
as `v1.5.0-beta.1`, opens a draft per template instead, and someone publishes
it from the panel.

| Input | Default | |
| --- | --- | --- |
| `panel` | | The panel's https address |
| `token` | | A publish token |
| `files` | | One `template:path = file` per line |
| `tag` | the pushed tag | The release tag |
| `link` | the commit | Where the release came from |
| `draft` | `false` | Open drafts even for a stable tag |

The action installs `atlas` with `curl -fsSL https://getatlas.gg/cli | sh`,
which checks the binary against the release's signed checksums.
