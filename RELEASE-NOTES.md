## OKK STUDIO 2.0.0

Based on upstream v1.0.29, with original copyright and licenses preserved.

- Keep original media available after timeline deletion; reinsert independent instances and preserve source/proxy choices through save/reopen.
- Follow completed export progress on the timeline; show preparation/render/concat phases and working/checkpoint/output locations.
- Extend validated resume support, move bounded checkpoint validation off the encoding path, and cache GPU LUT parsing while detecting replaced LUT content.
- Fix unrelated clip placement disabling GPU grading, and accurately identify the CUDA path in diagnostics.
- Fix toolbar menus hidden or cropped under large system fonts.

Validation: two complete related passes were executed, with findings corrected and affected cases rerun. Total: 366 executed, 1 failure(s), 0 error(s), zero skips in the initial second pass. Affected-module rechecks then passed 5 tests with no failures, errors or skips. No reported failure remains unresolved. Actual ProRes Alpha-track exports passed CPU H.264, CPU HEVC 10-bit and GPU HEVC NVENC checks in both passes. The output is an opaque composite. See [TEST-RESULTS.md](https://github.com/lianduan683/OKK-Studio-2.0.0/blob/main/TEST-RESULTS.md) for the initial failures, corrections, coverage, measured GPU improvement and limits.

Assets:

- `OKK-STUDIO-2.0.0-source.zip`: application source and reproducible build/verification instructions.
- `OKK-STUDIO-2.0.0-core.zip`: Windows x64 core maintenance package; external media runtimes and effect packs are required for their respective functions. This is not a complete Setup installer.
- `OKK-STUDIO-2.0.0-third-party-sources.zip`: corresponding dependency source, including Pedalboard and submodules.
- `SHA256SUMS.txt`: asset checksums.

The application's own source is MIT; the combined core includes GPLv3 components and retains each dependency's notices. Read `BINARY-LICENSE.txt`. No private chat or personal media is included.
