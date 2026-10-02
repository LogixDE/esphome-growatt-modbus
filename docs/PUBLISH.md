# Publish this repository on GitHub

## 1. Create the repository

Create a new **public** repository on GitHub:

- Owner: `LogixDE`
- Repository: `esphome-growatt-modbus`
- Do **not** add a README, .gitignore or license on GitHub; they are already included here.

## 2. Push the files

From a terminal in this repository directory:

```bash
git init
git add .
git commit -m "Initial release v1.0.0"
git branch -M main
git remote add origin https://github.com/LogixDE/esphome-growatt-modbus.git
git push -u origin main
```

Or, with GitHub CLI already authenticated:

```bash
git init
git add .
git commit -m "Initial release v1.0.0"
git branch -M main
gh repo create LogixDE/esphome-growatt-modbus --public --source=. --remote=origin --push
```

## 3. Create the v1.0.0 tag

```bash
git tag -a v1.0.0 -m "v1.0.0"
git push origin v1.0.0
```

Then open **Releases → Draft a new release**, select `v1.0.0`, and use the `CHANGELOG.md` v1.0.0 section as the release notes.

## 4. Suggested repository settings

Suggested GitHub topics:

`esphome`, `growatt`, `home-assistant`, `modbus`, `modbus-rtu`, `esp32`, `rs485`, `solar`, `inverter`

Recommended settings:

- Enable Issues.
- Keep Actions enabled so the ESPHome compile check runs on pushes and pull requests.
- Protect `main` later if external contributions start arriving.
- Do not commit `secrets.yaml`.
