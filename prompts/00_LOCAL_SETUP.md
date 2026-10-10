# Prompt 0 — Local workspace setup (run once, before 01_KICKOFF)

Open Claude Code on your own computer, in an empty folder where the game will live
(e.g. `Documents/Riftbound`), and paste everything between the lines.

---

<task>
Set up this folder as the workspace for a new Roblox game, RIFTBOUND: MULTIVERSE DEFENSE.
This session is setup only: install and verify tools, create the folder structure, and put
the planning documents in place. Do not write game code, design systems, or plan features —
a later session does that.
</task>

<steps>
1. Check my machine: OS, whether Git is installed, whether Roblox Studio is installed. Tell
   me about anything missing and how to install it; don't install system-wide software
   without asking me first.
2. Get the planning documents: clone https://github.com/reldeagle/Roblox-Game-Prompt
   (branch `claude/riftbound-multiverse-defense-h5el07`) into a temporary folder, then copy:
   - `builder/CLAUDE.md` → `./CLAUDE.md`
   - `builder/SPEC.md` and `builder/QUALITY_GATES.md` → `./docs/`
   - the `prompts/` folder → `./prompts/`
   Then delete the temporary clone. If the clone fails (it's a private repo), ask me to
   paste or download the files instead.
3. Install the Roblox toolchain per-project with Rokit (install Rokit if missing, after
   asking me): Rojo, Wally, StyLua, Selene, and Lune. Pin versions in `rokit.toml`.
   Verify each one runs by printing its version.
4. Create an empty folder skeleton only (with `.gitkeep` files): `src/client`, `src/server`,
   `src/shared`, `tests`, `scripts`, `assets`, `docs/reference`, `docs/screens`.
   Add a `.gitignore` suitable for Rojo/Wally projects.
5. Run `git init`, then make one commit: "Workspace setup".
6. Check whether the Roblox Studio MCP server is available to you. If it isn't, give me
   short step-by-step instructions to connect it to Claude Code (check current Roblox
   documentation if you have web access) and to install the Rojo plugin in Studio. Don't
   claim it's connected unless you've actually called one of its tools.
</steps>

<report>
Finish with a short checklist: each tool with its version (or "missing"), whether the files
are in place, whether Studio MCP works, and the exact things I still need to do myself.
Then tell me the next step is to fill in and send prompts/01_KICKOFF.md in a fresh session.
</report>

---
