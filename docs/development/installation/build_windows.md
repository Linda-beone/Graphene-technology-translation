::: {#build-windows}

===========================================

 **原文链接**：<http://dev.bitshares.works/en/master/development/installation/build_windows.html>

**译者**：https://github.com/ArcticCat1220 （北极猫）

**校对者**：

============

通过Visual Studio 2015搭建BitShares-Core
========================================

::: {.contents}
目录
:::

------------------------------------------------------------------------

环境准备
--------

-   64位Windows系统，例如Windows Server 2012 R2标准版
-   Visual Studio 2015 Update 1(*我们的测试成功完成*)

### 安装Visual Studio 2015 Update 1

\*常见问题: [There is a problem compiling with VS 2015 Update
3](https://github.com/bitshares/bitshares-core/issues/389)

-   **下载Visual Studio 2015 Update 1**

<!-- -->

    http://download.microsoft.com/download/5/7/A/57A99666-126E-42FA-8E70-862EDBADD215/vs2015.1.com_enu.iso
    Visual Studio Community 2015 with Update 1 (x86 and x64) - DVD (English)
    SHA1: FB5AE6B57BDC495AFB29646AFCA088756363A263

-   **使用虚拟光驱软件加载上述镜像文件**

> 如果您没有虚拟光驱软件，你可以使用WinCDEmu；
>
> -   从该链接下载: <http://wincdemu.sysprogs.org/download/>

-   **安装Visual Studio 2015 Update 1**

> -   安装时请选择C++

### 下载软件及工具

-   **激活perl**

> -   下载和安装
>
> 如果您的设备上没有\`perl\`软件，那么请您进行安装。因为稍后您将会用到:
>
>     https://www.activestate.com/activeperl/downloads

-   **NASM**

> -   下载和安装NASM
>
> 如果您没有安装NASM，编译OpenSSL时将会报错"ml64 could not
> find"（虽然仍有其它方式去解决，这里不作过多介绍）:
>
>     http://www.nasm.us/
>     http://www.nasm.us/pub/nasm/releasebuilds/2.13.01/win64/nasm-2.13.01-installer-x64.exe

-   **OpenSSL**

> BitShares Core依靠OpenSSL version 1.0.1 or
> 1.0.2的支持，因此您必须搭建它。本文中使用1.0.1u版本作为范例。OpenSSL
> 1.1.0版本是无法使用的。
>
> *参考文档*:
>
>     http://p-nand-q.com/programming/windows/building_openssl_with_visual_studio_2013.html
>     http://developer.covenanteyes.com/building-openssl-for-visual-studio/
>     https://www.npcglib.org/~stathis/blog/precompiled-openssl/
>     http://blog.csdn.net/fireroll/article/details/51242518
>
> **注意:**
> 虽然上述的一些链接中提供OpenSSL库的下载，但我在下载的过程中却遇到了一些问题，最终我通过源码解决了。
>
> -   可通过以下链接下载OpenSSL的源代码:
>
>         https://www.openssl.org/source/
>         https://www.openssl.org/source/openssl-1.0.2l.tar.gz
>           or
>         https://www.openssl.org/source/old/1.0.1/openssl-1.0.1u.tar.gz
>
> -   解压缩到基目录 \`C:bts\`.
> -   源代码目录为 [C:btsopenssl-1.0.1u]{.title-ref}（无嵌套目录）

-   **Boost**

> BitShares
> Core依靠Boost库的支持，且只包括1.57至1.60版本。本文使用的是1.57.0版本。（1.60版本会遇到命令行参数解析问题）
>
> -   可通过以下链接下载Boost的源代码:
>
>         http://www.boost.org/
>         https://sourceforge.net/projects/boost/files/boost/1.57.0/
>         https://sourceforge.net/projects/boost/files/boost/1.57.0/boost_1_57_0.zip/download
>
> **注意：**
> 编译后的库也可以在网络上下载，但我在下载和使用的过程中遇到了问题。之后，我通过自己编译解决了。
>
> -   提取（解压）源代码到基目录\`C:bts\`.
> -   源代码目录为\`C:btsboost\_1\_57\_0\` （无嵌套目录）

-   **CMake**

> -   可通过以下链接下载CMake:
>
>         https://cmake.org/download/
>         https://cmake.org/files/v3.9/cmake-3.9.4-win64-x64.zip
>
> -   提取（解压）源代码到基目录 \`C:bts\`.
> -   源代码目录为\`C:btscmake-3.9.4-win64-x64\` （无嵌套目录）

### 搭建库依赖

-   **搭建OpenSSL DLLs**

> -   运行\**VS2015 x64 Native Tools Command Prompt*\*
>
> **注意：**
> 事实上这是一种捷径，路径为：C:ProgramDataMicrosoftWindowsStart
> MenuProgramsVisual Studio 2015Visual Studio ToolsWindows Desktop
> Command Prompts。内容是:
>
>     %comspec% /k ""C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat"" amd64
>
> -   在命令行接口中执行如下指令：
>
> **请依照实际情况修改为真实的目录。**:
>
>     set PATH=C:\Program Files\NASM;%PATH%
>
>     c:
>     cd C:\bts\openssl-1.0.1u
>     perl Configure VC-WIN64A --prefix=C:\bts\openssl-1.0.1u-x64-release-static
>     ms\do_win64a
>     nmake -f ms\nt.mak
>     nmake -f ms\nt.mak install
>
> -   在编译完成后，一个
>     \`C:btsopenssl-1.0.1u-x64-release-static\`目录将会生成，内容是一个编译完成的库文件。
>
> **遇到的问题及相应解决方案**
>
> -   如果问题是NASM在路径（PATH）中无法被找到，那么在添加路径后，你还需要执行nmake的清空指令来清空之前生成的临时文件，或者删除源代码并重新解压一遍该源码。
>     然后用scratch开始编译步骤，否则您将遇到asm临时文件为空的问题。
> -   <https://stackoverflow.com/questions/31595869/how-to-resolve-the-module-machine-type-x86-conflicts-with-target-machine-type>

-   **搭建Boost**

> -   执行\**VS2015 x64 Native Tools Command Prompt*\*并执行如下指令:
>
>         c:
>         cd C:\bts\boost_1_57_0
>         bootstrap
>         b2 architecture=x86 address-model=64 --build-type=complete --toolset=msvc-14.0 --threading=multi --variant=release release stage
>
-   **Doxygen (可选)**

> Doxygen并不是必须的。它的用处在于生成文档和在线帮助。例如，在命令行钱包中，你能够用gethelp（求助）命令来看命令的参数描述。
>
> -   下载Doxygen:
>
>         http://www.stack.nl/~dimitri/doxygen/download.html
>         http://ftp.stack.nl/pub/users/dimitri/doxygen-1.8.13.windows.x64.bin.zip
>
> -   解压到\`C:btsdoxygen-1.8.13.windows.x64.bin\` (不要有多个嵌套目录)

-   **Git**

> -   下载并安装git:
>
>         Https://git-scm.com/download/win
>
### BitShares-Core

-   **下载并安装BitShares-Core源代码**

> 从开始目录找到并运行\`Git Bash\`。在打开的命令行接口中，执行如下指令:
>
>     cd /c/bts
>     git clone https://github.com/bitshares/bitshares-core
>     cd bitshares-core
>     git checkout <LATEST_RELEASE_TAG>
>     git submodule update --init --recursive
>
> **注意：**
>
> -   请根据实际情况修改具体目录
> -   请将\`\<LATEST\_RELEASE\_TAG\>\`替换为最新发布版本的bitshares-core。如果你需要编译其它版本，请依情况进行修改。详见\[BitShares
>     Core latest
>     release\](<https://github.com/bitshares/bitshares-core/releases>)。
>
> **在最后，你的基目录应该呈现出如下结果**:
>
>     c:\bts
>     +- bitshares-core
>     +- boost_1_57_0
>     +- cmake-3.9.4-win64-x64
>     +- openssl-1.0.1u
>     +- openssl-1.0.1u-x64-release-static
>     +- doxygen-1.8.13.windows.x64.bin (if you downloaded)

### 部署搭建环境

-   **创建一个文件C:btssetenv\_x64.bat**

> -   添加如下代码并保存:
>
>         @echo off
>         Set GRA_ROOT=C:\bts
>         Set OPENSSL_ROOT=%GRA_ROOT%\openssl-1.0.1u-x64-release-static
>         Set OPENSSL_ROOT_DIR=%OPENSSL_ROOT%
>         Set OPENSSL_INCLUDE_DIR=%OPENSSL_ROOT%\include
>         Set BOOST_ROOT=%GRA_ROOT%\boost_1_57_0
>         Set CMAKE_ROOT=%GRA_ROOT%\cmake-3.9.4-win64-x64
>
>         Set DOXYGEN_ROOT=%GRA_ROOT%\doxygen-1.8.13.windows.x64.bin
>
>         Set PATH=%BOOST_ROOT%\lib;%CMAKE_ROOT%\BIN;%DOXYGEN_ROOT%;%PATH%
>
> -   运行\**VS2015 x64 Native Tools Command Prompt*\*
> -   执行如下指令:
>
>         c:
>         cd C:\bts
>         setenv_x64.bat
>         cmake-gui
>
> **cmake接口将会弹出**

-   **CMake GUI**

> -   在CMake GUI中设置值
>     -   源代码位置：输入或选择bitshares-core基目录\`C:/bts/bitshares-core\`
>     -   二进制文件搭建位置：输入或选择编译输出路径，例如\`C:/bts/bin\`
> -   点击\[配置\]按钮
>     -   如果提示编译的输出目录不存在，点击\[Yes\]来创建目录
> -   在弹出框
>     -   在第一个下拉框Specify the generator for this
>         project中选择\**Visual Studio 14 2015 Win64*\*
>     -   在第二个输入框Optional toolset中填入(argument to -T)
>     -   在之后的单选框中，选择\**default native compiler*\*
>     -   点击\[Finish\]并稍作等待，生成按钮将会亮起
> -   点击\[Generate\]并稍作等待，打开项目按钮将会亮起
> -   点击\[Open Project\]来打开Visual Studio

### Visual Studio

-   **搭建**

> -   编译Bitshares-core
>     -   在上级工具栏中，默认的是\`Debug\`，将其修改为\`Release\`
>     -   另一个可选模式默认为\`x64\`，该项不需要修改
> -   创建两个可执行项
>     -   在Solution
>         Explorer的接口右侧，滚动下拉找到\**cli\_wallet和witness\_node*\*，右击并选择创建。
>
> 在编译完成后，会生成可执行文件:
>
>     * C:\bts\bin\programs\witness_node\Release\witness_node.exe
>     * C:\bts\bin\programs\cli_wallet\Release\cli_wallet.exe

### 其它

> **你的下一步\...**
>
> -   上述提到的编译文件\`witness\_node.exe\`和\`cli\_wallet.exe\`可以被复制到其它电脑，但还需要用到\`msvcp140.dll\`和\`vcruntime140.dll\`文件。
>
>     复制到相同的目录，即\`C:Program Files (x86)Microsoft Visual Studio
>     14.0VCredistx64Microsoft.VC140.CRT\`路径下。
>     解决方式包括静态连接或安装可再发行包。本文在此不再详述。
>
> -   如果您使用\`cli\_wallet.exe\`来连接API服务器以使用wss，那么你需要使PEM文件中包含服务器根证书。
>     -   参考此页面 - \[CLI-Wallet on Windows
>         (x64)\](../installation/windows\_cli\_tool.md\#cli-wallet-on-windows-x64)

------------------------------------------------------------------------

-   文档贡献人: \@abit

注意：这是一份由Abit More贡献文档的译文。原文可见：

-<https://github.com/abitmore/bts-cn-docs/blob/master/%E4%BD%BF%E7%94%A8VisualStudio2015%E7%BC%96%E8%AF%91BitShares-Core.txt>

同样，本文参考了如下链接：

-   <https://github.com/bitshares/bitshares-core/wiki/BUILD_WIN32>
