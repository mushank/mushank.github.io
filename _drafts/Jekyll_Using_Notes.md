# Jekyll Using Notes

$ jekyll build  
 => 当前文件夹中的内容将会生成到 ./_site 文件夹中。

$ jekyll build --destination <destination>  
 => 当前文件夹中的内容将会生成到目标文件夹<destination>中。

$ jekyll build --source <source> --destination <destination>  
 => 指定源文件夹<source>中的内容将会生成到目标文件夹<destination>中。

$ jekyll build --watch  
 => 当前文件夹中的内容将会生成到 ./_site 文件夹中，  
    查看改变，并且自动再生成。  
    
---

$ jekyll serve  
 => 一个开发服务器将会运行在 http://localhost:4000/  
 Auto-regeneration（自动再生成文件）: 开启。使用 `--no-watch` 来关闭。  

$ jekyll serve --detach  
 => 功能和`jekyll serve`命令相同，但是会脱离终端在后台运行。  
    如果你想关闭服务器，可以使用`kill -9 1234`命令，"1234" 是进程号（PID）。  
    如果你找不到进程号，那么就用`ps aux | grep jekyll`命令来查看，然后关闭服务器。  
    
$ jekyll serve --no-watch  
 => 和 `jekyll serve` 一样，但不会监测变化。  

---

配置：http://jekyllcn.com/docs/configuration/

