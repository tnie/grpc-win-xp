
# 生产 grpc 的静态库

推荐使用 vcpkg，此处不做讨论

# 生产 grpc-win-xp 的静态库

1. 参考 [grpc-win-xp][1] 倒带；
2. 使用 grpc-win-xp 项目中的更改制作补丁

    ```shell
    # 检查补丁是否可用
    git apply --check ../grpc-patch/0001-Modified-for-windows-xp-sp3-support.patch
    # 打补丁（使用 commit message
    git am ../grpc-patch/0001-Modified-for-windows-xp-sp3-support.patch
    ```
3. 针对如何生成静态库，制作补丁

[1]:https://github.com/gavxin/grpc-win-xp
