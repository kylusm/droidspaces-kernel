# Droidspaces-enabled MT6895 GKI kernel

This repository contains a reproducible GitHub Actions build for the **5.10.260** kernel source at:

- Source: https://github.com/V1NAY007/android_kernel_oplus_mt6895
- Source branch: `upstream-test` (intentionally pinned to this branch name)
- Architecture: arm64
- Defconfig: `gki_defconfig`

## What the workflow changes

It applies the two Droidspaces GKI kABI-preserving patches for kernels 5.10 and older, then enables only the GKI options documented as required/recommended for Droidspaces:

- `CONFIG_SYSVIPC=y`
- `CONFIG_POSIX_MQUEUE=y`
- `CONFIG_IPC_NS=y`
- `CONFIG_PID_NS=y`
- `CONFIG_DEVTMPFS=y`
- `CONFIG_NETFILTER_XT_MATCH_ADDRTYPE=y`

It explicitly leaves `CONFIG_CFS_BANDWIDTH` and `CONFIG_CGROUP_PIDS` disabled to avoid changing GKI ABI. `CONFIG_MODVERSIONS` is preserved.

Patch sources are stored in [patches/](patches/) so the build does not depend on downloading mutable patch contents at build time.

## Build

The workflow runs on pushes to `main` and can also be started manually from **Actions → Build Droidspaces GKI kernel → Run workflow**.

The output is uploaded as a GitHub Actions artifact named `droidspaces-mt6895-gki-5.10.260`. It contains the compressed arm64 kernel image, final config, and build metadata.

## Important limitation

The artifact is a **kernel image**, not a complete device-specific boot image. This workflow does not generate a boot.img/vendor_boot.img or device DTB packaging, and it does not rebuild vendor kernel modules. The kABI patches are specifically intended to preserve compatibility with the existing GKI vendor-module ABI. Do not flash an image until it has been matched to the device's boot-image layout and a recovery/rollback path is available.

This build does not enable CPU or PID cgroup resource limits.
