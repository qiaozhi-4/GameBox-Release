# GameBox-Release 协议骨架

当前仓库处于 `bootstrap` 阶段。`catalog.json` 和 `so/games/index.json` 已建立协议入口，
但在真实 APK/so 经过构建、摘要和 ABI 校验并发布前，不创建可被客户端消费的 `latest.json`。

## JSON 文件

- `catalog.json`：公开仓库、稳定入口和支持 ABI 的目录清单。
- `schemas/catalog.schema.json`：目录清单 schema。
- `schemas/app-update.schema.json`：App 更新 JSON schema。
- `schemas/so-update.schema.json`：Native so 更新 JSON schema。
- `schemas/protocol-baseline.md`：P0.3 冻结的制品 ID、版本、ABI、依赖和 tag 规则。
- `so/core/baseline.json`：P0.4 记录的当前 core 本地构建基线；它不是运行时更新入口。
- `so/games/index.json`：未来游戏 so 的发现入口；每项至少需要 `soId`、`displayName` 和 `latestUrl`。

所有发布 JSON 使用 `schemaVersion: 1` 和明确的 `kind`。未知字段默认禁止。

## 稳定与不可变路径

- `app/latest.json` 和每个 so 目录下的 `latest.json` 是可移动的稳定指针。
- 历史 App JSON：`app/versions/v<version-name>.json`。
- 历史 core JSON：`so/core/versions/v<version-name>.json`。
- 历史游戏 JSON：`so/games/<so-id>/versions/v<version-name>.json`。
- App 更新内容：`app/changelog/v<version-name>.md`。
- core 更新内容：`so/core/changelog/v<version-name>.md`。
- 游戏更新内容：`so/games/<so-id>/changelog/v<version-name>.md`。

历史版本路径、Release tag 和 Release 资产不得覆盖。`latest.json` 只有在对应资产和 Markdown
均存在且摘要校验通过后才能更新。

`so/core/baseline.json` 的 `observed-unpublished` 状态只用于审计当前构建输出，不提供下载地址，
也不能被客户端当作 `so-update` 使用。

## Markdown 规则

Markdown 使用 UTF-8 和 LF 换行。更新说明至少包含“更新内容”和“兼容性”两个小节；内容必须
人工提供，不能从提交标题自动拼接成长篇说明。
