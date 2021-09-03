# The new FunSDKDemo is still being updated and improved (the old FunSDKDemo can be used normally and will continue to be updated)

## By Gradle integration
### 1.1 Create a project: Create a new project in Android Studio.
### 1.2 Configure build.gradle：Add the dependencies downloaded in the integration preparation to the build.gradle file.
```
repositories {
    mavenCentral()
}

defaultConfig {
    ndk {
        abiFilters "armeabi","arm64-v8a"
    }
 }
dependencies {
    // The latest stable version of FunSDK:
    implementation 'io.github.xmcamera:libxmfunsdk:2.1'
}
```

## Initialization
### 1. Go (https://oppf.xmcsrv.com/#/docs?md=readGuide) The new guidelines, an application for registration as an open platform for developers, and then to the [console] - [page] create applications to create Android applications, and other applications can be obtained through the audit after the AppKey, movedCard and AppSecret and other information.
### 2. In AndroidManifest.xml, add AppKey, movedCard, AppSecret and other information applied by the platform
```
<meta-data
    android:name="APP_UUID"
    android:value="Replaced content" />
<meta-data
    android:name="APP_KEY"
    android:value="Replaced content" />
<meta-data
    android:name="APP_SECRET"
    android:value="Replaced content" />
<meta-data
    android:name="APP_MOVECARD"
    android:value="Replaced content" />
```
### 3. Application processing
```
XMFunSDKManager.getInstance().initXMCloudPlatform(this);
//If it is a low-power device (doorbell, door lock, etc.), you also need to call the following methods:
FunSDK.SetFunIntAttr(EFUN_ATTR.SUP_RPS_VIDEO_DEFAULT, SDKCONST.Switch.Open);
//Initial printing:
XMFunSDKManager.getInstance().initLog();
```

