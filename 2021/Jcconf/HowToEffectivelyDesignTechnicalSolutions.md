# 如何有效地建構技術方案？

### Speaker: Shihpeng Lin

2021/11/19

## 開閉原則的解讀

* Open/Closed Principle(OCP)是SOLID中，爭議最多的一條
* 什麼叫「不修改原始碼就可以擴展系統行為」？
* 從90年代到2017年之間，社群有許多關於OCP的討論，最終大家基本一致認同的解讀為：
    * 系統要能妥善預測複雜度的發生點，並在該處建立合適的擴展點(extension point); 以便當需要`改變/新增`行為時，能夠透過擴展點接受變更(open for predicated extension)。
    * 而原有主流程或使用該擴展點的client本身不需要修改(closed for client modification)。
    * [Fred聊聊SOLID設計原則](https://youtu.be/e0UOuQ_lCUY)

## 構思技術方案時的誤區

* 思考`僅侷限於「如何完成業務功能」`，並在其上堆砌各類工具、技術、模式
* `考慮維度顧此失彼`，例如認為高效能就是非同步、高併發就是加機器、開發時細心設計模型行為造成工時過長等。
    * 街口: 高併發是資料庫貧頸，而非加機器。
* 各維度都考慮到了，但`沒有考慮各維度未來的演化空間`。
* `技術能力與視野不夠`，不知道如何能有更佳解。

## 非功能性需求限制

* 效能: app使用者對互動的等待時間極限是8秒，超過12秒直接放棄。
* 可伸縮性: 多容易使系統能乘載更大流量，間接影響組織收益
    * e.g., stateless,
* 可用性: 系統可用時間，直接影響組織收益
* 可過展性: 多容易添加新功能，直接影響服務競爭力。
* 可觀測性: 多容易可以觀測到系統/業務健康度，直接影響可用性。
* 安全性: OWASP Top10已是業界標準，任何圈線都會讓系統暴露於風險中，影響組織業務。

## 其他考量

* 可演化架構: evolutionary architecture強調系統的每個非功能性需求維度都應該保持一定的可演化性。可演化性可透過適應性函數(fitness functions)來定義。
    * [Building Evolutionary Architectures： Support Constant Change](https://www.amazon.com/Building-Evolutionary-Architectures-Support-Constant/dp/1491986360)
* 組織發展階段: 不同企業在不同階段有不一樣的發展考量，會影響專案決定
* 產業特性: 金融、互聯網、零售、航運...etc.

## 功能性需求決定`能否取得`某項收益，非功能性需求與其他考量決定了`能取得多少`該項收益。

## 如何有效地建構技術方案？

1. 避免淺增思考
    * 在構思技術方案時，要有意識的避免僅著眼於功能性需求與`特定系統複雜度面向`，要盡可能納入更多的考量。
    * 開發業務需求時，考慮產業、未來變同可能來決定是否使用可擴展性更大的方案。
    * 開發系統功能時，需從使用場景、體量來考量是否需要高效能、高併發，徒增複雜度與開發成本。
2. 對需求/問題探索本質，瞭解衝突所在
    * 我們感受到的問題可能只是`問題本質所表現出來的現象`(Symptoms vs. Root Cause)
    * 對業務型需求問題，可以多問「我們的訴求是什麼？」，來深入瞭解業務上的需求本質。
    * 對技術型問題，可透過:
        1. 刻意忽略實現細節
        2. 宏觀上思考更多背景
        3. 微觀上從觀察到的問題開始逐步追蹤，確認最大的限制、瓶頸為何來找到本質問題

3. 找出問題複雜度最核心的維度與最重要價值
    * 核心問題的特徵 ＊ 通常會是一句以「我們需要」開頭，不會太長的句子，例如:
        * 「我們需要一個能在授權前先確認使用者是否滿足限制的機制。」
        * 「我們需要一個在向機關方請款失敗時能告知業務方的機制。」
        * 「我們需要一個能讓業務方可以進行ad-hoc BI查詢的方式。」
        * 「我們需要一個充許10000用戶完成秒殺搶券的機制。」
            * 通常，這個問題可以與原始的技術和業務問題相互對應

    * 尋找核心複雜度與價值
        * 有了要解的問題後，即可再深入探索兩個問題:
            * `這個問題所對應的複雜度是什麼?`
            * `解決這個問題的最大價值為何?`
        * 找出所對應的複雜度後，即能對可使用的技術工具選型有更明確的指引
        * 問題的價值在於協助我們`確認問題解決的方向是對的`

    * Tip:
        * 如果是業務需求方面問題，`複雜度80%是在可擴展性(extensibility)上`。
        * 技術工具方面可優先考量`業務流程組件化、設計擴展點屏蔽業務變化部分`來處理。


4. 挑選能最佳解決該維度問題的技術工具
    * Tip：多看、多學

## 建議：專精產業領域

* 各個產業在各個發展階段對軟體/IT專案的核心問題與對應複雜度都不盡相同。
* 甚至物量網產業中，不同業務域的技術深度也差異甚大。
    * To C產品需要有`高併發、高可用`能力。
    * Fintech產品需要有很好的`安全性、資損防控`建設能力。
    * To B產品要有很好的`平台化、標準化`能力。
* `強烈建議工程師要嘗試在一個領域深耕並成為開領域的技術專家，才能在開領域給出更為優秀、專業、有價值的方案。`

## References

* [Jcconf 2021 - 如何有效地建構技術方案？ By 林世鵬](https://youtu.be/y6EFcpiEd9U)

