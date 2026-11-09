---
name: cue-library-workflow
description: "Use when CUE files have乱码, uncertain character encoding, incomplete track names, or need verification against cover art, liner notes, or authoritative album tracklists."
---

# CUE Library Workflow

用于整轨音乐的 CUE 编码修复、歌单核对和最小范围编辑。

公开仓库说明见 [README.md](README.md)。遇到命名、目录结构或发布前检查问题时，读取 [references/library-conventions.md](references/library-conventions.md)。

## Scope

- Confirm the mounted/source path and the audio file referenced by `FILE` before editing.
- Inspect CUE encoding with `file`, raw bytes, and trial decoding. Common candidates are UTF-8, GBK/GB18030, and Big5.
- Use the same-folder cover, back cover, booklet, or text file as primary evidence for track names. Use official label/store/catalog pages as secondary evidence; cite the source when web research is used.
- Distinguish editions and media: CD, Blu-ray, DVD, bonus tracks, and Behind The Music chapters may have different track counts.
- Compare track count, audio duration, `INDEX` points, and `FILE` name. Do not rename tracks solely because an online list looks similar.

## Encoding rules

1. If UTF-8 displays correctly, do not transcode.
2. If GBK/GB18030 or Big5 decodes the Chinese text correctly, convert to UTF-8 while preserving line endings, `FILE`, `INDEX`, `FLAGS`, and other metadata.
3. If conversion encounters invalid bytes or competing decodings, stop and report the ambiguity. Do not silently invent or drop track names.
4. Keep the audio file untouched. Default to in-place CUE editing and do not create a backup unless the user asks for one.

## Metadata rules

- Preserve verified track boundaries; change only `TITLE` and, when supported by evidence, `PERFORMER`.
- Keep collaborator annotations faithful to the source, using either the title or performer field consistently.
- If the supplied list is for Blu-ray but the files are CD, map against the CD-specific list and account for omitted chapters or bonus tracks.
- Report mismatches as `已确认`, `部分确认`, `歌单不一致`, or `需要人工确认`; never present inferred timings as original disc timings.

## Validation

After an authorized edit, verify:

- `file` reports UTF-8 when conversion was intended.
- Chinese titles contain no replacement characters or mojibake.
- The referenced audio file exists and the CUE parses in the target player.
- Track numbers are consecutive and `INDEX` times were not accidentally changed.
- Test the first, a middle, and the final track in foobar2000 or the user's named player.

## Add CUE tracks to foobar2000 (macOS)

- Select the destination playlist first. Add each valid `.cue` file itself; do not add its referenced whole-file WAV/APE as well, or tracks will duplicate. For folders without a CUE, add the standalone audio file instead.
- Prefer dropping the CUE onto the destination playlist when reliable UI control is available. For batch additions, opening explicit CUE paths with `open -a /Applications/foobar2000.app <cue-paths...>` can send them to foobar2000; pass CUEs only, not their audio files or containing folders. Do not use folder import when the user asked to add individual CUEs.
- macOS foobar2000 may ignore incoming CUEs when its system integration excludes `*.CUE`. Check the relevant preferences before troubleshooting the files. If changing preferences is needed, record their original values, temporarily allow CUE files through and enable routing incoming files to the existing playlist, add the paths, then restore the exact original values.
- Verify the destination playlist shows the expected per-track titles and count, and that no duplicate whole-file entry was added. If incoming files created a separate playlist or were ignored, stop and inspect the result before retrying; avoid repeated additions that create duplicates.
- Keep the playlist the user named as the destination. Do not export, import, clear, or replace a playlist as a workaround for adding tracks.

Final reports should state the exact CUE path, audio path, original encoding, changes made, evidence used, validation performed, and any unresolved uncertainty. Do not claim a Bluetooth/audio bitrate from a source-file bitrate or from a player field that does not expose the transport value.
