# vitamin_local_manifests

Local manifest for the **OnePlus Nord 3 5G / Ace 2V** (`vitamin`, MediaTek MT6983) on
**PixelOS sixteen-qpr2** (Android 16 QPR2).

Device sources come from the maintainers' org [mt6983-development](https://github.com/mt6983-development)
on branch `lineage-23.2`, the same LineageOS revision PixelOS sixteen-qpr2 pins its own LineageOS repos to.

> PixelOS moved its manifest to `PixelOS-AOSP/android_manifest`. The old `PixelOS-AOSP/manifest`
> repo stopped in July 2025 and must not be used.

## Sync

```bash
mkdir pixelOs && cd pixelOs
repo init -u https://github.com/PixelOS-AOSP/android_manifest -b sixteen-qpr2 --git-lfs
mkdir -p .repo/local_manifests
curl -o .repo/local_manifests/vitamin.xml https://raw.githubusercontent.com/Vaibhav-Solanki/vitamin_local_manifests/main/vitamin.xml
repo sync -c -j$(nproc --all) --no-clone-bundle --no-tags
```

Kernel source is optional (the tree ships a prebuilt kernel). To sync it too:

```bash
repo sync -c -g default,kernel
```

## What the manifest provides

| Path | Repo | Branch | Why |
|---|---|---|---|
| `device/oneplus/vitamin` | Adarsh0127-Elite/android_device_oneplus_vitamin | lineage-23.2 | device tree |
| `device/oneplus/vitamin-kernel` | gitlab Adarsh0127-Elite/android_device_oneplus_vitamin-kernel | lineage-23.2 | prebuilt Image.gz, dtb, modules |
| `vendor/oneplus/vitamin` | gitlab Adarsh0127-Elite/proprietary_vendor_oneplus_vitamin | lineage-23.2 | blobs |
| `vendor/mediatek/ims` | Adarsh0127-Elite/android_vendor_mediatek_ims | android-16-qpr2 | `device.mk` inherits `ims.mk` |
| `hardware/oplus` | mt6983-development/hardware_oplus | lineage-23.2 | oplus-fwk, vintf |
| `hardware/mediatek` | mt6983-development/android_hardware_mediatek | lineage-23.2 | `lineage.dependencies` |
| `device/mediatek/sepolicy_vndr` | mt6983-development/android_device_mediatek_sepolicy_vndr | lineage-23.2 | `BoardConfig.mk` |

Everything else the device tree references (`hardware/google/pixel`, `hardware/lineage/interfaces`,
`hardware/lineage/compat`, `vendor/lineage/config`, `vendor/qcom/opensource/vibrator`,
`packages/modules/Virtualization`, `external/avb`) is already in the PixelOS sixteen-qpr2 manifest.

## Known porting work (makefiles, not manifest)

- **Aperture patch** required by the maintainers:
  https://github.com/Adarsh0127-Elite/android_packages_apps_Aperture/commit/9509277efc852ad8bdcce204e0d9cfe104b6d190
- `lineage_vitamin.mk` → PixelOS product naming (`aosp_vitamin.mk` or equivalent).
- The tree is currently branded as the Ace 2V (`PRODUCT_MODEL := PHP110`, PHP110 fingerprint and blobs).
  Same hardware; change model and fingerprint to CPH2493 if you want Nord 3 identity.
- Lineage-only packages that may need dropping: `LineageSDKResTarget`, `vendor.lineage.health-service.default`.

## Kernel

The prebuilt kernel is built from OnePlus's Android 16 source (PHP110_16.0.2.400) plus Lineage patches.
To rebuild it from source, follow https://github.com/mt6983-development/kernel_manifest.
