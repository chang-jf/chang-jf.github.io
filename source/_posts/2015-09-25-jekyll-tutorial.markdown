---
layout: post
title: "Jekyll tutorial"
date: 2015-09-25 07:29:02 +0000
comments: true
categories: 
---
# Jekyll tutorial
*介紹Jekyll的安裝, 工作流程以及調整方式*

## 摘要
- 建立環境
    - 安裝Jekyll
    - 開通GitHub blog
    - 上傳公鑰
    - 調整編輯環境

- 使用Jekyll
    - New Blog
    - 同步GitHub上已有的Blog內容
    - New Post
    - Build Blog
    - 本地預覽
    - 發佈至GitHub

- 細部修改
    - 基本調整
    - 修改主題
    - 文章鍊結型式
    - 繼續閱讀按鈕
    - 變更markdown render為RedCarpet
    - tag cloud

## 環境
- Ubuntu 15.04 vivid
- vim 7.4 1-488

## 前置條件
- 了解linux, git, vim基礎操作
- 已有git帳號
- 需安裝ruby-dev

## 參考
[Jekyll安裝文件][1]  

### 建立環境 ###

#### 安裝Jekyll *redcarpet內定已安裝*

```bash
$ sudo apt-get install build-essential ruby-dev   
$ sudo gem install execjs therubyracer jekyll
```

