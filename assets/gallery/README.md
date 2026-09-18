# 命定红瞳 · 画廊系统初版

这是独立于 MVU 状态栏的画廊原型。界面采用古希腊、古罗马神殿回廊风格，支持从远端清单随机抽取 CG，并在浏览器中保存已收藏记录。

## 当前文件

- `画廊系统初版.html`：可直接打开的单文件原型。
- `gallery-manifest.json`：本地示范清单；正式使用时可部署到 GitHub Pages、GitHub Raw 或 jsDelivr。
- `assets` 目录中的测试图只用于开发和未来上传仓库，不会写入角色卡。正式运行时，画廊从用户配置的 HTTPS manifest 读取图片地址；未配置清单时显示明确空状态，不再假装存在“卡内演示图”。

本次测试素材：`红瞳1.png` 已压缩为 `assets/cg_red_eyes_initial.jpg`，绑定画廊条目“初识”；`红瞳2.jpg` 已压缩为 `assets/cg_red_eyes_sunset.jpg`，绑定画廊条目“落日”。原图仍保留在用户桌面目录，项目只保存测试副本。

## 视觉说明

- 两侧柱身使用精确重复 24 次的凹槽纹理。
- 柱头由顶板、颈饰和一对向下延伸的涡卷组成，参考用户提供的古典柱式线稿。
- 柱廊正下方使用鎏金古希腊语铭文，替代原有红白交织装饰；铭文保留足够对比度和移动端换行。
- 抽取动画只影响中央提示和 CG 揭幕，柱子始终保持静止。

## 资源清单约定

远端 JSON 顶层包含 `schemaVersion`、`galleryId`、`version` 和 `entries`。每个条目至少需要：

```json
{
  "id": "稳定且唯一的-cg-id",
  "title": "CG 名称",
  "imageUrl": "https://.../full.jpg",
  "thumbUrl": "https://.../thumb.webp",
  "enabled": true
}
```

`description`、`subtitle`、`rarity`、`chapter`、`tags` 和 `provenance` 可以省略。正式线上图片与清单必须使用 HTTPS；清单服务器还需允许浏览器跨域读取。

## 抽取与保存规则

1. 优先从尚未收藏的启用 CG 中随机抽取。
2. 全部收藏后才会从完整池中重复抽取。
3. 图片成功加载后才写入收藏；某个候选链接失效时会继续尝试本次候选池中的其他图片，全部失败才结束本次抽取。
4. 浏览器只保存 CG ID、解锁时间、抽取次数和一份最小展示快照，不保存图片二进制。
5. 原型使用 `localStorage`；正式接入角色卡时，由 MVU 的 `画廊` 字段作为剧情存档，界面本身的滚动位置等仍留在本地。

## 接入接口

页面公开 `window.MingdingHongtongGallery`：

- `draw()`：抽取一张 CG。
- `loadManifest(url)`：读取新的 HTTPS 清单。
- `registerAcquired(ids)`：把一组 CG ID 登记为已收藏。
- `update(galleryState)`：读取 MVU 风格对象；识别 `状态: 已解锁` 与 `图片ID`。
- `getState()`：获取当前原型状态副本。

首次抽到新 CG 时派发 `mingding-hongtong:gallery-unlocked`，其中包含 `galleryId` 和建议写入 `stat_data.画廊` 的 `requestedRecord`。界面仍保留本机收藏用于演示，但不会直接改写 MVU。

通过 HTTP(S) 打开开发原型时，可以读取同目录 `gallery-manifest.json`。封装进角色卡后不携带图片，也不自动假定相对目录存在；需要在“资源清单”中填写已部署的 HTTPS manifest。

正式 GitHub 仓库地址尚未确定，因此初版没有虚构默认线上地址。打开右上角“资源清单”即可填入真实 HTTPS manifest。
