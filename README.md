# Facebook_Ads
Set up Facebook Ad network in your android project

>Step 1. Add the JitPack repository to your app-level build.gradle file
```
repositories {
  mavenCentral()
}
```
>Step 2.Add the ependencie build.gradle file with current version:
```
dependencies {
compile 'com.facebook.android:audience-network-sdk:6.+'
}
```

>Step 3.Updating Your AndroidManifest.xml File
```
<uses-permission android:name="android.permission.INTERNET" />  
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />  
```

>Step 4.Adding a Layout Container for the Banner Ad in xml:
```
<LinearLayout
android:id="@+id/banner_container"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:layout_alignParentBottom="true"
android:orientation="vertical"
app:layout_constraintBottom_toBottomOf="parent"
/>
```
  
  >Step 5. Implementing the Banner in your Activity:
```
private AdView adView;

@Override
public void onCreate(Bundle savedInstanceState) {
...
adView = new AdView(this, "IMG_16_9_APP_INSTALL#YOUR_PLACEMENT_ID", AdSize.BANNER_HEIGHT_50);

// Find the Ad Container
LinearLayout adContainer = (LinearLayout) findViewById(R.id.banner_container);

// Add the ad view to your activity layout
adContainer.addView(adView);

// Request an ad
adView.loadAd();
}
```
  
  >Step 6.add the following code to your activity's onDestroy():
```
@Override
protected void onDestroy() {
if (adView != null) {
adView.destroy();
}
super.onDestroy();
}
```
  >Step 7.Showing Interstitial Ads:
```
private final String TAG = InterstitialAdActivity.class.getSimpleName();
    private InterstitialAd interstitialAd;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
      
        interstitialAd = new InterstitialAd(this, "YOUR_PLACEMENT_ID");
        InterstitialAdListener interstitialAdListener = new InterstitialAdListener() {
            @Override
            public void onInterstitialDisplayed(Ad ad) {
                // do something here after displadey ad
            }
            @Override
            public void onInterstitialDismissed(Ad ad) {
                // do something here after Dismissed ad
            }
            @Override
            public void onError(Ad ad, AdError adError) {
                // do something here if ad Error
            }
            @Override
            public void onAdLoaded(Ad ad) {
                // Interstitial ad is loaded and ready to be displayed
                // Show the ad
                interstitialAd.show();
            }
            @Override
            public void onAdClicked(Ad ad) {
                // do something here if ad click
            }
            @Override
            public void onLoggingImpression(Ad ad) {
                // Ad impression logged callback
            }
        };
        // For auto play video ads, it's recommended to load the ad
        // at least 30 seconds before it is shown
        interstitialAd.loadAd(
                interstitialAd.buildLoadAdConfig()
                        .withAdListener(interstitialAdListener)
                        .build());
    }
}
```
  
  >Step 8.(It Not necessary)Display the ad in a few seconds or minutes after it is successfully loaded.:
```
private void showAdWithDelay() {
       Handler handler = new Handler();
       handler.postDelayed(new Runnable() {
           public void run() {
                // Check if interstitialAd has been loaded successfully
               if(interstitialAd == null || !interstitialAd.isAdLoaded()) {
                   return;
               }
                // Check if ad is already expired or invalidated, and do not show ad if that is the case. You will not get paid to show an invalidated ad.
               if(interstitialAd.isAdInvalidated()) {
                   return;
               }
               // Show the ad
                interstitialAd.show(); 
           }
       }, 1000 * 60 * 15); // Show the ad after 15 minutes
    }
}
```
  
  >Step 9.(Replace with step6:when use banner and interstitial both)add the following code to your Activity's onDestroy():
```
@Override
protected void onDestroy() {
    if (interstitialAd != null) {
        interstitialAd.destroy();
       
    }
    super.onDestroy();
}
```
  
  
