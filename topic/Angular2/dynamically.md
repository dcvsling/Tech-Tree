# 動態產生元件

---

Angular 2 中如果我們想要動態產生Component , Directive 甚至是 module 與過去相較最大的問題在於

如何處理Decorator 的 static value

### 關於Decorator

於Angular 2 中，Decorator 不是Magic ，而是一個Function

詳細可以參考 [TypeScript官網](http://www.typescriptlang.org/docs/handbook/decorators.html) 所以我們如何建立一個包含Decorator的Object

方法其實很簡單，以Component為例子

    @Component({
        selector:'app-root',
        templateUrl:`root.component.html`
    })
    class RootComponent { }
    //--------------------------------------
    Component({
        selector:'app-root',
        templateUrl:`root.component.html`
    })(class RootComponent { })

在上述的例子中，上半段與下半段一個為靜態程式碼，另一個則生成Type&lt;RootComponent&gt;

這樣我們就可以提供Angular Engine 所需要的Type&lt;any&gt; 這樣的物件

### 關於Type&lt;T&gt;

Type&lt;any&gt; 我們從原始碼上看到的是 interface new \(...args:any\[\]\):T

顧名思義為，我們可以寫 let t:T = new Type&lt;T&gt;\(\); 

但是在JS中可以達到上述的作法的東西不只有這個

例如 let C = class A{}; then let c:A = new C\(\); 也有同樣的效果

### 關於partial dynamic

如果你不想要完全動態，而是有幾個既定的模型只是有部分需要參數化時

基於JS 的Class 實際是由Function所建構而成

所以你可以在TypeScript 中這樣寫

```
PartialDynamicComponent(c:Component) {
    @Component(c)
    class PartialComponent { }
    return PartialComponent;
}
```

這雖然在物件導向的語言上看起來很奇怪，但是因為編譯成JS之後就會變成這樣

```
function PartialDynamicComponent(c) {
    var PartialComponent = (function () {
        function PartialComponent () {}
        return PartialComponent 
    }())
    return PartialComponent;
}
```

如果是這樣的話，其實也不會那麼奇怪了

