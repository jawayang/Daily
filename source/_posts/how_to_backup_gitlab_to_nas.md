## ubuntu 安裝 Fish shell

> 遇到：apt-get install software-properties-common
解決方式
``` sh
apt-get install software-properties-common
```

``` sh
sudo apt-add-repository ppa:fish-shell/release-2
sudo apt-get update
sudo apt-get install fish
```

Make Fish your default shell:
```
chsh -s /usr/bin/fish
```

詳細教學
----
https://hackercodex.com/guide/install-fish-shell-mac-ubuntu/

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


備份資料 gitlab 
---
```
sudo sh -c 'umask 0077; tar -cf $(date "+etc-gitlab-%s.tar") -C / etc/gitlab'
```


設定排程
---
```
crontab
```
> [參考資料](https://code.kpman.cc/2015/02/11/%E5%88%A9%E7%94%A8-crontab-%E4%BE%86%E5%81%9A-Linux-%E5%9B%BA%E5%AE%9A%E6%8E%92%E7%A8%8B/)

切換身份並設定排程
```
sudo crontab -e -u root
```
設定工作項目
```
15 04 * * 2-6  umask 0077; tar cfz /secret/gitlab/backups/$(date "+etc-gitlab-\%s.tgz") -C / etc/gitlab
```

備份 gitlab 設定檔
---
```
sudo sh -c 'umask 0077; tar -cf $(date "+etc-gitlab-%s.tar") -C / etc/gitlab'
```

備份 gitlab 專案
---
備份資料會放在 /var/opt/gitlab/backups.
```
/opt/gitlab/bin/gitlab-rake gitlab:backup:create
```

同步資料夾
---
```
rsync -av /var/gitlabBackup/ username@10.1.1.31::IT\ Resources/gitlabBackup
```
> [rsync 的詳細說明](http://newsletter.ascc.sinica.edu.tw/news/read_news.php?nid=1742)


