# mingding-hongtong-media

《命定红瞳》（尘世命轨）的媒体资源仓库。这里只放图片和清单，不放角色卡本体。

角色卡和画廊外接组件通过 HTTPS 地址远程读取本仓库的清单与图片，所以仓库必须保持公开可读。

## 目录结构

| 路径 | 内容 | 现状 |
| --- | --- | --- |
| `assets/gallery/` | 画廊 CG 图与画廊清单 | 29 张图，清单 29 条，`version: 0.9.4` |
| `assets/achievements/` | 成就图与成就清单 | 10 张图，清单 10 条，`version: 0.6.1` |
| `assets/music/` | 音乐、音频素材 | 目前只有 `zanwe.docx`，是 0 字节空文件 |

## 线上地址

画廊清单（角色卡默认读取的就是这个）：

```
https://raw.githubusercontent.com/leonardlife/mingding-hongtong-media/main/assets/gallery/gallery-manifest.json
```

成就清单：

```
https://raw.githubusercontent.com/leonardlife/mingding-hongtong-media/main/assets/achievements/manifest.json
```

清单里的图片地址写的是相对路径，基准是「清单文件自己的位置」。以画廊为例，
`assets/cg_xxx.jpg` 实际会拼成：

```
https://raw.githubusercontent.com/leonardlife/mingding-hongtong-media/main/assets/gallery/assets/cg_xxx.jpg
```

注意末尾多了一层 `assets/`，这是最容易搞错的地方。

## 加新图的标准流程

1. 把图片压缩到合理体积，放进对应目录（画廊放 `assets/gallery/assets/`）。
2. 在对应清单的 `entries` 里加一条记录。
3. 按 `assets/gallery/README.md` 里的规则逐条自检。
4. 提交并推送。
5. 用浏览器直接打开清单地址，确认能读到 JSON。

## 几条硬性限制

- 清单文件不能超过 **1 MB**。
- `schemaVersion` 必须是数字 `1`，写成字符串 `"1"` 会被拒绝。
- 图片地址必须能解析成 **HTTPS**。
- 单个清单最多 **500** 条。

更完整的解析规则、报错文案对照和上线检查清单，见 `assets/gallery/README.md`。

## 素材来源

仓库里的图片目前都是测试素材，`provenance` 里标注为「仅作当前项目测试」。
正式发布前需要逐条补上真实来源与授权信息。