# GameBox 发布协议基线

状态：P0.3 已冻结（2026-09-04）

## App

- `componentId`：`gamebox-app`
- 当前源码版本基线：`versionCode=37`、`versionName=1.2.28`
- App tag：`app-v<versionName>`
- App 资产文件名必须包含版本和 ABI，例如：`GameBox-v1.2.29-arm64-v8a.apk`

## 当前 core

- `soId`：`core`
- CMake target：`xmg_core`
- 构建输出文件名：`libxmg_core.so`
- 发布资产文件名：`libxmg_core-v<versionName>-<abi>.so`
- `packaging`：`separate-core`
- core 首版协议版本：`versionCode=1`、`versionName=1.0.0`
- 当前 App 兼容基线：`minAppVersionCode=37`
- `supportedAbis`：当前仅 `arm64-v8a`
- core 制品自身的 `requiresCore`：`false`
- core 制品自身的 `dependencies`：`[]`
- core tag：`so-core-v<versionName>`

`minAppVersionCode=37` 是当前源码基线的兼容声明；只有在 P0.4 录入真实制品时，确认构建和
运行契约仍兼容 App 37 后，才能用于该制品的正式 JSON。

## 未来游戏 so

- 每个游戏使用独立、稳定的小写连字符 `soId`，不得使用展示名称作为 ID。
- `self-contained-game` 必须使用 `requiresCore=false` 和 `dependencies=[]`。
- 仍需独立 core 的游戏必须使用 `separate-core`，并在 `dependencies` 中显式声明：
  `{ "soId": "core", "minVersionCode": 1 }`。
- 游戏 tag：`so-<so-id>-v<versionName>`。

## 通用约束

- App、core 和每个游戏 so 使用独立的 `versionCode`/`versionName` 计数器，版本号单调递增。
- 同一组件同一版本号禁止发布不同 SHA-256 的资产，也禁止覆盖旧 tag 或旧资产。
- 所有资产文件名必须包含制品 ID、版本和 ABI；摘要必须是 64 位小写 SHA-256。
- 当前 ABI 发布范围为 `arm64-v8a`；schema 已预留 `armeabi-v7a`、`x86` 和 `x86_64`。
- 在 P0.4/P1 取得真实资产、大小和摘要前，不创建 `app/latest.json` 或 `so/core/latest.json`。
