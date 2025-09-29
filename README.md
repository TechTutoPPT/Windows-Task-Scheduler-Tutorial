# Windows-Task-Scheduler-Tutorial

[![](https://github.com/TechTutoPPT/Windows-Task-Scheduler-Tutorial/blob/main/cover.PNG)](https://youtu.be/RUNBfFD5F8s)

如果你有嘗試過Windows+Linux雙重開機系統的話, 你有沒有留意到當你啟動過Linux後再進入Windows系統時, 系統顯示的時間與實際時間有對不上的情況?
原因是這兩系統使用的定時標準不同, Linux使用的是UTC, Windows則使用RTC, 那可以怎樣解決呢? 方法後簡單, 只要Windows與NTP伺服器作出同步便可:
以系統管理員身打開PowerShell, 執行以下指令:
```
w32tm /resync
```

每次啟動都要做上述的手動操作太麻煩了吧, 我們將它編入排程工作, 這樣就方便多了:
按Win+R, 輸入taskschd.msc開啟工作排程器, 點擊右手邊的建立工作...
1. 一般>名稱:Sync Time>剔選以最高權限執行
2. 觸發程序>新增>開始工作:登入時, 剔選延遲工作的時間:30秒>確定
3. 動作>新增>程式或指令碼:w32tm>新增引數(可省略):/resync>確定
4. 條件>取消剔選只有在電腦使用AC電源時才啟動這個工作>剔選只有在下列網路連線可以使用時才啟動: 你的WiFi SSID>確定
這樣便創建好排程工作, 現在可以重新開機, 再進入工作排程器, 查看執行的結果.

再多講一些創建工作排程的經驗, 假如在一般頁面中你點選的是不論使用者登入與否均執行的話, 那你的帳戶必需設有密碼, 
為帳戶設立密碼可執行以下指令(以系統管理員身打開PowerShell):
```
net user 使用者名稱 密碼
```
(此方法僅適用於本機帳號, 不適用於 Microsoft帳號)

然後假如你設立密碼後還想不用輸入密碼便能自動登入的話, 可執行以下指令:
```
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "DefaultUserName" -Value "使用者名稱"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "DefaultPassword" -Value "密碼"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "AutoAdminLogon" -Value "1"
```
若要刪除以上登錄檔項目, 則可執行:
```
Remove-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "DefaultUserName"
Remove-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "DefaultPassword"
Remove-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "AutoAdminLogon"
```






