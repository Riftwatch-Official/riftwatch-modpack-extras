# Distant Horizons (patched) 3.2.0-b-patched

An unofficial build of [Distant Horizons](https://gitlab.com/distant-horizons-team/distant-horizons) 3.2.0-b for Minecraft 1.21.1 (Fabric and NeoForge), made for the Riftwatch modpack. Distant Horizons is free software by the Distant Horizons team under the GNU Lesser General Public License version 3 (`LICENSE.LESSER.txt`, which refers to `LICENSE.txt`); its logos are by Pankakes under CC BY-SA 4.0 (`LICENSE-LOGOS.txt`). This modified version is distributed under the same licence.

## Modification Notice

Modified by Riftwatch on 2026-10-05.

| | |
|---|---|
| Original | `DistantHorizons-3.2.0-b-1.21.1-fabric-neoforge.jar` from [Modrinth](https://modrinth.com/mod/distanthorizons/version/3.2.0-b), SHA-512 `d4199f92f992fbd2c75a3b0e4e81c8a98bee889013f7347f2149ffa62c86748bde22135e9b2c82a10875db94fa576571c661c5ee16d2f567bd8a93d6f255fd22` |
| Upstream source | tag [`3.2.0b`](https://gitlab.com/distant-horizons-team/distant-horizons/-/tree/3.2.0b) (commit `eb6bf9ae78`) |
| Result | `DistantHorizons-3.2.0-b-patched-1.21.1-fabric-neoforge.jar`, SHA-512 `1b19491d01a11ea4376a5044733a256fb5da28f421c6dc0995e6a1fe14d8913b180ec48069903344205438052c65529e2d1e06f6d992469f4ffb887e54974228` |

### Why

On macOS, `GLState.saveState` crashes the game at random (SIGSEGV or SIGBUS with `0x1b02` in the fault address, often right after joining a world). It reads `GL_POLYGON_MODE` with `glGetInteger`, which reserves room for one integer, while Apple's OpenGL driver writes two (front and back mode), so the memory next to the buffer is overwritten.

### What Changed

1. `common/src/main/java/com/seibel/distanthorizons/common/render/openGl/glObject/GLState.java`, line 131 of the upstream source:

   ```
   - this.polyMode = GL33.glGetInteger(GL33.GL_POLYGON_MODE);
   + this.polyMode = GL33.GL_FILL;
   ```

   `GL_FILL` (`0x1B02`) is the only polygon mode Minecraft renders with, so the saved and restored state is the same as before on every platform. In the jar this is the bytecode of `GLState.class`: `sipush 2880; invokestatic GL33.glGetInteger(I)I` becomes `sipush 6914; nop; nop; nop` (same length, nothing else moves).
2. `META-INF/neoforge.mods.toml` and `fabric.mod.json`: the version becomes `3.2.0-b-patched` and the display name `Distant Horizons (patched)`, so the build is never mistaken for the official one.

Every other file of the jar is unchanged. The Riftwatch modpack turns the Distant Horizons auto updater off, so the patched build is not replaced by an unpatched update.

## Corresponding Source

The complete source of this build is the upstream source at tag `3.2.0b` with the one line change above. `build` reproduces the released jar from the original Modrinth file.
