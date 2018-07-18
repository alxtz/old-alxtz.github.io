---
title: "[Daily Oops] 取消輸入框的 spellcheck 和 autocorrect"
date: 2018-07-18 20:48:55
tags:
---
![](https://i.imgur.com/gII3aQi.png)

在某些 case，瀏覽器會去呼叫 OS 內建的拼字檢查或錯字校正 API，並在輸入框提醒你打錯字了。

今天被設計師提醒，才想到這功能也許能關閉。

在使用 `disable spell check input` 搜尋後，很快的找到 HTML 有三個屬性可以操作這類行為。分別是

- **`autocorrect="off"` 可以避免自動完成**
- **`autocapitalize="off"` 可以避免自動首字大寫**
- **`spellcheck="false"` 可以避免在拼字錯誤顯示紅點**

# 參考資訊
[spellcheck](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/spellcheck) - MDN
[input](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input) - MDN
[How can i disable auto text correction?](https://stackoverflow.com/questions/3496658/html-how-can-i-disable-auto-text-correction-in-my-textarea) - Stack Overflow
