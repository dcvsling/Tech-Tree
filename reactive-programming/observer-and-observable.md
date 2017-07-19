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



