## 自主开发集成聊天配置指导(Android) ##

### 1.在自行开发的应用中，集成环信聊天需要把libs文件夹下easemobchat_2.0.0.jar和3rdpartylibs文件夹下httpmime-4.2.jar拷贝到你的项目的libs文件夹底下。###


![alt text](demo_dirs.jpg "demo") 

 ![](http://i.imgur.com/NrMwsez.jpg)

### 2.在清单文件AndroidManifest.xml里加入以下权限，以及写上你注册的appkey

	<uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_MOCK_LOCATION" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>  
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.CALL_PHONE" />
    <uses-permission android:name="android.permission.GET_TASKS" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    
		
		<!--  设置环信SDK的appkey -->
	<meta-data android:name="EASEMOB_APPKEY"  android:value="你申请的appkey" />
	<service android:name="com.easemob.chat.EMChatService" />

关于EASEMOB_APPKEY，请登录或注册环信开发者后台(http://console.easemob.com),申请APPKEY后，进行相关配置。（测试demo中 APPKEY为easemob-demo#chatdemo）

###怎么获取APPKEY

通过http://console.easemob.com登录进入到开发者后台（若没有账户先要进行注册）

####注册 (企业ID会在生成的APPKEY中存在）

![alt text](consoleregister.png "app")

####激活

 注册完成，进入到邮箱进行激活，并登陆。

####登陆 

进入后台中，首先要创建一个应用，并输入应用的名称(应用名称会在你生成的APPKEY中存在)

![alt text](creatapp.png "app")

输入完应用名称，点击确定，得到下图中相关信息，你的appkey已生成成功

![alt text](appkey.png "app")