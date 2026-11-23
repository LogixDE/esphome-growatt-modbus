# Release checklist

1. Run the GitHub Actions ESPHome compile workflow.
2. Test with a connected inverter: heartbeat, standard registers and FC32.
3. Test one number write, AC charging and one time-window write; verify readback.
4. Bump `esphome.project.version` in `packages/growatt.yaml` and add a matching `## [X.Y.Z]` section to `CHANGELOG.md`.
5. Pin the example configurations (`examples/growatt-example.yaml`, README) to the new tag.
6. Merge to `main`. The `Release` workflow creates the tag `vX.Y.Z` and a GitHub Release with the changelog section as notes, if that tag does not exist yet. It can also be started manually (workflow_dispatch).
