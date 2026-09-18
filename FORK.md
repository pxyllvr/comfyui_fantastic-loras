# Fork notes — classic loader restored

This branch is `restore-classic-loader` on top of upstream `83bf758` (v2.2.1).

## Why

Upstream v2.0.0 (`ea6599b`, 14 Aug 2026) did two breaking things after `d891ad1`:

1. Deleted the `FantasticLoraLoader` class. Workflows that stored that type show a missing node.
2. Reused `FantasticLoraLoaderMulti` for a slot-grid UI and changed its extra outputs from `MODEL 2–5` to `MODEL 2, CLIP 2, MODEL 3, CLIP 3, …`. Link indices in old graphs no longer match.

The compact list UI (➕ Add Lora / ✕ remove) was replaced by a 12-slot grid.

## What this fork changes

| Type key | Display name | UI |
|---|---|---|
| `FantasticLoraLoader` | Fantastic Lora Loader (Classic) | v1 compact list |
| `FantasticLoraLoaderMulti` | Fantastic Lora Loader (Classic Multi) | v1 compact list + extra MODEL outputs |
| `FantasticLoraLoaderSlots` | Fantastic Lora Loader (Slots) | v2 slot grid |

Plotter, Mimic, Any Selector, Seeds, Grid Viewer stay on the v2 implementation.

## Publish your own GitHub fork

You have to click Fork on GitHub yourself (this environment cannot create a repo under your account).

```bash
# 1. In the browser: Fork https://github.com/Adudeguyman/comfyui_fantastic-loras
# 2. Clone YOUR fork, then pull these commits onto it:

git clone https://github.com/<you>/comfyui_fantastic-loras.git
cd comfyui_fantastic-loras
git remote add patched /path/to/this/folder
git fetch patched
git checkout -b restore-classic-loader patched/restore-classic-loader
git push -u origin restore-classic-loader
```

Or copy the changed files onto a fresh fork of `main`:

- `nodes.py`
- `web/lora_folder_loader.js`
- `web/lora_folder_loader_classic.js` (new)
- `README.md`
- `pyproject.toml`
- `example_workflows/Fantastic Loras Ideogram4.json`

## Install over the upstream pack

```bash
cd ComfyUI/custom_nodes
# remove or rename the official clone first
rm -rf comfyui_fantastic-loras
git clone -b restore-classic-loader https://github.com/<you>/comfyui_fantastic-loras.git
```

Restart ComfyUI and hard-refresh the browser (Ctrl+Shift+R).

Do not install this fork *and* the official pack at the same time — the class names would collide.

## Comfy Registry

`pyproject.toml` still has `PublisherId = "adudeguyman"`. Change that before you publish to the Comfy registry, and consider renaming the project if you do not want it to look like an official release.

## If a graph was already saved on official 2.x

Those loaders are type `FantasticLoraLoaderMulti` but expect slot outputs. Swap that node for **Fantastic Lora Loader (Slots)**. Graphs saved at or before `d891ad1` do not need that swap.
