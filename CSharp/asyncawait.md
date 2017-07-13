### 排程 Task & 非同步 Async / Await

---

Task : 是讓指定的Delegate 以另一個線程來執行.

async : 指定目標delegate 為存在非同步語法的方法，回傳值改變為Task & Task&lt;T&gt; \(這裡不建議以void 作為回傳值\(註1\)\)

await : 等待目標排程結束

所以使用法如下：

```
public void NonAsyncMethod() 
{
    Task task = Task.Run(() => DemoAction()); // 基本使用方法Task.Run中的Action or Func<T> 會在另一個線程完成
                                                        // 排程會回傳型別為Task的物件
    Task<object> resultTask = Task.Run<object>(() => DemoFunc()); //具有回傳值的非同步方法則用泛型指定回傳型別
                                                                          //具回傳值的排程型別為Task<T>
    resultTask.Wait();  //當需要回傳值的時候必須以Wait method 等待排程完成　否則通常都會拿到null (因為回傳值還沒回來)
    object result = resultTask.Result; //以Task.Result取得回傳值 
    
    // 無回傳值的Task 可以無需執行Wait方法
    // await task; 也同樣可以利用Wait　方法等待排程完成後　在繼續往下執行
}

async public Task InvokeAsync() //async 標示法可以寫在開頭,個人建議如已知包含非同步內容的method加入Async在name後面
{
    await DemoTask();                       // 於標示為非同步方法的方法中　可以用await 來取代 Task.Wait()
    var result = await DemoFuncTask();      // 亦可以使用await 來直接得到 Task<T>.Wait() 後的　Task.Result
    Task.Run(() => DemoTask())              // 於非同步標記的方法中亦可以使用Task.Run 來另開排程, 
                                               那此排程就會與當下的線程走不同線程
    await Task.Run(() => DemoFuncTask());   // 當然也可以宣告Task.Run之後用await等待,只是這與 await DemoTask()　並無差異
    result = await Task.Run<obejct>(() => { // 所以通常會要這樣用是因為使用了lambda 來臨時執行多個 method 
        DemoAction();
        return DemoFunc();                  // 這裡一樣可以回傳所需的回傳值
    });
    result = await Task.Run(DemoActionManyMethod); // 如果不想寫lambda 寫成Method 並帶入名稱也是可以的
    
    
    // 因為仍然沒有等待中途執行的　Task.Run(() => DemoTask()) 所以該牌程仍然在持續進行中
    // 以此可以做出類似MultiThread的效果 
}

// Property Value 為 Delegate時亦可以把Property當Method呼叫
// 當無回傳值的Task,且無標示async的時候,可回傳Task.CompletedTask 以作為完成工作的回傳Task
public Func<Task> DemoActionTask => () => Task.CompletedTask; 

// 無標示async 且具有Task<T>回傳值的方法可用Task.FromResult<T>(T t)來把結果回傳
public Func<Task<object>> DemoFuncTask
   => () => Task.FromResult<object>(new object())
  
// ConfigureAwait(bool) 的意思是指設定是否此awaiter 需要留存上下文　預設為false（沒寫亦為false）
// 這裡的上下文是指於排程建立時的那個線程的SynchronizationContext之類 會方法執行前後有關連性的東西
// ex: TraceStack (呼叫堆疊),如果ConfigureAwait(true) 時　可以看到最前面的堆疊　反之指能看到排程開始時的堆疊
// 建議ConfigureAwait(false)主要還是效能首選考量
// 但如因為不留上下文而讓開發時難以追蹤問題的話　不訪設定變數來統一開關這個flag 也不失一方法
public void ConfigAwaitDemo() => Task.Run(DemoAction).ConfigureAwait(false);　　

// 以下demo 用示範用例
public void DemoActionManyMethod() {
DemoAction();
DemoAction();
DemoAction();
}
public Action DemoAction => () => {}
public Func<object> DemoFunc => () => new Object();

```





