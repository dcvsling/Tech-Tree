## 從IoC的角度來看Observable ,Observer 與 Rx

---

```
public interface IObservable<T>
{
    IDisposable Subscribe(IObserver<T> observer);
}
```

這是Observable 與 Observer 所定義的關係

所以

From Some IObservable&lt;T&gt; maybe return oo.oo.oo.IObservable&lt;T&gt;;

IObservable&lt;T&gt;.xx.xx.xx.Subscribe\(IObserver&lt;T&gt;\)

其實就是

```
public class MyClass<T> : IObservable<T>
{
    private IObservable<T> observable;
    public Observable(IObservable<T> observable)
    {
        this.observable = observable;
    }

    IDisposable Subscribe(IObserver<T> observer)
    {
        observable.xx.xx.xx.Subscribe(observer);
    }
}

new MyClass<T>(oo.oo.oo.IObservable<T>);
```



