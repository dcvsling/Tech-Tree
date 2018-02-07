# Angular 2 如何詮釋 MVC Pattern

### ng2 component sample :

```
import { Component,AppService } from '@angular/core'
@Component({
  selector: 'app-root',
  templateUrl: './root.component.html',
  providers:[ AppService ]
})
export class RootComponent {
  constructor(private service:AppService)
}
```

上面為常見的Component 寫法

而這樣的模型與常見的MVC模型看起來差別很大

但其實這是一個還滿明確的MVC Model

只是基於一些開發者慣性 進而演變而成的model

## 深入Angular 2 Api

所有的Compoent 於 NgModule 中都只能定義一次,但可以在每一個Module中使用多次

這是因為NgModule 為一個DI\(Dependency Injection\) Container 的設定模型

所以實際上Ng Engine 在讀取完NgModule之後會藉由這個模型所載入的Component 去部屬配置實際的Html

我們可以於@angular/core 中找到 ComponetFactory 與 ComponentFactoryResolver

ComponentFactoryResolver 可以透過Class 去找到對應的 CompoentFactory

再由ComponentFactory 去取得 ComponentRef

而ComponentRef 將會作為實際部屬Html的物件

所以Engine 藉由這樣的模式依序建構出使用者期望的網頁模型

所以 Component中的 Template 在MVC為View 的角色



而我們會在Class 中定義一些Property or Method 與View 作互動

所以我們可以透過Class 達到View and View , View and Service 之間的互動與溝通

所以Class 則扮演了 Controller 的腳色



而 在Components中的providers 為 Controller所需要的Service

除了import 外還需要將他寫在providers中的理由是 

讓Service 可以透過 Ng2 Engine 幫我們Inject到constructor中 \(用Inject的理由 請參考使用DI的理由\)

而Service 比較常見的作用亦是 各種model 的處理 取得 等等

所以 Service 則是扮演了 M 的角色 而Component的providers只是inject的跳板

### MVC Pattern :

我們在撰寫常見的MVC 模型時 View 與 Controller是有很明確的分離

但是在angular 2 的這個架構模型中 View 與 Controller 看起來是黏在一起的

而其實angular 2 有準備好讓Controller可以自由控制View的Solution

@ViewChild - 讓Controller 可以取得Template 的 Object 

在Dynamic Component 的實作上 會將ViewChild 取得的物件以ViewContainerRef呈現

ex: @ViewChild\(name, {read : ViewContainerRef}\) name:ViewContainerRef

ViewContainerRef 裝載了Component中的 實際Element 實體與結構

所以我們可以透過這個Object 去操作 View 的實際呈現方式

如此一來就可以由Controller 來決定Model 與 View 之間的互動

### 改變 !? : \(以下個人見解\)

至於為何不與過去的MVC模型採用一樣的方式

以WebForm 與 MVC Framework 來當作案例

在WebForm 上其實一直有個 MVC 難以取代的地位

就是如果我們不需要複雜的頁面邏輯時 MVC就會顯得過度複雜

而且我們也並非在每一層都需要具有這樣程度的複雜邏輯

所以 這樣的Model 可以讓我們自由的去選擇

當我們需要使用MVC 的模型時 再將View 拉出來做控制

不然就像一般的 static page 一樣去輸出即可

而且藉由Controller 為Class 的特性

讓Controller 具有繼承 封裝 等等的屬性 且更易於與Service溝通

這不但可以符合 初階開發者的 convension 也可以提供進階開發者較大的發展空間

所以我個人認為這樣的改變絕對是可以符合更多的開發者所使用的model

