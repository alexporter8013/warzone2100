# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Warzone 2100: open-source 3D RTS (C++, GPL). Cross-platform (Windows/macOS/Linux/Emscripten), OpenGL/GLES/Vulkan backends, JavaScript (QuickJS) scripting for rules, campaigns and AIs. Homepage https://wz2100.net.

## Build

CMake + vcpkg (manifest mode, `vcpkg.json`; overlay ports/triplets in `.ci/vcpkg/`). 3rdparty libs are git submodules (`git submodule update --init --recursive`).

On this Windows checkout the existing build tree is `build/` using the **Visual Studio 17 2022** generator (multi-config), so pass `--config`:

```powershell
cmake -S . -B build                          # reconfigure (only needed after CMake changes)
cmake --build build --config Debug --target warzone2100
```

- Dependency helper scripts: `get-dependencies_win.ps1`, `get-dependencies_linux.sh`, `configure_mac.cmake`.
- `WZ_ENABLE_WARNINGS_AS_ERRORS` is ON by default: new warnings break the build.
- Backends/features are toggled by `WZ_ENABLE_*` options (e.g. `WZ_ENABLE_BACKEND_VULKAN`, `WZ_ENABLE_BASIS_UNIVERSAL`, `WZ_ENABLE_WEBM`).
- Game data in `data/` is packaged into `.wz` archives at build time (`data/CMakeLists.txt`); `data/base` is the core/campaign data, `data/mp` the multiplayer ruleset, `data/mods` optional mods.

## Tests

There is no unified test runner. Pieces:
- Opt-in standalone unit tests, enabled with CMake options and built as their own targets: `WZ_BUILD_TERRAIN_SURFACE_TEST`, `WZ_BUILD_VP9_BITSTREAM_TEST`, `WZ_BUILD_RENDER_GRAPH_TEST` (sources in `tests/*.cpp`). Run one by building its target (`terrain_surface_test`, `vp9_bitstream_test`, `render_graph_pass_sequence_test`) and executing it.
- Determinism/fidelity scripts in `tests/gamestate/` (bash/python, need a built binary, many need Linux + Xvfb). They compare per-tick sync CRCs from `--gamestate-crc-trace`; see `tests/gamestate/README.md` for each script (`forkcrc.sh`, `matrix.sh`, `replayrewind.sh`, ...). Run the standard set: `tests/gamestate/matrix.sh <bin> [filter]`.
- Benchmarks: `tests/movebench/`, `tests/pathbench/`, `tests/perf/` (each has `run.sh` and a baseline JSON).
- `<test>` arguments are `--skirmish` settings files in `data/mp/tests`, `mb:<scenario>` movement-bench scenarios, or `cam:<level>`.
- CI lint (`.github/workflows/CI_linter.yml`) runs ESLint (`eslint.config.mjs`) over the JS in `data/`.

## Code style (doc/CodingStyle.md)

Deliberately old-school: **tabs** for indentation, braces on their own line, always brace `if` bodies, no exceptions, avoid templates/fancy C++, don't use legacy `UDWORD`-style types in new code, no commented-out code. Formatting is astyle-based (options in `doc/CodingStyle.md`); no `.clang-format`. Comment *why*, not what.

## Architecture

Top-level layout: `lib/` = reusable engine libraries, `src/` = the game itself (~160 .cpp, flat), `data/` = game content + JS scripts, `doc/` = scripting API docs (`js-*.md`, `Scripting.md`) and design docs.

### `lib/`
- `framework/` – base layer: debug/logging, PhysFS wrappers, string-resource and `.wrf/.lev` resource parsers (flex/bison `.lpp/.ypp`), `frame.h` (global player identity, see below).
- `gamelib/gtime` – the lockstep game clock (game ticks, stall handling such as `shouldWaitForPlayerSlot`).
- `netplay/` – networking: `netplay.cpp` (message queues, per-player `NETQUEUE`), `netjoin`, `netlobby`, `netreplay`, `nettypes`; transport behind a connection-provider abstraction (`gns/` = GameNetworkingSockets, plus TCP; port mapping via libplum/miniupnpc).
- `ivis_opengl/` – renderer abstraction (`gfx_api`) with GL/GLES/Vulkan backends, shaders in `data/base/shaders`, PIE models, textures.
- `widget/` – the UI widget toolkit; `sound/`, `sequence/` (video), `sdl/` (windowing/input), `wzmaplib/` (map file I/O), `exceptionhandler/`.

