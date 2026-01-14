# Won't compile out of box -- no stdlib

```shell
$ riscv64-unknown-elf-gcc -D__riscv_float_abi_soft -DportasmHANDLE_INTERRUPT=handle_trap -I . -I ../Common/include -I /home/chrisj/workspace/rocketometer_rtos/FreeRTOS/FreeRTOS/Source/include -I /home/chrisj/workspace/rocketometer_rtos/FreeRTOS/FreeRTOS/Source/portable/GCC/RISC-V -I /home/chrisj/workspace/rocketometer_rtos/FreeRTOS/FreeRTOS/Source/portable/GCC/RISC-V/chip_specific_extensions/RV32I_CLINT_no_extensions -mabi=ilp32 -mcmodel=medany -Wall -fmessage-length=0 -ffunction-sections -fdata-sections -fno-builtin-printf -march=rv32imac_zicsr -O2 -MMD -MP -c main_blinky.c -o build/main_blinky.o
main_blinky.c:32:10: fatal error: stdio.h: No such file or directory
   32 | #include <stdio.h>
      |          ^~~~~~~~~
compilation terminated.
```

This is because the riscv toolchain I installed is truly minimal, and includes no standard library. It's just a toolchain.
I got it like this:

```shell
sudo apt install qemu-system-riscv64 gcc-riscv64-unknown-elf binutils-riscv64-unknown-elf gdb-multiarch
```

Grok says I need something like newlib-nano, but that can't be easily integrated with this toolchain. It recommends
a different toolchain:
```shell
# Install the xPack manager (if not already)
npm install --global xpm

# Install the latest 64-bit embedded toolchain (supports rv32 via flags)
xpm install --global @xpack-dev-tools/riscv-none-elf-gcc@latest

# Add to PATH (add to ~/.bashrc or ~/.zshrc)
export PATH="$PATH:~/.local/xPacks/@xpack-dev-tools/riscv-none-elf-gcc/15.2.0-1.1/.content/bin"  # check exact folder after install
```
If I need to uninstall the above, it will be like this:
```shell
xpm uninstall --global @xpack-dev-tools/riscv-none-elf-gcc # uninstall xpack first using xpm
npm uninstall --global xpm # then uninstall xpm
```

# Copy demo to main project
We can now duplicate the demo project.

```shell
cd FreeRTOS/FreeRTOS/Demo/RISC-V-Qemu-virt_GCC/
make clean
cp -av * ~/workspace/rocketometer_rtos/
```

We will modify it to build first, before adding it.

```shell

```