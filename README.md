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

## Also handy:
For information on the hardware, how to unlock bootloader, mtkclient, etc.: https://github.com/AlikornSause/Notes-on-QIN-F21-PRO

Some detailed hardware information relevant to fixing the kernel: https://4pda.to/forum/index.php?showtopic=1030274&st=6280#entry144725465
