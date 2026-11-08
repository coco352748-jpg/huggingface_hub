---
name: activate
description: Activate Copilot Browser in the current session, upgrade its registration, or repair incomplete setup.
---

Perform a controlled activation of this Browser Use plugin installation.

Session start normally performs this activation automatically. Use this skill to enable Browser
immediately in the current session, upgrade the active registration, repair incomplete or
mismatched setup, or when the user explicitly requests activation.

Resolve `../../scripts/activate.mjs` relative to this skill's directory and run exactly:

```text
node <absolute-path-to-activate.mjs>
```

Wait for the activation process to finish before using Browser tools.

The script checks connectivity itself for up to 15 seconds and returns early when a browser
connects. Report the activation result without adding your own sleep or connectivity polling.
Use `/browser-use:doctor` if Browser has connection problems.

If activation fails, report the failure without retrying with modified arguments.