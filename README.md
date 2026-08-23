# Vendor Boot Ramdisk Fix

A GitHub Actions workflow that fixes a `vendor_boot.img` which is missing its **system/platform ramdisk** — this happens when building a recovery (TWRP / OrangeFox) that lives *inside* `vendor_boot.img` as `recovery.cpio`. The build only produces the `recovery.cpio`; the `ramdisk.cpio` (the platform ramdisk responsible for booting the actual OS) comes out empty, so the image can't boot into the system — only into recovery.

This is a generic tool: it is **not tied to any specific device**, and it's **not related to custom ROM building**. It works on any `vendor_boot.img` using the standard Android boot image header v3/v4 layout (i.e. any device where `magiskboot` can unpack/repack the vendor_boot), for anyone building a TWRP/OrangeFox that's packed into vendor_boot.

## The problem

Some devices ship TWRP/OrangeFox as a `recovery.cpio` inside `vendor_boot.img` instead of a separate `recovery.img`. When you build that recovery, the build only outputs a valid `recovery.cpio` — the `ramdisk.cpio` (platform ramdisk, needed to boot the OS) is left empty. Flashing that vendor_boot as-is means the device can no longer boot into the OS, only into recovery.

## The fix

You provide two images:

1. **Built vendor_boot** — the one you built (TWRP/OrangeFox), which has a valid `recovery` ramdisk but an empty/broken platform ramdisk.
2. **Reference (stock) vendor_boot** — the stock vendor_boot for your device, used only as the source of a correct platform ramdisk.

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
- If your built image's platform ramdisk is genuinely fine, you don't need this tool — it's specifically for the "recovery.cpio built, ramdisk.cpio empty" case that comes up when packing TWRP/OrangeFox into vendor_boot.
- Repacking can take a few minutes depending on the size of the recovery ramdisk, since `magiskboot` compresses it with LZMA.

## Credits

Workflow based on the original `fix_vendor_boot.sh` script by [@mkvenompro](https://github.com/mkvenompro).
