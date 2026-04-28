**==AI电脑开机信息==**
- 账号:root
- 密码: 不需要输入密码直接进入或者123456
- 进入后是ios用户
- sudo su -
- root用户密码:123456


**==AI管理页面==**
- http://dava.lgdisplay.com:8080/xray/


**==AI电脑显示器像素==**
- 1440×900


**==使用中的显卡==**
- Nvidia型号: GeForce RTX 2080Ti
- 显卡驱动:NVIDIA-Linux-x86_64-450.80.02


**==备品PC==**
- Nvidia型号: GeForce RTX 3080Ti    
- 显卡驱动: NVIDIA-Linux-x86_64-525.89.02
- 备品PC的显卡驱动向下兼容原PC的显卡，但原PC的显卡驱动不支持备品PC的显卡


**==显卡驱动安装方法==**
1. **卸载此前的显卡驱动**
```
sudo apt-get purge nvidia*
```
➣ 更换显卡后的步骤
- Ctrl + Alt + F4 : 进入命令行
- Ctrl + Alt + F7 : 进入图形界面
2. **英伟达官网搜索相应显卡驱动后下载驱动放到/home/ios目录下**
- 驱动为NVIDIA-Linux-x86_64-525.89.02.run
3. **==修改相关配置文件==**
```
sudo vi /etc/modprobe.d/blacklist.conf
```
- 最后一行加入下面的语句
```
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist rivatv
blacklist nvidiafb
```
4. **==更新文件==**
```
sudo update-initramfs -u
```
5. **==关闭图形界面==**
```
sudo service lightdm stop
```
6. **==进入到.run目录中，安装驱动==**
```
sudo ./NVIDIA-Linux-x86_64-525.89.02.run -no-x-check -no-nouveau-check -no-opengl-files
```
➣ 在安装过程中会出现：
- he distribution-provided pre-install script failed! Are you sure you want to continue?     选择 yes 继续。
- Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later?            选择NO继续
- Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up.  选择 Yes  继续
7. **==开启图形界面==**
```
sudo service lightdm start
```
8. **==验证安装==**
```
nvidia-smi
```



**==用ubuntu集成显卡进入系统==**
- nouveau为集成显卡的驱动
- 在上述blacklist.conf中注释掉blacklist nouveau就可以了


**==运行AI软件报错:Import CV2, ImportError: libcudart.so.10.0: cannot open shared object file等等==**
1. **将正常使用中的AI PC中的CUDA库复制到故障PC**
- CUDA库源路径: /usr/local/cuda/lib64
- cp -r 源文件目录 目标文件目录
2. **运行如下命令** 
```
sudo ldconfig /usr/local/cuda/lib64
```
3. **运行AI软件**
- 软件部署位置: /data/xray/src


**==启动AI软件方法==**
1. **sudo su -**
- 取得root权限，密码:123456
2. **运行下面的命令**
```
cd /data/xray/src
python xray.py
```


**==AI PC构成图==**
![[IMG_20240131_170518.jpg]]


