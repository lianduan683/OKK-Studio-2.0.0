# 来源与维护范围

原项目：<https://github.com/lianduan683/OKK-STUDIO-1.0.0>，原项目作者及贡献者的版权声明保持不变。当前公开仓库为维护版本：<https://github.com/lianduan683/OKK-Studio-2.0.0>。

- 上游版本：`v1.0.29`。
- 基线提交：`27ef4a3cabba6aa61ea9ade3a04a9ec568e076ac`。
- 基线源归档 SHA256：`42a60ea4795d8b64b66f80b68001659863c63b9993bf596e16c593b96bb25290`。
- 当前应用版本：`2.0.0`。

原 `LICENSE` 原文保留，包括 `Copyright (c) 2026`；未擅自补写原许可证没有列出的人名。应用代码保留 MIT 许可，外部组件分别依照各自许可证分发。

本次维护集中在原片素材库与时间轴实例分离、导出进度显示、后台检查点保存、依赖变化时缓存失效以及 GPU LUT 缓存，并补充回归测试。公开 stage 中的应用源码和测试从维护工作副本按白名单复制；公开构建脚本与说明文档为本次新增。

本仓库包含顶层 Python 源码、Python 测试、效果契约 schema/示例、固定依赖、16 个列入 `small_assets.json` 的 UI 小图和许可材料。上游完整效果素材、外部运行库和完整安装构建资源不在本仓库范围内；它们的存在不代表获得了再分发授权。

独立的 `build_core.py` 从此仓库构建公开核心包，不依赖上游素材 ZIP。Pedalboard 的源码与五个固定子模块单独作为对应源码包提供；来源见 `pedalboard-source-manifest.json`，组装归档校验值见 `pedalboard-source-archive.json`。这不构成对任意 FFmpeg/MPV 构建或其他未包含组件源码完整性的声明。
