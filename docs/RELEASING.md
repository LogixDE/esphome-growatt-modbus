# Release checklist

1. Run the GitHub Actions ESPHome compile workflow.
2. Test with a connected inverter: heartbeat, standard registers and FC32.
3. Test one number write, AC charging and one time-window write; verify readback.
4. Update `CHANGELOG.md`.
5. Commit and push to `main`.
6. Create an annotated tag, for example `v1.0.0`.
7. Create a GitHub Release from the tag and paste the matching changelog section.
8. Keep example configurations pinned to the stable tag.
