## Overview
This repository is create for songbai
## Getting Started

```
allprojects {
	repositories {
	    maven { url 'https://jitpack.io' }
        maven { url 'https://dl.bintray.com/umsdk/release' }
        maven { url "https://lax.bintray.com/easyweb" }	}
}
 
implementation 'com.lax.ezweb:easyweb:0.0.4'
```

## In app gradle
```
manifestPlaceholders = [
	GETUI_APP_ID      : "",
	GETUI_APP_KEY     : "",
	GETUI_APP_SECRET  : "",
	UMENG_APP_KEY     : "",
    UMENG_CHANNEL     : "origin",
	ADJUST_CODE       : "",
	ADJUST_TRACK_TOKEN: "",
	FACEBOOK_APP_ID   : "",
	CHANNEL     : "",
]
```

## In Manifest
```
<meta-data
    android:name="io.branch.sdk.BranchKey"
    android:value="{Branch Key}" />
<meta-data
    android:name="io.branch.sdk.BranchKey.test"
    android:value="" />
<meta-data
    android:name="io.branch.sdk.TestMode"
    android:value="false" />
<meta-data
    android:name="PUSH_APPID"
    android:value="${GETUI_APP_ID}" />
<meta-data
    android:name="PUSH_APPKEY"
    android:value="${GETUI_APP_KEY}" />
<meta-data
    android:name="PUSH_APPSECRET"
    android:value="${GETUI_APP_SECRET}" />

<meta-data
    android:name="com.facebook.sdk.ApplicationId"
    android:value="${FACEBOOK_APP_ID}" />

<provider
    android:name="com.facebook.FacebookContentProvider"
    android:authorities="com.facebook.app.FacebookContentProvider${FACEBOOK_APP_ID}"
    android:exported="false" />

<activity
    android:name="com.facebook.FacebookActivity"
    android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
    android:label="@string/app_name" />

<activity
    android:name="com.facebook.CustomTabActivity"
    android:exported="true">
    <intent-filter><action android:name="android.intent.action.VIEW" />

        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <data android:scheme="fb${FACEBOOK_APP_ID}" />
    </intent-filter>
</activity>

<provider
    android:name="com.lax.ezweb.EzWebInitProvider"
    android:authorities="${applicationId}.ezwebprovider"
    android:exported="false" />

```
### In launcher 
###example

```
<activity
android:name=".SplashActivity"
android:theme="@style/AppTheme.Splash">
	<intent-filter>
	    <action android:name="android.intent.action.MAIN" />
	
	    <category android:name="android.intent.category.LAUNCHER" />
	</intent-filter>
	
	
	<!-- Branch URI Scheme -->
	<intent-filter>
	    <data android:scheme="{your scheme}" />
	    <action android:name="android.intent.action.VIEW" />
	
	    <category android:name="android.intent.category.DEFAULT" />
	    <category android:name="android.intent.category.BROWSABLE" />
	</intent-filter>
	
	<!-- Branch App Links (optional) -->
	<intent-filter android:autoVerify="true">
	    <action android:name="android.intent.action.VIEW" />
	
	    <category android:name="android.intent.category.DEFAULT" />
	    <category android:name="android.intent.category.BROWSABLE" />
	
	    <data
	        android:host="{Default Link Domain}"
	        android:scheme="https" />
	    <!-- example-alternate domain is required for App Links when the Journeys/Web SDK and Deepviews are used inside your website.  -->
	    <data
	        android:host="{Alternate Link Domaininfo_outline}"
	        android:scheme="https" />
	</intent-filter>
</activity>
```
## In Launch Activity

```
override fun onStart() {
    super.onStart()
    Branch.sessionBuilder(this).withCallback(BranchListener).withData(intent.data).init()
}

override fun onNewIntent(intent: Intent) {
    super.onNewIntent(intent)
    this.intent = intent
    Branch.sessionBuilder(this).withCallback(BranchListener).withData(intent.data).reInit()
}

object BranchListener : Branch.BranchReferralInitListener {
    override fun onInitFinished(referringParams: JSONObject?, error: BranchError?) {
        if (error == null) {
            Log.i("BRANCH SDK", referringParams.toString())
        } else {
            Log.e("BRANCH SDK", error.message)
        }
    }
}
```