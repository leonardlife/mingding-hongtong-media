# 命定红瞳 · 画廊资源

本目录存放画廊 CG 图和画廊清单 `gallery-manifest.json`。
角色卡与画廊外接组件通过 HTTPS 远程读取这里的文件，所以必须保持公开可读。

## 现状

- `assets/`：29 张 CG 图，合计约 34.3 MB。
- `gallery-manifest.json`：29 条，`version: 0.9.4`，`updatedAt: 2026-09-22`，约 21.5 KB。
- 每条的 `imageUrl` 和 `thumbUrl` 当前指向同一张图。

## 清单格式

顶层字段：

```json
{
  "schemaVersion": 1,
  "galleryId": "mingding-hongtong-campus",
  "version": "0.9.4",
  "updatedAt": "2026-09-22",
  "entries": []
}
```

`updatedAt` 只是备注，程序不读。

条目字段：

```json
{
  "id": "cg-xxx",
  "title": "CG 名称",
  "subtitle": "副标题",
  "description": "说明文字",
  "imageUrl": "assets/cg_xxx.jpg",
  "thumbUrl": "assets/cg_xxx.jpg",
  "rarity": "珍贵",
  "chapter": "章节名",
  "tags": ["标签一", "标签二"],
  "enabled": true,
  "provenance": {
    "sourceType": "user-provided-test-asset",
    "originalFilename": "原文件名.jpg",
    "license": "仅作当前项目测试"
  }
}
```

## 必须遵守的几条

- `schemaVersion` 必须是数字 `1`。写成字符串 `"1"` 会被拒绝，报「清单格式不受支持」。
- `id` 长度 3~80，首字符必须是字母或数字，只能用字母、数字、`.`、`_`、`-`，不能有空格和中文。
- `id` 是存档的关联键，**发布之后不能改**。改了等于换了一张新图，用户已收藏的记录会对不上。
- `rarity` 只写 `传说` / `秘藏` / `珍贵` / `稀有` 之一。
  这是**包含匹配**：不含这四个词的一律**静默降级**成最低档「记忆残章」，不报错也不警告。
  - 实例：`cg-red-eyes-crows`（赤月鸦影）原本写的是 `珍藏`，「珍藏」不含「珍贵」，
    所以一直被降级成记忆残章；2026-09-22 已改成 `珍贵`。
- `tags` 最多 8 个，每个最多 24 字符。
- 图片地址必须能解析成 **HTTPS**。
- 相对路径以「清单自身位置」为基准拼接。`assets/cg_xxx.jpg` 会拼成
  `.../assets/gallery/assets/cg_xxx.jpg`，多一层 `assets/`。
- 整个清单不超过 **1 MB**，条目不超过 **500** 条。

## 当前稀有度分布

传说 5 / 珍贵 16 / 秘藏 6 / 稀有 2，共 29 条。

## 加新图的流程

1. 压缩图片，文件名用 `cg_` 开头的小写下划线名，放进 `assets/`。
2. 在 `entries` 里加一条，`id` 用 `cg-` 开头的短横线名。
3. 自检：`id` 唯一且合法、`imageUrl` 指向的文件真实存在、`rarity` 只用四档之一、`tags` 不超限。
4. 提交推送后，用浏览器直接打开清单地址，确认能读到 JSON。

更完整的解析规则、报错文案对照和上线检查清单，见项目工作区的 `画廊清单规范.md`。