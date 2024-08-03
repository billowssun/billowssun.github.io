# 这个问题困扰了我近一个月，今天探索出来解决方案。
[通过这位大佬的分享的解决方案解决了这个问题](https://zhuanlan.zhihu.com/p/128968214)
docker启动ollama，端口号为11434，错误代码为 500
使用`netstat -ano | findstr :11434`查看，没有被占用
使用`netsh int ipv4 show dynamicport tcp`查看系统默认端口占用范围
使用`netsh interface ipv4 show excludedportrange protocol=tcp`查看Hyper-V使用的端口
解决方案：使用`netsh int ipv4 set dynamicport tcp start=49152 num=16383`修改动态端口范围
然后运行`netsh winsock reset`重启电脑。

总结就是：当我们开启Hyper-V后，系统默认会分配给一些保留端口供Hyper-V使用，修改这个动态范围即可。


