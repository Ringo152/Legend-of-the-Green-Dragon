# Repository Restructuring Plan

## Goal
Transform the legacy flat-file structure into a modern, PSR-4 compliant application layout. This increases security by moving logic out of the web root and improves maintainability.

## Current vs. Target Structure

### Current Layout (Root Clutter)
```mermaid
graph TD
    Root[/] --> Bin[bin/]
    Root --> Classes[classes/]
    Root --> Lib[lib/]
    Root --> Modules[modules/]
    Root --> PhpFiles[*.php (90+ files)]
    Root --> MdFiles[*.md]
```

### Target Layout (Modern)
### Target Layout (Modern)

The goal is to separate public-facing entry points (Web Root) from the application logic (Src) and configuration.

```text
/
├── bin/                        # Internal executables
│   ├── socket-server.php
│   └── server-watcher.php
│
├── config/                     # Configuration (Moved from root)
│   ├── dbconnect.php
│   └── config.php
│
├── docs/                       # Documentation
│   ├── CHANGELOG.md
│   └── websocket.md
│
├── lib/                        # Legacy Procedural Libraries
│   ├── commentary.php
│   └── output.php
│
├── public/                     # Mapped to Nginx Root (WEB ACCESSIBLE)
│   ├── index.php               # Main Entry Point
│   ├── village.php             # Game Scene
│   ├── assets/                 # CSS, JS, Images
│   │   ├── css/
│   │   ├── js/
│   │   └── images/
│   └── install.php
│
├── src/                        # PSR-4 Namespace: LotGD\
│   ├── Core/                   # Core Logic
│   │   ├── Game.php
│   │   └── Session.php
│   ├── Modules/                # Game Modules (Logic)
│   │   └── Forest.php
│   └── Templates/              # View Templates
│
└── vendor/                     # Composer Dependencies
```

## Migration Steps

### Phase 1: Organization & Documentation (Immediate)
- [x] Create `docs/` and move `*.md`, `*.txt`, `CHANGELOG*`, `LICENSE` there.
- [x] Create `config/` (copied `dbconnect.php`). <!-- id: 38 -->

### Phase 2: Namespace & Autoloading
- [x] Update `composer.json` to map `LotGD\\` to `src/`.
- [x] Move `classes/*` to `src/`.
- [x] Run `composer dump-autoload`.

### Phase 3: Public Entry Point (The Big Move)
- [x] Create `public/` directory.
- [x] Move all entry point PHP files (`index.php`, `village.php`, etc.) to `public/`.
- [x] **CRITICAL**: Update `lib/constants.php` or `common.php` to handle path changes (e.g. `__DIR__ . '/../'` instead of `__DIR__`).
- [ ] Update Nginx configuration to point root to `/var/www/html/lotgd.io/public`.

## Nginx Configuration Updates
When moving to Phase 3, update `tynastera.com.conf` as follows:

```nginx
server {
    # ...
    # CHANGE: Updates root to public/
    root /var/www/html/lotgd.io/public;

    # PREVENT DIRECT ACCESS to non-public files
    # (If root is still parent, use this to protect src)
    location ^~ /src/ {
        deny all;
        return 404;
    }
    
    # ...
}
```

## Notes
- **Autoloading**: We already use `LotGD\\` -> `classes/`. Moving to `src/` requires updating this mapping.
- **Legacy Includes**: Many files use `require "lib/..."`. These relative paths will break if files move to `public/` without ensuring the include path includes the project root.
