---
title: ubuntu 安裝 Fish shell
tags: 
- gitlab 
- fish shell
---

ubuntu 安裝 Fish shell
===
> 詳細教學 [install-fish-shell-mac-ubuntu](https://hackercodex.com/guide/install-fish-shell-mac-ubuntu/)

``` sh
sudo apt-add-repository ppa:fish-shell/release-2
```

遇到：執行 add-apt-repository 指令之後，出現「Command not found」的訊息  
解決方式  
``` sh
  apt-get install software-properties-common
```

然後升級 apt-get 然後 install 

```
sudo apt-get update
sudo apt-get install fish
```


把 Fish 設定為預設 shell 工具:
```
chsh -s /usr/bin/fish
```

升級 apt ( Advanced Packaging Tools )
----
```
apt-get update
```
```
apt-get upgrade
```


升級 git 
---
```
sudo add-apt-repository ppa:git-core/ppa  
sudo apt-get update  
sudo apt-get install git  
```

安裝 OMF
---
```
curl -L https://get.oh-my.fish > install
fish install --path=~/.local/share/omf --config=~/.config/omf
```

安裝主題
---
```
omf install bobthefish
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v1.0.0/FiraCode.zip
sudo unzip FiraCode.zip -d /usr/local/share/fonts/
sudo fc-cache -fv
```
