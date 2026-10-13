# Riftwatch Modpack Extras

Modified third party files shipped in the [Riftwatch](https://riftwatch.net) modpack, published here with their licences, the exact changes and reproducible build scripts. The modpack downloads them from the releases of this repository.

| Folder | File | Based on | Licence | Release |
|---|---|---|---|---|
| `distant-horizons/` | `DistantHorizons-3.2.0-b-patched-1.21.1-fabric-neoforge.jar` (used by the modpack) and `DistantHorizons-3.3.3-patched-1.21.1-fabric-neoforge.jar` (not used: renders nothing on macOS) | [Distant Horizons](https://gitlab.com/distant-horizons-team/distant-horizons) 3.2.0-b and 3.3.3 by the Distant Horizons team | LGPL-3.0 (logos CC BY-SA 4.0) | `distant-horizons-3.2.0-b-patched`, `distant-horizons-3.3.3-patched` |
| `riftwatch-shader/` | `Riftwatch-Shader-3.4.1a.zip` | [Mellow](https://modrinth.com/shader/mellow) v3.4.1a by TheCMK | MIT (parts Apache-2.0) | `riftwatch-shader-3.4.1a` |

These are unofficial builds. Please do not report problems with them to the original authors; open an issue here or ask in the Riftwatch Discord.

## Build

Each folder has a `build` script (Python 3, no dependencies). It downloads the original file from Modrinth, checks its SHA-512, applies the documented change and writes the result to `output/`. The output is byte for byte reproducible and matches the release asset.

```
python3 distant-horizons/build
python3 riftwatch-shader/build
```
