###Getting Started

Follow the steps in our [Android SDK Integration](https://github.com/RedTroops/Android-SDK/wiki/Home) until you reach Editing The Source Code section.

You should have:
* Imported RedTroops SDK.
* Edited the manifest file, including activities and meta-data.
* Added Google Play Services (if you need push notifications integration).
* Have no compilation errors.

###Unity Code

To show an interstitial ad, add this code to the event in your Unity Script:

**C#:**
```csharp
using (AndroidJavaClass cls_UnityPlayer = new AndroidJavaClass("com.unity3d.player.UnityPlayer"))
    {
        using (AndroidJavaObject obj_Activity = cls_UnityPlayer.GetStatic<AndroidJavaObject>("currentActivity"))
        {
        obj_Activity.CallStatic("showInterstitialAd");
        }
    }
```

**JavaScript:**
``` javascript
#if UNITY_ANDROID
var cls_UnityPlayer =  new AndroidJavaClass("com.unity3d.player.UnityPlayer");
var obj_Activity = cls_UnityPlayer.GetStatic.<AndroidJavaObject>("currentActivity");
obj_Activity.CallStatic("showInterstitialAd");
#endif
```

To show a banner ad, add this code:
**C#:**
```csharp
using (AndroidJavaClass cls_UnityPlayer = new AndroidJavaClass("com.unity3d.player.UnityPlayer"))
    {
        using (AndroidJavaObject obj_Activity = cls_UnityPlayer.GetStatic<AndroidJavaObject>("currentActivity"))
        {
        obj_Activity.CallStatic("showBannerAd");
        }
    }
```

**JavaScript:**
``` javascript
#if UNITY_ANDROID
var cls_UnityPlayer =  new AndroidJavaClass("com.unity3d.player.UnityPlayer");
var obj_Activity = cls_UnityPlayer.GetStatic.<AndroidJavaObject>("currentActivity");
obj_Activity.CallStatic("showBannerAd");
#endif
```

###Java Code in your Android Project

1) For unity integration, add a global variable at the top of your class as follows:
```java
	public static Activity mContext;
```
In your activity's `onCreate`, add:
```java
	mContext = this;
``` 

2) In your main activity's `onCreate`, call:
```java
	RedTroops.getInstance(mContext).init(initFinishedListener);
```
Where `initFinishedListener` is a listener to declare as follows:
```java	
	private initFinishListener initFinishedListener = new initFinishListener() {

		@Override
		public void onSuccess() {
			// TODO Do on init success. Most probably showInterstitialAd();
		}

		@Override
		public void onFail() {
			// TODO Do on init failure
		}
	};
```

> You might want to add `RedTroops.getInstance(mContext).showInterstitialAd(mContext);` in your onSuccess()

3) Add two methods to your Activity:
```java
	public static void showInterstitialAd(){
		mUnityPlayer.currentActivity.runOnUiThread(new Runnable() {
			
			@Override
			public void run() {
				RedTroops.getInstance(mContext).showInterstitialAd(mContext);
			}
		});
	}
```
```java
	public static void showBannerAd(){
		mUnityPlayer.currentActivity.runOnUiThread(new Runnable() {
			
			@Override
			public void run() {
				RedTroops.getInstance(mContext).showBanner(mContext, Gravity.BOTTOM | Gravity.CENTER_HORIZONTAL);
			}
		});
	}
```

4) In your activity's `onPause`:
```java
	RedTroops.getInstance(mContext).onPause();
```

5) In your activity's `onResume`:
```java
	RedTroops.getInstance(mContext).onResume();
```

You may refer to UnityTest app that is available.
