# dvlsh - Dievilz Shell

## 🧭 Core Philosophy

* **POSIX-compliant**, Ksh-compatible shell
* Written entirely in **Zig**, with modules designed to be cleanly testable and maintainable
* Clear separation between core shell, plugins, CLI, configuration, and runtime state
* Emphasis on **interactive shell UX**: modern prompt, autosuggestions, syntax highlighting, and more
* No fallback or automatic behaviors: user must opt-in explicitly to advanced features.

---

## 🧱 Architecture Summary (v1 + v3 combined)

```plaintext
src/
├── main.zig
├── shell/            → Shell lifecycle, executor, session, job control
├── input/            → Editor, keymaps, autosuggest, history
├── output/           → Prompt, themes, ANSI control
├── parser/           → Lexer, parser, AST
├── builtins/         → cd, set, trace, jobs, etc.
├── config/           → Loader, schema, registry, multiple format support
│   └── format/       → zigrc.zig, gitstyle.zig
├── core/             → Env, vars, shellcontext, hooks
├── posix/            → Signal handling, redirection, fds
├── util/             → str.zig, io.zig, fs.zig
├── plugins/          → registry.zig, api.zig
└── test/             → Unit + integration tests
```

---

## 🔩 Config System

* Supports multiple formats: Zig DSL, JSON, Git-style
* Provenance tracking: every config entry tagged with source (default, user, plugin)
* Reloading must be **explicitly triggered** by user (`:reload-config`) — no live file-watching.

---

## 🧠 Script & Function Layer

* POSIX scripting is the only supported script language for now
* POSIX scripts can use shell-defined functions
* No separation between POSIX and non-POSIX behavior for now — design must remain coherent.

---

## ⚠️ Error Handling & Diagnostics

* POSIX `set` options supported: `-e`, `-u`, `-x`
* Add `:trace` command for live session tracing
* Errors should clearly display filename and context
* Tracing should be enabled **on demand**, not just at shell startup.

---

## 🔌 Plugin System

* Users do **not** need to write Zig for plugins — secondary language planned (TBD)
* Plugins may override or extend default behavior
* Features baked into shell but overridable:

  1. Prompt customization
  2. Syntax highlighting
  3. Autosuggestions
  4. History substring completion
  5. Command completion support and more.

---

## 💡 UX Innovations

### Fixed Prompt Zone

* Prompt is **always rendered at the bottom** of the terminal
* Older prompts remain in scrollback with faded visuals
* Prompt redraws cleanly (not reflows) on window resize
* Prompt zone and buffer zone are clearly separated
* Feature must be **explicitly enabled** — no automatic fallback in tmux or multiplexers

---

## 🔎 Traceability & Live Monitoring

* `dvlsh` will expose internal shell state (env, config, vars, history, trace logs)
* Users can inspect this **live** during a session or via external tools (e.g. Ghostty Inspector)
* Mechanism:

  * UNIX domain socket-based stream (initial)
  * Optional: Plan 9-style virtual FS (e.g. `/tmp/dvlsh-inspect/config/PATH`)
  * Future: experimental 9P protocol interface

---

## ✅ Confirmed Milestones Implemented

*

---
## Author's note
This is a POC so I can learn Zig and systems programming, sounds like the ultimate learning project at that but I can accept the challenge 😅. 
