# GameTests

Every mod runs its GameTests in CI with `./gradlew runGameTestServer`. The rules below follow the [Horizon-QA principles](https://github.com/GTNewHorizons/Horizon-QA/blob/master/PRINCIPLES.md) used by GTNH.

1. **Functional tests, not mocks.** Place real blocks and run the real machine.
2. **Mock supply, not validation.** A test may give a machine its inputs any way it likes, but it never bypasses recipe logic.
3. **Wait on state, not ticks.** Tick counts are only timeouts.
4. **Negative tests matter.** For every new multiblock, add at least a "forms" test and a test that it does not form, or does not run, when it should not.
5. **Leave no trace.** Remove any block placed outside the template, and never leave a synthetic recipe registered.
6. **Failures explain themselves.** The message says what went in, what was expected and what happened.

## Sweep tests

Each mod has tests that sweep over everything it registers, so new content is checked the day it is added:

- every multiblock forms from its own preview;
- no GT recipe has an empty input or output, or a duration of 0;
- every machine tooltip builds without a raw translation key;
- every name exists in en_us, pt_br and zh_cn.

## Filtering GameTests

Mods that depend on TypeOmega Lib get GameTest filtering automatically via `-Dtypeomega.tests=<terms>` (`GameTestFilter`), matching comma-separated terms (case-insensitive substring match) against the method name, simple class name or batch name.

To pass `-Ptests=...` from Gradle, expose the property in your `gameTestServer` run configuration:

```groovy
runs {
    gameTestServer {
        systemProperty('typeomega.tests', project.findProperty('tests') ?: '')
    }
}
```

### Heavy batch

Slow or expensive tests (such as auto-build sweeps) should be marked with `batch = TestBatches.HEAVY` or `@HeavyTest` (on a method or class). Heavy tests are excluded by default and from normal filtered runs; they only run when `heavy` or `all` is passed.

### Fast workflow vs. final commit

During active development, filter down to the area you are working on:

```sh
./gradlew runGameTestServer -Ptests=nanoforge
./gradlew runGameTestServer -Ptests=nanoforge,tooltip
./gradlew runGameTestServer -Ptests=heavy
```

**Before the final commit**, always run the full suite including heavy tests:

```sh
./gradlew runGameTestServer -Ptests=all
```
