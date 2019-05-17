
# Cocoapods的安装和使用

## Install
* 打开终端

* 如果你的gem太老，可以尝试使用如下命令升级gem:
  ```
  sudo gem update --system
  ```

* 另外，ruby的软件源rubygems.org因为使用亚马逊的服务器，所以会被屏蔽，需要更新一下ruby的源，在终端输入如下命令将官方的ruby源替换成国内淘宝源：
  * 移除现有的ruby源
    ```
    gem sources --remove https://rubygems.org/
    ```
  * 使用新的ruby源
    ```
    gem sources -a https://gems.ruby-china.com/
    ```
  * 验证新的ruby源是否替换成功
    ```
    gem sources -l
    ```
  
* Mac下都自带ruby，使用ruby的gem命令下载安装：
  * 以前的安装命令
     ```
    sudo gem install cocoapods
    ```
  * 苹果系统升级OS X EL Capitan后改为
    ```
    sudo gem install -n /usr/local/bin/cocoapods
    ```
  


## Usage
  * 使用时需要新建一个名为Podfile的文件
      * 打开终端
      * cd到项目文件夹的根目录
      * 使用命令新建Podfile文件
        ```
        vim Podfile
        ```
   
  * 搜索第三方框架(如：MJRefresh)
      ```
      pod search MJRefresh
      ```
      
  * 如下格式，将依赖库名依次添加到Podfile中(swift项目中，在target下添加：use_frameworks!)
      ```
      platform:ios , '7.0'
      target '项目工程名' do
      pod 'AFNetworking', '~>3.0.4'
      pod 'MJRefresh'
      end
      ```
  * 编辑完成后保存退出(i可编辑，esc退出编辑，w保存，q退出)
    
  * 导入第三方库文件
      * 更新所有的库
        ```
        pod install
        ```
      * 只更新有变化的库文件
        ```
        pod install --no-repo-update
        ```
      
  
  
