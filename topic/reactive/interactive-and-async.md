# Interactive & Async
---

## 為流程而產生的擴展
---

事實上前篇所提到的 Do 在進一步的擴展中就出現了

也就是 [Interactive Extensions (Ix.Net)](https://github.com/Reactive-Extensions/Rx.NET/tree/develop/Ix.NET/Source)

在Ix.Net中除了仍然保有 Iterator , yield　等多項功能外

裡面還有像是　Buffer, Generate, Throw, Using 等 Api 可供使用

如此一來就可以透過Ix.Net來達成更多流程且不會綁手綁腳的

而解決了就問題，勢必就會有新問題與需求產生

當我們來到非同步的環境中時發現一個大問題

基於IEnumerable<T> 的Linq 與 基於Task<T>的 async/await 似乎會衝突

這是什麼樣的狀況呢?

## IEnumerable<T> vs Task<T>
---

在理想中的非同步列舉行為應該是怎樣的呢？

應該是各自以不同步的方式依序迭代回傳

但 Task<IEnumerable<T>> 是等到全部都完成後才取得序列