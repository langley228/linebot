返回[LineBot 相關主題目錄](README.md)
# 使用ngork快速建置Https測試環境
- 前言

https是LINE官方對於使用Webhook連結Bot必要的條件之一，可以使用ngork能夠讓你在本機端運行的應用程式通過一個公開的URL作為通道直接連結，以便測試



- 建置流程 
> - 註冊並登入[ngork](https://ngrok.com/)帳號

![ngrok1](/imgs/ngrok1.jpg)
> - 連結帳號（之後只需要啟動環境就行了）

![ngrok2](/imgs/ngrok2.jpg)
> - 以本機Web port號啟動環境（如：port 80)

![ngrok3](/imgs/ngrok3.jpg)
> - 啟動後，得到Http、Https 兩組URL，Https即可作為webhookURL使用
>>（注意每次啟動網址都會不一樣，webhook記得要跟著修改，免費就不要計較這麼多了，可以開發測試就好）