## Account login and local login
### 1.Account login
```
XMAccountManager.getInstance().login("Username", "Password", LOGIN_BY_INTERNET, new BaseAccountManager.OnAccountManagerListener() {
            @Override
            public void onSuccess(int msgId) {
                
            }

            @Override
            public void onFailed(int msgId, int errorId) {

            }

            @Override
            public void onFunSDKResult(Message msg, MsgContent ex) {

            }
        })
```
### 2.Local login
```
String localPath = Environment.getExternalStorageDirectory();//Phone local path
LocalAccountManager.getInstance().login(localPath, new BaseAccountManager.OnAccountManagerListener() {
            @Override
            public void onSuccess(int msgId) {
                
            }

            @Override
            public void onFailed(int msgId, int errorId) {

            }

            @Override
            public void onFunSDKResult(Message msg, MsgContent ex) {

            }
        })
```
### 3.Get device status
##### You must get the device status before logging in to the device
```
AccountManager.getInstance().updateAllDevStateFromServer( AccountManager.getInstance().getDevList(), new BaseAccountManager.OnDevStateListener() {
    @Override
    public void onUpdateDevState(String devId) {
        //Single device status callback
    }

    @Override
    public void onUpdateCompleted() {
        //All device status acquisition end callback
    }
});
```
## Equipment operation
### 1. Add device
#### 1.1 Quick configuration to add equipment
```
DeviceManager.getInstance().startQuickSetWiFi(wifiInfo, scanResult, dhcpInfo, pwd, new DeviceManager.OnQuickSetWiFiListener() {
    @Override
    public void onQuickSetResult(XMDevInfo xmDevInfo, int i) {
        AccountManager.getInstance().addDev(xmDevInfo, isUnbindDevUnderOther, new BaseAccountManager.OnAccountManagerListener() {
            @Override
            public void onSuccess(int msgId) {
                Toast.makeText(iDevQuickConnectView.getContext(),
                        "Added successfully",Toast.LENGTH_LONG).show();
                iDevQuickConnectView.onAddDevResult();
            }

            @Override
            public void onFailed(int msgId, int errorId) {
                Toast.makeText(iDevQuickConnectView.getContext(),
                        "Add failed:" + errorId,Toast.LENGTH_LONG).show();
                iDevQuickConnectView.onAddDevResult();
            }

            @Override
            public void onFunSDKResult(Message message, MsgContent msgContent) {

            }
        });
    }
});
```
#### 1.2 Add device manually
```
SDBDeviceInfo deviceInfo = new SDBDeviceInfo();
G.SetValue(deviceInfo.st_0_Devmac,devId);
G.SetValue(deviceInfo.st_5_loginPsw,pwd);
G.SetValue(deviceInfo.st_4_loginName,userName);
XMDevInfo xmDevInfo = new XMDevInfo();
xmDevInfo.sdbDevInfoToXMDevInfo(deviceInfo);
AccountManager.getInstance()..addDev(xmDevInfo, new BaseAccountManager.OnAccountManagerListener() {
    @Override
    public void onSuccess(int i) {
        if (iDevSnConnectView != null) {
            iDevSnConnectView.onAddDevResult(true);
        }
    }

    @Override
    public void onFailed(int i, int i1) {
        if (iDevSnConnectView != null) {
            iDevSnConnectView.onAddDevResult(false);
        }
    }

    @Override
    public void onFunSDKResult(Message message, MsgContent msgContent) {

    }
});
```
### 2.Delete device
```
AccountManager.getInstance().deleteDev(devId, new BaseAccountManager.OnAccountManagerListener() {
    @Override
    public void onSuccess(int msgId) {
        
    }

    @Override
    public void onFailed(int msgId, int errorId) {

    }

    @Override
    public void onFunSDKResult(Message message, MsgContent msgContent) {

    }
});
```
### 3.Login device
```
DeviceManager.getInstance().loginDev(devId, new DeviceManager.OnDevManagerListener() {
    @Override
    public void onSuccess(String devId, int operationType, Object o) {

    }

    @Override
    public void onFailed(String devId, int msgId, String jsonName, int i1) {

    }
});
```
### 4.Live Video
```
MonitorManager mediaManager = createMonitorPlayer(viewGroup, devId);
mediaManager.setStreamType(SDKCONST.StreamType.Extra);
mediaManager.setChnId(channelId);
mediaManager.startMonitor();//Open Video
mediaManager.pausePlay();//Pause playback
mediaManager.rePlay();//Resume playback
mediaManager.stopPlay();//Stop play
mediaManager.destroyPlay();//Destroy Play
mediaManager.capture(String savePicPath);//Capture Picture
mediaManager.startRecord(String saveVideoPath);//Start video recording
mediaManager.stopRecord();//End video recording
mediaManager.openVoiceBySound();//Turn on audio
mediaManager.closeVoiceBySound();//Turn off audio
```
### 5.Video playback
```
/**
 * mediaType:
 * MediaManager.PLAY_DEV_PLAYBACK:Local video playback
 * MediaManager.PLAY_CLOUD_PLAYBACK:Cloud storage remote video playback
 */
RecordManager mediaManager = createRecordPlayer(playView,getDevId(), recordType);
mediaManager.pausePlay();//Pause playback
mediaManager.rePlay();//Resume playback
mediaManager.stopPlay();//Stop play
mediaManager.destroyPlay();//Destroy Play
mediaManager.capture(String savePicPath);//Capture Picture
mediaManager.startRecord(String saveVideoPath);//Start video recording
mediaManager.stopRecord();//End video recording
mediaManager.openVoiceBySound();//Turn on audio
mediaManager.closeVoiceBySound();//Turn off audio
```
### 6. Alarm push subscription
```
//Initialization
XMPushManager xmPushManager = new XMPushManager(xmPushLinkResult);
String pushToken = XUtils.getPushToken(this);
if (!StringUtils.isStringNULL(pushToken)) {
    SMCInitInfo info = new SMCInitInfo();
    G.SetValue(info.st_0_user, XMAccountManager.getInstance().getAccountName());
    G.SetValue(info.st_1_password, XMAccountManager.getInstance().getPassword());
    G.SetValue(info.st_2_token, pushToken);
    xmPushManager.initFunSDKPush(info,PUSH_TYPE_XM);
}
//Subscription
xmPushManager.linkAlarm(devId,0);
//Unsubscribe
xmPushManager.unLinkAlarm(devId,0);
//Receive push information
private XMPushManager.XMPushLinkResult xmPushLinkResult = new XMPushManager.XMPushLinkResult() {
        @Override
        public void onPushInit(int pushType, int errorId) {
            super.onPushInit(pushType, errorId);
            if (errorId >= 0) {
                System.out.println("Push initialization is successful:" + pushType);
            }else {
                System.out.println("Push initialization failed:" + errorId);
            }
        }

        @Override
        public void onPushLink(int pushType, String devId, int seq, int errorId) {
            super.onPushLink(pushType, devId, seq, errorId);
            if (errorId >= 0) {
                System.out.println("Push initialization is successful:" + devId);
            }else {
                System.out.println("Push initialization failed:" + devId + ":" + errorId);
            }
        }

        @Override
        public void onPushUnLink(int pushType, String devId, int seq, int errorId) {
            super.onPushUnLink(pushType, devId, seq, errorId);
            if (errorId >= 0) {
                System.out.println("Push initialization is successful:" + devId);
            }else {
                System.out.println("Push initialization failed:" + devId + ":" + errorId);
            }
        }

        @Override
        public void onAlarmInfo(int pushType, String devId, Message msg, MsgContent ex) {
            super.onAlarmInfo(pushType, devId, msg, ex);
            String pushMsg = G.ToString(ex.pData);
            System.out.println("Alarm message received:" + pushMsg);
            parseAlarmInfo(pushMsg);
        }
    };

    private void parseAlarmInfo(String pushMsg) {
        AlarmInfo alarmInfo = new AlarmInfo();
        alarmInfo.onParse(pushMsg);
        Toast.makeText(getApplicationContext(),"Alarm message received:" +
                XMPushManager.getAlarmName(getApplicationContext(),alarmInfo.getEvent()) + ":" +
                alarmInfo.getStartTime(),Toast.LENGTH_LONG).show();
    }
```
### 7.Device Configuration
```
DevConfigInfo devConfigInfo = DevConfigInfo.create(new DevConfigManager.OnDevConfigResultListener() {

    @Override
    public void onSuccess(String devId, int msgId, Object result) {
       
    }

    @Override
    public void onFailed(String devId, int msgId, String s1, int errorId) {
    
    }

    @Override
    public void onFunSDKResult(Message message, MsgContent msgContent) {
       
    }
});

------------------------------Most configurations----------------------------------------
devConfigInfo.setJsonName(jsonName);//Set configuration JsonName:Find the JsonName information you want to get the configuration in the JsonConfig class
devConfigInfo.setChnId(chnId);//Channel ID, the configuration of the entire device uses -1, the channel will pass the corresponding channel number -1, for example, 0 means the first channel, 1 means the second channel, and so on
devConfigManager.getDevConfig(devConfigInfo);//Get configuration
devConfigManager.setDevConfig(devConfigInfo);//Save configuration

------------------------------Other special commands--------------------------------------

devConfigInfo.setJsonName(jsonName);//Set configuration JsonName: Find the JsonName information you want to get the configuration in the JsonConfig class
devConfigInfo.setJsonData(jsonData);//Send json data
//Restart the device
JSONObject obj = new JSONObject();
obj.put("Action", "Reboot");
String jsonData = HandleConfigData.getSendData(JsonConfig.OPERATION_OPMACHINE, "0x01", obj)
devConfigInfo.setJsonData(jsonData);//Send json data

devConfigInfo.setCmdId(cmdId);//Command ID: Find the command ID you want to set through the EDEV_JSON_ID class
devConfigInfo.setChnId(chnId);//Channel ID, the configuration of the entire device uses -1, the channel will pass the corresponding channel number -1, for example, 0 means the first channel, 1 means the second channel, and so on
devConfigManager.setDevCmd(devConfigInfo);
```


