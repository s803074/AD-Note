# AD-Note
![image](https://github.com/sciloa62/AD-Note/blob/master/picture/ad0001.png)
* 原因：AD物件因為Exchange Active sync物件鎖定所致，應把該物件刪除

* 解決方法：因為實際環境Exchange裡早就無此物件，只能從ADSI中強制提權，再刪除

	1. ADSI編輯器>右鍵"連線到">確定
	2. 展開預設命名內容[dc.domain]
	3. 展開目標所在OU，直到找到目標物件CN的目錄
	4. 進入目標物件CN的目錄，直到最底層"CN=*****流水號"，右鍵內容>安全性>進階>擁有者>變更擁有者為administrator@domain>確定
	5. 右鍵刪除物件(此步驟請務必確認目標正確，刪錯就沒了)
	6. 重覆步驟4.5刪除上一層目錄，直到目標物件完全刪除。
	7. 等待AD同步，結束
