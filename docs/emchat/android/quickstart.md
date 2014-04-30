---
title: 易聊
description: 5分钟为你的APP加入完整的微信聊天功能
category: emchat
layout: docs
---

# 快速入门（五分钟运行易聊demo) 


## 1.下载易聊demo (Android) 

###  1.1 什么是易聊demo

易聊demo展示了怎样使用易聊SDK快速创建一个完整的类微信聊天APP。展示的功能包括：易聊SDK初始化，登录，登出，注册消息接收listener, 发送消息

易聊demo源代码已在github上开源供开发者下载，以帮助开发者更好的学习了解易聊SDK。

### 1.2 下载易聊demo 

    

1. 下载易聊demo：[下载链接](#{site.base_url}/docs/downloads/downloads.html)

2. 解压缩androidsdk.zip后会得到以下目录结构：
 
 ![alt text](example_layout.png "Title")


## 2.运行易聊demo (Android) 

1. 在手机上安装chatdemo-nonui.apk
    
 
2. 运行chatdemo-nonui: 点击“发送文本消息”，会发送消息给测试机器人（其账号为"bot"）。该测试机器人接收到消息后会把接收的消息原封不动的自动发送回来

 ![alt text](demo.png "demo")


## 3.快速集成(Android) ##

### 3.1. 把libs文件夹下easemobchat_2.0.0.jar和3rdpartylibs文件夹下httpmime-4.2.jar拷贝到你的项目的libs文件夹底下。###

 ![](http://i.imgur.com/NrMwsez.jpg)

### 3.2. 在清单文件AndroidManifest.xml里加入以下权限，以及写上你注册的appkey

		<uses-permission android:name="android.permission.VIBRATE" />
	    <uses-permission android:name="android.permission.INTERNET" />
	    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
	    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	    <uses-permission android:name="android.permission.GET_TASKS" />
	    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
		
		<!--  设置易聊SDK的appkey -->
	    <meta-data android:name="EASEMOB_APPKEY"  android:value="你申请的appkey" />



## 4. 从源代码级别深入了解易聊demo (Android)

 
### 4.1 在Eclipse/IDEA中创建易聊demo project 


1. Eclipse IDE： 打开菜单“ File - New - Project“，选择”Android Project from Existing Code”， 选择解压后的"androidsdk/examples"目录下的chatdemo-nonui路径,点击“Finish”。


### 4.2. 深入理解易聊demo背后的代码 ###

#### 1.初始化： 见DemoApplication.java

    public class DemoApplication extends Application {
    
        public static Context appContext;
        @Override
        public void onCreate() { 
           super.onCreate();
           appContext = this;
     
           //初始化易聊SDK
           Log.d("DemoApplication", "Initialize EMChat SDK");
           EaseMobChat.getInstance().init(appContext);
        }
    }

#### 2. 注册listener,以接收聊天消息：见MainActivity.java ####

    @Override
    protected void onCreate(Bundle savedInstanceState) {

        //注册message receiver， 接收聊天消息
        msgReceiver = new NewMessageBroadcastReceiver();
        IntentFilter intentFilter = new IntentFilter(EMChatManager.getInstance().getNewMessageBroadcastAction());
        registerReceiver(msgReceiver, intentFilter);
    }

#### 3. 登录：见MainActivity.java ####

    @Override
    protected void onResume() {
        super.onResume();
        //登录到聊天服务器。此处使用了一个测试账号，用户名是test1，密码是123456。
        EMChatManager.getInstance().login("test1", "123456", new EMCallBack() {

            @Override
            public void onError(int arg0, final String errorMsg) {
                runOnUiThread(new Runnable() {
                    public void run() {
                        Toast.makeText(MainActivity.this, "登录聊天服务器失败：" + errorMsg, Toast.LENGTH_SHORT).show();
                    }
                });
            }

            @Override
            public void onProgress(int arg0, String arg1) {
            }

            @Override
            public void onSuccess() {
                runOnUiThread(new Runnable() {
                    public void run() {
                        Toast.makeText(MainActivity.this, "登录聊天服务器成功", Toast.LENGTH_SHORT).show();
                    }
                });
                
            }
        });
    }


#### 4. 退出登录：见MainActivity.java ####

    @Override
    protected void onPause() {
        super.onPause();
        
        //登出聊天服务器
        EMChatManager.getInstance().logout();
    }


#### 5. 发送消息：见MainActivity.java ####

    //本demo是发送消息给测试机器人（其账号为"bot"）。该测试机器人接收到消息后会把接收的消息原封不动的自动发送回来
    public void onSendTxtMsg(View view) {
        try {
            //创建一个消息
            EMMessage msg = EMMessage.createSendMessage(EMMessage.Type.TXT);
            //设置消息的接收方
            msg.setReceipt("bot");
            //设置消息内容。本消息类型为文本消息。
            TextMessageBody body = new TextMessageBody(tvMsg.getText().toString());
            msg.addBody(body);
        
            //发送消息
            EMChatManager.getInstance().sendMessage(msg);
            Log.d("chatdemo", "消息发送成功:" + msg.toString());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

#### 6. 接收聊天消息并显示：见MainActivity.java ####

    private class NewMessageBroadcastReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            //消息id
            String msgId = intent.getStringExtra("msgid");
            //消息发送方
            String msgFrom = intent.getStringExtra("from");
            //消息类型
            int msgType = intent.getIntExtra("type", 0);
            //消息内容
            String msgBody = intent.getStringExtra("body");
            
            Log.d("chatdemo", "new message id:" + msgId + " from:" + msgFrom + " type:" + msgType + " body:" + msgBody);
            
            tvReceivedMsg.append("from:" + msgFrom + " body:" + msgBody + " \r");
        }
    }

# 4. 易聊demo源代码 

 
易工厂提供了一系列demo以帮助开发者更好的学习了解易聊SDK。所有demo均已在github上开源供开发者下载使用。你可以clone这些项目来学习了解易聊SDK，也可以在这些demo基础上快速创建你自己的真正项目。易聊SDK（Android版）在github的下载地址是：

[https://github.com/easemob/sdkexamples-android](https://github.com/easemob/sdkexamples-android)


# 5. Bug报告跟踪 #

请使用以下地址来报告跟踪bug：

https://github.com/easemob/sdkexamples-android/issues


