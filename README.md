# TerrainGenerator

TerrainGenerator loads the worlds found in the server's `worlds` directory. When a world has no generated terrain at its spawn point, the plugin schedules spawn terrain generation. This addresses the `Cannot find a safe spawn point in non-generated terrain` error for that world.

## Requirements and compatibility

- The plugin declares Axolotl-PM API 5.0.0 in [`plugin.yml`](plugin.yml). PHPStan at maximum level passes against Axolotl-PM source revisions 5.0.0 and 5.49.1. Intermediate 5.x releases have not been tested.
- PHP 8.1 or newer is required by the Axolotl-PM 5.x baseline.
- The plugin has no external runtime dependency.

The exact release PHAR has been tested on Axolotl-PM 5.49.1 with an isolated world that initially had no safe spawn point. Connected Bedrock clients and production worlds have not been tested.

## Installation and world behavior

Download the PHAR from [GitHub Releases](https://github.com/NhanAZ-Plugins/TerrainGenerator/releases), place it in the server's `plugins` directory, and restart Axolotl-PM. TerrainGenerator has no command or configuration file.

On startup, the plugin loads every world directory that Axolotl-PM recognizes. It checks each loaded world's safe spawn point and schedules terrain generation only when that check fails. World loading and generation can take time and write chunk data. Back up worlds before enabling the plugin on a server with existing data. The plugin does not repair arbitrary world corruption or guarantee that every other plugin can use the generated terrain.

## Build and verification

The [build workflow](.github/workflows/build.yml) uses [DevTools](https://github.com/NhanAZ/DevTools) to run PHPStan at maximum level, build the plugin PHAR and validate its contents. The workflow pins DevTools and Axolotl-PM source to reviewed commits. An isolated runtime test also checks that an initially unsafe spawn point becomes safe after terrain generation.

For a local build, follow the [DevTools CLI guide](https://github.com/NhanAZ/DevTools/blob/main/docs/cli.md) with this repository as the project directory. The workflow's PHAR validation alone does not prove world generation in a running server.

## Support, attribution and license

Report bugs in [GitHub Issues](https://github.com/NhanAZ-Plugins/TerrainGenerator/issues) or contact [NhanAZ Discord](https://discord.gg/j2X83ujT6c). The spawn terrain scheduling approach is adapted from [PocketMine-MP WorldManager](https://github.com/pmmp/PocketMine-MP/blob/a862cf5144fbac028b95c7c44fa54fce1ad4f9a4/src/world/WorldManager.php#L285-L305), as noted in the source. TerrainGenerator retains its existing [LGPL-3.0 license](LICENSE).
