# Vendor Boot Ramdisk Fix

A GitHub Actions workflow that fixes a `vendor_boot.img` which is missing its **system/platform ramdisk** — a common issue on custom ROM builds where the recovery ramdisk is present, but the platform ramdisk was not generated correctly.

This is a generic tool: it is **not tied to any specific device**. It works on any `vendor_boot.img` using the standard Android boot image header v3/v4 layout (i.e. any device where `magiskboot` can unpack/repack the vendor_boot).

## The problem

When building a custom ROM, sometimes the resulting `vendor_boot.img` boots into recovery mode instead of the OS, or bootloops, because the **platform (system) ramdisk** is missing or empty — only the `recovery` ramdisk got packed in. Flashing this image as-is will not boot the OS correctly.

## The fix

You provide two images:

1. **Built vendor_boot** — the one you built, which has a valid `recovery` ramdisk but is missing (or has a broken) platform ramdisk.
2. **Reference (stock) vendor_boot** — any known-good vendor_boot for your device (e.g. extracted from a stock/official ROM), used only as the source of a correct platform ramdisk.

The workflow:

1. Downloads both images from direct links you provide.
2. Unpacks both using `magiskboot`.
3. Takes the **platform/system ramdisk** from the reference image.
4. Takes the **recovery ramdisk** from the built image, untouched.
5. Keeps the **DTB** from the built image as-is (falls back to the reference DTB only if the built one is missing or empty).
6. Repacks everything into `vendor_boot_fixed.img`.
7. Pads/truncates the output to exactly match the reference image's size, so it fits the device's vendor_boot partition.
8. Uploads `vendor_boot_fixed.img` as a workflow artifact.

Nothing inside `recovery.cpio` is modified, and the DTB is never touched — only the platform ramdisk is swapped in.

## Usage

1. Go to the **Actions** tab → **Fix Vendor Boot (ramdisk)** workflow.
2. Click **Run workflow**.
3. Provide:
   - `built_vendor_boot_url` — a direct download link to your built `vendor_boot.img`.
   - `reference_vendor_boot_url` — a direct download link to a known-good/stock `vendor_boot.img` for the same device.
4. Run the workflow and wait for it to finish.
5. Download `vendor_boot_fixed.img` from the workflow's **Artifacts** section.
6. Flash it:
   ```
   fastboot flash vendor_boot vendor_boot_fixed.img
   ```

## Requirements / notes

- Both links must be **direct, publicly accessible download links** (not pages requiring login) — the workflow uses `wget` to fetch them.
- The two images should be from the **same device**, and ideally the same boot header version, or the repack step may fail or produce an unbootable image.
- The final output size is matched to the reference image's size (padded with zeros or truncated) so it fits the target partition.
- If the built image's platform ramdisk is genuinely fine, you don't need this tool — it's specifically for the "recovery ramdisk present, platform ramdisk missing/broken" case.
- Repacking can take a few minutes depending on the size of the recovery ramdisk, since `magiskboot` compresses it with LZMA.

## Credits

Workflow based on the original `fix_vendor_boot.sh` script by [@mkvenompro](https://github.com/mkvenompro).
