---
title: 自動備份 gitlab 資料
tags: 
- gitlab 
- backup
---

備份哪些資料？
---
gitlab 上有哪些重要的資料是應該備份的呢？  

1. gitlab 設定檔  
2. 專案資料  

備份 gitlab 設定檔
---
``` sh
# 備份資料會放在 /var/opt/gitlab/backups  
sudo sh -c 'umask 0077; tar -cf $(date "+/var/opt/gitlab/backups/gitlabconfig-%s.tar") -C / etc/gitlab'
```

備份 gitlab 專案
--- 
``` sh
# 備份資料會放在 /var/opt/gitlab/backups.
/opt/gitlab/bin/gitlab-rake gitlab:backup:create
```


切換身份並設定排程
--- 
讓 linux 可以自動定時執行 script
- 參考資料 : [利用 crontab 來做 Linux 固定排程](https://code.kpman.cc/2015/02/11/%E5%88%A9%E7%94%A8-crontab-%E4%BE%86%E5%81%9A-Linux-%E5%9B%BA%E5%AE%9A%E6%8E%92%E7%A8%8B/)

``` sh
# 切換身份執行 
sudo crontab -e -u root
# 設定定時工作
15 04 * * 2-6  umask 0077; tar -cf $(date "+/var/opt/gitlab/backups/gitlabconfig-%s.tar") -C / etc/gitlab  
```

同步資料夾到 NAS 中
---

- 參考資料 [BACKING UP GITLAB](https://blog.droidzone.in/2013/11/18/backing-up-gitlab/)  

``` sh   
#!/usr/bin/bash
cd /home/git/gitlab
fn=`sudo -u git -H rake RAILS_ENV=production gitlab:backup:create | grep 'Creating backup archive:' | awk '{print $4}'`
scp /home/git/gitlab/tmp/backups/$fn root@x.y.z.a:/root/backups/
echo "Backed up file /home/git/gitlab/tmp/backups/$fn to root@x.y.z.a.."
```

~~如何設定 SCP~~
---
- ~~[Configuring SSH and SCP/SFTP on DSM 5.0 for Synology DiskStations](
https://joshdick.net/2014/04/12/configuring_ssh_and_scp_sftp_on_dsm_5.0_for_synology_diskstations.html)~~

~~如何設定 rsync~~
---
<!--```
rsync -av /var/gitlabBackup/ username@10.1.1.31::IT\ Resources/gitlabBackup
```-->
- ~~[rsync 的詳細說明](http://newsletter.ascc.sinica.edu.tw/news/read_news.php?nid=1742)~~

> 上面的設定都免了直接設定 NFS 就好了，直接備份到 NFS 上

NFS 設定方式  
---

- [如何在區域網路內存取 Synology NAS 上的檔案 (NFS)](https://www.synology.com/zh-tw/knowledgebase/DSM/tutorial/File_Sharing/How_to_access_files_on_Synology_NAS_within_the_local_network_NFS)

NFS client 
---
執行 mount
```
sudo mount 10.1.1.31:/volume1/NetBackup /mnt/
```

發生錯誤
```
mount: wrong fs type, bad option, bad superblock on 10.1.1.31:/volume1/NetBackup,
       missing codepage or helper program, or other error
       (for several filesystems (e.g. nfs, cifs) you might
       need a /sbin/mount.<type> helper program)
       In some cases useful info is found in syslog - try
       dmesg | tail  or so
```

原來是需要安裝 nfs 的套件
``` sh
sudo apt-get install nfs-common
```

然後就可以掛載
```
sudo mount 10.1.1.31:/volume1/NetBackup gitlabBackup/      
```

掛載前還需要先建立資料夾
```
mkdir gitlabBackup
```


結論
---
綜合以上的文獻資料，具體的作法：
1. 先將 NAS 資料夾掛接到 Gitlab Server
2. 透過 crontab 設定工作排程
3. 完成


