---
name: maker-showcase-site
description: Build or fix the user's personal/PULSE showcase website (alinvarat.github.io) or GitHub profile, including 3D robots and boards made from the user's real FreeCAD, KiCad or EasyEDA files. Use for bio/link pages, portfolio pages, 3D product scenes and publishing work to GitHub.
---

# Maker showcase site

The user is the founder of PULSE (line-follower robot boards, sensors, kits). Every visible fact, part and photo must come from the user's real files or words.

## Hard rules
1. **Real hardware only.** Build every robot part from a design file or a measured drawing. Never invent a part, size, connector or wiring. When a needed fact is missing, ask one short question instead of guessing.
2. **Released products only.** Never put an unreleased board on a public page. Design-project names are not product names: the EasyEDA project `PulseMiniV2.3` is the sold **PulseMini V2.2**. Confirm the version when names disagree.
3. **"ref" means reference.** Photos the user labels ref are for matching shape and colour; never publish them.
4. **Customer libraries stay binary-only.** Never publish Pulse library source or the internals of customer manuals.
5. **English on public pages and repos.** Keep a Thai copy only when the user asks.
6. **Show the person as founder of PULSE**, not "a student who runs it on the side".
7. **Verify by rendering after every change** (see step 5). Never report "fixed" from code alone.

## Workflow
1. **Find the real files by content, not by file name.** Read [design-file map](references/design-files.md). Search inside assemblies (for example `Pulse.FCStd` holds the V2.1 board group `EasyEDA_PCB_Model_0FP6`). When the user has the board open in EasyEDA, export from that exact project.
2. **Export and convert** with [CAD to web](references/cad-to-web.md). Keep materials separate; fix units, orientation, stray parts and face winding there.
3. **Assemble the robot from measurements**: plate drawing holes, rail length, standoff holes, connector positions. Write each number and its source as a code comment. Check fit with top and underside debug views.
4. **Work on the deploy copy.** If the project folder is kept in sync by a backup tool, **never stop, pause, filter or reconfigure it.** An edit made during a sync run can be rolled back, so work around it instead. Edit the git clone used for deployment, push it, then copy the files back into the project folder. Before editing a local file, check it still contains your latest change (`grep` for a marker); if not, restore it from the deploy copy.
5. **Render and look.** Use headless Chrome with the GPU and the debug views in [render checks](references/render-checks.md). Check desktop and 390 px mobile, the hero, the chase view and every product card.
6. **Publish** with [GitHub publishing](references/github-publish.md), then confirm the live URL serves the new code (`curl … | grep <marker>`).

## Report
Three short lines in Thai: what changed, what was verified (with the view or URL), what is still unknown or needs the user.
