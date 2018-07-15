---
title: 在 Hexo 裡加入 Applo Theme 和 Gitalk
date: 2018-07-16 01:06:25
tags:
---
註: 前面的 setup 可以參考 https://alxtz.github.io/2018/07/15/hexo-on-github/

大綱:
- 安裝 Applo theme
- hexo theme 魔改原理
- 加入 Gitalk

# 安裝 Apollo theme
因為原版的 [hexo-theme-apollo](https://github.com/pinggod/hexo-theme-apollo) 已經不維護了。
這邊將會使用由我自己 fork 的版本 [hexo-theme-apollo-pug](https://github.com/alxtz/hexo-theme-apollo-pug)

1.  進到你之前創的 hexo 資料夾，執行
  `yarn add hexo-renderer-pug hexo-generator-feed hexo-generator-sitemap hexo-browsersync hexo-generator-archive`
	![](https://i.imgur.com/U3zCNGw.png)

2.  `git clone https://github.com/alxtz/hexo-theme-apollo-pug themes/apollo`
  ![](https://i.imgur.com/5Lgnmph.png)
	在 hexo 裡，新增的 theme 都是以資料夾的形式存在，並放在 `theme/` 這個資料夾內
	![](https://i.imgur.com/9523XzG.png)


3.  回到 `_config.yml`，找到將 theme 改成 `apollo` 即可
    ![](https://i.imgur.com/jXVspDh.png)

重新執行 `hexo server`，就可以看到啟用新 theme 的 hexo 了
![](https://i.imgur.com/iqog3ut.png)

# hexo theme 魔改原理
在 hexo 裡，layout 都是由各個不同的 theme 來改動的。
現在我們想在每篇文章底下都加入 Gitalk，其實就是要改動每篇 Apollo 文章所使用的模板。

1. `cd themes/apollo/ && rm -rf .git`
   因為先前我們是透過 `git clone` 的方式來建立 `themes/apollo/` 的。
   之後要 commit 相關改動時，如果 `BLOG_HEXO_DEMO` 底下有資料夾包含了 `.git`，會被誤認為 git submodule。
   ![](https://i.imgur.com/A44QD5v.png)
   相對的，如果你想要徹底魔改你的 theme，推薦是創成另一個 git submodule 丟上 github 維護。

2.  打開 `themes/apollo/layout/post.pug`
    可以看到他是專門用來定義每篇 post 的模板。
    這邊可以發現他早已定義了 `comment.pug` 這個模板來給我們使用。
    ![](https://i.imgur.com/se7wwBb.png)

3.  打開 `themes/apollo/layout/partial/comment.pug`
    裡面有些邏輯是之前的 theme 所使用的。
    ![](https://i.imgur.com/30ZArbk.png)
    這邊的內容我們全部都要替換成 Gitalk，待會會來做改動。

# 加入 Gitalk
這邊我是參考 [Gitalk 官方 wiki](https://github.com/gitalk/gitalk/wiki/%E5%9C%A8hexo%E4%B8%AD%E4%BD%BF%E7%94%A8gitalk) 的內容。
不過同樣也有點年久失修，而且是 ejs 版。
所以以下一樣會做介紹。

1.  打開 `themes/apollo/layout/partial/head.pug`
    加入 Gitalk 所需要的兩個 CDN
    `https://unpkg.com/gitalk/dist/gitalk.css` 和 `https://unpkg.com/gitalk/dist/gitalk.min.js`

    ```pug
    link(rel="stylesheet",href="https://unpkg.com/gitalk/dist/gitalk.css")
    script(src="https://unpkg.com/gitalk/dist/gitalk.min.js")
    ```
    這樣會在部落格的每個頁面都引入 Gitalk 所需要的 css 和 js。
    ![](https://i.imgur.com/osw3xZt.png)

    需要 debug 的話，可以打開 dev tool 看是否有真的被引入。
    ![](https://i.imgur.com/mnLTQsx.png)

2.  Gitalk 本身的用法並不難，以下 12 行 script 就足以加入。
    不過在使用前，得先去讓給予 Gitalk 權限，讓他可以在我們的 repo 上開 issue。
    ```html
    <script type="text/javascript">
      var gitalk = new Gitalk({
        clientID: '',
        clientSecret: '',
        id: window.location.pathname,
        repo: '',
        owner: '',
        admin: '',
        distractionFreeMode: false
      })
      gitalk.render('gitalk-container')
    </script>
    ```
3.  點開 https://github.com/settings/applications/new
    Application name 可以任意填。
    Homepage URL 和 Authorization callback URL 都要輸入你的 blog 網址。
    ![](https://i.imgur.com/X0PWA9w.png)
    創建完後，就可以來設置 Gitalk 了
    ![](https://i.imgur.com/3EJku9Q.png)

4.  改動剛剛介紹的 `comment.pug`，內容改為
    ```javascript
    #gitalk-container

    script.
      var gitalk = new Gitalk({
        clientID: '',
        clientSecret: '',
        id: window.location.pathname,
        repo: 'hexo-demo.github.io',
        owner: 'hexo-demo',
        admin: ['hexo-demo'],
        distractionFreeMode: false
      })
      gitalk.render('gitalk-container')
    ```
    `clientID` 和 `clientSecret` 請填入剛剛創建完頁面所取得的資訊。
    `repo` 填入 GitHub repo 的名稱。
    `owner` 和 `admin` 填入你的使用者名稱 (註: admin 是一個陣列)。
    ![](https://i.imgur.com/iTP8VQJ.png)

5.  改完後就可以看到 Gitalk 被加入了 (可能會需要重開 hexo server，或重新 generate)
    (在 hexo server 環境下會無法成功登入，要 deploy 上去才能使用)
    ![](https://i.imgur.com/9Lc1HOB.png)

最後結果：
![](https://i.imgur.com/HdP3BhO.png)

# 參考資訊
hexo theme https://hexo.io/docs/themes.html
Gitalk README https://github.com/gitalk/gitalk
demo https://hexo-demo.github.io/2018/07/15/hello-world/
