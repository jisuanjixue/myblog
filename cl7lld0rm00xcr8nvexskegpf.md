## Debugging Ruby on Rails with Visual Studio Code

I am a front-end developer with more than five years of development experience. In recent years, I want to put my energy and time on the full stack. Currently, I can find a remote development position. Because I started software development with Ruby on rails, but couldn't use it at work for various reasons. So I still insist on my motivation to learn it, not only learning Ruby and rails, but also with the continuous growth of Ruby and the arrival of Ruby3, rails as a full-stack development framework, the explosion of rails7 this year has made me more Confidence and reason to continue learning Ruby on Rails. > In development projects, debug is a necessary skill for intentions. This year, let's talk about debug of Ruby on Rails, [here is the official debugging document](https: guides.rubyonrails.org/debugging_rails_applications.html), which mentioned several A debug method, among which Debugging with the debug gem, is the content of this article. > How to use it in detail, as well as some of its usage methods, will be mentioned again here. I say from the aspect of using vscode: 
1. First install [VSCode rdbg Ruby Debugger](https: marketplace.visualstudio.com/items?itemName=KoichiSasada.vscode-rdbg), then you need to install the latest debug gem and rdbg command Should be in $PATH. 
```
$ gem install debug 
``` 
2. Use the configuration to start, you can add the .vscode/launch.json file to the project,


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

It contains a "debug current file with rdbg" (launch) configuration and an "attach with rdbg" (attach) configuration. You can modify this configuration file. 

3. The general case is to attach rdbg when the program starts. I use rails7 to develop a project, start a new application and use esbuild, this becomes especially important, because Rails will generate a new bin/dev script, It combines your Rails server, CSS watcher and JS bundler into a single process managed by Foreman. This is very convenient for running Rails applications with a single command, so I would add rdbg to the bin/dev script together like: 

```
 # Procfile.dev file 
  web: rdbg -n --open=vscode -c -- bin/rails server -p 3000 
  css: yarn build:css --watch 
  js: yarn build --watch 

``` 
For specific instructions, see [rdbg command help](https: github.com/ruby/debug) This project starts the service After that, debger is also started. Generally, build tools like esbuild and vite can be added and placed in the startup script. 

4. Debugging in vscode, I won't go into details here, probably the process is to start the debugger, you can add a plugin, [launch configuration extension for running custom launch configurations with key bindings](https: marketplace. visualstudio.com/items?itemName=ArturoDent.launch-config), then click the red dot on the far left of the code line where you find the .rb file where you need to debug, and breakpoint from here. You can restart the web service and enter the project page to refresh, and you can see the relevant information at the breakpoint in vscode. The specific code is not demonstrated, you can watch the video to make it clearer. This friend explained it well [click to watch](https: blog.testdouble.com/talks/2022-08-22-debugging-ruby-on-rails-with-vscode/)