### Postfix - 後綴 \(以角色作為命名\)

---

* Content - 內容,通常指靜態內容
* Context - 上下文,描述當下情況,提供當下情況給多個服務
* Factory - 工廠,提供特定項目或服務的建立與回收
* Monitor - 監視器,提供多種監控項目
* binder - 連接,通常指不會中斷的連接\(connect會中斷\)
* Descriptor - 描述,對單一服務的靜態描述
* DecisionTree - 決策樹,
* Provider - 提供,提供指定項目
* Classifier - 分類,負責分類項目
* Activator - 啟動,啟用某個特定項目,
* Builder - 建立,建立某特定項目,
* formatter - 格式化,將訊息轉為特定格式
* Invoker - 調用,調用單一特定Method,
* Parser - 解析,解析特定資訊轉為另一個類型
* Executor - 執行,執行某特定流程
* Visitor - 造訪,可對項目做階層式的檢閱並提供服務
* Handler - 監控,對單一項目產生觸發點時提供CallBack
* Accessor - 訪問,通常具有getter與 setter
* Manager - 管理,管理特定項目,通常為該項目集合
* Part - 部分
* Callback - 回呼,用於特定觸發點被引動時的method
* Convention - 慣例,指通常會做的事情
* Collection - 集合
* Result - 結果,通常只結果狀態\(不一定是結果內容\)
* Entry - 入口
* Source - 來源,指特定項目所需要的來源資源

### 命名的行為描述

---

* Context 是具有上下文關係的內容 而 Content 是靜態內容
* Factory 通常是利用已知的 Provider 集合 以及可用於Provider的參數來Create 對象
* Convention 會預設對象的行為 , 而Configuration 則為對象要設定的參數, 所以Convention有可能使用Configuration作為預設
* Source 提供對象於建立時所需要的資源, 而Provider 則提供那個對象
* Result 紀錄執行結果, 如果有回船值也會記錄在這
* Callback 是執行時遇到各種觸發點時所需要執行的 function/method
* Monitor 可以定義指定觸發點所需要執行的 function/method
* Descriptor 會盡可能描述對象的各種方法 成員 及 狀態 以作為其他運用
* Provider 會具有對象的一種取得方式, 如果取得來源很多 那通常會有多個 Provider
* Activator 可以啟用對象 , 而當然啟用的對象可能會啟用更多的對象
* Builder 通常可以記錄建構的資訊,且藉由這些資訊來建立對象
* Invoker 可以Invoke指定目標
* Excutor 執行一個預定的流程或工作
* Handler 會在觸發點要執行Callback 時去Invoke Callback
* Manager 往往可以對於對象做全面的操作與監控
* Accessor 可以對對象做存取
* Visitor 會使用層級式Visitor Pattern 
* Parser 會建立一個由目標為基礎轉換的新對象
* Formatter 會建立一個以目標為基礎做格式上變化的新對象



