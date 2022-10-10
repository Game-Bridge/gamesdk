
## Instructions for developers
### Step 1:
注册账号

### Step 2:
分配游戏ID, 该游戏id用于区分游戏收益。
从[GameBridge](https://manager.gamebridge.games/)获取游戏ID

### Step 3:
接入SDK，可以在加载gamebridge.js的同时通过script标签传递一些属性。
```
<script 
    id="gamebridge-sdk"
    src="https://sdk.enjoy4fun.com/v1/gamebridge-sdk.js" 
    data-ad-frequency="30s" 
    data-gameid="gameid">
</script>
```
#### Script options
- id: SDK标识，示例中为固定值，必填
- src: SDK地址，示例中为固定值，必填
- data-ad-frequency: 广告的展示频率，默认为"120s"，最小值为"30s"
- data-gameid: 游戏id
- data-test: 启用测试模式，启用后将在广告位展示测试广告，如需开启传入"on"。

gamebridge-sdk.js加载成功后，在游戏开始时使用以下方法初始化SDK。
```
    // get gamebridge sdk version
    GameBridgeSDK.init().then(() => {
        console.log("GameBridge SDK successfully initialized");
        // fire your function to continue to game
    }).catch(() => {
        console.log("Initialized, but the user likely has adblock");
        // fire your function to continue to game
    });
```
为了使您的游戏提供准确的游戏指标，请在游戏开始加载开始和结束时触发以下事件。
```
    // start game loading
    GameBridgeSDK.gameLoadingStart();
    // finish game loading
    GameBridgeSDK.gameLoadingFinished();
```
接入游戏内事件，以更好的获取游戏内游戏指标，使用 🎮 gameplayStart()来记录用户开始游戏（游戏回合开始，或者游戏取消暂停），使用 🎮 gameplayStop()来记录游戏结束（游戏回合结束，游戏暂停，或者回到主菜单）
```
    // first level loads, player clicks anywhere
    GameBridgeSDK.gameplayStart();
    // player is playing
    // player loses round
    GameBridgeSDK.gameplayStop();
    // game over screen pops up
```
接入插屏广告，commercialBreak用于获取插屏广告，我们建议在用户开始关卡之前（即当用户表现出来想继续玩游戏的意图时）触发插屏广告。
```
    // pause your game here if it isn't already
    GameBridgeSDK.commercialBreak(() => {
      // you can pause any background music or other audio here
    }).then(() => {
      console.log("Commercial break finished, proceeding to game");
      // if the audio was paused you can resume it here (keep in mind that the function above to pause it might not always get called)
      // continue your game here
    });
```
接入奖励广告，rewardedBreak用户给用户播放广告以换取游戏特定的奖励时触发，在使用rewardedBreak时，应该事先给玩家说明即将播放广告，切观看完成后会得到相应的奖励。
```
    // pause your game here if it isn't already
    GameBridgeSDK.rewardedBreak(() => {
      // you can pause any background music or other audio here
    }).then((success) => {
        if(success) {
            // video was displayed, give reward
        } else {
            // video not displayed, should not give reward
        }
        // if the audio was paused you can resume it here (keep in mind that the function above to pause it might not always get called)
        console.log("Rewarded break finished, proceeding to game");
        // continue your game here
    });
```

#### Example
```
// gameplay stops (don't forget to fire gameplayStop)
// fire your mute audio function
// fire your disable keyboard input function
GameBridgeSDK.commercialBreak().then(() => {
    console.log("Commercial break finished, proceeding to game");
    // fire your unmute audio function
    // fire your enable keyboard input function
    GameBridgeSDK.gameplayStart();
    // fire your function to continue to game
});
```

### Step 4:
通过游戏管理后台，创建游戏表单进行提交。

### Step 5:
等待验证游戏，一般时间为三个工作日

### Step 6:
游戏验证通过后，将会进行发布。

## script options
### data-ad-frequency
广告的展示频率，默认为"120s"，最小值为"30s"
### Example
```
<script data-ad-frequency="30s"></script>
```

### data-gameid
游戏专属id
### Example
```
<script data-gameid="xxxx"></script>
```
  
