# S3TC with DRI drivers
> This repository has been archived for historical preservation. (v1.0.1)

Note: this page is no longer updated and the code no longer (offically) maintained.

## Additional info (2025/12/24)
The patent issue mentioned in the document appears to have been resolved due to the expiration of its term.

See https://www.phoronix.com/news/S3TC-Lands-In-Mesa


## Supported graphic cards
Currently, s3tc is supported on all graphic cards which use the radeon (original Radeon, VE, 7000, 7200, 7500, some Radeon IGP), r200 (Radeon 8500, 9100, 9000, 9200, IGP9100 (RS300)), 
i830 (i830, i845, i852, i855, i865 - of course only the chipset versions which actually have integrated graphics...) and i915 driver (btw the i830 driver is considered obsolete, use the i915 driver instead). Other DRI drivers are not supported, even if the hardware could do it,
at least not until someone writes the code for it (at least for the savage driver this would be possible).

## How this works
Previously, there was a patch which had to be applied to Mesa CVS. It contained the hardware-specific patches for the radeon, r200, i830 and i915 driver. It did **not** contain any functions to compress/decompress s3tc textures in software. However, it contained some functions which could call an external library to compress/decompress s3tc textures.  
**This patch no longer exists. It has been merged into Mesa CVS, so there no longer is a need to patch the graphic driver.**  
The source to the library for compressing/decompressing is still distributed separately.
The external library only exists because it might contain problematic code regarding
"IP" (or, more specifically, patents, since copyright is certainly no issue here). See also IP issues below.

## How to install
There are no binaries available (see IP issues below). Since Mesa now supports 
s3tc (albeit for full support an external library is required), 
you don't need to build your own 3d driver if you don't want to - of course you can still do that (see the dri [building page](http://dri.freedesktop.org/wiki/Building)).
You can just install a [driver snapshot](http://dri.freedesktop.org/wiki/Download).
Basically, this is all you need for support of **precompressed** textures with hardware decompression
(most newer games which use s3tc use precompressed textures, nwn, ut2k3 etc., a notable exception are QuakeIII based games
(QuakeIII, RTCW) and Doom III). So, in principle, you might not even need the external library (libtxc_dxtnxxx.tar.gz).
However, the driver behaviour of not supporting online-compression/decompression, but only precompressed textures,
is not OpenGL conformant. Therefore, in this case you need to specifically enable the s3tc extension with [driconf](http://dri.freedesktop.org/wiki/DriConf)
(if you don't want to use driconf for some reason, you could set "force_s3tc_enable=true"),
and you will get errors if applications try to use online-compression (like QuakeIII does),
or if they try to use software-decompression (like a (buggy) gimp plugin for decompressing .dds files does).
If you need software compression/decompression, you thus need to install the external libtxc_dxtn library.
Simply unpack the archive ("tar -xvzf /path/to/libtxc_dxtnxxx.tar.gz"), and do a "make" and "make install".
If you do this s3tc will be enabled per default. Also see IP issues below.

## Known Problems
There could be issues on big-endian and/or 64bit systems (only tested on i386).
Source code (in particular the software compression routines) is a mess (feel free to improve it...).

## "IP" issues
Depending on where you live, you might need a valid license for s3tc in order to be legally
allowed to use the external library. Redistribution in binary form might also be problematic
(I certainly don't impose any restrictions on redistribution, the code itself is all MIT licensed).
Ask your lawyer, the patent is supposedly held by VIA.
It is your responsibility to make sure you comply with the laws of your country, not mine!<

