# krmao.github.io.repo

## Create a new post
```
$ hexo new "My New Post"
```
[More info: Writing](https://hexo.io/docs/writing.html)

## Run server
```
$ hexo server
```
[More info: Server](https://hexo.io/docs/server.html)

## Generate static files
```
$ hexo generate
```
[More info: Generating](https://hexo.io/docs/generating.html)

## Deploy to remote sites
```
$ hexo deploy
```
[More info: Deployment](https://hexo.io/docs/deployment.html)

## 删除文章
找到 source 目录下对应的文章, 删除, 然后执行如下命令
```
hexo g && hexo d && hexo s
```

## 发布到 github pages
```
hexo g && hexo d 
cd ../krmao.github.io && git checkout . && git pull && rm -rf !(.git) && pwd && ls -al
cp -rf ../krmao.github.io.repo/public/ ./ && pwd && ls -al
git add . && git commit -m "update blogs" && git push && cd ../krmao.github.io.repo && pwd && ls -al
git add . && git commit -m "update blogs" && git push && pwd && ls -al
```

## 参考
[hexo-如何生成一篇新的post](http://oakland.github.io/2016/05/02/hexo-%E5%A6%82%E4%BD%95%E7%94%9F%E6%88%90%E4%B8%80%E7%AF%87%E6%96%B0%E7%9A%84post/)