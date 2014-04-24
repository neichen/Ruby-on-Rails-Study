这是一个在学习Ruby on Rails的过程中遇到的小问题集合
===================================================

在完成了[Ruby on Rails的环境配置](https://github.com/neichen/How-to-Install-Rails-on-Centos/) 之后，就可以进行Ruby on Rails的开发学习了。
---------

###以下一点点列出我在学习Ruby on Rails的过程中遇到的小问题。

创建一个新工程（在部署时这一步可能已经完成了，注意：执行以下命令时应在网站的存放目录下，Nginx的默认目录为/path/to/nginx/html）
	rails new my_project

**注意：执行以下命令时应在网站的根目录下**

创建一个新的控制器（welcome），并创建一个动作（index）
	rails generate controller welcome index
创建一个新的控制器（articles），不创建动作
	rails generate controller articles
上面的命令也可以简写成
	rails g controller articles

在config/route.rb文件中对网站的路由进行配置
	## root用来配置根目录的路由，如下的配置可将根目录解析到控制器welcome的动作index中
	root 'welcome#index'
	## get用来配置GET请求的路由，如下的配置将/welcome/index解析到控制器welcome的动作index中
	get 'welcome/index' => 'welcome#index'
	## resources可用来配置对指定资源的所有操作，如下是对articles资源操作的配置
	resources :articles
	## resources还可用来配置从属关系，如下是对comments资源操作的配置，comments资源属于articles资源
	resources :articles do
	  resources :comments
	end

查看当前路由解析情况
	rake routes

创建一个新的模型（Article），包括两个内容：title (string), text (text)
	rails g model Article title:string text:text
创建一个新的模型（Comment），从属于Article，包括两个内容：commenter (string) body (text)
	rails g model Comment commneter:string body:text article:references
	## 上述命令会在Comment模型中引入“belongs_to :article”依赖
	## 需自行添加对应的依赖“has_many:comments”至Article模型中
	## 随后即可以形如@article.comments的代码来获取指定article的comments
	## 还可以在Article模型中追加Comment的删除约束，上述依赖变为“has_many:comments, dependent: :destroy”
上面的命令会创建migration结构，可通过以下命令进行数据库表的创建
	rake db:migrate

*Rails有一些安全机制，其中一种称为“强参数”，即需要明确告诉Rails你需要什么样的请求参数*




所有的动态请求，均通过路由解析到不同控制器的动作中，进行对应的操作，获取相应的模型数据，最后返回对应的视图结果。
我们约定，控制器存放在app/controllers目录下，命名为“控制器名称_controller.rb”；模型存放在app/models目录下，命名为“模型名称.rb”；视图存放在app/views目录下，命名为“动作名称.html.erb”。

可以将具有类似样式的模板提取出来，命名为“_模板.html.erb”，由其它的视图文件通过命令“render '模板'”来将其引入自己的视图中。

