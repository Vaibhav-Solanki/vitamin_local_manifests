# vitamin_local_manifests

Local manifest for the **OnePlus Nord 3 5G** (`vitamin`, MediaTek MT6983) on **PixelOS fifteen** (Android 15).

Device trees come from [lineageos-on-vitamin](https://github.com/lineageos-on-vitamin) on branch `lineage-22.2`,
the same LineageOS revision PixelOS fifteen pins its own LineageOS repos to.

## Sync

```bash
mkdir pixelOs && cd pixelOs
repo init -u https://github.com/PixelOS-AOSP/manifest -b fifteen --git-lfs
mkdir -p .repo/local_manifests
curl -o .repo/local_manifests/vitamin.xml https://raw.githubusercontent.com/Vaibhav-Solanki/vitamin_local_manifests/main/vitamin.xml
repo sync -c -j$(nproc --all) --no-clone-bundle --no-tags
```

Kernel source is optional (the tree ships a prebuilt kernel). To sync it too:

```bash
repo sync -c -g default,kernel
```

## What the manifest provides

| Path | Repo | Why |
|---|---|---|
| `device/oneplus/vitamin` | lineageos-on-vitamin/android_device_oneplus_vitamin | device tree |
| `device/oneplus/vitamin-kernel` | lineageos-on-vitamin/android_device_oneplus_vitamin-kernel | prebuilt Image.gz, dtb, dtbo, modules |
| `hardware/oplus` | lineageos-on-vitamin/android_hardware_oplus | oplus-fwk, sepolicy, vintf |
| `hardware/mediatek` | LineageOS/android_hardware_mediatek | `lineage.dependencies` |
| `device/mediatek/sepolicy_vndr` | LineageOS/android_device_mediatek_sepolicy_vndr | `BoardConfig.mk` |
| `vendor/oneplus/vitamin` | gitlab.com/Mashopy/proprietary_vendor_oneplus_vitamin | blobs |

Everything else the device tree references (`hardware/google/pixel`, `hardware/lineage/interfaces`,
`vendor/qcom/opensource/vibrator`, `packages/modules/Virtualization`, `external/avb`) is already in the
PixelOS fifteen manifest.

## Known porting work (makefiles, not manifest)

The tree is written for LineageOS. Expect to patch:

- `vendor/lineage/config/*` references → `vendor/aosp/config/*` (PixelOS has the same files)
- `lineage_vitamin.mk` → PixelOS product naming
- Lineage-only packages: `LineageSDKResTarget`, `LineageSettingsProviderResTarget`, `vendor.lineage.health-service.default`
