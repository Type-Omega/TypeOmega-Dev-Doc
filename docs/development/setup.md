# Setup

All mods target **Minecraft 1.20.1, Forge 47.4.x, Java 17** and build with Gradle (ModDevGradle legacy Forge).

```sh
git clone https://github.com/Type-Omega/TypeOmegaCore.git
cd TypeOmegaCore
./gradlew build
./gradlew runGameTestServer -PtypeOmegaDevOptionalDependencies=true
```

| Command | What it does |
| --- | --- |
| `./gradlew runClient` | Starts a development client |
| `./gradlew runData` | Regenerates data (recipes, models, `en_us.json`) |
| `./gradlew runGameTestServer` | Runs every GameTest headlessly and exits with an error on failure |
| `./gradlew spotlessApply` | Formats the code |

TypeOmegaCore needs GTNA and AE2 at runtime. Without them, pass `-PtypeOmegaDevOptionalDependencies=true` so the test server can start.

### Dev runtime flags (`TypeOmegaCore`)

| Flag | What it does |
| --- | --- |
| `-PtypeOmegaDevOptionalDependencies=true` | Loads optional pack dependencies (Productive Bees, Ad Astra, AE2 WCWT, AE2 Wireless Terminals) at dev runtime and sets `integration_mandatory=false` |
| `-PtypeOmegaDevFtbTeams=false` | Disables FTB Teams in dev runtime (loaded by default) to test solo fallback paths |
| `-PtypeOmegaDevPlaytest=true` | Loads client playtest mods (FTB Chunks, AE2 WCWT, AE2 Wireless Terminals) for `runClient` |
| `-Ptests=<terms>` | Filters `runGameTestServer` by method, class or batch (`heavy` or `all` for heavy tests; see [GameTests](gametests.md#filtering-gametests)) |

## The modpack

The pack is managed with packwiz. `manifest/full.lock.json` is the source of truth: `tools/pack.py lock|fetch`, then `tools/packwiz_sync.py`. Quests are generated from `quests-src/` with `tools/questgen.py build` and checked with `tools/questgen.py check`.

## Organisation Maven

Released mod jars are published at **https://type-omega.github.io/maven/** (no login needed). The [maven repository](https://github.com/Type-Omega/maven) checks for new GitHub releases every 6 hours and keeps the newest 5 versions of each artifact.

```groovy
repositories {
    maven { url = 'https://type-omega.github.io/maven/' }
}
dependencies {
    modCompileOnly("com.raishxn.gtna:gtna:0.6.0") { transitive = false }
}
```

TypeOmegaCore compiles against GTNA from there. To compile against a local GTNA build instead, pass `-PgtnaApiJar=/path/to/gtna.jar`. To publish a new mod, add it to `artifacts.json` in the maven repository.
