> **2026-11-03 编辑修复源码：** 请下载[源码增量包](OKK-STUDIO-2.0.0-editor-fix-2026-11-03-source.zip)，解压到基础源码根目录并覆盖同名文件。修复内容、校验值和验证范围见[修复说明](EDITOR-FIX-2026-11-03.md)。原 2.0.0 Release 的二进制和基础源码包尚未包含此次修复。

# OKK STUDIO 2.0.0

> **完整源码下载：** [OKK-STUDIO-2.0.0-source.zip](OKK-STUDIO-2.0.0-source.zip)。本仓库以经过校验的源码压缩包交付全部应用、测试和构建文件；请先解压，再执行下文命令。下文的 `source/`、`verification/`、构建脚本及依赖许可证目录均位于源码包中。
>
> **Windows 核心包与第三方对应源码：** [2.0.0 Release](https://github.com/lianduan683/OKK-Studio-2.0.0/releases/tag/v2.0.0)。核心包不含外部媒体运行库及大型素材，详见下文。


Windows 多机位视频编辑器的维护版本，基于 [OKK-STUDIO-1.0.0 的 v1.0.29](https://github.com/lianduan683/OKK-STUDIO-1.0.0/tree/27ef4a3cabba6aa61ea9ade3a04a9ec568e076ac)。保留原作者及贡献者的 MIT 版权声明；来源、基线提交和下载校验值见 [UPSTREAM.md](UPSTREAM.md)。

## 本版改动

- 素材库保存原片引用，删除最后一个时间轴实例后仍能重新拖入。重新拖入创建独立实例，并保留所选原片的代理读取策略；同名文件显示路径以便区分。
- 导出光标跟随时间轴上连续完成的位置；并行任务后面的片段先完成时，光标不会提前跳过去。界面显示准备、渲染、拼接阶段和临时/续渲目录。
- 导出检查点在后台有界队列中保存，复用前校验片段和依赖；LUT 内容发生变化时，即使文件大小和时间相同也不复用旧结果。
- GPU LUT 处理增加缓存与回归覆盖，保留不支持条件下的兼容路径。

Alpha 轨道采用透明素材与下层画面的合成流程。H.264/HEVC 成片为不透明合成；这不等于导出保留 Alpha 通道。ProRes Alpha 的 GPU 路径可以是软件解码、上传 GPU 合成，再由 NVENC 编码；并非所有素材都能全程硬件解码。

## 公开包的范围

Release 提供 `OKK-STUDIO-2.0.0-source.zip`（应用源码）、`OKK-STUDIO-2.0.0-core.zip`（Windows 核心包）和 `OKK-STUDIO-2.0.0-third-party-sources.zip`（包含 Pedalboard 对应源码的第三方源包）。核心包包含 Python 运行环境和应用依赖，以及 `small_assets.json` 中的 16 个 UI 小图；不包含外部 FFmpeg、MPV、Gyroflow、ExifTool、第三方 VST3 插件或完整效果素材包。

完整素材的再分发授权和外部运行库的对应源码尚未全部核齐，因此本仓库不发布完整 Setup 安装包，也不提供自动获取这些素材的构建步骤。缺少相关运行库或素材时，对应功能不可用或使用软件支持的兼容路径。

应用自有代码继续使用 [MIT](LICENSE)。包含 Pedalboard 的核心二进制按 GPLv3 兼容条款分发，依赖保留各自许可；不能把整个核心包称为 MIT。详见 [BINARY-LICENSE.txt](BINARY-LICENSE.txt) 和 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。

## 运行源码

公开核心包面向 Windows 10/11 x64。源码建议使用同样的系统、CPython 3.12（包含 Tcl/Tk）和独立虚拟环境。以下 PowerShell 命令在仓库根目录执行：

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r source\requirements-build.txt
$env:OKK_CACHE_DIR = 'D:\OKK-STUDIO-cache'
.\.venv\Scripts\python.exe source\rtx5090_exporter.py
```

缓存目录必须位于可写的非系统固定磁盘；可将示例中的 D 盘改为自己的合适磁盘。软件会显示实际选用的缓存目录。

请自行合法准备所需外部运行库，并保留其许可证和所需来源材料：

| 功能 | 源码运行时的本地位置；核心包中对应程序目录 |
| --- | --- |
| 媒体探测、普通预览和导出 | `source/bin/ffmpeg.exe`、`source/bin/ffprobe.exe`；也支持在 PATH 查找 |
| 外部 MPV 播放 | `source/bin/mpv.exe`；也支持在 PATH 查找 |
| GPU 导出 | `source/bin/gpu_export/ffmpeg.exe`，需兼容 Vulkan/libplacebo；编码器能力还受硬件和驱动约束 |
| 原生 GPU 预览 | `source/bin/gpu_live/` 中的兼容 libmpv 及所需着色器/布局文件；只复制单个 DLL 不构成完整运行库 |

外部工具官方项目：[FFmpeg](https://ffmpeg.org/)、[mpv](https://mpv.io/)。本仓库不附带、不自动下载这些二进制。核心包解压后的 `bin/` 与源码目录下的 `source/bin/` 对应。可选 MCP 服务依赖单列在 `source/requirements-mcp.txt`。

## 构建公开核心包

发行构建固定使用 Windows 10/11 x64、CPython 3.12.10，以匹配随附的运行环境许可和来源清单；`--check` 会检查这一版本及固定依赖。在该环境中执行：

```powershell
.\.venv\Scripts\python.exe build_core.py --check
.\.venv\Scripts\python.exe build_core.py --output 'D:\OKK-build\core-2.0.0'
```

输出目录必须尚不存在。脚本调用 `OKK_STUDIO_2.0.0.spec`，校验 16 个小图与许可证哈希，构建应用，附上效果契约 schema/两个 JSON 示例、许可证和来源清单，再生成 `OKK-STUDIO-2.0.0-core.zip` 与 SHA256。它不使用上游转场 ZIP、完整资源安装脚本或网络下载器。每次构建的本机时间和工具链可能影响二进制字节，因此此流程提供可重复执行的构建方法，不承诺字节完全相同。

公开二进制时须一同提供该版本应用源码和对应的 Pedalboard 源码包；具体文件、来源和固定校验值见 [BINARY-LICENSE.txt](BINARY-LICENSE.txt)。保留 PyInstaller 生成的依赖许可文件及 `licenses/supplement/`。

构建脚本使用 Python 绝对路径，并把子进程 PATH 限定为该 Python 目录、其 `Scripts`、Windows `System32` 和 Windows 目录，避免从宿主工具目录解析 DLL。显式设置的 `PYTHONPATH` 会保留；各 wheel 的 `.libs` 仍由 PyInstaller hooks 收集。构建缓存和临时文件写入新的输出目录。此核心包使用 Windows 10/11 提供的 UCRT/API-set，产物审计拒绝附带 `ucrtbase.dll` 或 `api-ms-win-*.dll`；Python、VCRuntime 和 wheel 本身所需的其他运行库仍照常保留。

## 验证

复现脚本以编辑、导出、保留功能与 Alpha 验收三个组运行相关测试，并为每个组使用独立的临时和缓存目录：

```powershell
.\.venv\Scripts\python.exe verification\run_validation.py --source "$PWD\source" --output 'D:\OKK-tests\round-1' --scratch-root 'D:\OKK-t1'
.\.venv\Scripts\python.exe verification\run_validation.py --source "$PWD\source" --output 'D:\OKK-tests\round-2' --scratch-root 'D:\OKK-t2'
```

每轮使用新的输出与临时目录；`--scratch-root` 使用较短路径，避免深目录触发 Windows 路径限制。脚本默认允许 NVENC 验收；没有兼容硬件/运行库时，测试结果中会明确记录跳过。真实媒体测试依赖上述外部 FFmpeg；离屏 Tk 测试依赖可用的 Windows 桌面会话。测试生成自己的合成素材，不使用用户媒体。

`summary.json` 记录实际用例数、失败、错误和跳过原因，`tests.log` 记录本机输出。`source/tests/` 还包含上游的其他测试，部分需要公开包未包含的可选资源。发布验证已执行两轮相关套件；第二轮首次执行 366 项，包含 1 处测试数据设置失败，修正后完整受影响模块 5 项复测通过，没有未解决的已报告失败。初次失败、修正、Alpha 验收及适用边界详见 [TEST-RESULTS.md](TEST-RESULTS.md)，结构化记录见 [verification/results-2.0.0.json](verification/results-2.0.0.json)。测试日志、缓存、合成媒体和本机路径不随源码包发布。

## 目录

`source/` 为应用与测试源码，`third_party_licenses/` 为依赖许可原文，`verification/` 为复现和发布目录检查工具。依赖来源与校验值见 `core-compliance-manifest.json`、`pedalboard-source-manifest.json`、`pedalboard-source-archive.json`、`native-build-source-manifest.json` 和 `third-party-source-bundle.json`；获取与构建说明见 [CORE-SOURCE-ACCESS.txt](CORE-SOURCE-ACCESS.txt)。`.gitignore` 排除本机运行库、缓存、构建结果、日志和媒体。
