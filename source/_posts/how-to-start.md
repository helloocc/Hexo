---
title: Ubuntu系统Hexo+GithubPages搭建个人博客
date: 2016-05-31 19:50:11
tags:
---

#### 首先，在github里建立名字为username.github.io的repo。

### 安装nvm和nodejs
```bash
$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
$ nvm isntall 0.12
```
### 查看nodejs版本
```bash
node -v
```
### 安装hexo
```bash
$ npm install -g hexo-cli
```

<!--more-->


### 在本地建立博客文件夹
 ``` bash 
helloc@helloc-ubuntu:~$ mkdir Blogs
helloc@helloc-ubuntu:~$ cd Blogs
helloc@helloc-ubuntu:~/Blogs$ hexo init
helloc@helloc-ubuntu:~/Blogs$ npm install
helloc@helloc-ubuntu:~/Blogs$ hexo generate
helloc@helloc-ubuntu:~/Blogs$ hexo server
```
打开 http://localhost:4000 查看。Ctrl+C关闭server。

### 新建一篇博文
```bash 
helloc@helloc-ubuntu:~/Blogs$ hexo new firstblog
helloc@helloc-ubuntu:~/Blogs$ cd source/_posts
helloc@helloc-ubuntu:~/Blogs/source/_posts$ ll
```
> total 16
drwxrwxr-x 2 helloc helloc 4096 May 27 21:50 ./
drwxrwxr-x 3 helloc helloc 4096 May 27 21:46 ../
-rw-rw-r-- 1 helloc helloc   57 May 27 21:50 firstblog.md
-rw-rw-r-- 1 helloc helloc  826 May 27 21:46 hello-world.md


填写firstblog.md就是文章内容。填写完成直接在本地查看，不需要generate。


----------


### 安装tree（可省略，只是看文件夹）
```bash 
helloc@helloc-ubuntu:sudo apt-get install tree
helloc@helloc-ubuntu:~/Blogs$ tree -L 1```
>  .
├── _config.yml
├── db.json
├── node_modules
├── package.json
├── public
├── scaffolds
├── source
└── themes
5 directories, 3 files

### 修改主题，从github克隆
```bash
helloc@helloc-ubuntu:~/Blogs$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia```

### 更改_config.yml文件,修改主题为 theme: yilia

### 编辑主题文件配置（不编辑则为默认配置）：
```bash 
helloc@helloc-ubuntu:~/Blogs$ gedit /home/helloc/Blogs/themes/yilia/_config.yml```



### 部署到github，安装。
```bash
helloc@helloc-ubuntu:~/Blogs$ npm install hexo-deployer-git --save```

### 打开配置文件，修改 （**所有的双引号后空一个空格**，即看到type,repo,branch显示红色）：
>  deploy:

>    type: git
  
>    repo: https://github.com/helloocc/helloocc.github.io
   
>    branch: master

> message: first post(此行可省略)


### 直接部署：
可以clean一下（可选）
```bash 
helloc@helloc-ubuntu:~/Blogs$ hexo clean```
部署：```bash 
helloc@helloc-ubuntu:~/Blogs$ hexo generate
helloc@helloc-ubuntu:~/Blogs$ hexo deploy```

### (另一种部署)手动上传到github部署：
```bash
git clone https://github.com/username/username.github.io
cd username.github.io
git add -A
git commit -m "Add blog file."
git push -u origin master```

### 修改文章地址格式
默认：
> http://helloocc.github.io/2016/05/28/how-to-start/

打开_config.yml文件,修改为
> permalink: :year:month:day/:title/


