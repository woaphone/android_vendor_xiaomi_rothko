# android_vendor_xiaomi_rothko

Redmi K70 Ultra (rothko, mt6989) 的 LineageOS vendor 仓库 —— 由
`D:\rothko_dt_work\tools\gen_vendor.py` 离线生成，**未联网**。

- 源固件：HyperOS **OS2.0.210.0.VNNCNXM**（Android 15，security_patch=2025-09-01）
- 来源分区：`D:\mio\2.0.210` 的 vendor / odm 解包产物
- 内容：`proprietary/` 2769 个 blob（2.09 GB）+ `rothko-vendor.mk`（2796 条
  PRODUCT_COPY_FILES，含 27 条 rename 语义条目）+ `BoardConfigVendor.mk`
- 清单：WIP 设备树 `proprietary-files.txt` 修正版 —— 32 条死路径删除、20 条
  2.0.210 位移修正、144 条功能补齐（NFC/Dolby/GNSS-AIDL/fpsgo/电池计量/mali/
  gpuserv/mbrain/aee/misys/odm 触控振动 mitee/**init.project.rc 及其依赖**等），
  明细见 `D:\rothko_dt_work\docs\01` 与 `logs/gen_vendor.log`

## 在源码树中的位置

```
vendor/xiaomi/rothko/          <- 本仓库内容直接放这里
```

设备树通过 `$(call inherit-product, vendor/xiaomi/rothko/rothko-vendor.mk)` 与
`include vendor/xiaomi/rothko/BoardConfigVendor.mk` 引用，路径已对齐。

## blob fixup 状态

- 文本类（txpowerctrl.cfg 去TAB、neuralnetworks shim rc start→enable）：**已应用**
- ELF 类 12 组（patchelf）：**未应用** —— 有编译环境后重跑
  `device/xiaomi/rothko/extract-files.py` 即自动补全。

## 与手机实机的交叉验证（2026-09-06，docs/07）

- 手机 `/vendor/etc/sensors/hals.conf` 与本仓库带入 dt 的 hals.conf 一字不差；
- `init.project.rc`（相机 AF/NFC/热控/smart PA/migt 权限初始化）已按实机检查补入；
- 分区参数（super 9663676416 等）与实机 `/sys/class/block` 实测一致。

## 规范收尾（有 git 环境时）

```
git init -b lineage-22.2
git add -A && git commit -m "rothko: vendor blobs from HyperOS OS2.0.210.0.VNNCNXM"
```

> 同步注意：工作区重新生成后往本仓库同步时，`robocopy /MIR` 只用于
> `proprietary\` 子目录，根目录的 README.md 需保留。
