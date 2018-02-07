## 和過去的程式寫法差在哪裡 ?

---

Next/Error/Complete 這樣的模型其實與我們在Method 中使用 Try/Catch/Finally 或 Execute/Throw/Dispose是非常相似的，

所以我們在思考如何運用Rx 的時候，其實並沒有所謂的特殊情況或較為合適的情況，甚至是可以假想成ＦＰ在ＯＯＰ上的一種實現，

所以才會有所謂的響應式編程 : 以下為轉換範例 :

我們可能會寫一個ServiceBuilder 像是SomeBuilder 這樣子透過BuilderOptions來設定Service 應該如何Build，並且在不再需要Builder的時候可以Dispose

```
public class ServiceBuilder : IServiceBuilder,IDisposable
{ 
    BuilderOptions options; 
    public ServiceBuilder(BuilderOptions options) 
    { 
        this.options = options; 
    }

    public Service Build() 
    {
        // build something...
    }

    protected Dispose(bool isDispose) 
    {
        //dispose callback or alert
        //dispose member...
    }

    public Dispose() 
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
}
```

如果把剛剛的ServiceBuilder寫成Method 的模型就會變成GetService，透過參數BuilderOptions來取得Service，並且透過Try,Catch,Finally來handle Exception

```
public Service GetService(BuilderOptions options)
{
    ServiceBuilderFactory factory = new ServiceBuilderFactory();
    try
    {
        return factory.CreateBuilder(options).Build();
    }
    catch(Exception e)
    {
        throw;
    }
    finally
    {
        factory.Dispose();
    }
}
```

此時將Subscribe 的模型帶入GetService 將Service中的各區塊附加到　Subscribe上面

```
public class Subscribe<T> : IObserver<T>
    { 
    public virtual Action<T> OnNext { get;set; }
    public virtual Action<Exception> OnError { get;set; }
    public virtual Action OnComplete { get;set; }
    void IObserver<T>.OnNext(T t) => this.OnNext(t);
    void IObserver<T>.OnError(T t) => this.OnError(t);
    void IObserver<T>.OnComplete(T t) => this.OnComplete(t);
}

public void GetService(BuilderOptions options,Subscribe subscribe)
{
    ServiceBuilderFactory factory = new ServiceBuilderFactory();
    try
    {
        subscribe.OnNext = options => factory.CreateBuilder(options).Build();
    }
    catch(Exception e)
    {
        subscribe.OnError = e => { throw e; }
    }
    finally
    {
        subscribe.OnComplete = () => factory.Dispose();
    }
}
```

最後再以一個簡單的先後執行順序的ActionMerge來將從Subscribe來的與原本的邏輯重新執行過，最後在繼承Subscribe來些受這些合併後的Action，這樣就可以在GetService時，同時達到監控的效果，最後再進而將監控與實際回傳的Service邏輯合併後就是Rx了

```
public class ObserverSubscribe<T> : Subscribe<T>
{
    private IObserver<T> observer;
    public ObserverSubscribe(IObserver<T> observer) 
    {
        this.observer = observer;
    }
    public override Action<T> OnNext 
    { 
        get => base.OnNext;
        set => ActionMerge(observer,value);
    }
    public override Action<Exception> OnError 
    { 
        get => base.OnError;
        set => ActionMerge(observer,value); 
    }
    public override Action OnComplete 
    { 
        get => base.OnComplete;
        set => ActionMerge(observer,value); 
    }

    public Action<T> ActionMerge(Action<T> a,Action<T> b) 
    {
        return t => 
        { 
            a(t);
            b(t);
        }
    }
}
var subscribe = new ObserverSubscribe<BuilderOptions>(new UserObserver<T>());
var service = GetService(new BuilderOptions(),subscribe);
```



