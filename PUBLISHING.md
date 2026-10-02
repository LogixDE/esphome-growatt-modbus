# Publishing checklist

1. Create a public GitHub repository named `esphome-growatt-modbus`.
2. Repository links are already configured for `LogixDE/esphome-growatt-modbus`.
3. Commit and push the repository.
4. In GitHub repository settings, enable:
   - Issues
   - Private vulnerability reporting (recommended)
   - Dependabot/security features as appropriate
5. Add repository topics, for example:
   - `esphome`
   - `home-assistant`
   - `growatt`
   - `modbus`
   - `rs485`
   - `solar`
   - `photovoltaics`
6. Create tag `v1.0.0`.
7. Create a GitHub Release from that tag using the `CHANGELOG.md` entry as release notes.
8. Test the exact public package URL from a clean local ESPHome YAML.

## Git commands

```bash
git init
git add .
git commit -m "Initial public release v1.0.0"
git branch -M main
git remote add origin https://github.com/LogixDE/esphome-growatt-modbus.git
git push -u origin main

git tag -a v1.0.0 -m "v1.0.0"
git push origin v1.0.0
```
