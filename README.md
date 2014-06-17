这个项目使用的是 Awestruct 来从模板编译成静态网页, 但是现在在ruby gem上面的awestruct有问题, 需要装一个最新版本的, 所以, 要运行本项目, 首先

### 安装rvm

打开[rvm的网站](https://rvm.io)然后跟着这里的安装文档安装rvm

### 使用rvm来安装ruby

	rvm install 1.9.3

这里我们使用ruby 1.9.3这个版本

###  使用taobao的源来替换ruby gem的官方源(这样速度快些)

步骤查看[这里](http://ruby.taobao.org)

### 安装rake

	gem install rake bundle

### 编译awestruct

首先, 需要checkout出源代码

	git clone git@github.com:awestruct/awestruct.git
	
然后编译并安装

	rake install
	
### 开始编译网站

	git clone git@github.com:jervisliu/easemob.com.git
	cd easemob.com
	bundle install
	rake clean preview			
