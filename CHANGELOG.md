> `Changelog:`
> - All significant changes to this project will be documented here.
---

> [5.5.0]
>
> - Added `module_remove_check()` to `customize.sh` — removes old module before installation.
> - Added full `detect_root_all()` to `customize.sh` — replaces simple 3-line root detection with comprehensive multi-variant detection for KernelSU, Magisk, and APatch including spoofed variants, writes `.method` file on detection.
> - Added `VortexSU` to `detect_root_all()` in `customize.sh` — detected via `com.vortexsu.vortexsu`, spoofed variant handled in `ksud` fallback.
> - Added `Kokoro Mask` to Magisk variant detection in `customize.sh`.
> - Added `aapt`-based spoofed variant fallback to `detect_root_all()` in `customize.sh`, checked before the generic `ksud` fallback.
> - Added `device_info()` to `customize.sh` — merges `print_time()` and `print_device()` into a single function covering time, module version, device, RAM, Android codename, and kernel info.
> - Added `set_donate_link()` to `customize.sh` — inserts timezone-aware donate link (PayPal or Sociabuzz) into `module.prop` after banner line.
> - Added `backup_val()` to `cv` — saves original sysctl value to `$MODDIR/backup` before overwriting, skips if backup already exists.
> - Added `tune_kernel()` to `cv` — extracts kernel parameter tuning into a dedicated function.
> - Added `tune_vm()` to `cv` — extracts VM parameter tuning into a dedicated function.
> - Added `tune_net()` to `cv` — extracts network parameter tuning into a dedicated function.
> - Added `cleanup_temp()` to `cv` — extracts temporary file cleanup into a dedicated function.
> - Added full root detection from `.method` file to `service.sh` — reads KernelSU, Magisk, and APatch method and version on boot.
> - Changed `cv()` in `cv` — integrated `backup_val()`, removed `chown root:root`, `chmod 644`, and `chmod 444`, now writes directly with `echo` and `2>/dev/null || true`.
> - Changed shebang in `cv` from `#!/bin/sh` to `#!/system/bin/sh`.
> - Changed `optimize_zram` in `cv` — added `/proc/swaps` check before reset to prevent race condition.
> - Changed `detect_root_all()` structure in `customize.sh` — replaced `if/elif` chains with loop-based detection using `pkg:label` entries.
> - Changed root detection order in `customize.sh` and `service.sh` — now APatch → KernelSU → Magisk (previously KernelSU → Magisk → APatch).
> - Changed APatch `.method` file format in `customize.sh` and `service.sh` — expanded from 4 to 5 lines, adds `APATCH_APP_VER` on line 3.
> - Changed `APATCH_VER_CODE` source in `customize.sh` — now read from `/data/adb/ap/version`, falls back to `.method` line 5 if unavailable.
> - Changed `set_permissions()` in `customize.sh` — added `service.sh` to the executable permission loop.
> - Changed `post_install_actions` in `customize.sh` — wrapped into a proper named function.
> - Changed devil messages in `customize.sh` — expanded from 11 to 15 names, removed per-name descriptions.
> - Changed Android codename list in `customize.sh` — added SDK 32 (Snow Cone SL) and SDK 37 (Cinnamon Bun).
> - Changed `verify_module()` verified message in `customize.sh` — removed emoji, now outputs `Verified Success`.
> - Changed `service.sh` — root detection now reads from `.method` file instead of inline detection.
> - Changed `MODDIR` detection in `service.sh` — replaced `cd/dirname/pwd` with `dirname/readlink -f`.
> - Changed description format in `service.sh` — removed emoji and Android version, now outputs `✅ ${ROOT_METHOD} (${ROOT_VERSION}) | Improve the speed and smoothness...`.
> - Changed `uninstall.sh` — restore logic now reads from `$MODDIR/backup` and writes back original values before cleanup, with ZRAM handled separately.
> - Changed `uninstall.sh` — ZRAM restore handles `swapoff`, `reset`, `disksize`, `comp_algorithm`, and `max_comp_streams` in correct order.
> - Changed `uninstall.sh` — replaced extension-based file deletion in `/data/user_de` and `/data` with system cache directory cleanup.
> - Changed `uninstall.sh` — added `fstrim` across all major partitions on uninstall.
> - Changed `uninstall.sh` — added `exit 0` at end of script.
> - Changed `MODDIR` detection in `uninstall.sh` — replaced `cd/dirname/pwd` with `readlink -f`.
> - Changed license from GNU General Public License to Apache License 2.0.
> - Removed `hjggum.uaqona.iogbgn` package check from KernelSU Next detection in `customize.sh`.
> - Removed `SKIPUNZIP=1` and `DEBUG=false` from `customize.sh`.
> - Removed `MODPATH` and `TMPDIR` fallback lines from `customize.sh`.
> - Removed `print_time()` and `print_device()` standalone functions from `customize.sh`.
> - Removed `detect_snapdragon()` function and Snapdragon-only abort from `customize.sh` — module now supports all SoC.
> - Removed `SOC` variable and `ARCH` echo from `customize.sh`.
> - Removed inline root detection from `service.sh` — replaced by `.method` file reading.
> - Removed `ANDROID_VERSION`, `ANDROID_SDK`, and emoji `case` block from `service.sh`.
> - Removed `[FINAL]` tag injection from `service.sh`.
> - Removed `Kernel Tweak` name injection from `service.sh`.
> - Removed `/storage/emulated/0` junk file cleanup from `uninstall.sh` — avoids unintended deletion of user storage.
> - Removed `check_device()` from `cv` — no longer needed after Snapdragon restriction lifted.
> - Removed `SDK` variable from `cv` — unused after `check_device()` removal.
> - Removed `/data/local/tmp` from `cleanup_temp` in `cv` — avoids deleting files in use by ADB or tools.
---

