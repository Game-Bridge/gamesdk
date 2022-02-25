
## Instructions for developers
### Step 1:
注册账号

### Step 2:
分配游戏ID, 该游戏id用于区分游戏收益。

### Step 3:
接入SDK，可以在加载gamebridge.js的同时通过script标签传递一些属性。
```
<script 
    id="gamebridge-sdk"
    src="https://sdk.enjoy4fun.com/v1/gamebridge.js" 
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

gamebridge.js加载成功后，在适当的地方执行showAd或type所对应的快捷方法。
```
    // get gamebridge sdk version
    console.log(window.gamebridge.version);

    if(/*游戏开始前*/){
        window.gamebridge.showAd(type); // or window.gamebridge.showStart();
    }
```
#### Type options
- pause: 前贴片广告
- start: 游戏开始前广告
- pause: 游戏暂停时广告
- next: 进入下一关时广告
- browse: 非游戏主流程时广告，比如:进入商店或修改设置时
- reward: 奖励广告

### Step 4:
通过游戏管理后台，创建游戏表单进行提交。

### Step 5:
等待验证游戏，一般时间为三个工作日

### Step 6:
游戏验证通过后，将会进行发布。


## Open API
### showAd(type, googleSdkCon)
展示指定类型的广告
#### Params
1.type<string>: 广告类型，['start', 'pause', 'next', 'browse', 'reward', 'preroll']，每个type都有对应的快捷方法，如start对应: showStart()  
2.conf<object>: sdk 的配置项, 如: {name: 'xxx', beforeAd: () => {}}  
  
#### Example
```
showAd('start', {
    name: 'xxx',
    beforeAd: () => {}
});
```

### showPreroll(adBreakDone)
前贴片广告
#### Params
- adBreakDone<function>: 
#### Example
```
showPreroll(placementInfo => {
    console.log('preroll');
});
```

### showStart()
游戏开始前广告
#### Example
```
showStart();
```

### showPause()
游戏暂停时广告
#### Example
```
showPause();
```

### showNext()
进入下一关时广告
#### Example
```
showNext();
```

### showBrowse()
非游戏主流程时广告，比如:进入商店或修改设置时
#### Example
```
showBrowse();
```

### showReward(success, fail, done)
奖励广告
#### Params
- success<function>: 广告完成播放回调函数
- fail<function>: 广告未完成播放回调函数
- done<function(placementInfo)>: 广告完成或失败回调函数

##### placementInfo
- breakType: 广告类型
- breakName: 广告名称
- breakStatus 可指明此展示位置的状态，值可以是下面其中一个：
    - 'notReady': SDK 尚未初始化
    - 'timeout': 响应时间过长，导致展示位置超时
    - 'invalid': 展示位置无效并已被忽略。例如，每次网页加载时应只有一个前贴片广告展示位置，后续的前贴片广告都无法投放并显示此状态
    - 'error': 回调中存在 JavaScript 错误
    - 'noAdPreloaded': 广告尚未预加载，因此跳过了此展示位置
    - 'frequencyCapped': 由于向此展示位置应用了频次上限，因此广告未能展示
    - 'ignored': 用户在到达下一个展示位置之前没有点击奖励提示
    - 'other': 广告因其他原因未能展示
    - 'dismissed': 用户在看完激励广告之前将其关闭了
    - 'viewed': 用户观看了广告
#### Example
```
showReward(() => {
    console.log('success');
}, () => {
    console.log('fail');
});
```

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
  
