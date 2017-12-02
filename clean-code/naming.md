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



### 關聯性 

---

* Context has Descriptor , ex: ActionContext has ActionDescrptor
* Factory init by Providers , then Factory use Command Pattern to select Provider and Create item
* Part always mean an item in a bigger item like type is part of assembly, property is part of class
* Convention is always do something when do one behavior, like exception handler must exist anytime
* Source is an object with some information for some behavior need,
* Result is an answer flag , and may have result data
* Callback is invoke by trigger
* Monitor can set many trigger
* Descriptor to Describe information like Content
* Provider is provide one kind of item to create, and an item may have many Providers
* Activator to active a service and service may need to create many other items and composite them
* Builder need to config before build
* Invoker is invoke one method and get result if has
* Excutor is do many inner stuff then expose result
* Handler has a Callback and wait trigger to use
* Manager has some Collection to management
* Accessor can get or set one item
* Visitor can get deeper in any level and use them
* Parser need to analyse data than change to different type data
* Formatter will write exist info to another one
* Classifier will mark category any item
* DecisionTree will use category to decision
* binder wont disconnect in runtime , but connect will