> [4.0.0]
>
> - Added `main()` to `cv` — controls execution flow and call order of all optimization functions.
> - Added `final_kill()` to `cv` — terminates thermal, crash, and dump services on execution.
> - Added `tune_governor()` to `cv` — CPU governor tuning with schedutil and interactive support.
> - Added `tune_io_scheduler()` to `cv` — adaptive I/O scheduler selection with RAM-based read-ahead and queue tuning.
> - Added adaptive I/O tuning to `cv` — `read_ahead_kb` and `nr_requests` scaled based on total RAM.
> - Added advanced queue optimizations to `cv` — `io_poll`, `discard_max_bytes`, `max_sectors_kb`, `write_cache` for lower latency.
> - Added extra network parameters to `cv` — `somaxconn`, `optmem_max`, and netdev tuning for better throughput.
> - Added advanced VM optimizations to `cv` — `drop_caches`, `max_map_count`, `page-cluster`, `zone_reclaim_mode` for better multitasking.
> - Added advanced kernel tweaks to `cv` — `sched_child_runs_first`, autogroup disable, VA space randomization, and schedstats.
> - Changed `verify.sh` — improved structure, validation, and error handling.
> - Changed `cv` — kernel and VM tuning refactored from inline to `for p in ... case` loop structure.
> - Changed `cv` — ZRAM algorithm selection now conditional, prioritizes zstd for ≤4GB RAM and fallbacks to lz4.
> - Changed `cv` — net.core tuning merged into a single compact `key:val` loop.
> - Changed `cv` — expanded memory management with cache, OOM, and overcommit parameters.
> - Changed `cv` — removed forced performance governor, now tunes only schedutil and interactive.
> - Changed module deletion in `customize.sh` — replaced `while` loop with `find -exec rm -f {} \;` for efficiency.
> - Changed installation message in `customize.sh` — updated to more professional wording.
> - Changed cleanup directories — expanded for broader system coverage.
> - Removed leftover SDK 28 check from `customize.sh`.
---

> [3.5.0]
>
> - Changed overall module structure — major overhaul covering all aspects of the module.
---

> [3.0.0]
>
> - Added module banner for KernelSU Next.
> - Added network core optimization to `cv` — buffer tuning and `fq_codel` queue discipline.
> - Added ZRAM optimization to `cv` — automatic compression algorithm selection and dynamic sizing.
> - Added I/O scheduler optimization to `cv` — RAM-based read-ahead tuning and intelligent scheduler selection.
> - Added CPU governor tuning to `cv` — performance governor as default with advanced target load configuration.
> - Added automatic temporary file cleanup to `cv` — removes stale cache on execution.
> - Changed `README.md` — updated to more professional structure and content.
> - Changed `service.sh` — minor code improvement.
> - Changed `uninstall.sh` — improved deletion logic and messaging.
> - Changed `cv` — Snapdragon detection expanded to 8-layer mechanism for broader device compatibility.
> - Changed `cv` — parameter setting function improved with proper permissions and error handling.
> - Changed `cv` — VM parameters significantly expanded for improved memory management and responsiveness.
> - Changed `cv` — architecture refactored from array-based to modular function-based approach.
> - Changed SDK support — extended to Android 14 (SDK 29–36).
---

> [2.0.0]
>
> - Added `verify.sh` — automates module integrity check on installation.
> - Added `check_device()` to `cv` — verifies script only runs on supported SDK versions and Qualcomm chipsets.
> - Added `tune_governor()` to `cv` — dynamically detects and tunes CPU frequency scaling governor for all cores.
> - Added `main()` to `cv` — modular execution flow function.
> - Changed shebang in `cv` from `#!/system/bin/sh` to `#!/bin/sh` for broader POSIX compatibility.
> - Changed `cv` — replaced hardcoded configuration blocks with array-based structures for scalability.
> - Changed `cv` — replaced `update_param()` with loop-based parameter application system.
> - Changed `cv` — performance-oriented CPU tuning with `hispeed_freq`, `go_hispeed_load`, and `sampling_rate`.
> - Changed `customize.sh` and `service.sh` — code fixes and compatibility improvements.
> - Removed boot waiting loop from `cv` — allows faster initialization.
> - Removed unused and redundant variables from `cv`.
---

> [1.0.0]
>
> - Initial release.
---