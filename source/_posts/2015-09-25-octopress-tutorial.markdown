---
layout: post
title: "Octopress tutorial"
date: 2015-09-25 07:23:18 +0000
comments: true
categories: 
---
#### Octopress安裝流程
- 選定~/Dropbox/blog作為blog內容的存放點
- 在[GitHub](https://github.com) "GitHub"上註冊帳號後新增一個repository,  
  名稱為username.github.io  
  這邊username要換成申請到的帳號名稱,  
  新增成功後頁面會有一個ssh link像這樣 
>git@github.com:[username]/[username].github.io.git
  先抄在旁邊備用
- 本機端安裝必要套件並設定local repository
```
    $ sudo apt-get install nodejs ruby-dev build-essential
    $ cd ~/Dropbox/
    $ git clone git://github.com/imathis/octopress.git blog
    $ cd blog
    $ sudo gem install bundler execjs therubyracer
    $ bundle install
    $ rake install          //安裝默認主題
    $ rake preview
```
- 在瀏覽器上打開http://localhost:4000 with browser, 檢查預設blog是不是已經起來
- ssh-keygen製作公鑰,  
  ssh-keygen會要求指定key的存放位置, 照預設按enter即可,  
  當ssh-keygen要求輸入passphrase時按照要求輸入兩次, 我們要用的key會放在~/.ssh/id_rsa.pub內  
  ssh key的作用是讓GitHub識別更新內容是否為本人, passphrase則是作為第2道防線使用,  
  有設定的話日後更新blog內容時候還會要求輸入現在設好的passphrase, 嫌麻煩可以留空enter兩次不使用
```
    $ ssh-keygen
```
- 將個人公鑰存進GitHub  
```
    https://github.com/settings/profile -> SSH Keys -> Add SSH key,貼上~/.ssh/id_rsa.pub的內容
```
- 測試連線
```
    $ ssh -T git@github.com
```
  如果做的都對的話, 應該可以看到這樣的訊息, 到這裡連線就算準備好了
>Hi [username]! You've successfully authenticated, but GitHub does not provide shell access.

- 初始化local repository並且推上GitHub
```
    $ cd ~/Dropbox/blog
    $ git config --global user.name "username"
    $ git config --global user.email "username@gmail.com"
    $ rake setup_github_pages
    # 貼上新增repository時拿到的ssh link
    $ rake generate
    $ rake deploy
```
- 在瀏覽器打開http://[username].github.io, 確定blog已經上傳
- 這時候觀察GitHub上存的東西會發現存的是~/Dropbox/blog/public下的內容,
  就是Octopress產生出來的blog,
  我們希望自己辛苦寫的markdown原始檔以及調整過的Octopress設定也能放上GitHub,
  但是又不想干擾到blog的呈現, 因此我們使用分支的概念處理,
  將~/Dropbox/blog整個目錄納入source這個分支
- GitHub上開分支[備份原始碼][1] (主要是備份設定與source目錄下的blog原始碼, 特別是其中的\_posts目錄)
```
    $ git add .
    $ git commit -m "Blog updated"
    $ git push origin source
```
    到此為止GitHub上總共出現了兩個分支, master分支管理blog的呈現內容,
    就是Octopress產生出來的東西, source分支管理整個Octopress, 包含原始檔,
    設定等等.
    注意在文章中git pull/push後面跟著的分支名稱, 一般來說不用管master分支,
    那個使用Octopress的rake gen_deploy會在產生內容的同時幫你更新上去.
    當換了新電腦, blog有新文章, 或是調整過設定想存檔, git
    pull/push後面跟的就是source分支

###在新電腦上編輯 - 將GitHub上的最新內容同步到local repository
```
    $ mkdir ~/Dropbox/blog
    $ cd ~/Dropbox/blog
    $ git init
    $ git config --global user.name "username"
    $ git config --global user.email "username@gmail.com"
    $ git pull origin source
```

####日常編輯
檔案的位置會放在~/Dropbox/blog/source/\_posts/下, 檔名看起來像是yyyy-mm-dd-Post-Title.markdown   
yyyy-mm-dd是當前日期,這個檔案可以vim直接以markdown語法編輯, 檔案內有預先放進去的檔頭內容不要動   
編輯完以rake gen\_deploy同時生成頁面並發佈至github.
```
    $ cd ~/Dropbox/blog
    $ rake new_post["Post Title"]
    $ vim ~/Dropbox/blog/source/\_posts/yyyy-mm-dd-Post-Title.markdown
    $ rake gen_deploy
```


[1]: http://sushihangover.github.io/octopress-backing-up-your-source-directory/ "back up your source"
