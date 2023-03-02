# 简介

基于ChatGPT的企业微信聊天应用，通过 [OpenAI](https://github.com/openai/openai-quickstart-python) 接口生成对话内容，使用 [wechatpy](https://github.com/wechatpy/wechatpy) 实现企业微信应用号消息的接收和回复。已实现的特性如下：

- [x] **文本对话：** 接收发送给应用号的企业微信消息，使用ChatGPT生成回复内容，完成自动回复
- [x] **上下文记忆**：支持多轮对话记忆，且为每个好友维护独立的上下会话


# 更新日志
1、创建：基于 https://github.com/zhayujie/chatgpt-on-wechat 的微信对话项目，稍加改造，增加企业微信应用的功能。
2、更新：改用最新的GPT3.5-Turbo接口

# 快速开始

## 准备

### 1. OpenAI账号注册

前往 [OpenAI注册页面](https://beta.openai.com/signup) 创建账号，参考这篇 [教程](https://www.cnblogs.com/damugua/p/16969508.html) 可以通过虚拟手机号来接收验证码。创建完账号则前往 [API管理页面](https://beta.openai.com/account/api-keys) 创建一个 API Key 并保存下来，后面需要在项目中配置这个key。

> 项目中使用的对话模型是 davinci，计费方式是约每 750 字 (包含请求和回复) 消耗 $0.02，图片生成是每张消耗 $0.016，账号创建有免费的 $18 额度，使用完可以更换邮箱重新注册。


### 2.运行环境

支持 Linux、MacOS、Windows 系统（可在Linux服务器上长期运行)，同时需安装 `Python`。 
> 建议Python版本在 3.7.1~3.9.X 之间，3.10及以上版本在 MacOS 可用，其他系统上不确定能否正常运行。


1.克隆项目代码：

```bash
git clone https://github.com/mostlittlebee/chatgpt-on-wecom
cd chatgpt-on-wecom/
```

2.安装所需核心依赖：

```bash
pip3 install flask
pip3 install wechatpy
pycryptodome
pip3 install --upgrade openai
```
注：`openai`使用最新版本，需高于等于0.27.0 ，如果之前安装过请单独更新下pip3 install --upgrade openai

## 配置

配置文件的模板在根目录的`config-template.json`中，需复制该模板创建最终生效的 `config.json` 文件：

```bash
cp config-template.json config.json
```

然后在`config.json`中填入配置，以下是对默认配置的说明，可根据需要进行自定义修改：

```bash
# config.json文件内容示例
{ 
  "open_ai_api_key": "你的OPENAI KEY",
  "conversation_max_tokens": 最大返回字符,
  "WECHAT_TOKEN": "企业微信 回调token",
  "WECHAT_ENCODING_AES_KEY":"企业微信 编码后的AES Key",
  "WECHAT_CORP_ID":"企业微信 企业ID",
  "Secret":"企业微信 应用Secret",
  "AppId":"企业微信 应用ID",
  "character_desc": "你是ChatGPT, 一个由OpenAI训练的大型语言模型, 你旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。
}
```



## 运行

### 1.本地运行

如果是开发机 **本地运行**，直接在项目根目录下执行：

```bash
python3 app.py
```


### 2.服务器部署

使用nohup命令在后台运行程序：

```bash
touch nohup.out                                   # 首次运行需要新建日志文件                     
nohup python3 app.py & tail -f nohup.out          # 在后台运行程序并通过日志输出二维码
```

> **特殊指令：** 用户向应用号发送 **#清除记忆** 即可清空该用户的上下文记忆。 