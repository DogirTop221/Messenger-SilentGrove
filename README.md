# Messenger SilentGrove Desktop (Electron + PHP)

Desktop wrapper for Messenger SilentGrove on Windows.
Electron starts a local PHP server automatically and opens it inside a native app window.

## Architecture

```text
.
|-- electron/
|   |-- main.js
|   `-- preload.js
|-- renderer/
|   `-- README.md
|-- backend/
|   |-- public/
|   |-- src/
|   |-- database/
|   |-- storage/
|   |-- .env
|   `-- .env.example
|-- build/
|   |-- icon.ico
|   `-- icon.png
|-- package.json
`-- README.md
```

## How it Works

- Electron launches `php -S localhost:8000 -t public` inside `backend/`.
- If DB file is missing, Electron runs `php backend/database/setup.php` automatically.
- App opens `http://localhost:8000`.
- Current UI and PHP backend are preserved.
- On app close, Electron terminates spawned PHP process.
- Single-instance lock prevents duplicate app/server start.

## Requirements

- Windows 10/11
- Node.js + npm
- PHP 8+ with `pdo_sqlite` enabled (for development mode)

## Run in VS Code

1. Install dependencies:

```bash
npm install
```

2. (Optional, first run only) initialize DB manually:

```bash
php backend/database/setup.php
```

3. Start desktop app:

```bash
npm run start
```

If `php` is not in PATH, either:

- set `PHP_BINARY` to full path of `php.exe`, or
- prepare portable runtime in local `php/` folder (see next section).

## Build for End Users (No PHP Install Required)

Your users should run only installer/exe.  
To make that possible, bundle portable PHP into the app before build:

1. Download/extract Windows PHP (NTS x64) somewhere.
2. Prepare local runtime:

```bash
npm run prepare:php -- "C:\path\to\extracted\php"
```

3. Build installer:

```bash
npm run build
```

After this, the installer already contains `php.exe`, so end users do not install PHP.

## Build Windows .exe

```bash
npm run build
```

Build output:

- Installer: `dist/Messenger-SilentGrove-Setup-<version>.exe`
- Unpacked app folder: `dist/win-unpacked/`
- Main executable: `dist/win-unpacked/Messenger SilentGrove.exe`

## Offline / Local

App runs fully local on `localhost` and does not require internet if your backend/data are local.
