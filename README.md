# sacd-extract-gui

A small Electron GUI for the `sacd_extract` command-line extractor.

## Overview

This repository packages a GUI that drives the `sacd_extract` tool to extract audio from SACD ISOs.

## Windows distribution

For Windows you can use the prebuilt archive available at [sacd-extract-gui-win32-x64.zip](https://drive.usercontent.google.com/download?id=1sr6GenMgPHOp9V2HX7M5FqMGyeAk56iy&export=download&authuser=0). Download or copy that ZIP, extract it on your computer, then run `sacd-extract-gui.exe` from the extracted folder.

## Requirements

- Node.js (16+ recommended)
- npm
- `sacd_extract.exe` (Windows) — place the binary in the project root during development.

## Development

Install dependencies and run the app in development mode:

```bash
npm install
npm run start
```

During development keep `sacd_extract.exe` next to the project root (same folder as `package.json`). The app looks for the extractor in these locations:

- Packaged app: `process.resourcesPath/sacd_extract.exe`
- Development: `__dirname/../sacd_extract.exe` or `__dirname/sacd_extract.exe`

## Packaging

To package the application with Electron Forge:

```bash
npm run package   # create a packaged app in out/
npm run make      # create platform-specific installers
```

Important: Ensure `sacd_extract.exe` is included in the packaged `resources` folder. The project already uses `extraResource` in `forge.config.js`, but to guarantee the binary is unpacked and runnable, you can add `asarUnpack: ['sacd_extract.exe']` to `packagerConfig`.

Example `packagerConfig` snippet in `forge.config.js`:

```js
packagerConfig: {
  asar: true,
  extraResource: ['sacd_extract.exe'],
  asarUnpack: ['sacd_extract.exe'],
},
```

After packaging, the extractor should be at `resources/sacd_extract.exe` and the app will spawn it from `process.resourcesPath`.

## Troubleshooting

- Error `spawn ... app.asar\sacd_extract.exe ENOENT`: means the app attempted to spawn the binary inside the ASAR archive. Fix by adding `asarUnpack` or using `extraResource` so the binary is copied to `resources` (outside the ASAR).
- Verify the binary exists in the packaged `out/.../resources` directory before running the EXE.

## Notes

The application code prefers the extractor in `process.resourcesPath` when running as a packaged app, and falls back to development locations when running via `npm start`.

If you want, I can update `forge.config.js` to add `asarUnpack` now and/or run a local package build — tell me which you'd like.

