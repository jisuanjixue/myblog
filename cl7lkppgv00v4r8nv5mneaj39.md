## Debugging Ruby on Rails with Visual Studio Code

  > 我是一个前端开发人员， 有五年多的开发经验， 最近几年我想把自己的精力和时间放在全栈上， 目前是能找到一份远程开发岗位。因为自己从事软件开发的是由Ruby on rails开始的，但是由于种种原因没能用它在工作上。所以我依然坚持自己学习它的动力，不仅仅是学习过Ruby 和rails， 而且是随着Ruby的不断成长，迎接Ruby3的到来， rails作为全栈开发框架，今年rails7的大爆发， 让我更有信心和理由继续学习Ruby on Rails。
>      在开发项目中， debug是意向必备的技能，今年就来聊聊Ruby on Rails的debug, [这里是官方debugging文档](https://guides.rubyonrails.org/debugging_rails_applications.html)，里面提到了几种debug 的方法, 其中Debugging with the debug gem，是这篇文章讲的内容。
>     详细怎么用，以及它的一些使用方法,这里就再次提及。我从结合使用vscode的方面说：

1.  首先安装 [VSCode rdbg Ruby Debugger](https://marketplace.visualstudio.com/items?itemName=KoichiSasada.vscode-rdbg)， 然后您需要安装最新的调试 gem 并且 rdbg 命令应该在 $PATH 中。

```
$ gem install debug

``` 
2.  使用配置启动，可以自己在项目添加.vscode/launch.json文件，
```
{
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
                {
                        "type": "rdbg",
                        "name": "Debug current file with rdbg",
                        "request": "launch",
                        "script": "${file}",
                        "args": [],
                        "askParameters": true
                },
                {
                        "type": "rdbg",
                        "name": "Attach with rdbg",
                        "request": "attach"
                }
        ]
}
``` 
它包含“使用 rdbg 调试当前文件”（启动）配置和“使用 rdbg 附加”（附加）配置。你可以修改这个配置文件。

3. 一般情况是在程序启动时就把rdbg附加进去，我用rails7开发项目, 启动一个新应用程序并使用 esbuild，这一点就变得尤为重要，因为 Rails 将生成一个新的 bin/dev 脚本，它将您的 Rails 服务器、CSS 观察程序和 JS 捆绑器组合到由管理的单个进程中领班。这对于使用单个命令运行 Rails 应用程序非常方便，所以我会把rdbg 一起加到 bin/dev 脚本里，如：

```
# Procfile.dev 文件
web: rdbg -n --open=vscode -c  --bin/rails server -p 3000
css: yarn build:css --watch
js: yarn build --watch

``` 
具体指令可以查看[rdbg command help](https://github.com/ruby/debug)
这样项目启动服务后，debger也启动了。
一般使用到像esbuild， vite 这些build工具都可加进去，放在启动脚本里。

4. 在vscode里调试，这里就不细说， 大概流程就是启动调试器， 可以在添加一个插件，[ 用于运行带有键绑定的自定义启动配置的启动配置扩展](https://marketplace.visualstudio.com/items?itemName=ArturoDent.launch-config)，然后在找到.rb文件在所需要的调试的地方代码行最左侧点击红色点，从这里断点。可以从新启动web服务进入项目页面刷新一下后，就能在vscode 里看到断点处的相关信息。具体不用代码演示了，可以看视频更清楚。这位朋友讲解的不错[点击观看](https://blog.testdouble.com/talks/2022-08-22-debugging-ruby-on-rails-with-vscode/)。





   