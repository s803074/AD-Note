# Domain規劃
## 備援機制
## 備份機制
## 安全性原則
## 權限配置
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
	* 指令：net user %username% /DOMAIN

* 如何更換DC的IP？
```
net stop netlogon	      #netlogon服務停止後才可修改IP
ipconfig /flushdns
net start netlogon
ipconfig /registerdns
- 刪除舊有dns紀錄
- 刪除站台紀錄
```

