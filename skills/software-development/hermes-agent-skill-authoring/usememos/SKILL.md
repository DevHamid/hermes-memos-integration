---
name: usememos-skill
description: Integrate Hermes with Memos memory via usememos plugin.
version: 0.1.0
author: Ahmad Syarwan Hamid (ahmadsh), Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [usememos, memory, integration]
    related_skills: [hermes-agent]
---
# usememos-skill

## Overview

This skill enables Hermes to interact with the Memos self-hosted note service, allowing you to list, search, and create memos directly from the Hermes terminal or TUI.

## When to Use

- You want to store and retrieve notes programmatically from Hermes
- You need to reference memos in your workflows or agent tasks
- You have a running Memos instance with valid API credentials

**Don't use for:**
- Storing large binary files
- Real-time collaborative editing
- Persistent system state (use dedicated databases)

## Prerequisites

- Hermes installed and running
- Memos instance running (self-hosted or cloud)
- API URL and access token for Memos
- Environment variables set: `MEMOS_API_URL`, `MEMOS_ACCESS_TOKEN`
- Optionally create `usememos.json` in `$HERMES_HOME` for config

## How to Run

```bash
hermes chat -q "list recent memos"
hermes chat -q "search memos for test"
hermes chat -q "create memo: 'Task completed'"
```

## Quick Reference

- `hermes chat -q "list"` — list recent memos
- `hermes chat -q "search <keyword>"` — search memos by keyword
- `hermes chat -q "create memo: <content>"` — create a new memo

## Procedure

1. **Verify Memos is configured**  
   Run `hermes memory list` — if Memos is not available, you will see an error.  
   Ensure `MEMOS_API_URL` and `MEMOS_ACCESS_TOKEN` are set.

2. **List recent memos**  
   Run `hermes chat -q "list recent memos"` — should return a JSON list of memos with content, creation time, and visibility.

3. **Search memos**  
   Run `hermes chat -q "search memos for project"` — should return matching memos containing "project" in their content.

4. **Create a new memo**  
   Run `hermes chat -q "create memo: 'Task completed successfully'"` — should store the memo and return a success message with the memo ID.

5. **Verify memo creation**  
   Run `hermes chat -q "search memos for 'Task completed'"` — should find the memo you just created.

## Pitfalls

- Memos API rate limiting — if you get HTTP 429 errors, wait 1 minute before retrying.
- Invalid or expired API token — refresh the token in Memos settings and update `MEMOS_ACCESS_TOKEN`.
- Memos instance not reachable — ensure network connectivity and correct API URL.

## Verification

- Confirm `hermes memory list` returns memos without errors.
- Check that `hermes chat -q "create memo: test"` results in a new entry in `hermes chat -q "search memos for test"`.
- Ensure the memo's visibility matches the default setting.

**Note:** The Memos plugin uses standard HTTP calls and does not require additional dependencies beyond Hermes.