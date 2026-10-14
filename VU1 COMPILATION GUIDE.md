# VU1 COMPILATION GUIDE

This guide explains how to compile with Recompiled VU1 Programs for Shadow of the Colossus (SCES-53326 v1.00).

## Prerequisites
- Submodule `External/PS2Recomp` must be initialized (`git submodule update --init --recursive`).
- Build environment variables set (`VSROOT`, `SOTC_ISO`).
- The port built at least once (`build.bat ps2_recomp` and `Tools\generate.bat`).

## Step 1: Capture VU1 Microcode
Run the game with the capture flag enabled:
```powershell
mkdir capture_vu1
$env:PS2X_VU1_CAPTURE = (Resolve-Path "capture_vu1").Path
pwsh -ExecutionPolicy Bypass -File continue.ps1
```
Play through scenes (boot, opening cutscene, gameplay, colossus fights) to capture all VU1 images.
Files written:
- `capture_vu1\vu1_<hash>.bin`
- `capture_vu1\entries.txt`

## Step 2: Compile Captured Images
Use the `vu1_recomp` tool from the submodule:
```powershell
python External\PS2Recomp\ps2xTest\tools\vu1_recomp.py capture_vu1 Port\generated\vu1
```
Output: `Port\generated\vu1\vu1_<hash>_<n>.cpp`

## Step 3: Rebuild with Compiled VU1
```powershell
build.bat
```

## Step 4: Verify
```powershell
$env:PS2X_VU1_VERIFY = "1"
run.bat
```
This compares compiled VU1 code against the interpreter (expect 0 mismatches).

## Optional Settings
- `PS2X_VU1_TIMING=0` (set for faster emulation, disables VU1 timing model).
- `PS2X_VU1_RECOMP=0` (disable compiled VU1, fall back to interpreter).
