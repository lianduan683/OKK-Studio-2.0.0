# 2.0.0 validation results

Validation used Windows x64, CPython 3.12.10, an NVIDIA GeForce RTX 5090 Laptop GPU and driver 610.91. Inputs were generated synthetic media, not personal footage. The related suite is reproducible with `verification/run_validation.py`; it is not a claim that every test module or every supported codec/hardware combination was exercised.

## First pass and corrections

The first grouped pass executed 345 tests and recorded 6 failures and 27 errors, including a failed class setup. It was **not a clean pass**. Logs were retained locally. The failures were investigated and affected cases were rerun:

- Deep temporary paths exceeded legacy Windows cleanup limits. Short isolated TEMP/cache roots fixed the harness issue. The complete export group recheck passed 97 of 98 cases; the remaining viewport timing case passed in the subsequent complete 11-case viewport recheck.
- Menu separators and scrolling past the last video row caused two legacy fixture failures, both reproduced on upstream. Corrected fixtures retain the original behavior assertions. All 10 batch UI cases passed, including two false timeouts caused by checking the deadline before an already-satisfied condition.
- Project fixtures now normalize source-library paths and restore reconstructable references before full object equality. Project upgrade: 18/18; full session save/reopen: 4/4. Missing fixture work directories now use isolated TEMP.
- A real upstream toolbar issue hid/cropped menu buttons under large system fonts. The style was corrected; all 12 keyboard/settings cases passed, including the new large-font regression.
- The four retained media tests affected by cleanup errors were rerun with the full four-case GPU media class: 5/5 passed with actual NVENC and no skips.
- Independent review found and fixed a new source-library proxy-policy persistence edge case. Source-library, upgrade and session checks: 35/35 passed after the fix.
- The GPU route regression was reproduced before its fix (3 failing cases), then all 6 route/geometry/trace checks passed. Real media checks also passed.

Recheck counts overlap the first pass and are not added to it as unique coverage.

## Second complete related pass

| Group | Executed | Failures | Errors | Skipped | Seconds |
| --- | ---: | ---: | ---: | ---: | ---: |
| editing | 182 | 1 | 0 | 0 | 1294.9 |
| export | 98 | 0 | 0 | 0 | 305.2 |
| retained_and_alpha | 86 | 0 | 0 | 0 | 522.0 |

**Total: 366 executed, 1 failure(s), 0 error(s), zero skips in the initial second pass. Affected-module rechecks then passed 5 tests with no failures, errors or skips. No reported failure remains unresolved.** This pass used new cache and temporary directories and includes all final production fixes and added regressions.

The second pass exposed an audio fixture that updated the timeline's known `has_audio` metadata but left the same original source at `None`. Correctly preserving that difference requires schema 25. The fixture now consistently describes the same known media on both references and retains its original schema 20 and complete roundtrip assertions. A separate regression verifies that a real metadata difference is saved in schema 25. Only the fixture/tests changed after the second pass; production code did not change. The complete affected audio-version module was rerun, and its test IDs are included in the result JSON.

## Alpha-track acceptance

The test imports a generated ProRes 4444 clip with real alpha into the application's upper video track, saves/reopens the project, and compares actual preview and exported frames. It covers alpha 0, 0.5 and 1, soft/hard edges, a recognizable lower background, and positioned material. Each output is 320×180, 24 fps, 2 seconds, 48 frames.

- CPU H.264: passed.
- CPU HEVC Main 10 with placement: passed.
- Actual HEVC NVENC Main 10: passed. Trace: `cuda_pipeline → gpu_composite_upload → cuda_pipeline`, with no CPU-compositor retry.

Alpha acceptance ran successfully in both grouped passes. Six frames per output were compared with preview plus independent interior/edge checks. GPU differences from CPU preview were assessed with explicit lossy/color-conversion tolerances; they were not claimed bit-exact. ProRes decoding is software followed by GPU upload/compositing. The delivery MP4 is an opaque composite, **not a file retaining an Alpha channel**.

## GPU performance evidence and limits

An unrelated source/program placement previously disabled GPU grading for a current untransformed segment. The fix limits the decision to the segment's actual geometry. Current placement and both sides of transitions keep their required transforms and safe fallback. Plain CUDA execution is now identified accurately in traces.

A 1920×1080, 30 fps, 8-second, 240-frame generated case was measured immediately before and after this routing fix (not a whole-version speed comparison):

| Run | Before fix | After fix |
| --- | ---: | ---: |
| First, including initialization | 6.774 s | 5.813 s |
| Second in the process | 6.306 s | 2.670 s |

The second-run reduction was 57.66% in this case. Restored GPU output matched the normal GPU control in all 240 decoded frame hashes. Sampled GPU utilization peaked at about 22% and encoder utilization at 13%. This demonstrates real GPU work and a faster export, not universal saturation. CPU decode for unsupported formats, CPU filters where required, disk I/O and per-segment FFmpeg startup remain. Long real-world 8K/1,020-cut projects, every GPU vendor and every effect combination were not benchmarked.

## Packaging

The final controlled-build executable also completed an independent headless portable export: exit 0, explicit CUDA surfaces and HEVC NVENC, 1920×1080, 30 fps, 8.000 seconds, 240 frames. The synthetic input hash was unchanged. Build PATH was restricted to the selected Python and Windows directories; app-local Windows UCRT/API-set DLLs from the host tooling are not shipped. The public core targets Windows 10/11 x64.

The public core excludes external FFmpeg, mpv, Gyroflow, ExifTool, third-party plug-ins and effect/media packs. It includes required notices and corresponding-source information. Local logs, screenshots, test media, private chat, caches and installed-user state are not public deliverables. See `BINARY-LICENSE.txt` for binary licensing and `README.md` for required runtimes.
