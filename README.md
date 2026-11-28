# sailfish-on-duoqin-f21pro

Currently this repo is just a place for me to keep some notes. Very early days.

## Refer to:
https://github.com/VerdandiTeam/android_kernel_duoqin_mt6761

https://github.com/VerdandiTeam/android_device_duoqin_f21pro

https://github.com/VerdandiTeam/droid-config-f21pro

https://github.com/VerdandiTeam/droid-hal-f21pro

https://github.com/VerdandiTeam/droid-hal-version-f21pro


## Notes:

Sync with `repo sync --force-sync -cj6 --no-clone-bundle`, *not* `repo sync --fetch-submodules` like the hadk says!

repo sync writes 102 GB to $ANDROID_ROOT

I had to apply this patch to build droid-hal and mic successfully:

```
diff --git a/droid-hal-device.inc b/droid-hal-device.inc
index 7717982..f56e0b0 100644
--- a/droid-hal-device.inc
+++ b/droid-hal-device.inc
@@ -753,6 +753,9 @@ if [ $android_version_major -ge "10" ] && [ $android_version_major -le "14" ]; t
     fi
     if [ ! -f "$art_path/$apex_path/lib/bionic/libc.so" ]; then
         art_path=%{android_root}/out/target/product/%{device}
+        apex_path=system/apex/com.android.runtime
+    fi
+    if [ ! -f "$art_path/$apex_path/lib/bionic/libc.so" ]; then
         apex_path=system/
     fi

diff --git a/helpers/build_packages.sh b/helpers/build_packages.sh
index f0e4800..a1ed055 100755
--- a/helpers/build_packages.sh
+++ b/helpers/build_packages.sh
@@ -414,7 +414,7 @@ if [ "$BUILDIMAGE" = "1" ]; then
             --tokenmap=$tokenmap \
             --record-pkgs=name,url \
             --outdir=$imgname \
-            --pack-to=sfe-$DEVICE-$RELEASE"$EXTRA_NAME".tar.bz2 \
+            --pack-to=sfe-$DEVICE-$RELEASE"$EXTRA_NAME".tar \
             "$ANDROID_ROOT"/$ks
     else
         sudo mic create loop --arch=$PORT_ARCH \
```

The Android block devices:
```
1|phhgsi_arm64_ab:/ $ ls -l /dev/block/platform/bootdevice/by-name/
total 0
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 boot_a -> /dev/block/mmcblk0p22
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 boot_b -> /dev/block/mmcblk0p35
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 boot_para -> /dev/block/mmcblk0p1
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 dtbo_a -> /dev/block/mmcblk0p24
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 dtbo_b -> /dev/block/mmcblk0p37
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 expdb -> /dev/block/mmcblk0p3
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 flashinfo -> /dev/block/mmcblk0p45
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 frp -> /dev/block/mmcblk0p4
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 gz_a -> /dev/block/mmcblk0p20
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 gz_b -> /dev/block/mmcblk0p33
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 lk_a -> /dev/block/mmcblk0p21
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 lk_b -> /dev/block/mmcblk0p34
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 logo -> /dev/block/mmcblk0p15
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 md1img_a -> /dev/block/mmcblk0p16
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 md1img_b -> /dev/block/mmcblk0p29
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 md_udc -> /dev/block/mmcblk0p7
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 metadata -> /dev/block/mmcblk0p8
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 nvcfg -> /dev/block/mmcblk0p5
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 nvdata -> /dev/block/mmcblk0p6
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 nvram -> /dev/block/mmcblk0p14
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 otp -> /dev/block/mmcblk0p44
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 para -> /dev/block/mmcblk0p2
lrwxrwxrwx 1 root root 23 2026-11-25 20:57 preloader_a -> /dev/block/mmcblk0boot0
lrwxrwxrwx 1 root root 23 2026-11-25 20:57 preloader_b -> /dev/block/mmcblk0boot1
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 proinfo -> /dev/block/mmcblk0p13
lrwxrwxrwx 1 root root 20 2026-11-25 20:57 protect1 -> /dev/block/mmcblk0p9
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 protect2 -> /dev/block/mmcblk0p10
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 scp_a -> /dev/block/mmcblk0p18
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 scp_b -> /dev/block/mmcblk0p31
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 sec1 -> /dev/block/mmcblk0p12
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 seccfg -> /dev/block/mmcblk0p11
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 spmfw_a -> /dev/block/mmcblk0p17
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 spmfw_b -> /dev/block/mmcblk0p30
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 sspm_a -> /dev/block/mmcblk0p19
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 sspm_b -> /dev/block/mmcblk0p32
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 super -> /dev/block/mmcblk0p39
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 tee_a -> /dev/block/mmcblk0p25
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 tee_b -> /dev/block/mmcblk0p38
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 userdata -> /dev/block/mmcblk0p43
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 vbmeta_a -> /dev/block/mmcblk0p26
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 vbmeta_b -> /dev/block/mmcblk0p40
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 vbmeta_system_a -> /dev/block/mmcblk0p27
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 vbmeta_system_b -> /dev/block/mmcblk0p41
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 vbmeta_vendor_a -> /dev/block/mmcblk0p28
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 vbmeta_vendor_b -> /dev/block/mmcblk0p42
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 vendor_boot_a -> /dev/block/mmcblk0p23
lrwxrwxrwx 1 root root 21 2026-11-25 20:57 vendor_boot_b -> /dev/block/mmcblk0p36
```

We have [non-updatable apex](https://sailfishos.wiki/link/20#bkmrk---experimental-fix-f)

## Also handy:
For information on the hardware, how to unlock bootloader, mtkclient, etc.: https://github.com/AlikornSause/Notes-on-QIN-F21-PRO

Some detailed hardware information relevant to fixing the kernel: https://4pda.to/forum/index.php?showtopic=1030274&st=6280#entry144725465
