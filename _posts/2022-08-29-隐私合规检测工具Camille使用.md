
## 前置条件
   1. 需要root手机(替代方案，使用模拟器)
   2. 需要安装python

## 一、环境配置
    1. 安装[frida](https://github.com/frida/frida)
    ```
    pip install frida-tools
    pip install frida
    ```
    
    2. 安装[frida-server](https://github.com/frida/frida/releases/tag/15.2.2)
    根据frida和手机cpu版本，下载对应的![frida-server](https://github.com/frida/frida/releases/tag/15.2.2)
    ```
    //查看 frida 版本
    frida --version
    
    //查看手机CPU架构
    adb shell getprop ro.product.cpu.abi
    
    //将frida-server push到手机
    adb push ./frida-server /data/local/tmp/
    
    //切换root用户，更改frida-server权限
    adb shell
    su
    cd /data/local/tmp/
    chmod 777 frida-server
    ```
    
    3. 安装[Camille](https://github.com/zhengjim/camille)
    ```
    git clone https://github.com/zhengjim/camille.git
    cd camille
    pip install -r requirements.txt
    //检查是否安装成功
    python camille.py -h
    ```
    
## 二、使用步骤
    
    1. 手机端以root身份开启frida-server
    ```
    adb shell
    su
    cd /data/local/tmp/
    ./frida-server
    ```
    2. 进入camille目录，运行camille工具
    ```
    //简单使用
    python3 camille.py <package name>
    
    //将APP行为轨迹保存到EXCEL
    python3 camille.py <package name> -ns -f demo.xls
    ```
    
## 注意
   1. camille工具需要进入camille目录运行，里面的`scritp.js`脚本是相对路径引用
   2. 如果报找不到文件`scritp.js`，尝试给一下权限`sudo chmod +x ./script.js`
   3. 手机端开启`frida-server`需要root权限
    
