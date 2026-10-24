# 🧠 Hermes ↔ useMemos Integration

`usememos-skill` — connect Hermes Agent to your self-hosted useMemos instance so you can list, search, and create memos directly from the Hermes terminal or TUI.

---

## 📌 Overview

This skill bridges Hermes Agent with [useMemos](https://usememos.com) (self-hosted note service), letting you manage notes without leaving your chat.

| Capability | Command |
|---|---|
| 📋 List recent memos | `hermes chat -q "list recent memos"` |
| 🔍 Search memos | `hermes chat -q "search memos for <keyword>"` |
| ✍️ Create memo | `hermes chat -q "create memo: <content>"` |

---

## 🛠️ Prerequisites

- Hermes installed and running
- Self-hosted useMemos instance reachable
- `MEMOS_API_URL` — e.g. `http://<memos-host>:5230`
- `MEMOS_ACCESS_TOKEN` — token from your useMemos settings

---

## ⚡ Activation (required before any tool call works)

1. **Set credentials**  
   ```bash
   export MEMOS_API_URL="http://<memos-host>:5230"
   export MEMOS_ACCESS_TOKEN="<token>"
   ```
   Or write them to `$HERMES_HOME/usememos.json` (permissions `0o600`).

2. **Activate provider**  
   ```bash
   hermes config set memory.provider usememos
   ```

3. **Restart gateway** so the plugin loads  
   ```bash
   hermes -p <profile> gateway run --replace
   ```

4. **Confirm availability**  
   `hermes doctor` or verify `is_available()` returns true; `memos_list`, `memos_search`, `memos_add` should appear in the toolset.

---

## 📂 Repository Structure

```
├── skills/software-development/hermes-agent-skill-authoring/usememos/SKILL.md   # skill definition
├── skills/.../usememos/scripts/memos-import-markdown.py                        # bulk markdown import
└── README.md                                                                   # this file
```

---

## 📦 Bulk Import — one memo per file

Import a directory of `.md` files into useMemos, each file becoming exactly one memo.

```bash
export MEMOS_API_URL="http://<memos-host>:5230"
export MEMOS_ACCESS_TOKEN="<token>"
export MEMOS_VISIBILITY="PRIVATE"   # optional, default PRIVATE
export MEMOS_DEDUP="false"          # optional, skip duplicates

python3 scripts/memos-import-markdown.py /path/to/markdown/files
```

Output per file: `Imported`, `Skipped (empty)`, or `Skipped (duplicate)`. Ends with totals.

---

## ⚠️ Pitfalls

| Issue | Fix |
|---|---|
| HTTP 429 rate limit | Wait 1 minute, retry |
| Invalid/expired token | Refresh token in useMemos settings, update `MEMOS_ACCESS_TOKEN` |
| Instance not reachable | Check network and `MEMOS_API_URL` |

---

## ✅ Verification

- `hermes memory list` returns memos without errors
- `hermes chat -q "create memo: test"` → appears in `hermes chat -q "search memos for test"`
- Memo visibility matches your default setting

---

## 📜 License

MIT