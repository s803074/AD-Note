# AD-Note
## AD的Directory Partition有哪些？
* Schema Directory Partition
  * 儲存整個樹的物件定義
  * 整個**樹**共用此Partition
* Configuration Directory Partition
  * 儲存整個樹的目錄結構
  * 整個**樹**共用此Partition
* Domain Directory Partition
  * 儲存整個網域的物件
  * 整個**網域**共用此Partition
* Application Directory Partition
  * 由應用程式自行建立
## AD如何複寫？

* multi-master replication mode
> 例如
>> 物件建立
* single-master replication mode
> 例如
>> 網域建立
* 同一站台下，ADDS資料庫**快速複寫**、**不會壓縮**
* 不同站台下，ADDS資料庫**排程複寫**、**進行壓縮**
* 複寫使用的通訊協定
  * RPC over IP
    * 可用於同站台或跨站台間複寫，具驗證身份及資料加密
  * SMTP
    * 僅支援站台間的複寫，適用於網路品質不佳的環境
    * 不能複寫網域目錄分割區
    * 需Enterprise CA憑證
* 複寫時間

|資料類型|更新頻率|發佈至|
|----|----|----|
|ADDS資料庫(同一站台)|異動後15秒<br>未異動每小時|Direct replication partner|
|ADDS資料庫(不同站台)|每3小時|Bridgehead|
|群組原則|異動後15秒|PDC|
|緊急複寫<br>- 帳戶鎖定<br>- 帳戶鎖定原則異動<br>- 密碼原則異動|立即|Direct replication partner|

## 複寫拓撲如何建立？
* 由每個DC中的KCC(Knowledge consistency checker)程序來自動建立
* 來源DC至Direct replication partner至Transitive replication partner的hop count不應超過3台
* 來源DC與每個Direct replication partner複寫皆有3秒的間隔，不會一次全部推送
* 每個目錄分割區有自己獨立的複寫拓撲
> 如何查詢不同分割區的複寫拓撲？


