# AD-Note
## Debug-螢幕保護程式群組原則套用失效
* 原因1：其他應用程式阻檔，以***系統管理員***執行
```
powercfg /requests
```
可查看是否有程式造成阻檔，正常情況應該要如下圖

![image](https://github.com/sciloa62/AD-Note/blob/master/picture/gpo0001.png)
參考網站：https://dotblogs.com.tw/chou/archive/2011/04/25/23559.aspx
* 原因2：網域信任失效，重新加入網域即可。
