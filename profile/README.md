<div align="center">

# einky

**A distraction-free handheld for reading Ren'Py visual novels on e-ink.**

[![Docs](https://img.shields.io/badge/docs-docs.einky.fr-000?logo=readthedocs&logoColor=white)](https://docs.einky.fr)
[![Engine](https://img.shields.io/badge/Ren'Py-8.5.2-673ab7)](https://www.renpy.org)
[![OS](https://img.shields.io/badge/InkyOS-Buildroot%202026.05-c00)](https://github.com/einky/buildroot_os)
[![License](https://img.shields.io/badge/license-MIT-blue)](https://github.com/einky/meta/blob/main/LICENSE)

</div>

---

**einky** is a custom handheld console that runs [Ren'Py](https://www.renpy.org)
visual novels and narrative games on a **3.97″ 800×480 e-ink panel**, built on a
**Raspberry Pi Zero 2 W**. It is an End-of-Studies Project (EIP) developed at
**Epitech Montpellier**.

The design leans into what e-ink is good at:

- 🔋 **Minimal battery draw** — e-ink holds a frame with no power
- 🌙 **Zero blue light** — no backlight, easy on the eyes
- 📖 **Distraction-free reading** — a device built for one thing

## How it works

The device runs **InkyOS**, a purpose-built [Buildroot](https://buildroot.org)
appliance that boots straight into Ren'Py — no package manager, no desktop in
the boot path. Ren'Py renders headless into a virtual framebuffer; a pipeline
dithers each frame to 1-bit and pushes it to the panel over SPI, while seven
hardware buttons are fed back into the engine as input.

```
InkyOS boots → Xvfb virtual display → Ren'Py renders headless (llvmpipe SW GL)
   → frame captured → greyscale → Floyd–Steinberg dither → pack 1-bit
   → C SPI driver → GDEM0397T81P e-ink panel
   ← 7 GPIO buttons → key/event injection back into Ren'Py
```

The same frame + input pipeline serves the **launcher** (itself a Ren'Py game
that acts as the boot menu) and every game it starts.

## The repositories

einky is a **polyrepo**. Clone [`meta`](https://github.com/einky/meta) first and
run `./bootstrap.sh` — it clones every other repo as a sibling on disk.

| Repo | Responsibility |
|---|---|
| [**meta**](https://github.com/einky/meta) | Workspace entry point — `bootstrap.sh`, version pins, ADRs, and `shared/`: the single source of truth for the pinout, keymap, and wire protocols |
| [**.github**](https://github.com/einky/.github) | This repo — org profile, shared workflows, issue templates |
| [**docs**](https://github.com/einky/docs) | Architecture, onboarding, and hardware guides → [docs.einky.fr](https://docs.einky.fr) |
| [**buildroot_os**](https://github.com/einky/buildroot_os) | **InkyOS** — the Buildroot device image that boots to a game |
| [**runtime**](https://github.com/einky/runtime) | Canonical owner of the on-device logic: frame pipeline, GPIO input, C SPI driver |
| [**launcher**](https://github.com/einky/launcher) | The Ren'Py game shown at boot — scans `games/` and starts a title |
| [**server**](https://github.com/einky/server) | FastAPI backend — game catalog and telemetry |
| [**web**](https://github.com/einky/web) | Web frontend — catalog and admin tooling |
| [**case**](https://github.com/einky/case) | Enclosure CAD, BOM, and wiring |
| [**games**](https://github.com/einky/games) | Ren'Py game sources |

## Tech at a glance

| | |
|---|---|
| **Hardware** | Raspberry Pi Zero 2 W · GDEM0397T81P e-ink panel (800×480, 1-bit) · 7 GPIO buttons |
| **Device OS** | InkyOS — Buildroot 2026.05 appliance image |
| **Engine** | Ren'Py 8.5.2, built from source on the device (Mesa `llvmpipe` software GL) |
| **Runtime** | Python 3.14 frame/input pipeline + C SPI driver |
| **Backend** | FastAPI · Postgres · web frontend |

## Getting started

```bash
git clone https://github.com/einky/meta.git
cd meta
./bootstrap.sh        # clones every repo as a sibling; add --ssh for ssh remotes
```

New here? Start with the [documentation](https://docs.einky.fr) — the
[architecture overview](https://docs.einky.fr/docs/architecture/overview) and
[developer onboarding](https://docs.einky.fr/docs/getting-started/developers)
are the fastest way in.

<div align="center">

---

Made with 🖋 by the einky team · Epitech Montpellier EIP · MIT licensed

</div>
