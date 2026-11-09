# CUE Library Workflow

一个用于整理无损音乐库的 Codex skill，重点处理 CUE 文件乱码、音频引用错误、曲目资料核对和 Foobar2000 导入。

作者： [**CZ83TT**](https://github.com/CZ83TT)

它适合这些情况：

- CUE 文件显示乱码，需要判断 UTF-8、GB18030 或 Big5。
- CUE 中的 `FILE` 引用与同目录音频文件名不一致。
- 歌手、专辑、曲名或曲序混乱，需要根据封面、封底或可信歌单核对。
- 分轨 WAV/FLAC 需要统一命名，并保持 CUE 可播放。
- 需要把 CUE 文件加入 Foobar2000，同时避免整轨音频重复加入。

## 设计原则

- 先确认目录、音频文件和 CUE 的实际对应关系，再修改。
- 封面、封底、说明书和本地曲目表优先于网上转载歌单。
- 不凭网上相似专辑猜测版本、曲序或时间点。
- 音频内容保持不变；只修改得到证据支持的文件名、CUE 字段和标签。
- 每次修改后检查 CUE 编码、引用、曲序，并抽查首曲、中间曲和末曲。
- 未确认的内容标为“需要人工确认”，不伪造信息。

## 安装

将整个 `cue-library-workflow` 目录放入 Codex skills 目录：

```text
~/.codex/skills/cue-library-workflow/
```

也可以在 Codex 对话中显式调用：

```text
$cue-library-workflow
```

## 推荐目录结构

```text
音乐库/
├── 艺术家 - 专辑/
│   ├── 01 - 曲名.wav
│   ├── 02 - 曲名.wav
│   ├── 艺术家 - 专辑.cue
│   ├── 专辑封面.jpg
│   └── 专辑封底.jpg
└── 合集 - 专辑名/
    ├── CD1/
    └── CD2/
```

目录约定的详细说明见 [references/library-conventions.md](references/library-conventions.md)。

## 处理边界

这个 skill 不下载或重新编码 APE/WAV/FLAC 音频，也不凭空补齐缺失的分轨文件。网上歌单只能用于辅助核对；如果版本或曲目数量对不上，应保留现状并报告差异。

## 许可证

MIT License。详见 [LICENSE](LICENSE)。