~~- [jekyll/jekyll-compose](https://github.com/jekyll/jekyll-compose "jekyll/jekyll-compose") ~~
~~簡化Jekyll撰寫文章的流程 ~~
~~``` ~~
~~    $ cd ~/blogname ~~
~~    $ gem install jekyll-compose ~~
~~``` ~~
~~使用方法: ~~
~~`$ jekyll-compose {draft | post | publish}` ~~
~~draft "NAME": Create a new draft post with the given NAME ~~
~~post "NAME": Create a new post with the given NAME ~~
~~publish _drafts/my_draft.md [--date yyyy-mm-dd]: Moves a draft into the _posts directory ~~

#### 開通GitHub blog
在GitHub上新增一個名為*username/username.github.io*的repository, 將username置換為自己的GitHub帳號
Repository的access url會是git@github.com:username/username.github.io.git

#### 上傳公鑰
使用ssh-keygen產生公鑰並貼上GitHub

```bash
    $ ssh-keygen
    $ cat ~/.ssh/id_rsa.pub
```

[GitHub](http://github.com "GitHub") -> Account Settings -> SSH Keys ->貼上公鑰檔案內容 -> Add key
測試是否有效

```bash
    $ ssh -T git@github.com
```

如果結果如下, 公鑰設置成功:

> Hi *username*! You've successfully authenticated, but GitHub does not provide shell access.

#### 調整編輯環境 *Vim*
使用vundle安裝 - 將下列敘述加入~/.vimrc並執行`:PluginInstall`  

~~Plugin 'tpope/vim-markdown'~~
~~ Plugin 'jtratner/vim-flavored-markdown' ~~

```
Plugin 'gabrielelana/vim-markdown'
```


~~在~/.vimrc內加入強制指定檔案格式為ghmarkdown~~

~~```~~
~~augroup markdown~~
~~    au!~~
~~    au BufNewFile,BufRead *.md,*.markdown setlocal filetype=ghmarkdown~~
~~augroup END~~
~~```~~

### 使用Jekyll ###
大部份jekyll的命令都可以簡寫, 例如'jekyll build' == 'jekyll b', 直接執行jekyll有提示.

#### New Blog ####   
*以~/blogname為範例*

```bash
`$ cd ~/`
`$ jekyll new blogname`
```

#### 同步GitHub上已有的Blog內容 ####
*通常是在新的機器上編輯自己的blog, 所以不需要`$ jekyll new blogname`, 直接開一個目錄把blog pull下來即可*

```bash
    $ cd blogname
    $ git init
    $ git config --global user.name "Your Name"
    $ git config --global user.email "your_mail@address"
    $ git remote add origin git@github.com:username/username.github.io.git
    $ git pull origin master
```

#### New Post ####
Markdown文件位置位於 *./blogname/_posts/* 文檔格式為:

1. 檔名為yyyy-mm-dd-filename.markdown *副檔名可改為.md, 見**基本配置**說明*
2. 檔案內容開頭為:

> \---
> layout: post
> title:  "**Welcome to Jekyll!**"
> date:   2015-09-22 16:01:40
> categories: **jekyll update**
> ---

title與categories可以自己指定, 從最下方的`---`的下一行接著寫
*每次都要這麼搞有點煩, 所以看是自己寫script做掉, 還是去找plugin處理*

#### Build Blog ####
`$ jekyll build --source <source> --destination <destination> --watch`
- 不指定參數 -- 將當前目錄內容輸出至./_site
- --source -- 指定source directory, md原始檔的存放目錄
- --destination <destination> -- 指定destination directory, Jekyll產生出來可以發佈的靜態網站內容
- --watch --監看source directory的原始檔案, 有變更就重新build到destination directory

#### 本地預覽 ####
`$ jekyll serve --detach --no-watch`
- 不指定參數 -- 預覽伺服器開在http://localhost:4000, 會自動監看原始檔案, 一有變動就自動build
- --detach -- 預覽伺服器轉到幕後執行, 需要關閉的時候透過ps找到pid 使用 kill 停掉
- --no-watch -- 不監看原始檔案, 要自己手動build

#### 發佈至GitHub ####
如果是第一次deploy需要設定遠端位址

```bash
$ cd ~/blogname
$ git init
$ git config --global user.name "Your Name"
$ git config --global user.email "your_mail@address"
$ git remote add origin git@github.com:username/username.github.io.git
```

接下來每次新增文章後, 將_site的內容推上去即可

```bash
git add .
git commit -m "blog updated"
git push -u origin master
```

### 細部修改 ###
- 基本調整 *[Jekyll Configuration](http://jekyllrb.com/docs/configuration/ "Jekyll Configuration")*
    - 編輯~/blogname/_config.yml, 調整title, email, description, url, twitter_username, github_username

> **title**: Your awesome title
> **email**: your-email@domain.com
> **description**: > # this means to ignore newlines until "baseurl:"
> Write an awesome description for your new site here. You can edit this
> line in _config.yml. It will appear in your document head meta (for
> Google search results) and in your feed.xml site description.
> baseurl: "" # the subpath of your site, e.g. /blog/
> **url**: "http://yourdomain.com" # the base hostname & protocol for your site
> **twitter_username**: jekyllrb
> **github_username**:  jekyll

- 修改主題
    - [Jekyll Themes](http://jekyllthemes.org/ "Jekyll Themes")
    - [Jekyll 主題收藏](http://yongyuan.name/blog/collect-jekyll-theme.html "Jekyll 主題收藏")
    - [Jekyll 博客主題 Kunka](http://www.zhanxin.info/jekyll/2013-08-11-jekyll-theme-kunka.html "Jekyll 博客主題 Kunka")
    - [Jekyll Bootstrap Theme explorer](http://themes.jekyllbootstrap.com/ "Jekyll Bootstrap Theme explorer")
    - [jekyll/jekyll on
      GitHub](https://github.com/jekyll/jekyll/wiki/Themes"Jekyll/jekyll on
      github")
- Plugins
    - [Jekyll-Plugins](http://www.jekyll-plugins.com/ "Jekyll-Plugins")
    - [Jekyll Documentation-Plugins](https://jekyllrb.com/docs/plugins/ "Jekyll Documentation-Plugins")

- 文章鍊結型式

- 繼續閱讀按鈕

- 指定markdown render
Default是krandowm, 改為RedCarpet, 效能較佳, 將下列選項加入_config.yml

```
redcarpet: 
extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "strikethrough", "superscript", "with_toc_data"]
```

- tag cloud

[1]: http://jekyllrb.com/docs/installation/ "Jekyll安裝文件"
