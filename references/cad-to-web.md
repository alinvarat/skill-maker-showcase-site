# CAD to web (three.js GLB)

## Export
- **KiCad:** `kicad-cli pcb export glb --force --subst-models --include-tracks --include-pads --include-silkscreen --include-soldermask -o out.glb board.kicad_pcb`. Units are **metres**.
- **FreeCAD (read-only):** `freecadcmd` treats `-c` code correctly only as `freecadcmd -c "exec(open('/abs/script.py').read())"`; plain script paths can print nothing. Scripts and outputs must be under the home folder (Flatpak sandbox). Export with `MeshPart.meshFromShape(Shape=s, LinearDeflection=0.08, AngularDeflection=0.35)` per part, using `obj.getGlobalPlacement()`. For `App::Link` parts use `Part.getShape(root, 'Sub.Path.Link.', needSubElement=False, transform=True)` and keep only `s.Solids`. Write OBJ, then pack to GLB with one material per part type.
- **EasyEDA Standard:** in the user's logged-in Chrome open the project, then File → Export → **3D Model (obj)** (zip with OBJ + MTL). Units are **10 mil (0.254 mm)**, z up, board body from z = 0 to its top. Convert with `npx -y obj2gltf -i board.obj -o board.glb`.
- **EasyEDA board artwork:** File → Export → SVG, Merged layer, **Full Color**, layers TopLayer, TopSilk, BoardOutline, Multi-Layer, Hole. Recolour by `layerid` in the SVG `<style>` (1 copper → light purple, pads/11 → gold, 3 silk → white stroke with no fill, 10 outline → hidden), add a solder-mask rectangle, render with headless Chrome after setting the SVG `width`/`height` to pixels, crop to the outline, flood-fill the outside to transparent and erode the edge a few px. Lay it on the PCB top as a plane the size of the PCB body.

## Optimise
`npx -y @gltf-transform/cli@4 optimize in.glb out.glb --compress meshopt --simplify-ratio 0.3 --simplify-error 0.002 --texture-compress false --instance false --join false --palette false`
- The defaults (`instance`, `join`, `palette`) merged parts, moved instanced parts and collapsed materials into one; keep all three off.
- The page needs `MeshoptDecoder` with `GLTFLoader`.

## Fix in the page
- **Units:** scale by measured size (KiCad x1000, EasyEDA x0.254), then one world scale `k` from a known length (the 150 mm sensor bar).
- **Stray parts:** vendor models and EasyEDA exports carry parts far off the board. Use robust bounds (1st–99th percentile of vertices) and hide meshes outside them; never trust a plain `Box3`.
- **Bounds after meshopt:** geometry is quantised with a node transform, so `geometry.boundingBox` is not in model units. Use `new Box3().setFromObject(mesh)` in a known frame.
- **Centre on the PCB body,** not on all parts (terminals overhang).
- **Invisible faces:** EasyEDA winds some faces backwards; set `side: DoubleSide` on its materials.
- **Black blocks/NaN:** recompute normals and replace zero-length normals.
- **Camera clipping:** for models in mm set `camera.near = size/1000`, `camera.far = size*20`.
- **Colours:** use the real board colour (PULSE boards are purple), white lights for product shots, no tinted rim light on product cards.
