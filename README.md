# MakingCocoaPods
* 我想大家做项目都会利用CocoaPods来引用别人的框架吧, 就像的AFN，SDWebImage这些知名的大神级框架, 但是你知道这样的CocoaPods依赖库是怎样制作的吗? 刚好前几天写了点 [东西](https://github.com/Kejiasir/LaunchAnimation-demo) 想共享给大家使用, 所以呢! 费了些神找Google要了资料, 在这记录下制作过程吧, 给有需要的童鞋做些参考, 先放个 [CocoaPods的官方Blog链接](http://blog.cocoapods.org/CocoaPods-Trunk/#transition) 吧 !

## 下面来说下制作步骤
* 首先在Github创建一个Repository不用说了吧, 大家都会的 ;
* 第二 <img src="IMAGE/截图000.png?v=3&s=100" alt="GitHub" title="Clone" width="150" height="30"/> 你刚才创建的Repository到本地 ;
* 第三 话说大神的框架都会提供一个Demo给人参考是咋使用的吧, 嘿嘿, 所以我们在Xcode创建一个和仓库同名的工程放在刚刚 Clone 到本地的文件夹下面, 就跟大家平时新建工程项目一样的操作就好啦 ;
* 然后呢, 注意啦, 最重要的一步, 现在请打开你的终端, 执行如下命令:

 ```
  $ pod spec create your framework name
  # your framework name 你的依赖库的名字, 比如 SDWebImage
 ```
* 这个命令会帮我们创建一个 `.podspec` 的文件, 这是一个非常非常非常重要的文件, 你可以在本地使用文本编辑器打开来进行编辑, 但最好是提交到github上来编辑, 比较方便, 里面有很多CocoaPods帮我们生成的内容, 包括有完整的注释说明, 但大部分是我们不需要的, 直接删掉留下有用的就好 ; 

* 以我的项目为例, 先截个本地代码库结构目录图 :
* 如图: 你需要再另外创建一个和你的 `.podspec` 文件对应的同名的文件夹用来放你的依赖库的源代码, 文件夹的存放路径至少是和 `.podspec` 文件同个目录的, 当然你可以放到更里面的子目录中去, 就像我的, 但是这样的话 `.podspec` 文件里的 `s.source_files` 路径就要写对了 ;

<img src="IMAGE/截图001.png?v=3&s=100" alt="GitHub" title="本地代码库结构目录图" width="600" height="400"/>

* 下面就是 `.podspec` 文件中我们需要编辑内容, 对于一个简单的依赖库有这些就足够了, 当然如果是像 AFN, SDWebImage 这样的大神级开源框架那就可能不止这些了, 感兴趣的童鞋可以自己去找来打开看看哈 .

```Objective-C
Pod::Spec.new do |s| 
  s.name         = "LaunchAnimation" 
  s.version      = "0.1.0" 
  s.summary      = "只需要一行代码就可以使您的项目增加一个启动动画效果"
  s.description  = <<-DESC
                  只需要一行代码就可以使您的项目增加一个启动动画效果,简单快捷,没有任何依赖
                   DESC
  s.homepage     = "https://github.com/Kejiasir/" 
  s.license      = "MIT" 
  s.author       = { "Arvin" => "yasir86@126.com" } 
  s.platform     = :ios, "7.0" 
  s.source       = { :git => "https://github.com/Kejiasir/LaunchAnimation-demo.git", :tag => "0.1.0" }
  s.source_files = "LaunchAnimation-demo/LaunchAnimation-demo/LaunchAnimation/*.{h,m}" 
  s.requires_arc = true 
end
````
* `s.name` 和 `s.summary` 声明库的名称和说明文档 , `pod search` 命令就是根据这两项内容作为搜索文本的 ;
* `s.version` 依赖库源代码的版本 ;
* `s.description` 依赖库的详细功能描述 ;
* `s.homepage` 声明依赖库的主页 ;
* `s.license` 所采用的开源许可协议 ; 
* `s.author` 依赖库的作者 ;
* `s.platform` 依赖库支持的系统平台, 可以指定平台版本 ;
* `s.source` 声明依赖库源代码的地址, 后面的Tag是每发布一个Release版本时打上的标签, 注意:每个版本的Tag需要与`s.version`声明的一致 ;
* `s.source_files` 是包含依赖库所有源代码的目录, 以上述的格式为例, 前面部分 `LaunchAnimation-demo/LaunchAnimation-demo/LaunchAnimation/` 是一个相对目录路径, 目录的层级关系一定要跟(本地及线上)代码库的保持一致, 最后的 `*.{h,m}` 则是一个类似正则表达式的字符串, 表示匹配所有以`.h`和`.m`为扩展名的文件 ;
* `s.requires_arc` 声明是否使用ARC, 'true'表示YES, 'flase'则表示NO

### 这里先插播说下Git打标签吧
* 常用的就两种方式
 * 第一, 含附注的标签
   * 创建一个含附注类型的标签非常简单, 用-a(取annotated的首字母)指定标签名字即可:
```
$ git tag -a 0.1.0 -m 'my version 0.1.0'
# -m 则指定了对应的标签说明, Git 会将此说明一同保存在标签对象中, 类似commit 代码时附带的-m说明
& git show
# 用此命令查看相应标签的版本信息, 并连同显示打标签时的提交对象
```
 * 第二, 轻量级标签
  * 轻量级标签, 实际上就是一个保存着对应提交对象的校验和信息的文件, 后面神马都不带, 我一般就使用这种
```
$ git tag 0.1.0
```
* 打完标签需要将标签分享到远端服务器, 默认情况下, `git push` 并不会把标签传送到远端服务器上, 只有通过显式命令才能分享标签到远端仓库, 其命令格式如同推送分支, 使用如下命令即可:
```
$ git push origin 0.1.0
```

### 完成了上述步骤后, 把文件提交到github, 我们需要对 `.podspec` 文件进行验证
* 使用如下命令
```
$ pod lib lint 
```
* 如无意外, 你将看到如下图这样的结果, 表示验证通过 ;

<img src="IMAGE/截图002.png?v=3&s=100" alt="GitHub" title="验证截图" width="700" height="150"/>

* 如果是 Error 呢? 哈哈O(∩_∩)O哈！我没遇到, 但是会有提示处理错误的信息, 跟着做就好了 ;

## 最终, 顺利进入最后阶段, 将`.podspec` 文件提交到CocoaPods的Specs库
* 听说啊! 在2014年5月20日之前是黑暗的日子, 提交到CocoaPods的Specs库相当麻烦, 最终还要官方人工审核通过才行, 真是淡淡的忧桑!
* 如开头所链接的CocoaPods官方博客所说, 现在CocoaPods已不再接受向CocoaPods/Specs的`pull request`, 原因是为了安全考虑, 防止每个人的pod被其他人修改, 于是CocoaPods团队开发了trunk服务, 这样每个人都是其发布的pod的owner, 没有权限的人无法修改, 这样更安全, 也更方便快捷, 省去了之前的人工审核和繁琐流程, 大大提高了效率哦 .
* so! 让我们来看看是如何使用trunk新建并提交我们的我们的pod吧:
 * 先来张CocoaPods官方的trunk提交架构图
 
 <img src="IMAGE/architecture-diagram.png?v=3&s=100" alt="GitHub" title="CocoaPods官方的提交架构图" width="700" height="600"/>

 * 说明: trunk需要CocoaPods 0.33版本以上, 使用`pod --version`命令来查看版本
 * 如果版本低, 需要先升级, 使用如下命令:
 ```
 $ sudo gem install cocoapods
 $ pod setup
 ```
 * 如果升级完成(版本大于0.33的请忽略上面的过程), 那么开始注册, 执行如下命令
 ```
 $ pod trunk register  arvinSir.86@gmail.com 'arvin' --description='macbook pro' --verbose
 # 上面的命令是我注册时使用的, 你需要把邮箱和名字以及描述替换成你的, 加上--verbose可以输出详细的debug信息, 方便出错时查看
 ```
 * 注册后CocoaPods会给你的邮箱发送验证链接, 点击后就注册成功了
 * 完了可以使用如下命令查看自己的注册信息:
 ```
 $ pod trunk me
 ```
 * 如下图是我的注册信息
 
 <img src="IMAGE/截图003.png?v=3&s=100" alt="GitHub" title="注册信息截图" width="800" height="150"/>

### 最后就是将你的`.podspec` 文件提交到CocoaPods的Specs库了
* 使用如下命令:
```
pod trunk push LaunchAnimation.podspec
# 请将 LaunchAnimation 替换成你的依赖库名字
```
* `pod trunk push` 命令做了如下三个工作:
 * 验证你本地的 `.podspec` 文件, 这个步骤我们前面已经做过 ;
 * 上传你的 `.podspec` 文件到trunk ;
 * 将你的 `.podspec` 文件转化成trunk需要的Json文件 .

* 就像上面官方提供的提交架构图描述的那样, 你在trunk中的操作依然会在CocoaPods/Specs仓库中更新, 以后再做更改时只需要更新版本号然后通过trunk来提交, 不用再向`CocoaPods/Specspull request`并等待审核和Merge了.
* 使用如下命令来更新你的Pods依赖库就OK啦
```
 $ pod setup
```
* 现在就使用命令 `pod search` 试试吧, 提交成功后马上就能搜索到自己的库了
* 就像酱紫, O(∩_∩)O哈哈哈~, 至此大功告成啦! 

<img src="IMAGE/截图004.png?v=3&s=100" alt="GitHub" title="搜索结果截图" width="600" height="130"/>

