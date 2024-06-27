# Instructions for developers
## Step 1: 注册账号
登录[管理端](https://manager.gamebridge.games/login.html), 创建开发者账号。

## Step 2: 创建游戏
在管理端选择`Developers`下的`Create Game`创建一个新的游戏，创建后填写必要的游戏信息。

## Step 3: 在游戏内接入SDK
### 1.Initialize the SDK
将`gamebridge-sdk.js`放置于head区域内，加载顺序需在游戏脚本之前。
```html
<script
	id="gamebridge-sdk"
	src="https://sdk.beesads.com/v1/gamebridge.js"
	data-test="on"
	data-gameid="{gameId}">
</script>
```
#### Parameter Info
- id: 脚本固定标识，必传
- src: 脚本固定加载地址，必传
- data-test: 是否启用测试广告，用于广告调试时使用，产品环境该参数需清除。可用值: 'on', 'off'
- data-gameid: 游戏唯一标识，在管理端创建游戏后生成，必传

`gamebridge-sdk.js`加载后，将在window上挂载`GameBridgeSDK`对象，通过该对象执行初始化方法: `window.GameBridgeSDK.init()`。
```javascript
// 执行初始化函数
window.GameBridgeSDK.init().then(() => {
    // 建议在这里执行游戏的初始化方法
});
```

### 2.Loading Event
游戏资源加载事件，通过该事件开发者可以向SDK传递资源加载何时开始、何时结束。
```javascript
// 游戏资源加载开始时调用，会自动触发前帖片广告
window.GameBridgeSDK.gameLoadingStart();

// 游戏资源加载成功后调用
window.GameBridgeSDK.gameLoadingFinished();
```

### 3.Config
开发者可以通过在window上挂载`GAME_BRIDGE_CONFIG`对象的方式，对SDK进行配置，目前支持的配置项:暂停,恢复。SDK在广告调用或结束时，会通过配置项中的暂停与恢复方法对游戏进行相应的操作。
```
window.GAME_BRIDGE_CONFIG = {
	// 注册游戏暂停事件
	pause: () => {
		// 游戏的暂停方法
	}, 
	
	// 注册游戏恢复事件
	resume: () => {
		// 游戏的暂停方法
	}
}
```

### 4.Game Event
为了更好的分析游戏行为，开发者需要在合适的位置调用以下方法，比如说在关卡类游戏的开始位置调用`window.GameBridgeSDK.gameplayStart()`。
```javascript
// 回合类游戏: 开始
window.GameBridgeSDK.roundStart();

// 回合类游戏: 结束
window.GameBridgeSDK.roundEnd();

// 关卡类游戏: 开始
window.GameBridgeSDK.gameplayStart();

// 关卡类游戏: 结束
window.GameBridgeSDK.gameplayStop();

// 欢乐时刻
window.GameBridgeSDK.happyTime();
```

### 5.获取SDK状态
#### SDK ready status
```javascript
// 通过window.GameBridgeSDK.ready获取SDK是否就续
if (window.GameBridgeSDK.ready) {
    // 已就续
} else {
    // 未就续
}
```
#### Whether the current device has ads disabled
```javascript
if (window.GameBridgeSDK.isAdBlocked()) {
    // 当前设备禁用了广告
} else {
    // 当前设备没有禁用广告
}
```

### 6.Show Ad
#### commercialBreak
`commercialBreak`用于展示插屏广告，应在关卡结束或其它处于休息期的时间点触发。
```javascript
window.GameBridgeSDK.commercialBreak(() => {
    // 调用前函数
}).then((status) => {
	// 调用完成函数，其中参数status返回当次广告展示状态
});
```

#### rewardedBreak
`rewardedBreak`用于展示奖励类广告，应由用户自主选择是否触发(例如游戏过关后，用户可以选择观看广告获取更多的收益)。
```javascript
window.GameBridgeSDK.rewardedBreak().then((status) => {
	// 调用完成函数，其中参数status返回当次广告展示状态
});
```

## Step 4: 提交游戏
在管理端的游戏列表内，选择版本功能进行版本列表。新建版本，并提交游戏包。

## Step 5: 游戏审核
等待验证游戏，一般时间为三个工作日

## Step 6: 游戏发布
游戏验证通过后，将会进行发布。
