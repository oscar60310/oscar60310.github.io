---
title: Hexo 使用 Travis 自動化部屬
date: 2017-05-12 22:37:46
tags: 
    - NodeJs
    - Git
    - Hexo
---
## 前言
今天剛開始接觸 [Hexo](https://hexo.io) 這個部落格工具，想架設在 Github Pages 上，這個工具會把產生的靜態檔案放在 `public` 資料夾中，我把原始檔案放在 project 分支，gh-page 檔案則是在 master 分支，一般來說我必須設定兩次 git 要求，後來發現其實可以用 [Travis CI](https://travis-ci.org/) 來自動化部屬。

## 設定檔案
詳細的設定步驟以後想到在寫(其實這篇文章是為了測試 Theme XD)

```yml
language: node_js
node_js: stable

# S: Build Lifecycle
install:
  - npm install


#before_script:
 # - npm install -g gulp

script:
  - hexo g

after_script:
  - cd ./public
  - git init
  - git config user.name "goofy"
  - git config user.email "oscar60310@gmail.com"
  - git add .
  - git commit -m "Update docs"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master
# E: Build LifeCycle

branches:
  only:
    - project
env:
 global:
   - GH_REF: github.com/oscar60310/oscar60310.github.io.git
```