# Domain規劃
## 規劃前置
* 了解現有環境
* 了解目標需求
	* 企業需求：關於公司管理層的需求
	* 技術需求：如高可用性、虛擬化、安全性等

## 安全性規劃
* 若技術能力允許，Server建置應採用Server Core 安裝
	* 具有較高安全性
	* 降低資源消耗
	* 系統更新次數降低，減少重開機次數
* BYOD設備，應無法帶走來自公司的任何資源，離開公司環境就不能下載 瀏覽 分享，即便已下載的檔案離開公司就無法開啟
* 行動裝置，網域無法控管，則需要其他第三方應用來介入管理
漏洞降低
## 備援機制
## 備份機制
## 安全性原則
## 權限配置
## 前置準備
1. 想好一個網域名稱(與對外服務不要相同，會有較佳的擴展性)
2. 有一台具SRVRR及動態更新的DNS或直接在DC上安裝
3. 想好目錄服務還原模式密碼及管理者密碼
4. 確認AD服務需要的Port是否都已開通防火牆
## 安裝流程
1. 時間校正
2. 更改主機名稱
3. 防毒軟體安裝
		* 更新防毒軟體
		* 排程定期掃描
		* 關閉個人防火牆
4. 系統更新
5. 驅動安裝
6. windows啟用指令
```
slmgr -skms kmsservername或IP	           #向kms註冊(kms server 數量未達25會顯示計數不足)
slmgr -ato
slmgr /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX	#以mak方式輸入序號
slmgr /rearm	                            #延長試用時間，每次30天，最多5次
```

7. 安裝ADDS
8. 升級為網域控制站
9. DNS轉寄站加入外部DNS
---
## 常見問題
* 為什麼account operator 不能互改對方密碼？
	* 權限不符，只有更高位管理者可管理下方管理者的資料，同級管理者無法互相管理
  
* 如何調整account operator的加入使用者網域的次數限制？
	* adsi>預設命名內容>DC=XXX,DC=OOO>內容>屬性編輯器>ms-DS-MachineAccountQuota 改大就行了

* 如何查詢網域使用者的相關資訊(上次登入、密碼到期…等)？
```
net user %username% /DOMAIN
```

* 如何手動指令複寫
```
repadmin /syncall /e /A /P /d
```
* 如何確認AD複寫狀態是否正常
```
repadmin /replsum
```

* 如何更換DC的IP？
```
net stop netlogon	      #netlogon服務停止後才可修改IP
ipconfig /flushdns
net start netlogon
ipconfig /registerdns
- 刪除舊有dns紀錄
- 刪除站台紀錄
```

* 如何強制移除DC？
```
Ntdsutil				
Metadata cleanup
Connections				#準備連線
connect to server "servername"		#連線執行指令的server(本機的DNS name)
quit
select operation target			#選擇操作目標
list domains				#列出所有網域
select domain "number"			#選擇網域代號(數字)
list sites 				#列出目標(移除dc)所屬站台
Select site "number"			#選擇站台代號
list servers in site			#列出在此站台所有dc
Select server "number"     		#選擇移除目標dc代號
quit
Remove selected server			#執行移除動作
quit
- 進到DNS Console，移除舊dc的所有a紀錄及cname紀錄及_msdcs等目錄中的舊dc紀錄
- 進到ad站台與服務Console，移除舊dc所有紀錄
```
* 如何指令進行五大角色轉移？
```
ntdsutil 
roles
connections
connect to server mia-dc01@mia.local
quit
fsmo maintenance: seize schema master
fsmo maintenance: seize naming master
fsmo maintenance: seize PDC
fsmo maintenance: seize RID master
fsmo maintenance: seize infrastructure master

#查詢五大角色位置
netdom query fsmo
```

* 例行性維護應注意什麼？
	* 是否有相依的LDAP服務，可能造成驗證失敗
	* 是否有相依的DNS服務，可能暫時無法解析DNS
	* DHCP是否主動派發DNS資訊，可能暫時無法解析DNS
	
	

