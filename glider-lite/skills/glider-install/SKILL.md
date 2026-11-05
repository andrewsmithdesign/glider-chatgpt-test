---
name: glider-install
description: Use when the user asks to install, update, list, check the version of, or remove a company Skill or Glider Skill.
---

You manage the user's company Skills. They live in one private plugin called `glider-skills`, which you create and update with Plugin Creator's tools.

Catalog (stand-in for Glider's `list` tool):
- pirate-greeting, released version 1.1.0. Packages:
  - 1.0.0: https://raw.githubusercontent.com/andrewsmithdesign/glider-chatgpt-test/main/packages/pirate-greeting-1.0.0.zip
  - 1.1.0: https://raw.githubusercontent.com/andrewsmithdesign/glider-chatgpt-test/main/packages/pirate-greeting-1.1.0.zip

Every installed Skill's description ends with a stamp: `(glider <name> v<version> plugin:<id>)`. The `plugin:` part is the backend ID of `glider-skills`.

Install or update `<name>` (released version unless the user names one):
1. Download the package URL with a plain HTTP fetch. Do not rebuild it from memory. If you cannot fetch it, say so and stop.
2. Unzip it. It contains `<name>/SKILL.md`.
3. Find the `glider-skills` plugin ID: read the `plugin:` value from the stamp of any installed Glider Skill. If none is installed, there is no plugin yet.
4. In the downloaded `SKILL.md`, replace `PLUGIN_ID` in the stamp with the real ID (after step 5 on first install).
5. No plugin yet: build a plugin archive with a root `plugin.json` named `glider-skills` and `skills/<name>/SKILL.md`, call `create_plugin`, then put the returned ID into the stamp and call `update_plugin` once so the stamp is correct. Tell the user to click + on glider-skills in Settings → Plugins → Personal, once.
6. Plugin exists: call `get_plugin_files` for its current release ID, then `update_plugin` with `skills/<name>/SKILL.md`. No click needed.
7. Reply in one line: "Installed <name> v<version>." Never claim it is active in this conversation.

Status ("which version of X do I have?"): read the stamp from your skill list. Do not call any tool.

Remove `<name>`: `update_plugin` with `delete_paths: ["skills/<name>"]`.
