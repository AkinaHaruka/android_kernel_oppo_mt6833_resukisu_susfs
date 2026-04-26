# For Re-SukiSu:

## STEP1: Add Re-SukiSu Modules
Please run this on the project root dir

```shell
curl -LSs "https://raw.githubusercontent.com/ReSukiSU/ReSukiSU/main/kernel/setup.sh" | bash
```

## STEP2: Apply Re-SukiSu Modules
### For Re-SukiSu Only
1. Please run this on the project root dir
    ```shell
    git checkout su/resukisu
    ```
2. Locate to `arch/arm64/configs` and edit your defconfig file to add this
    ```
    CONFIG_KSU=y
    CONFIG_KSU_MANUAL_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_INPUT_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_SETUID_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_INITRC_HOOK=y
    ```
3. Make as usual
### For Re-SukiSu and SUSFS
1. Please run this on the project root dir
    ```shell
    git checkout su/resukisu_susfs
    ```
2. Locate to `arch/arm64/configs` and edit your defconfig file to add this
    ```
    CONFIG_KSU=y
    CONFIG_KSU_SUSFS=y
    CONFIG_KSU_MANUAL_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_INPUT_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_SETUID_HOOK=y
    CONFIG_KSU_MANUAL_HOOK_AUTO_INITRC_HOOK=y
    ```
3. Make as usual