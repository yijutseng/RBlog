---
title: 在Ubuntu伺服器架設Jupyter Notebook
author: Yi-Ju Tseng
date: '2018-12-14'
slug: JupyterNotebook
categories:
  - Environment
tags:
  - R
  - Server
Description: ''
menu: ''
---

## RStudio Server
一直以來都是[RStudio](https://www.rstudio.com/)的愛用者，在RStudio出現以前，只能用R的陽春Client版介面的超級黑暗期實在是不堪回首（配合無止盡的博士修業過程，好像也蠻搭的）。RStudio也有server版本，只要透過瀏覽器就能在任何電腦使用RStudio進行分析專案。

開始教書以後，發現RStudio提供免費的[RStudio Server Pro](https://www.rstudio.com/products/rstudio-server-pro/)給[教學](https://www.rstudio.com/pricing/academic-pricing/)使用，二話不說提出申請，也很快就得到回覆，並直接提供授權金鑰，雖然要每年寫信重新申請，但仍然是很棒的使用者體驗。
其實[RStudio Server](https://www.rstudio.com/products/rstudio/#Server)的免費版本也很夠用了，有興趣的人可以從免費版開始上手。

## Jupyter Notebook
（本篇標題是Jupyter Notebook，好像完全離題了）除了RStudio Server外，要以瀏覽器透過Web介面來執行資料分析專案當然不能不提**Jupyter Notebook** [官網在此](http://jupyter.org/)，跟RStudio不同的是，**Jupyter Notebook**多種程式語言，比如 Python、R等等，而且可依需求將程式碼分成不同段落，分段排版與執行，並隨意的加上說明文字（配合markdown語法排版），輕鬆結合資料處理分析與結果呈現等步驟，生成程式與結果與說明並茂的文章。另外執行程式碼的結果會存在記憶體中，整個頁面可以當成一個網頁分享給任何人。

另外這種可以架在伺服器上，並且用瀏覽器可輕鬆存取的資料分析平台非常適合上課使用，在課堂上電腦教室的環境不一定與學生自有電腦相同，同學常常回家執行時發生各式各樣的問題，讓學習體驗變得很糟糕。而且電腦教室的電腦通常有還原卡，同學上課的程式碼若無自行備份，哪天想起來要取用程式碼時也只能再重打一次，然後就直接放棄治療，實在很可惜。如果不管同學用哪裡的電腦都能取用跟上課時一模一樣的環境，也不用受限於電腦教室的爛電腦（開機要一百萬年），真的能解決很多不必要的苦難。

因為上述原因，還有要跟上時代潮流，就在實驗室也架了**Jupyter Notebook**，過程記錄如下：

## 架設自己的Jupyter Notebook
### 伺服器說明

Ubuntu Server 16.04.4 LTS [下載連結](https://www.ubuntu.com/download/server)

### 更新Ubuntu套件
```
sudo apt-get update
sudo apt-get upgrade
```

### 安裝python3-pip
```
sudo apt-get install python3-pip
conda install jupyter
```

```
sudo apt-get install npm nodejs-legacy
sudo npm install -g configurable-http-proxy
```
### Jupyterhub

首先先裝Anaconda，這邊是裝3.5.1.0版本，可參考[官網](https://www.anaconda.com/download/#linux)安裝最新版本。
```
wget https://repo.continuum.io/archive/Anaconda3-5.1.0-Linux-x86_64.sh
bash Anaconda3-5.1.0-Linux-x86_64.sh
export PATH=~/anaconda3/bin:$PATH

```

```
conda install -c conda-forge jupyterhub  # installs jupyterhub and proxy
conda install notebook
```

```
sudo jupyterhub --generate-config -f /etc/jupyterhub/jupyterhub_config.py
jupyterhub --no-ssl
```

```
c.JupyterHub.ip = '192.168.1.5'
c.JupyterHub.port = 8000
c.PAMAuthenticator.encoding = 'utf8'


c.LocalAuthenticator.create_system_users = True
c.Authenticator.admin_users = {'yjtseng'}

c.Spawner.cmd=['jupyterhub-singleuser']
c.JupyterHub.statsd_prefix = 'jupyterhub'
```

```
sudo systemctl start jupyterhub
sudo systemctl status jupyterhub
```

```
[Unit]
Description=Jupyterhub

[Service]
User=root
Environment="PATH=/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/anaconda3/bin:/home/yjtseng/anaconda3/bin"
ExecStart=/bin/bash/home/yjtseng/anaconda3/bin/jupyterhub -f /etc/jupyterhub/jupyterhub_config.py

[Install]
WantedBy=multi-user.target
```

```
sudo pip install bash_kernel
sudo pip install octave_kernel
devtools::install_github(paste0('IRkernel/', c('repr', 'IRdisplay', 'IRkernel')))
IRkernel::installspec(user = FALSE)                 
```

## 參考資料

- [快速安裝架設 Jupyter Notebook 並修改主題樣式](https://jerrynest.io/install-jupyter-with-style/)
- [Jupyter Notebook Doc](http://jupyter-notebook.readthedocs.io/en/stable/)
https://github.com/jupyterhub/jupyterhub/wiki/Run-jupyterhub-as-a-system-service

https://stackoverflow.com/questions/45776003/fixing-a-systemd-service-203-exec-failure-no-such-file-or-directory