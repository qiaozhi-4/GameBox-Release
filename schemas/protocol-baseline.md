# GameBox 发布协议基线

状态：统一整包发布策略已生效（2026-09-04）

## 统一版本与 Release

- App 的 `versionCode`/`versionName` 是整包唯一版本源。
- 每次发布使用一个统一版本和一个统一 Tag：`v<versionName>`。
- 一个 Release 必须同时包含 App、core 以及本次声明的全部游戏 so 资产。
- App、core 和游戏 so 不再维护独立版本号、独立 Tag 或独立 Release。
- 同一统一版本的历史 JSON、Markdown 和 Release 资产不可覆盖；需要重新构建时必须递增 App 版本。

## App

- `componentId`：`gamebox-app`
- 当前源码版本基线：`versionCode=37`、`versionName=1.2.28`
- App 资产文件名必须包含版本和 ABI，例如：`GameBox-v1.2.29-arm64-v8a.apk`

## 当前 core

- `soId`：`core`
- CMake target：`xmg_core`
- 构建输出文件名：`libxmg_core.so`
- 发布资产文件名：`libxmg_core-v<versionName>-<abi>.so`
- `packaging`：`separate-core`
- core 使用整包的 `versionCode`、`versionName` 和 `releaseTag`，不维护自身版本计数器。
- 当前 App 兼容基线：`minAppVersionCode=37`
- `supportedAbis`：当前仅 `arm64-v8a`
- core 制品自身的 `requiresCore`：`false`
- core 制品自身的 `dependencies`：`[]`

`minAppVersionCode=37` 是当前源码基线的兼容声明；只有在录入真实制品时，确认构建和运行契约
仍兼容 App 37 后，才能用于该制品的正式 JSON。

## 未来游戏 so

- 每个游戏使用独立、稳定的小写连字符 `soId`，不得使用展示名称作为 ID。
- `self-contained-game` 必须使用 `requiresCore=false` 和 `dependencies=[]`。
- 仍需独立 core 的游戏必须使用 `separate-core`，并在 `dependencies` 中显式声明：
  `{ "soId": "core", "minVersionCode": 37 }`。
- 游戏使用整包的 `versionCode`、`versionName` 和 `releaseTag`，不能单独更新。

## 通用约束

- 所有资产文件名必须包含制品 ID、统一版本和 ABI；摘要必须是 64 位小写 SHA-256。
- 当前 ABI 发布范围为 `arm64-v8a`；schema 已预留 `armeabi-v7a`、`x86` 和 `x86_64`。
- 只有在全部真实资产、摘要和 Markdown 校验通过后，才能更新各制品的 `latest.json`。
- 统一版本的任一资产发生变化，都必须递增 App 版本，不得静默覆盖已发布版本。
