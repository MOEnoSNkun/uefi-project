# Main Change Index

1. run-ovmf: makefile文件与用户态命令解析文件
```
- Makefile    // makefile of building and run
- edkfs.c     // usr app for parsing command, args and call syscall
```

2. linux: linux kernel中新syscall与runtime_service的定义与注册
```
- drivers/firmware/efi/runtime-wrappers.c   // add new Runtime Services function call wrappers: virt_efi_sample_runtime_service
- init/main.c                               // define sample_runtime_service function interface
- kernel/sys.c                              // add new syscall SYSCALL_DEFINE4 to parse command and call runtime service
- arch/x86/entry/syscalls/syscall_64.tbl
- include/linux
  - syscalls.h
  - efi.h                                   // install sample_runtime_service to runtime services table
```

3. edk2: 注册新运行时服务
```
- MdeModulePkg/Core
  - /Dxe/DxeMain/DxeMain.c                  // install (EFI_SAMPLE_RUNTIME_SERVICE)CoreEfiNotAvailableYetArg3 to runtime service table
  - /RuntimeDxe/Runtime.c                   // install convert pointer for SampleRuntimeService
- MdePkg
  - /Include/Uefi/UefiSpec.h                // install EFI_SAMPLE_RUNTIME_SERVICE
  - /Library/UefiRuntimeLib/RuntimeLib.c    // install EfiSampleRuntimeService int RuntimeLib
```

4. edk2/SampleRuntimeDriver: 核心driver以及EasyFilesystem源文件
```
  - SampleRuntimeDriver.c                   // driver source file
  - SampleRuntimeDriver.inf                 // driver inf file
  - EasyFile.h, EasyFile.c                  // EasyFile System Interface and implement
  - EasyBlock.h, EasyBlock.c                // Block function interface and implement inside EasyFile system
  - EasyDefs.h                              // Error code macro for EasyFile system
```