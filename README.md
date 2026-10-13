# sailfish-on-duoqin-f21pro

Currently this repo is just a place for me to keep some notes. Very early days.

## Refer to:
https://github.com/sharks-dev/android_kernel_duoqin_mt6761

## Also handy:
For information on the hardware, how to unlock bootloader, mtkclient, etc.: https://github.com/AlikornSause/Notes-on-QIN-F21-PRO

How to build the kernel: https://github.com/ravindu644/Android-Kernel-Tutorials

Some detailed hardware information relevant to fixing the kernel: https://4pda.to/forum/index.php?showtopic=1030274&st=6280#entry144725465

## Notes:

I have never tried to build an Android kernel from scratch. I used [ravindu644's build_4.19.sh](https://github.com/ravindu644/Android-Kernel-Tutorials/blob/main/build_scripts/build_4.19.sh) to get me an Image.gz.

Roadmap:
- [X] Learn how to build an Android kernel
- [ ] Get a boot img
- [ ] Boot stock Android on the custom kernel
- [ ] Fix the custom kernel (touch & sound at minimum)
- [ ] Port Sailfish

Currently I'm still tossing up whether to port Sailfish based on a Halium GSI (as GSI ROMs are common on this device) or if I go to the effort to learn how to make an Android device tree as Mister_Magister suggests.

Not much progress will be made for the next fortnight due to life, but after that I'm getting stuck right into it.
