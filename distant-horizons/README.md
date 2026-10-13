# Distant Horizons (patched) 3.3.3-patched

An unofficial build of [Distant Horizons](https://gitlab.com/distant-horizons-team/distant-horizons) 3.3.3 for Minecraft 1.21.1 (Fabric and NeoForge), made for the Riftwatch modpack. Distant Horizons is free software by the Distant Horizons team under the GNU Lesser General Public License version 3 (`LICENSE.LESSER.txt`, which refers to `LICENSE.txt`); its logos are by Pankakes under CC BY-SA 4.0 (`LICENSE-LOGOS.txt`). This modified version is distributed under the same licence.

## Modification Notice

Modified by Riftwatch on 2026-10-13.

| | |
|---|---|
| Original | `DistantHorizons-3.3.3-1.21.1-fabric-neoforge.jar` from [Modrinth](https://modrinth.com/mod/distanthorizons/version/3.3.3-1.21.1), SHA-512 `d14b32d3fd143a06514d6ad388eee11e9d657c84e1a937aaf8d033b1137fb43ef8b05cff1011a994cc42f3106823c51234f8bc18d72bb05bc317a20ea83c50ed` |
| Upstream source | tag [`3.3.3`](https://gitlab.com/distant-horizons-team/distant-horizons/-/tree/3.3.3) (commit `a54dd3eb`) |
| Result | `DistantHorizons-3.3.3-patched-1.21.1-fabric-neoforge.jar`, SHA-512 `fb2f61a1ec1bd5e99d114805d9406c6eb2313aaec769be61edd237842cf28ee946954c9b70c768c5803f1fe045ec1fbe3481976fe0d327efee432f3f501d227e` |

### Why

On macOS, `GLState.saveState` crashes the game at random (SIGSEGV or SIGBUS with `0x1b02` in the fault address, often right after joining a world). It reads `GL_POLYGON_MODE` with `glGetInteger`, which reserves room for one integer, while Apple's OpenGL driver writes two (front and back mode), so the memory next to the buffer is overwritten. Every other `glGetInteger` call of this version reads a single value.

On a NeoForge server with Chunky, 3.3.3 crashes during the start: when the spawn chunks load, its new Chunky integration calls `ChunkyProvider.get()` before Chunky has started, which throws `IllegalStateException: Chunky is not loaded` inside the chunk load and stops the server. Upstream changed this after 3.3.3 (commit `bc691d7d`, "Improve chunky API handling"), which is not released for Minecraft 1.21.1 yet.

### What Changed

1. `common/src/main/java/com/seibel/distanthorizons/common/render/openGl/glObject/GLState.java`, line 133 of the upstream source:

   ```
   - this.polyMode = LWJGL.glGetInteger(GL11.GL_POLYGON_MODE);
   + this.polyMode = GL11.GL_FILL;
   ```

   `GL_FILL` (`0x1B02`) is the only polygon mode Minecraft renders with, so the saved and restored state is the same as before on every platform. In the jar this is the bytecode of `GLState.class`: `getstatic LWJGLServiceProvider.LWJGL; sipush 2880; invokeinterface ILWJGLService.glGetInteger(I)I` becomes `sipush 6914` and eight `nop` (same length, nothing else moves, the class passes the bytecode verifier).
2. `neoforge/src/main/java/com/seibel/distanthorizons/neoforge/wrappers/modAccessor/ChunkyAccessor.java`, the body of `bindOnGenerationProgressEvent`:

   ```
   - ChunkyProvider.get().getApi().onGenerationProgress((event) -> this.onGenEvent());
   + (empty)
   ```

   Distant Horizons no longer listens to Chunky's generation progress on NeoForge, so it does not pause its own world generation while a Chunky task runs, exactly as before 3.3.3. In the jar the 17 bytes of the call chain in `ChunkyAccessor.class` become `nop` before the unchanged `return` (the method has no branches and no stack map table). The Fabric class is unchanged.
3. `META-INF/neoforge.mods.toml` and `fabric.mod.json`: the version becomes `3.3.3-patched` and the display name `Distant Horizons (patched)`, so the build is never mistaken for the official one.

Every other file of the jar is unchanged. The Riftwatch modpack turns the Distant Horizons auto updater off, so the patched build is not replaced by an unpatched update.

## Corresponding Source

The complete source of this build is the upstream source at tag `3.3.3` with the two changes above. `build` reproduces the released jar from the original Modrinth file.

## Earlier Builds

`3.2.0-b-patched` (release `distant-horizons-3.2.0-b-patched`, upstream tag `3.2.0b`, the same change at line 131 with `GL33.glGetInteger(GL33.GL_POLYGON_MODE)`) stays available for older modpack versions.
