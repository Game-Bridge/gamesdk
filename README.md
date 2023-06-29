# Instructions for developers
## Step 1: Register an account
Log in to the [management end](https://manager.gamebridge.games/login.html) and create a developer account.

## Step 2: Create the game
Select `Create Game` under `Developers` on the management end to create a new game, and fill in the necessary game information after creation.

## Step 3: Connect the SDK to the game
### 1.Initialize the SDK
Place `gamebridge-sdk.js` in the `head` area, loading before the game script.
```html
<script
    id="gamebridge-sdk"
    src="//sdk.enjoy4fun.com/v1/gamebridge-sdk.js"
    data-test="on"
    data-gameid="{gameId}">
</script>
```
#### Parameter Info
- id: the fixed ID of the script, which is mandatory.
- src: the fixed loading address of the script, which is mandatory.
- data-test: indicates whether to enable test advertising. It is used for advertising debugging. This parameter must be cleared in the product environment. Available values: 'on', 'off'
- data-gameid: Unique identifier of the game. It is generated after the game is created on the management end, which is mandatory.

After `gamebridge-sdk.js` loads, the `GameBridgeSDK` object will be mounted on the window. initialization methods: `window.GameBridgeSDK.init()`.
```javascript
// Execute the initialization function
window.GameBridgeSDK.init().then(() => {
    // Recommended to execute the initializer for the game here
});
```

### 2.Loading Event
Game resource loading event, which allows developers to communicate to the SDK when resource loading starts and ends.
```javascript
// When the game resource is loaded, it will automatically trigger the pre-movie adverts
window.GameBridgeSDK.gameLoadingStart();

// Invoked after the game resource is successfully loaded
window.GameBridgeSDK.gameLoadingFinished();
```

### 3.Config
Developers can configure the SDK by mounting the GAME_BRIDGE_CONFIG object on the window. Currently supported configuration items: pause, resume. When the AD is called or finished, the SDK will perform corresponding operations on the game through the pause and resume method in the configuration item.
```
window.GAME_BRIDGE_CONFIG = {
    // Registered game suspension
    pause: () => {
        // The method of pausing the game
    }, 
	
    // Register the game back
    resume: () => {
        // The method of pausing the game
    }
}
```

### 4.Game Event
To better analyze game behavior, the developer should call the following methods at appropriate points.
For example, call `window.GameBridgeSDK.gameplayStart()` at the start of a level game.
```javascript
// Turn-based games: Start
window.GameBridgeSDK.roundStart();

// Turn-based games: End
window.GameBridgeSDK.roundEnd();

// level game: Start
window.GameBridgeSDK.gameplayStart();

// level game: End
window.GameBridgeSDK.gameplayStop();

// Happy hour
window.GameBridgeSDK.happyTime();
```

### 5.Get SDK status
#### SDK ready status
```javascript
// Check window.GameBridgeSDK.ready to see if the SDK is ready
if (window.GameBridgeSDK.ready) {
    // ready
} else {
    // not ready
}
```
#### Whether the current device has ads disabled
```javascript
if (window.GameBridgeSDK.isAdBlocked()) {
    // The current device disable ads
} else {
    // The current device does not disable ads
}
```

### 6.Show Ad
#### commercialBreak
`commercialBreak` is used to display interstitial ads. It should be triggered at the end of a level or other time point during the break period.
```javascript
window.GameBridgeSDK.commercialBreak(() => {
    // Invoke pre-function
}).then((status) => {
    // Invoke the completion function, where the status parameter returns the current AD display status
});
```

#### rewardedBreak
`rewardedBreak` is used to display reward ads, which should be triggered by the user (for example, after completing the game, the user can choose to watch ads for more revenue).
```javascript
window.GameBridgeSDK.rewardedBreak().then((status) => {
     // Invoke the completion function, where the parameter：status  returns the current AD display status
});
```

#### displayAd
`displayAd` is used to display banner ads and should be displayed in idle areas when the game is at rest.
```javascript
// dom: Container of the Ads that need to be displayed
// size: Size of the Ads that need to be displayed,such as: 300x250
window.GameBridgeSDK.displayAd(dom, size);
```

#### destroyAd
`destroyAd` is used to close banner ads, It is invoked near the end of the game's break period.
```javascript
// dom: Container of the Ads that need to be displayed
window.GameBridgeSDK.displayAd(dom);
```

## Step 4: Submit a game
In the list of games on the management end, select the version function to perform the version list. Create a new version and submit the game pack.

## Step 5: verification game
Wait for the verification game, the general time is three working days

## Step 6: Game release
After game verification through, it will be released.
