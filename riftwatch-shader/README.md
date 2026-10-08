# Riftwatch Shader 3.4.1a

The default shader pack of the Riftwatch modpack. It is the shader pack [Mellow](https://modrinth.com/shader/mellow) v3.4.1a by TheCMK ([source on Codeberg](https://codeberg.org/TheCMK/mellow-shader)), redistributed under its original licence (`LICENSE`, MIT, Copyright (c) 2026 TheCMK; some files under `LICENSE-APACHE`, Apache-2.0). All credit for the shader goes to TheCMK. Following the author's wish, this build does not use the Mellow name or its assets.

## Modification Notice

Modified by Riftwatch on 2026-10-06.

| | |
|---|---|
| Original | `Mellow v3.4.1a.zip` from [Modrinth](https://modrinth.com/shader/mellow), SHA-512 `d9ded566950e9f67de1084c5df748e2bd37bcc15234f4f9b6f026faf9ee2eef7f84c09fc9de1ac25107d900051d555490a4c3d6888b82afbc4888d53610e8387` |
| Result | `Riftwatch-Shader-3.4.1a.zip` (installed as `Riftwatch Shader 3.4.1a.zip`) |

Changes:

1. Every `profile.FAST=` line in the `.properties` and `.lang` files under `shaders/` is removed, so the Fast profile is gone. Its settings let players see through Distant Horizons terrain on the Riftwatch server.
2. `RIFTWATCH.txt` is added with the credit and this notice.
3. The zip is rebuilt with sorted entries and a fixed date, so `build` reproduces it byte for byte.

Nothing else is changed. `LICENSE` and `LICENSE-APACHE` stay intact inside the zip.
