# Enhancement module

## Features

* Guarantee _Storage Redirect_ starts early than normal apps during boot stage
* Guarantee redirected app's logic runs later than redirect
* Monitor file access in public storage (only monitors `open open64 openat openat64 creat creat64 mkdir mkdirat` call in `libc` from app processes)

## Precautions

* Try to disable (delete) it if you have any problem
* If there is a problem, it is helpful for developers to provide the log since booting
* For devices that have encryption enabled and the decryption process is performed after the boot is completed (usually due to enable accessibility apps), if decryption (unlocking for the first time) is too late, it may cause the redirected accessibility apps or IME apps (and apps support Direct boot) unable to use. If you need to use the input method to unlock, do not enable redirect for this input method.

## Install

_Since this solution needs to replace system files, we only provide Magisk modules for now._

### Install with Magisk (recommended)

> The Magisk module approach uses [Magisk](https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445) to replace system files without modifying real `/system`

From v13, enhance module is a module of [Riru](https://github.com/RikkaApps/Riru), install Riru is required.

> Riru is open-source thing to inject app process by a replace system file. Riru module will able to exectue its own code in app process with Riru.

1. IMPORTANT: **Delete all old module before v12 （if there is)**
2. IMPORTANT: Make sure version of Storage Redirect is **1.0.0.r1083 or above**
3. Make sure version of Magisk is v17 or above
4. Download [[Magisk] Riru - Core v6 for arm/arm64](https://github.com/RikkaApps/Riru/releases/download/v6/magisk-riru-core-arm-arm64-v6.zip)
5. Download [[Magisk] Riru - Storage Redirect v14 for arm/arm64](https://github.com/RikkaApps/StorageRedirect-assets/releases/download/assets/magisk-riru-storage-redirect-arm-arm64-v14.zip)
6. Install in _Magisk Manager_

   IMPORTANT: Before installation, **please make sure to confirm that you have learned how to remove the module when system can't start**, otherwise, you will not be able to enter the system if your system is not compatible with it.

## How to check if it works

* During the boot process, check if there is log like `StorageRedirectInject: replaced com.android.internal.os.Zygote#nativeForkAndSpecialize` (you must connect your device to PC and use adb logcat to check this log, because it will be triggered during the very early progress of booting)
* When opening any app, check if there is log like  `StorageRedirectInject: nativeForkAndSpecialize called, uid=` (use anything that can read log is ok)