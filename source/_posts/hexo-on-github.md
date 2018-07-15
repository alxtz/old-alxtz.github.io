---
title: 如何在 GitHub 上架一個 Hexo 部落格
date: 2018-07-15 19:51:33
tags:
---
#### 大綱
- 建一個新 repo
- 建立 hexo 環境
- 開啟 hexo-server
- deploy 到 GitHub


# 創建新的 GitHub repo
首先，點進 https://github.com/new 來創建一個新的 repo

![](https://i.imgur.com/ciOa1ze.png)

這邊要注意，repo name 一定得是 `${username}.github.io`

像是我剛創的使用者帳號為 `hexo-demo`，這個 repo 一定得叫 `hexo-demo.github.io`

![](https://i.imgur.com/2mEKjYG.png)

按下 create 後，這個 repo 就先放著，後來會用到

![](https://i.imgur.com/J4F9MZ4.png)

# 建立 hexo 環境

網路上其實已經有不少 [gist](https://gist.github.com/btfak/18938572f5df000ebe06fbd1872e4e39) 介紹要用哪些 command 來安裝 hexo 了

> [環境]
> `nodejs=9.9.0`
> `npm=5.6.0`
> `yarn=1.6.0`

1.  `npm install -g hexo-cli@1.1.0`

    ![](https://i.imgur.com/Ld5AuIR.png)

2.	安裝完後，可以使用 `hexo -v` 來試試看是否有裝到正確的版本
		![](https://i.imgur.com/wZznxtf.png)

3.  `hexo init <資料夾名稱>`
    ![](https://i.imgur.com/8ePCyYS.png)

4.  執行成功的結果
    ![](https://i.imgur.com/J1RVFK1.png)

5.  `cd BLOG_HEXO_DEMO`, `ls`
  ![](https://i.imgur.com/6yEpq5l.png)
	進到剛剛創建的資料夾來看看。
	如果有仔細研究 `4.` 的執行結果的話。
	可以發現 hexo 預設使用的是 yarn，並且已經先執行了 `yarn` 來裝好所有 dependency 了。

# 開啟 hexo-server
1.  `yarn add hexo-server@0.3.2`
    ![](https://i.imgur.com/Hwrakia.png)
    hexo 本身的 hexo-server 和 hexo-cli 是分開的，所以要額外裝 ([doc](https://hexo.io/docs/server.html))

2.  將 hexo server 寫成一個 npm script

	 打開 `BLOG_HEXO_DEMO` 底下的 package.json，新增一段 npm script
	 ![](https://i.imgur.com/MbCLgDS.png)

3.  `yarn run dev`
  ![](https://i.imgur.com/tXrCVR5.png)
  執行後可以開啟 hexo-server，url 為 `https://localhost:4000`。
	打開瀏覽器就可看到預覽內容。
	![](https://i.imgur.com/oWtKLoP.jpg)

# deploy 到 GitHub
[參考文件](https://hexo.io/docs/deployment.html)

1.  修改 `BLOG_HEXO_DEMO` 底下的 `_config.yml`
    ![](https://i.imgur.com/xafr7DV.png)
	找到 `deploy:` 這段，將他改成
    ```yml
    deploy:
      type: git
      repo: https://github.com/hexo-demo/hexo-demo.github.io
      branch: master
    ```
    url 請使用先前創建的 repo url
    ![](https://i.imgur.com/2ejvRof.png)

2.  init repo
    `git init`
    ![](https://i.imgur.com/lCnZSEb.png)
    用 quick & dirty 的方式將所有檔案加進 commit 裡
    ![](https://i.imgur.com/HnbFhoH.png)

3.  推上 dev
    為了後續好管理，我決定使用 dev 來當作放 blog source code 的 branch。
    而 master 已經在先前設置過，將會當作顯示 hexo 網頁用的 branch。
    ![](https://i.imgur.com/8etnI7X.png)
    用 `git remote add GitHub https://github.com/hexo-demo/hexo-demo.github.io`
    和 `git push GitHub dev` 推上去即可
    ![](https://i.imgur.com/JDvWqc1.png)
    可以看到我們 blog 的 source code 已經推上 GitHub 的 dev
    ![](https://i.imgur.com/fMiv9SR.png)

4.  在 `package.json` 裡新增另一段 script
    `"deploy": "hexo clean && hexo deploy"`
    ![](https://i.imgur.com/4OlaqKE.png)

    接著 `yarn add hexo-deployer-git`
    有了 `hexo-deployer-git`，`hexo deploy` 才能使用 git 自動 push 上 master

    ![](https://i.imgur.com/jWzv8jf.png)

5.  `yarn run deploy`
    ![](https://i.imgur.com/yFXnttk.png)
    接著上剛剛 repo 的 `master` 看，會發現 hexo 生成的 html, css 都已經被自動推上去了。
    ![](https://i.imgur.com/yYMxuHQ.png)
    打開 `https://hexo-demo.github.io/`
    就可以看到我們剛生成的部落格。
    ![](https://i.imgur.com/eLyWjte.jpg)

# 參考資料

hexo 有非常不錯的[官方文件](https://hexo.io/docs/setup.html)
這篇文章 demo 使用的 [repo](https://github.com/hexo-demo/hexo-demo.github.io)
demo 結果 [https://hexo-demo.github.io/](https://hexo-demo.github.io/)
