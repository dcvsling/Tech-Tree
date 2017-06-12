## .Net Core DI

---

.Net Core DI 是微軟繼Unity\(不是做遊戲的Unity\)之後，以Linq Expression為底層實作的DI Container．

namespace : Microsoft.Extensions.DependencyInjection\(.Abstractions for 抽象層\)

這個DI與其它的各家廠商的DI 皆不太一樣 \(實際上每一家廠商的DI 都各自有特性\)

基於主要語言提供廠商的原因，所以與Unity所表現的功能有非常大的不同，

各家廠商利用多樣化的註冊方法，來盡可能地符合開發者的開發需求

而微軟的這個DI\(我習慣稱它為微軟DI or Microsoft DI\)則是更接近一個 [Pure DI ](http://blog.ploeh.dk/2014/06/10/pure-di/)的概念

連結中這位大師是近期我很推崇的一位

有關他在[PluralSight上的SOLID教學](https://app.pluralsight.com/library/courses/encapsulation-solid/table-of-contents)也是我很推薦的必看教學 \(**前面那一個半小時後才是最最最關鍵的重點**\)

所以，在Microsoft DI中我們只會看到很簡單的LifeTime與Constructor 的設定，

而Microsoft DI 僅提供了Constructor Injection 的功能，他利用Linq Expression 串接每個物件的建構式，

做法與[Composition Root](http://blog.ploeh.dk/2011/07/28/CompositionRoot/) 是一樣的模式，而且通常各家廠商也都會提供Constructor Injection的實作

然而其實僅僅是單純的Constructor Injection ，就足以幫我們省下許多的心力，後續會在介紹如何實作

## Add & GetService? vs Register & Resolve

---

這部分也許只有用習慣了過去的DI Container才會有些許的不適應症狀，

.Net Core 將所有的Object 皆當成提供服務的Service

所以，Register 也就是加入服務\(AddService\)

而Resolve 就是取得服務\(GetService\)

Microsoft DI在GetService 採用了過去已經存在的 IServiceProvider 這是一件非常有趣的事情\(至少我覺得很有趣 XD\)

