前景： atom书写markdown还是很舒服的，接下去推荐几款插件继续提升体验性

操作系统：window

#### file-icons
一个扩展插件能够给右侧导航栏的文件按照文件类型添加图标。

#### markdown-scroll-sync
可以同步滚动你写的markdown 和预览。

#### markdown-table-editor

可以快速添加表格语法：使用tab键和空格键

#### Markdown Preview Enhanced

一个很棒的插件，关于预览方面
使用参照[插件使用教程
](https://atom.io/packages/markdown-preview-enhanced)

我觉得其中阅览效果和toc生成目录的方面很nice。


如何用这个插件生成目录：
```
//在任意地方使用这段代码
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->
<!-- tocstop -->
```

####  markdown-scroll-sync

这个插件真的是神器

鉴于有时候我要换电脑或者重装atom，都要重装一下atom以及重新安装插件还有添加插件的配置，每次都搞得心很累。这时候就需要这个神器了：markdown-scroll-sync。

我们可以在任何地方用这个插件保存和恢复我们的配置。

这个插件通过gist去手动备份或者恢复以下内容：
```

Sync Atom's and package settings
Sync installed packages
Sync user keymaps
Sync user styles
Sync user init script
Sync snippets
Sync user defined text files
```


setup步骤：

1. 打开Atom->setting,安装好markdown-scroll-sync插件
2. [点击创建一个token](https://github.com/settings/tokens/new)

![创建token](http://oi2e3199v.bkt.clouddn.com/bfd84693d4c4e4c7e0a5ecdd27e75a92.png?imageView2/2/w/700/h/500)

3. 获取tokenID：
![tokenID](http://oi2e3199v.bkt.clouddn.com/2c129e4df211cb9e5a430b419b04b410.png?imageView2/2/w/700/h/500)

4. [点击创建gists](https://gist.github.com/)

![创建gists](http://oi2e3199v.bkt.clouddn.com/2cded0f913d2888f3dbf843b2ed4d295.png?imageView2/2/w/700/h/500)

5. 获取gistsID

![GistsID](http://oi2e3199v.bkt.clouddn.com/0d92c2168807449d762893cfb3feffe2.png?imageView2/2/w/700/h/500)

6. 填入tokenID和GistsID

![填入setting](http://oi2e3199v.bkt.clouddn.com/84051b3bcc59b544cbdeb6b449caa728.png?imageView2/2/w/700/h/500)


到这里我们就完成了安装过程

那么接下去就是使用了

我们ctrl+shift+p调出命令控制台

```
sync-settings:backup//备份
sync-settings:restore//恢复

sync-settings:view-backup//在你的gists上查看备份的情况
sync-settings:check-backup//检查备份是否可用

```
