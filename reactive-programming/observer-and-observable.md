## Observable vs Observer

---

```
interface IObserver<T>
{
    void OnNext(T t);
    void OnError(Exception e);
    void OnComplete();
}

interface IObserable<T>
{
    IDisposable Subscribe(
        Action<T> onNext,
        Action<Exception> onError,
        Action onComplete
    );
}

Observer.Create<T>(Action<T> onNext, Action<Exception> onError = null, Action onComplete = null);

Observable.Create<T>(Func<IObserver<T>,IDisposable> observer);
Observable.Start<T>(Action<T> action);
Observable.Subscribe<T>(
this IObservable<T> observable,
Action<T> onNext, 
Action<Exception> onError = null, 
Action onComplete = null
);
```

以上為來自於Rx.Net 的Method

Observer Create 為建立某個方法的參數推入行為監控

ex: t =&gt; { ... } 的行為變成 t =&gt; Observer =&gt; { ... }

Observable Create 為 接收Observer 所送出的值

ex:

如果依照上述的Method來加以推論

```
class Subscribe<T> : IObserver<T>
{
void OnNext(T t){ 收到T }
void OnError(Exception e) { 收到發生的錯誤 }
void OnComplete() { 收到完結的訊號 }
}
```

IObserver&lt;T&gt; observer = new Subscribe&lt;T&gt;\(\);

每當Observer得到新的值時 就帶入OnNext 發生或收到錯誤時 就帶入 OnError 完成要結束時 就執行OnComplete

所以回頭看Observer Create即為 建立一個 IObservable的Subscribe Method 的實體

Observable Create 即為 建立一個完成Subscribe流程的實體

而關係會是像這樣 Observable \( Observer \)

那 Observable Start 則是 建立一個 Observable 以及 一個 Observer在 t =&gt; { } 上

最後我們把 t =&gt; { ... } 變為 t =&gt; Observer =&gt; { .. } 這樣的東西改成 t =&gt; Observer =/&gt; { ... }

這樣就可以將邏輯放入 Observer 中 並加以監控

---

### 是否應該要花時間去學會這個? \(請統一看[Introduction](/README.md) FAQ\)

~~我個人是覺得任何東西都至少要到 \[看得懂\] 或是 接近有需要就能很快看懂的狀況，以我自己的狀況，我會去了解它的概念並且了解期簡單Source實作法以驗證我是否真的了解，嘗試以自己現有的技能知識接軌，這對我而言是很重要的，這項技術也許不見得要完全會用，但完全會用的時候，你的邏輯概念就很厲害了，至於為何要去了解它實際運作，這裡舉一個案例~~

[~~ASP.NET Core FileProvider.Watcher~~](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/file-providers#watching-for-changes)

~~這裡的IChangeToken 如何去達成CallBack 以及 應該如何去使用才能達成自己所期望的情境，這也是上述概念中的部分來加以解釋的，而他已經與過去的　FileSystemWatcher的EventPattern 相當不同了，而且使用上這樣的作法可能會越來越多，也越來越自由的情況下，了解一項技術的基礎並從自己的技術延伸，如果我覺得這東西很接近我的專業領域的話，我通常會採取這樣的方式．~~

