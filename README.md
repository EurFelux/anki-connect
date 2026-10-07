# AnkiConnect (patched)

> **Note**: This is an unofficial mirror of [AnkiConnect](https://git.sr.ht/~foosoft/anki-connect) with local compatibility patches for newer Anki versions (25.x).
> The upstream author (Alex Yatskov / FooSoft) has moved development to SourceHut; the old [GitHub repo](https://github.com/FooSoft/anki-connect) is archived.
> All credit for the original code goes to the upstream author. This repo exists so the patched version can be installed on multiple devices.

AnkiConnect 是一个 Anki 插件，在本地 `127.0.0.1:8765` 暴露 HTTP API，允许外部程序通过 JSON 请求操作 Anki（加卡、查牌组、读写笔记等）。

## Local patches（本地补丁）

本仓库在上游版本基础上包含以下补丁（代码中以 `local fix` 注释标记）：

1. **`__init__.py` — `addNote()` 修复**：新版 Anki 的 `addNote()` 会忽略 `createNote()` 中设置的未保存 notetype 的 `did`（牌组 ID），导致卡片加错牌组。补丁改为显式按牌组名查找并调用 `collection.add_note(ankiNote, deck['id'])`。
2. **`edit.py` — `buttonBox` 兼容**：新版 Anki 从编辑窗口表单中移除了 `buttonBox`，直接访问会报 `AttributeError`。补丁加了 `hasattr` 保护。

这些补丁已邮件提交给上游作者，暂未收到回复。

## 安装

在其他设备上：

```bash
# 进入 Anki 插件目录（macOS 示例）
cd "$HOME/Library/Application Support/Anki2/addons21"
git clone https://github.com/EurFelux/anki-connect.git 2055492159
```

或手动将本仓库所有文件复制到 `addons21/2055492159/` 目录，然后重启 Anki。

> 目录名 `2055492159` 是 AnkiConnect 在 AnkiWeb 的插件 ID，沿用它可以继续接收配置兼容；若不希望 Anki 自动更新覆盖补丁，可在 Anki 插件设置中禁用该插件的自动更新。

## 使用

Anki 启动后，API 监听在 `http://127.0.0.1:8765`：

```bash
curl -s http://localhost:8765 -X POST -d '{"action":"version","version":6}'
# => {"result": 6, "error": null}
```

完整 API 文档见上游仓库的 README 及 [AnkiConnect 文档](https://git.sr.ht/~foosoft/anki-connect)。

## License

与原项目一致：[GNU General Public License v3.0 or later](LICENSE)。
Copyright 2016-2019 Alex Yatskov（原作者）；本仓库的修改部分同样以 GPL-3.0-or-later 发布。
