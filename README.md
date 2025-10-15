# qumu

## 环境配置

### windows

- 安装[MSYS2](https://www.msys2.org/)
- 在项目的根目录运行`c:/msys64/msys2_shell.cmd -defterm -here -no-start -mingw64`

- 执行以下命令：

```bash
pacman -S wget base-devel git
wget https://raw.githubusercontent.com/msys2/MINGW-packages/refs/heads/master/mingw-w64-qemu/PKGBUILD
# Some packages may be missing for your environment, installation will still
# be done though.
makepkg --syncdeps --nobuild PKGBUILD || true

pacman -S mingw-w64-x86_64-riscv64-unknown-elf-gcc
riscv64-unknown-elf-gcc --version  # 验证编译器是否可用

./configure --target-list=riscv64-softmmu \
            --extra-cflags="-O0 -g3" \
            --cross-prefix-riscv64=riscv64-unknown-elf- 
```

**注意：**

- 不建议在windos上开启`enable-rust`选项,因为`./configure`时会出现`../meson.build:91:12: ERROR: Rust compiler rustc --target x86_64-pc-windows-gnu -C linker=cc -C link-arg=-m64 cannot compile programs.`。
- windows上可能出现`Generating docs/QEMU manual with a custom commandFAILED: [code=1] docs/docs.stamp`,这是文档未知原因编译的错误，并不影响我们测试