# 我是这样在GitHub上发布自己的代码库的

* 将本地代码库推送到GitHub上
    * 在GitHub上新建一个远程仓库

    * 打开终端，cd到项目文件夹根目录

    * 给本地项目初始化一个git仓库，生成了隐藏的.git文件
      * 隐藏的.git文件可使用如下命令查看
        ```
        ls -aF
        ```
    * 新建一个README.md文件，该文件是关于工程代码的介绍，类似于使用说明书
      ```
      touch README.md
      ```
    * 把项目中的所有文件都添加到仓库中
      ```
      git add --all
      ```
    * 执行提交说明
      ```
      git commit -m '提交描述'
      ```
    * 如有旧仓库存在，可先移除旧仓库
      ```
      git remote rm origin
      ```
    * 添加本地仓库origin和指定远程仓库地址
      ```
      git remote add origin 远程仓库地址
      ```
  
    * 推送本地仓库到远程指定的master分支上
      * 推送前如果有冲突，可先用如下命令(打开后添加：write your merge message，再退出),可合并冲突
        ```
        git merge origin/master --allow-unrelated-histories
        ``
      * 然后。。。
        ```
        git pull origin master
        ```
        ```
        git push -u origin master
        ```
    
  * 将自己的代码库上传到GitHub上并用Cocoapods管理（需要再终端上将工程发布到Cocoapods上，这样才能用Cocoapods进行管理）
    * 注册pod账号（如果没有pod账号）
      ```
      pod trunk register 邮箱账号 [名字]
      ```
    * 如果已经注册，可在终端中输入一下命令查看个人账号信息
      ```
      pod trunk me 
      ```
    * cd到工程根目录下，以工程名创建一个.podspec文件
      ``` 
      pod spec create 项目工程名
      ```
    * 然后进入.podspec文件中进行配置
      ```
      Pod::Spec.new do |s|
       s.name         = 'ZBGuideViewController'                         
       s.version      = '0.0.1'                                         
       s.license      = { :type => "MIT", :file => "LICENSE" }          
       s.homepage     = 'https://github.com/AnswerXu/ZBGuideViewControllerDemo.git'  
       s.author       = { "AnswerXu" => "zhengbo073017@163.com" }       
       s.summary      = '实现引导页和广告图功能，可定制性高'                  
       s.platform     = :ios, '8.0'                                     
       s.source       = { :git => 'https://github.com/AnswerXu/ZBGuideViewControllerDemo.git', :tag => s.version.to_s }
       s.source_files = 'ZBGuideViewControllerDemo/ZBGuideViewController/*.{h,m}',
       s.frameworks   =  'UIKit'
       s.requires_arc = true
       s.dependency 'SDWebImage', '~> 3.7'
      end
      ```
      ```
      s.name         -> .podspec文件名
      s.version      -> 版本号
      s.license      -> 开源协议
      s.homepage     -> 主页,这里要填写可以访问到的地址，不然验证不通过
      s.author       -> 作者信息
      s.summary      -> 项目功能描述
      s.platform     -> 开发平台及版本
      s.source       -> :git-GitHub仓库地址, :tag-对应的版本号
      s.source_files -> 依赖源文件，路径从和.podspec同级文件开始(oc: 源文件路径/*.{h,m} ; swift: 源文件路径/*.swift)
      s.frameworks   -> 所需的framework，多个用逗号隔开
      s.requires_arc -> 是否使用ARC
      s.dependency   -> 依赖关系，该项目所依赖的其他库，如果有多个需要填写多个s.dependency
      ```
      
    * 配置完成后保存退出，并添加到远程仓库
      ```
      git add 工程名.podspec
      ```
      ```
      git commit 工程名.podspec
      ```
      ```
      git push -u origin master
      ```
    * 创建工程的tag
      ```
      git tag '版本号'
      git add * 
      git commit -m 'add tag'
      git push origin 版本号
      ```
    * 再输入以下命令来检测以下是否有错误❌或警告⚠️,有则改之，每次本地的工程中有改动并上传到GitHub上之后需要改变tag,在改变了tag 之后需要将.podspec文件中的s.version和s.source 的tag 改成同样的值。当看到HUPhotoBrowser passed validation.时，说明验证通过了。
      ```
      pod spec lint
      ```
    * 在检测你的podspec时候，如果直接用pod spec lint xxx.podspec的话，出现错误它只会直接一句红色的话The spec did not pass validation, due to 1 error.告诉你的有多少个error和warning，而不会具体的指出你的错误出在哪里，这时候你可以在这句指令后面加上参数--verbose 这样就会告诉你具体的错误信息。这样根据它提示你的错误信息去解决就可以了。
      ```
      pod spec lint --verbose
      ```
    * 如果没有错误的话就可以发布了
      ```
      pod trunk push
      ```
    * 如果上一步遇到了警告⚠️
      ``` 
      pod trunk push --verbose --allow-warnings
      
      
  * 错误日志
      * Error
        ```
        - ERROR | [iOS] unknown: Encountered an unknown error (/Applications/Xcode.app/Contents/Developer/usr/bin/xcrun simctl list devices
        ```
      * Answer
        * 这个错误是因为依赖库（s.dependency）包含了.a静态库造成的。虽然这并不影响Pod的使用，但是验证是无法通过的。可以通过--use-libraries来让验证通过:
        ```
        pod spec lint xxx.podspec --verbose --use-libraries
        ```
        * 在push的时候使用
        ```
        pod trunk push  xxx.podspec --allow-warnings --use-libraries
        ```
        
      
