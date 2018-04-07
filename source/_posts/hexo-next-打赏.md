---
title: hexo next 打赏
tags:
  - hexo
  - next
categories:
  - hexo
abbrlink: 910053190
date: 2018-04-06 21:45:29
---
hexo next添加打赏功能
<!-- more -->
### 微信和支付宝二维码设置
先生成微信和支付宝二维码，将图片放在`themes/next/source/images`下
### 配置`_config.yml`
找到如下语句并修改
``` 
# Reward
reward_comment: 您的支持将鼓励我继续创作！
wechatpay: /images/wechatpay.png
alipay: /images/alipay.png
```
### 删除打赏文字闪动样式
进入`themes\next\source\css\_common\components\post\post-reward.styl`,找到以下样式并删除
``` 
#wechat:hover p{
    animation: roll 1s infinite linear;
    -webkit-animation: roll 1s infinite linear;
    -moz-animation: roll 1s infinite linear;
}
#alipay:hover p{
    animation: roll 1s infinite linear;
    -webkit-animation: roll 1s infinite linear;
    -moz-animation: roll 1s infinite linear;
}
#bitcoin:hover p {
    animation: roll 1s infinite linear;
    -webkit-animation: roll 1s infinite linear;
    -moz-animation: roll 1s infinite linear;
}
```