### `src/` – game
- **Entry / loop**: `main.cpp` (init, CLI via `clparse.cpp`, main loop, game-state mode switching), `init.cpp` (resource/data loading, mod handling), `loop.cpp` (per-frame/per-tick update), `levels.cpp` + `mission.cpp` (level/`.lev` loading, campaign mission transitions, off-world missions, transporters).
- **Simulation is deterministic lockstep**: all clients simulate the same ticks from the same inputs; game-affecting state changes must go through the net message queue (`multiplay.cpp`, `multisync.cpp`, `order.cpp`) and use `syncDebug()` so desyncs can be located. Never use non-deterministic data (wall-clock, unsynced RNG, float-dependent branching) in simulation code. Per-tick sync CRCs underpin multiplayer desync detection, replays, and the gamestate tests.
- **Game objects**: `baseobject`, `droid`, `structure`, `feature`, `projectile`, `weapon`, `combat`, plus stats loaded from JSON in `data/*/stats` (`stats.cpp`, `research.cpp`, `power.cpp`, `template.cpp`). AI/movement: `action`, `order`, `move`, `astar`, `fpath`, `steering/`, `corridor_*`.
- **Scripting**: `qtscript.cpp` (script context/event dispatch, QuickJS), `wzapi.cpp` (the API exposed to JS; shared with docs in `doc/js-*.md`), `wzscriptdebug`. Rules (`data/{base,mp}/script/rules.js`), campaign libs (`data/base/script/campaign`), and skirmish AIs (`data/mp/multiplay/skirmish`) are JS. API changes must be reflected in the docs.
- **UI**: `hci.cpp` + `hci/` (in-game HUD panels), `display.cpp`/`display3d.cpp` (world rendering), `titleui/` (front-end screens, the lobby is `multiint.cpp` + `titleui/multiplayer.h`), `frontend.cpp`, `ingameop.cpp`.
- **Save/load & snapshots**: `game.cpp` (legacy savegame format), `gamestate_*.cpp` (newer GameState serialization: `gamestate_savegame`, `gamestate_snapshot` / `_snapshot_ring` for in-process rewind, `gamestate_replay_seek`, `gamestate_harness` for test options). Replays use the net queue recording in `lib/netplay/netreplay`.
- Many dev-only CLI flags (`--gamestate-*`, `--skirmish`, `--autogame`, `--headless`) are defined in `src/clparse.cpp`; headless works only with `--autogame/--autohost/--skirmish`.

## Co-op campaign work (branch `add-coop`, in progress)

Not in upstream. All humans share **one campaign player** (player 0) while each keeps its own connection slot.
- **Player vs controller split**: *player* = faction (`selectedPlayer`; world, UI, scripts); *controller* = connection slot (`realSelectedPlayer`; networking). Use `localPlayer()` / `localControllerSlot()` from `lib/framework/frame.h`, not the raw globals, in new code. Helpers in `multiplay.cpp`: `isCoopGame()` (campaign + networked), `coopControlsPlayer(slot, player)`, `coopHostControlsFaction(slot, player)`, `mySlotResponsibility(slot)`, `isHumanGamePlayer(player)`.
- **Slot numbers are also enemy faction numbers** (the second human's slot 1 is faction 1, the first enemy). `whosResponsible()`/`responsibleFor()` take a *slot*; `myResponsibility()` takes a *faction* and in co-op means "I'm a controller of the shared player, or I'm the host (which runs every other faction)". Never destroy or re-own a faction because a slot with the same number left (`handlePlayerLeftInGame`, `clearPlayer`).
- Menus that list connection slots (`multimenu.cpp`) map a human row to the shared faction for units/alliances and use the controller slot as "us".
- The lobby reuses `WzMultiplayerOptionsTitleUI` with `game.type == CAMPAIGN`. Co-op saves live in `savegames/coop/`; hosting a save goes through the lobby (`multiLobbyBeginCoopLoad`).
- Mission end / next level is a host-driven handshake (`NET_COOP_NEXT_LEVEL`, `coopMissionUpdate` in `mission.cpp`); pause is `NET_COOP_PAUSE` (Ctrl+P; the in-game menu does not pause in co-op). Co-op is a campaign `GameMode`, so no replay is recorded.
- Loading a save inside a running co-op game is host-only (`coopHostOfferLoad`, `NET_COOP_LOAD`): the save is offered, each client consents and downloads it over `NET_FILE_*` (served via `multiLobbyHostServeCoopSave`), says ready, and the existing next-level GO/ACK handshake (`COOP_NEXT_LEVEL_GO_LOAD`) then starts `LMS_LOADGAME` on every machine. F7/F8 QuickSave/QuickLoad work the same way (`savegames/coop/QuickSave`).
- `--coop-level=<level>` starts a new co-op campaign at that level (test carry-over without playing through).
- Keep networking code on the controller side, and keep each change a **no-op outside co-op campaign** so it can later be split into small upstream PRs (planned order: accessors+audit, controller→player mapping, campaign-through-MP guards, menu+lobby, mission transitions).
- Mid-game join is deferred; it would build on async join approval (`NETsetAsyncJoinApprovalRequired/Result`), the lockstep stall in `gtime.cpp`, and gamestate snapshots + `NET_FILE_PAYLOAD`.

## Useful docs

`doc/Scripting.md` (+ `js-*.md`), `doc/CampaignMods.md`, `doc/Debugging.md` (debug menu `ctrl+o` in cheat mode `shift+backspace`, `objTrace`, `camTrace`), `doc/ConfigDir.md`, `doc/CmdInterface.md`, `doc/hosting/`.
