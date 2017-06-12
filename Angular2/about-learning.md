## 關於Angular 2 如何學習

---

### 適用對象 :

1. C\# 開發者 或 熟悉OOP的開發者 - 本篇會以此方向來解釋
2. 曾使用過 js 作網站開發者 - 可以不用jquery

---

### 本篇會提到的背景知識 :

1. typescript with decorator - 就是class 上面那一塊很像 attribute 的東西
2. reactive programming
3. DI - Dependency Injection
4. JS Module

### 本篇的目的 : 解釋Angular 2 看似Magic 的手法

### 本篇不會提到的事情 : 基礎教學 && 環境建置 , \(angular-cli 很好用 試一試吧 ~ XD\)

### 本篇不會做的事情 : 吸引你使用Angular 2 \(這種事情自己評斷即可\)

---

### Everything Start From Hello World

```
import { Component } from '@angular/core'
@Component{
  selector : 'app-root',
  template : '<span>Hello World</span>'
}
export class AppComponent {}
```

這是一個最簡單的 hello world \( 還有什麼module 的待會再說吧 \)

他的運作原理是這樣子

### Component :

Component 這樣的撰寫方式為TypeScript 的 [Decorator](http://www.typescriptlang.org/docs/handbook/decorators.html) ． 風格與C\# 的 Attribute 幾乎是一樣的 但是功能更為強大，在C\# 中我們如果要讓Attribute 能夠發揮功用，通常必須得額外使用其他方式主動存取Attribute，進而實現 [Decorator](http://www.dofactory.com/net/decorator-design-pattern) ，在JS中Decorator **其實只是一個Function ，**Hello World 的例子就是，Angular Engine 可以透過這樣的撰寫模式將你定義的所有不同類型的類別通通都變成名為Component 的物件

### Class & Object :

在OOP 中，我們會寫Class c = new Class \(\); ，在JS中我們也可以let c:Class = new Class\(\); 但最後的結果，基於JS並非強行別語言，所以產出的Object 是一個JSON的結構，所以接續1.最後的論述，只要JSON結構長的像Component的物件即可，所以無論是哪一種物件，Engine都可以透過你在參數中的設定將你的Class 變為 Component的結構，所以是完成度超高的Decorator模型，而在Angular中想要自己完成這件事情也很容易，程式碼如下：

```
let type = Component({selector:'app-root',template : '<span>Hello World</span>'})(AppComponent);
let component = new type();
```

Component Function 實際的結構為，帶入一個JSON Object，會得到一個新的Function，而這個新的Function可以讓你帶入任何的Class Type 然後得到這個Class Type 的 可以被 new 的變數，最後再new 此變數而得到Component，如果要寫成C\#的模式，大概會長這樣 Func&lt; IComponent, Func &lt; T, Func &lt; object\[\], IComponent &gt; &gt; &gt;因為C\# class 無實體物件，所以用Func&lt;object\[\],IComponent&gt;  
來代替走到這裡無論是 [React](https://github.com/facebook/react/blob/master/src/isomorphic/classic/class/ReactClass.js) or [Vue](https://github.com/vuejs/vue/blob/dev/src/core/vdom/create-component.js) 他們最後也會變出像是這樣的物件來對於View 作Render

### MVC & 模組化：

為何Angular 藥用這樣子的手法來結合Component ，這點我們可以從前述的Component 實踐程式碼來觀察，前面因為Component Object 與Class 並沒有直接的相依性，所以無論另一邊怎麼抽換都沒有關係，所以在Angular 2 中可能你以為每一種新的Component 就得創造一個新的類別含一堆文件，其實沒有這回事，像是HelloWorld 這樣的Class 無用途的Component ，其實可以透過Factory 來建立，像是這樣子

```
function(component:IComponent) {
   return Component(component)(Class {});
}
```

Class {} 是匿名Class ，如同C\# 只是JS的Class 是有實質物件的，所以我們可以在TypeScript 中這樣寫，回傳型別為[Type&lt;any&gt;](https://github.com/angular/angular/blob/master/modules/%40angular/core/src/type.ts)，

```
interface Type<T> { new ():T; }
```

這個interface 所描述的意思就是　你可以對型別為Type&lt;T&gt;的變數 type 作 new type\(\) 的行為　並且得到結果T

當我們可以自由的去選擇Component 與其對應的Class 的組合時，MVC的概念就會慢慢浮現，Component本身就代表了View，而Class 則是Controller，至於為何要用Decorator 的方式來表現，我覺得有很大的原因是上手難度問題，而且用單一文件來表達單一Component 且撰寫上緊密結合使用上鬆散耦合的模式，算是很顧及全盤了．

而且單一文件表現單一Class 是在C\#以存在的原則，並且也符合[功能單一原則](https://zh.wikipedia.org/wiki/%E5%8D%95%E4%B8%80%E5%8A%9F%E8%83%BD%E5%8E%9F%E5%88%99)，這樣的模型雖然初期可能會覺得雜亂，在維護時期上的好處可是多很多的

### Dependency Injection : 相依性注入

未完成

### Reactive Programming & RxJS : 響應式編程

未完成

### About JS Module :

未完成

