
# 生产 grpc 的静态库

推荐使用 vcpkg，此处不做讨论

# 生产 grpc-win-xp 的静态库

确保 visual studio 2015、cmake 等编译工具存在，且环境变量正确配置

1. 对 `third-party/grpc` 使用 `third-party/grpc-patch` 中的补丁（按序）
2. 更新 grpc 的第三方库
3. 手工下载并解压 OpenSSL，注意文件夹名称

    `test/distrib/cpp/run_distrib_test_cmake.bat` 脚本中因下载过慢，我将其注释掉了。需要自行下载并解压到 grpc 根路径
    
    在脚本中存在正式的源，此处提供 [OpenSSL-Win32.zip 副本](https://github.com/tnie/grpc-win-xp/releases/tag/v0.1)

4. 手动修改 `run_distrib_test_cmake.bat` 脚本，选择安装目录和生成模式

    ```bat
    REM set subdir=%date:~0,4%%date:~5,2%%date:~8,2%
    REM set CONF= Release
    set subdir=%date:~0,4%%date:~5,2%%date:~8,2%d
    set CONF= Debug
    ```

    确认 `echo %date:~0,4%%date:~5,2%%date:~8,2%` 作为文件夹名称合法有效


5. 打开 powershell 切换到 `test/distrib/cpp/` 工作目录，执行 `./run_distrib_test_cmake.bat`

如果存在编译或安装错误，请自行检测、调试。

## 发布

如果不做更多修改，可以 [下载][r] 已经编译好的结果直接使用。

```
操作系统名称：Microsoft Windows 10 专业版（64 位）
版本：10.0.18363 版本 18363

编译工具：cmake version 3.14.0

Microsoft Visual Studio Enterprise 2015
版本 14.0.25431.01 Update 3
Microsoft .NET Framework
版本 4.8.03752
```

## 当前项目如何组织起来的

对于三方依赖如何管理，[一直有困惑][4]，没有好的思路。
灵感来源于 vcpkg，从 0 到 1 源于 [Adding third party libraries - Applying Patches][3]。
具体步骤如下，类似 commit log

1. 参考 [grpc-win-xp][1] 倒带；
2. 使用 grpc-win-xp 项目中的更改制作补丁

    ```shell
    # 检查补丁是否可用
    git apply --check ../grpc-patch/0001-Modified-for-windows-xp-sp3-support.patch
    # 打补丁（使用 commit message
    git am ../grpc-patch/0001-Modified-for-windows-xp-sp3-support.patch
    ```
3. 针对如何生成静态库，制作补丁
4. 参考 [How to get rid of Git submodules untracked status?][2] 针对 `.gitmodules` 打补丁

[1]:https://github.com/gavxin/grpc-win-xp
[2]:https://stackoverflow.com/questions/5126765/how-to-get-rid-of-git-submodules-untracked-status
[3]:https://github.com/SmingHub/Sming/wiki/Adding-third-party-libraries#applying-patches
[4]:https://github.com/tnie/printlog-demo
[r]:https://github.com/tnie/grpc-win-xp/releases
