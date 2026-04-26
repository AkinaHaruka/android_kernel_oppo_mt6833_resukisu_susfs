# For Re-SukiSu:

## STEP0: Clone Repos
```shell
git clone https://github.com/oppo-source/android_kernel_modules_oppo_mt6833 android_kernel_oppo_mt6833_resukisu_susfs 
cd android_kernel_oppo_mt6833_resukisu_susfs 
git clone https://github.com/AkinaHaruka/android_kernel_oppo_mt6833_resukisu_susfs kernel-4.14
cd kernel-4.14
```

## STEP1: Add Re-SukiSu Modules
Please run this on the project root dir

```shell
curl -LSs "https://raw.githubusercontent.com/ReSukiSU/ReSukiSU/main/kernel/setup.sh" | bash
```

## STEP2: Apply Re-SukiSu Modules
1. Checkout branch

   ### For Re-SukiSu Only
   
   ```shell
   git checkout su/resukisu
   ```
   
   ### For Re-SukiSu and SUSFS
   ```shell
   git checkout su/resukisu_susfs
   ```

2. Locate to `arch/arm64/configs` and edit your defconfig file to add this
    ```
    CONFIG_KSU=y
    CONFIG_KSU_MANUAL_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_INPUT_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_SETUID_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_INITRC_HOOK=y
    ```
## STEP3: Install Environments
```shell
sudo apt update && sudo apt install -y \
    binutils-aarch64-linux-gnu \
    binutils-arm-linux-gnueabi \
    build-essential \
    bc \
    bison \
    flex \
    libssl-dev \
    libelf-dev \
    gcc-aarch64-linux-gnu \
    gcc-arm-linux-gnueabi \
    git \
    zip \
    unzip \
    curl \
    make \
    python3 \
    libncurses5-dev \
    device-tree-compiler
```
```shell
mkdir -p toolchains && cd toolchains
git clone https://github.com/kdrag0n/proton-clang.git --depth=1 clang
git clone https://github.com/mvaisakh/gcc-arm --depth=1 gcc32
git clone https://github.com/mvaisakh/gcc-arm64 --depth=1 gcc64
```
## STEP4: Generate Config
```shell
make O=out ARCH=arm64 YOUR_DEFCONFIG
```
### FOR ReSukiSu AND SUSFS

   Edit `out/.config` and set `CONFIG_KSU_SUSFS` to y
   
   Run this
   ```shell
   make O=out ARCH=arm64 oldconfig
   ```
   and answer script`s questions.

## STEP5: Build
```shell
export CLANG_BIN=$(pwd)/toolchains/clang/bin

make -j4 O=out ARCH=arm64 \
    CC=$CLANG_BIN/clang \
    LD=$CLANG_BIN/ld.lld \
    AR=$CLANG_BIN/llvm-ar \
    NM=$CLANG_BIN/llvm-nm \
    OBJCOPY=$CLANG_BIN/llvm-objcopy \
    OBJDUMP=$CLANG_BIN/llvm-objdump \
    STRIP=$CLANG_BIN/llvm-strip \
    CLANG_TRIPLE=aarch64-linux-gnu- \
    CROSS_COMPILE=$(pwd)/toolchains/gcc64/bin/aarch64-linux-android- \
    CROSS_COMPILE_ARM32=$(pwd)/toolchains/gcc32/bin/arm-linux-androideabi- \
    HOSTCC=gcc \
    HOSTCXX=g++ \
    KCFLAGS="-fno-builtin-stpcpy -Wno-error=pointer-to-int-cast -Wno-pointer-to-int-cast -Wno-strict-prototypes -Wno-error=strict-prototypes" \
```