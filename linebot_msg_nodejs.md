返回[LineBot 相關主題目錄](README.md)
# LineBot Messaging API 推播與回覆 Node Js (express) 篇
- 前言

本文以實作為主，Messaging API 建立請參考LineBot Messaging API 推播與回覆 Java 篇

- 快速建置express框架專案

> - Express-Generator 快速建置網頁 : npm install -g express-generator
> - 產生專案：express 專案名稱

![linebot_msg_nodejs_1](/imgs/linebot_msg_nodejs_1.jpg)
- 進入專案資料夾

![linebot_msg_nodejs_2](/imgs/linebot_msg_nodejs_2.jpg)
> - 安裝套件：npm install

![linebot_msg_nodejs_3](/imgs/linebot_msg_nodejs_3.jpg)
> - 啟動站台：npm start

![linebot_msg_nodejs_4](/imgs/linebot_msg_nodejs_4.jpg)
- 開啟瀏覽器：http://localhost:3000/

![linebot_msg_nodejs_5](/imgs/linebot_msg_nodejs_5.jpg)
- 停止站台：Ctrl +C

![linebot_msg_nodejs_6](/imgs/linebot_msg_nodejs_6.jpg)

- 安裝linebot套件
> - npm install linebot

![linebot_msg_nodejs_7](/imgs/linebot_msg_nodejs_7.jpg)
- LineBot Messaging API Webhook設定

> - 修改 app.js 增加 ./routes/callback

```
//...........
app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/callback', require('./routes/callback'));
//...........
```

> - 增加 routes/callback.js , 回傳OK
```
var express = require('express');
var router = express.Router();
router.post('/',(req, res, next) => {
    bot.parse(req.body);
    res.send('OK');
});
module.exports = router;
```


- [使用ngork快速建置Https測試環境](ngork.md)

- 到[Line Developer](https://developers.line.me/console/)設定Messaging API Channel，修改Webhook URL→Verify →Success
![linebot_msg_nodejs_10](/imgs/linebot_msg_nodejs_10.jpg)

- 回覆reply
> - 修改 routes/callback.js 
```
var express = require('express');
var router = express.Router();
//linebot 物件初始化
var linebot = require('linebot');
var bot = linebot({
    channelId: 'channelId',
    channelSecret: 'channelSecret',
    channelAccessToken: 'channelAccessToken'
});
//linebot event 事件
bot.on('message', function (event) {
    if (event.message.type = 'text') {
        var msg = event.message.text;
        //重覆使用者說的訊息
        event.reply("您說：" + msg).then(function (data) {
            // success
            console.log(event);
        }).catch(function (error) {
            // error
            console.log('error:' + error);
        });
    }
});
bot.on('follow', function (event) { console.log(event); });
bot.on('unfollow', function (event) { console.log(event); });
bot.on('join', function (event) { console.log(event); });
bot.on('leave', function (event) { console.log(event); });
bot.on('postback', function (event) { console.log(event); });
bot.on('beacon', function (event) { console.log(event); });
router.post('/',(req, res, next) => {
    bot.parse(req.body);
    res.send('OK');
});
module.exports = router;
```


- 實測回覆reply 

![linebot_msg_nodejs_8](/imgs/linebot_msg_nodejs_8.jpg)

- 推播push
> - 修改 app.js 增加　./routes/push
```
//...........
app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/callback', require('./routes/callback'));
app.use('/push', require('./routes/push'));
//...........
```
> - 增加 routes/push.js 


```
var express = require('express');
var router = express.Router();



//linebot 物件初始化
var linebot = require('linebot');
var bot = linebot({
    channelId: 'channelId',
    channelSecret: 'channelSecret',
    channelAccessToken: 'channelAccessToken'
});

router.post('/',(req, res, next) => {
    var userId = req.body.userId;
    var sendMsg = req.body.sendMsg;
    bot.push(userId, [sendMsg]);
    console.log('userId: ' + userId);
    console.log('send: ' + sendMsg);
    res.send('OK');
});
module.exports = router;

```



- 實測回覆push

![linebot_msg_nodejs_9](/imgs/linebot_msg_nodejs_9.jpg)

 

- 總結

以上基本入門的推播回覆就完成了，接下來總結一下Node Js開發的優缺點。
> - 缺點：
開發階段需要Https測試環境
需要略懂Node Js 環境與Web框架
> - 優點：
express框架可以快速建立專案
linebot套件可以直接使用, 非常方便





