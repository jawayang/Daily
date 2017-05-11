---
title: 折騰人的 Hexo 自動發佈設定
tags: 
- hexo 
- travis
---


翻了很多文章，取出最後精華在這

- https://www.karlzhou.com/2016/05/28/travis-ci-deploy-blog/


有空再來整理

## 安裝佈景主題

- 參考文章：http://devtian.me/2015/03/17/blog-sync-solution/

1. 先 fork 一份，修改的時候才能 commit 到自己的 respostory

例如：安裝 tranquilpeak 

```
git submodule add git@github.com:jawayang/hexo-theme-tranquilpeak.git themes/tranquilpeak
```

2. 處理 submodules 的時候會遇到權限問題
```
Submodule 'themes/tranquilpeak' (git@github.com:jawayang/hexo-theme-tranquilpeak.git) registered for path 'themes/tranquilpeak'
Cloning into 'themes/tranquilpeak'...
Warning: Permanently added the RSA host key for IP address '192.30.253.113' to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.
```
> 參考：http://stackoverflow.com/questions/15674064/github-submodule-access-rights-travis-ci

改用 修改 .gitmodules 的設定
把 url 從 ssh 改成 https 的方式
並且加上 
```
git:
    submodules: false
```
用手動的方式執行
