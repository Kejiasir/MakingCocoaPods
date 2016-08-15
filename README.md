# MakingCocoaPods
* 我想大家做项目都会利用CocoaPods来引用别人的框架吧, 就像的AFN，SDWebImage这些知名的框架, 但是你知道这样的CocoaPods依赖库是怎样制作的吗? 刚好前几天写了点'辣鸡'想共享给大家, 所以呢! 费神找Google要了些资料, 在这记录下制作过程吧, 给有需要的朋友做些参考, 我尽量写通俗点吧, 好让大家都看的懂 ! 
* 这里先放个CocoaPods官方的 [Blog链接](http://blog.cocoapods.org/CocoaPods-Trunk/#transition)吧

## 说下步骤吧
* 首先在Github创建一个Repository不用说了吧
* 第二 `Clone or download` 你刚创建的Repository到本地
* 第三一般大神的框架都会提供一个Demo给人参考是咋用的吧, 所以我们在Xcode创建一个和仓库同名的工程放在刚Clone在本地的文件夹下
* 第四, 重要的一步, 打开你的终端, 输入命令 

 ```
  pod spec create 你的库的名字
 ```
 如图![img]()


