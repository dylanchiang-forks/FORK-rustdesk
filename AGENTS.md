# RustDesk 衍生工程 — Agent 規則

先讀本檔、`RULES.md` 與精簡的 `MEMORY.md`（存在時）；歷史只按問題查找，不預設全文載入。

- 倉庫：`dylanchiang-forks/FORK-rustdesk`；本機 `/002/dylanchiang-forks/FORK-rustdesk/`。
- Rust 原生核心加 Flutter 介面；保留 origin 與 upstream 的區別，不自行改寫上游歷史。
- 先讀下方 Rust／Tokio 規則；建置、跨平台命令與配置位置按任務讀開發參考。
- 修改前核對分支、origin、upstream 與工作樹；保留既有修改，只處理本次相關檔案，不強推或自行重設歷史。
- 現況改寫 MEMORY 原條目；詳細經過放 docs/memory，已結束的歷史退出根入口，不每輪追加。
- 不把憑證、環境值或私人狀態寫入文件、日誌或 Git；不新增追蹤及未要求的網路呼叫。
- 驗證依實際 scripts 與變更範圍；純協作文件檢查差異與連結，不啟動服務或資料庫。提交、推送與部署遵守當次授權。

## RustDesk Project Layout

### Directory Structure
- `src/` Rust app
- `src/server/` audio / clipboard / input / video / network
- `src/platform/` platform-specific code
- `src/ui/` legacy Sciter UI (deprecated)
- `flutter/` current UI
- `libs/hbb_common/` config / proto / shared utils
- `libs/scrap/` screen capture
- `libs/enigo/` input control
- `libs/clipboard/` clipboard
- `libs/hbb_common/src/config.rs` all options

### Key Components
- **Remote Desktop Protocol**: Custom protocol implemented in `src/rendezvous_mediator.rs` for communicating with rustdesk-server
- **Screen Capture**: Platform-specific screen capture in `libs/scrap/`
- **Input Handling**: Cross-platform input simulation in `libs/enigo/`
- **Audio/Video Services**: Real-time audio/video streaming in `src/server/`
- **File Transfer**: Secure file transfer implementation in `libs/hbb_common/`

### UI Architecture
- **Legacy UI**: Sciter-based (deprecated) - files in `src/ui/`
- **Modern UI**: Flutter-based - files in `flutter/`
  - Desktop: `flutter/lib/desktop/`
  - Mobile: `flutter/lib/mobile/`
  - Shared: `flutter/lib/common/` and `flutter/lib/models/`

## Rust Rules

- Avoid `unwrap()` / `expect()` in production code.
- Exceptions: tests; lock acquisition where failure means poisoning, not normal control flow.
- Otherwise prefer `Result` + `?` or explicit handling.
- Do not ignore errors silently.
- Avoid unnecessary `.clone()`.
- Prefer borrowing when practical.
- Do not add dependencies unless needed.
- Keep code simple and idiomatic.

## Tokio Rules

- Assume a Tokio runtime already exists.
- Never create nested runtimes.
- Never call `Runtime::block_on()` inside Tokio / async code.
- Do not hide runtime creation inside helpers or libraries.
- Do not hold locks across `.await`.
- Prefer `.await`, `tokio::spawn`, channels.
- Use `spawn_blocking` or dedicated threads for blocking work.
- Do not use `std::thread::sleep()` in async code.

## Editing Hygiene

- Change only what is required.
- Prefer the smallest valid diff.
- Do not refactor unrelated code.
- Do not make formatting-only changes.
- Keep naming/style consistent with nearby code.

[依模組查開發參考](docs/agent-development.md)。

[整理前規則](docs/archive/AGENTS-before-20261116.md) 僅供歷史追溯；舊路徑、部署日誌與過時命令不作為目前環境依據。
