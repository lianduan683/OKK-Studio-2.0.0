# 第三方组件与许可

应用自有代码的 MIT 声明见 [LICENSE](LICENSE)。公开核心包包含的外部组件不因此改为 MIT；二进制分发说明见 [BINARY-LICENSE.txt](BINARY-LICENSE.txt)。

`third_party_licenses/` 保留已收集的许可原文与 NOTICE。构建核心包后，它们位于 `licenses/supplement/`；PyInstaller 收集的包内许可证和 dist-info 文件也应保持原样。

| 组件 | 来源和说明 |
| --- | --- |
| CPython 3.12.10、Tcl/Tk | Python、Tcl、Tk 许可原文附带；Python 中使用的 OpenSSL/zlib 许可另附 |
| NumPy 2.5.4 | 官方源码许可及内含组件声明，含数学/随机数等子组件许可 |
| Pillow 13.0.0 | 官方许可文件保留其收录的图像依赖声明 |
| tkinterdnd2 0.7.0 / TkDND 2.9.5 | 分别保留 Python 包与 TkDND 许可 |
| OpenTimelineIO 0.18.1 | Apache-2.0，保留 LICENSE 与 NOTICE |
| OTIO AAF adapter 2.0.0 | Apache-2.0，保留 LICENSE |
| pyaaf2 1.7.1 | MIT，保留 LICENSE |
| Pedalboard 0.9.22 | GPL-3.0，保留 LICENSE/NOTICE，并提供含固定子模块的对应源码；子模块各自声明在对应源码中 |
| PyInstaller 6.22.3 | 保留 COPYING（含 bootloader exception），不改变应用原有许可 |

依赖源码来源、版本和 SHA256 记录在 `core-compliance-manifest.json`。Pedalboard 主仓库及 JUCE、Rubber Band、LAME、libgsm、pybind11 五个子模块的固定提交记录在 `pedalboard-source-manifest.json`，完整对应源码归档见同一 Release。对照清单时不要删除包内额外的原始声明；补充目录不替代组件自身携带的许可文件。

`small_assets.json` 限定本次公开核心包的 16 个 UI 小图。完整效果素材、用户媒体、VST3 插件和外部 FFmpeg/MPV/Gyroflow/ExifTool 运行库均不随本仓库或核心包提供。应用的 MIT 声明不授予这些外部素材或程序的再分发权。
